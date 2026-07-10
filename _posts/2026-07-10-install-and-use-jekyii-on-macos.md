---
layout: post
title: "install and use jekyII on macos"
date: 2026-07-10 15:51:05 +0800
categories:
  # Enable applicable categories by uncommenting one or more lines below.
  # - AI
 - Cloud
 - Devops
  # - Internet of Things
  # - IoT
  # - Kubernetes
  # - Life
 - Linux
  # - One Line Tip
  # - Product
  # - Python
  # - Robotics
  # - Technology
  # - Web
author: "Xiao Bo Jiang"
render_with_liquid: false
---

For my wordpress migration to github page, I use jekyII for setup and build my pages, here is the manual for your potential interest.

This note documents how I installed Jekyll on macOS using `ruby-install` and `chruby`.

The main reason for doing this is simple: macOS already includes a system Ruby at `/usr/bin/ruby`, but it is not recommended for installing Jekyll gems. A separate Ruby installation keeps the Jekyll environment clean and avoids changing the system Ruby.

## Install dependencies

First install Homebrew if it is not already installed.

Then install `ruby-install` and `chruby`:

```bash
brew install ruby-install chruby
```

`ruby-install` is used to install Ruby versions. `chruby` is used to switch between installed Ruby versions.

## Install Ruby

List available Ruby versions:

```bash
ruby-install
```

Install Ruby:

```bash
ruby-install ruby 4.0.5
```

After installation, Ruby is installed under:

```text
~/.rubies/ruby-4.0.5/
```

At this point, the new Ruby exists, but the shell may still use the macOS system Ruby:

```bash
which ruby
ruby -v
```

If the output is still:

```text
/usr/bin/ruby
```

then `chruby` has not been enabled yet.

## Enable chruby in zsh

On macOS, the default shell is usually `zsh`.

For Apple Silicon Macs, add this to `~/.zshrc`:

```bash
# chruby
source /opt/homebrew/opt/chruby/share/chruby/chruby.sh
source /opt/homebrew/opt/chruby/share/chruby/auto.sh
chruby ruby-4.0.5
```

For Intel Macs, the Homebrew path may be different:

```bash
# chruby
source /usr/local/opt/chruby/share/chruby/chruby.sh
source /usr/local/opt/chruby/share/chruby/auto.sh
chruby ruby-4.0.5
```

Reload the shell configuration:

```bash
source ~/.zshrc
hash -r
```

Now check Ruby again:

```bash
which ruby
ruby -v
```

Expected result:

```text
/Users/<username>/.rubies/ruby-4.0.5/bin/ruby
ruby 4.0.5 ...
```

## Add a project Ruby version

Inside the Jekyll site repository, create a `.ruby-version` file:

```bash
cd ~/MyGit/xiaobojiang.github.io

echo "ruby-4.0.5" > .ruby-version
```

With `chruby` auto-switching enabled, entering this folder will automatically select the correct Ruby version.

Check it:

```bash
cd ~/MyGit/xiaobojiang.github.io

chruby
which ruby
ruby -v
```

## Install Jekyll dependencies

Inside the site repository, install the gems:

```bash
bundle install
```

Then verify Jekyll:

```bash
bundle exec jekyll -v
```

## Serve the site locally

Run:

```bash
bundle exec jekyll clean
bundle exec jekyll serve
```

Open the local site:

```text
http://127.0.0.1:4000/
```

## Common issue: Ruby still points to /usr/bin/ruby

If Ruby still points to `/usr/bin/ruby`, check these items:

```bash
echo $SHELL
grep chruby ~/.zshrc
ls ~/.rubies
chruby
type -a ruby
```

Common causes:

```text
chruby is installed but not loaded in ~/.zshrc
The wrong Homebrew path is used
A new terminal was not opened after editing ~/.zshrc
No Ruby version was selected with chruby
The project has no .ruby-version file
```

Manually select Ruby:

```bash
chruby ruby-4.0.5
```

Then check again:

```bash
which ruby
ruby -v
```

## Publish changes

After confirming the site works locally:

```bash
git status
git add .
git commit -m "Add post: Install Jekyll on macOS"
git push origin main
```

The GitHub Actions workflow will build and deploy the site to GitHub Pages.

## Summary

The key point is that `ruby-install` installs Ruby, but it does not automatically make the shell use it.

The actual Ruby switching is done by `chruby`.

The final working setup is:

```text
ruby-install  -> installs Ruby into ~/.rubies/
chruby        -> switches the shell to that Ruby
.zshrc        -> enables chruby for every new terminal
.ruby-version -> selects the Ruby version for the Jekyll repo
bundle exec   -> runs Jekyll using the repo's Ruby gems
```
