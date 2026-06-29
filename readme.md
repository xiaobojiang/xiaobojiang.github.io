# Xiao's Notes

This repository contains the source for my personal Jekyll site:

```text
https://xiaobojiang.github.io/
```

The site is built with Jekyll and deployed to GitHub Pages through GitHub Actions.

## Repository layout

```text
.
├── _posts/                 # Blog posts
├── assets/
│   └── images/             # Images used by posts
├── bin/
│   └── newpost             # Helper script for creating new posts
├── .github/
│   └── workflows/
│       └── pages.yml       # GitHub Pages deployment workflow
├── _config.yml             # Jekyll site configuration
├── index.markdown          # Landing page / article list
└── README.md
```

## Create a new post

Use the helper script:

```bash
bin/newpost "My New Article Title"
```

The script creates:

```text
_posts/YYYY-MM-DD-my-new-article-title.md
assets/images/YYYY/MM/my-new-article-title/
```

Example:

```bash
bin/newpost "Use Legacy SCP Protocol"
```

This creates something like:

```text
_posts/2026-06-23-use-legacy-scp-protocol.md
assets/images/2026/06/use-legacy-scp-protocol/
```

## Edit the post

Open the generated Markdown file:

```bash
nano _posts/YYYY-MM-DD-my-new-article-title.md
```

A post should have front matter like this:

```yaml
---
layout: post
title: "My New Article Title"
date: 2026-06-23 10:00:00 +0800
categories: [notes]
author: "Xiao Bo Jiang"
render_with_liquid: false
---
```

Use `render_with_liquid: false` for technical posts that may contain code snippets with `{{ ... }}`, Ansible/Jinja syntax, Docker templates, Helm templates, or other text that looks like Liquid syntax.

## Add images

Put images in the folder created by the script:

```text
assets/images/YYYY/MM/post-slug/
```

Example:

```bash
cp ~/Downloads/screenshot.png assets/images/2026/06/use-legacy-scp-protocol/
```

Reference the image in Markdown:

```markdown
![Screenshot](/assets/images/2026/06/use-legacy-scp-protocol/screenshot.png)
```

Because technical posts use `render_with_liquid: false`, prefer direct image paths like `/assets/images/...` instead of Liquid expressions such as `{{ "/assets/..." | relative_url }}`.

## Preview locally

Run:

```bash
source ~/.zshrc
bundle exec jekyll clean
bundle exec jekyll serve
```

Open:

```text
http://127.0.0.1:4000/
```

Check:

```text
The new post appears on the Articles page.
The title and date are correct.
Images load correctly.
Code blocks render correctly.
No secrets, tokens, passwords, or API keys are included.
```

## Secret check before pushing

Before pushing, run a quick scan:

```bash
grep -R -n -i "password\|secret\|token\|client_secret\|api_key" _posts assets --exclude-dir=_site
```

If the result contains real credentials, replace them with placeholders before committing.

Good examples:

```text
client_secret: "REDACTED"
api_key: "<your-api-key>"
password: "<your-password>"
```

## Publish

After local preview looks good:

```bash
git status
git add .
git commit -m "Add post: My New Article Title"
git push origin main
```

Then check:

```text
GitHub repository → Actions
```

Wait for the GitHub Pages workflow to finish successfully.

The live site will update at:

```text
https://xiaobojiang.github.io/
```

## Helper script location

The post helper script lives here:

```text
bin/newpost
```

Make sure it is executable:

```bash
chmod +x bin/newpost
git update-index --chmod=+x bin/newpost
```

## Typical workflow

```bash
cd ~/MyGit/xiaobojiang.github.io

bin/newpost "Article Title"

# Edit the generated post
nano _posts/YYYY-MM-DD-article-title.md

# Add images if needed
cp ~/Downloads/image.png assets/images/YYYY/MM/article-title/

# Preview
source ~/.zshrc
bundle exec jekyll serve

# Publish
git add .
git commit -m "Add post: Article Title"
git push origin main
```

## Notes

- Jekyll posts live in `_posts/` and use the `YEAR-MONTH-DAY-title.MARKUP` filename pattern.
- Images and other static files can be stored under `assets/`; Jekyll copies normal static files into the generated site.
- Keep `_site/`, `.jekyll-cache/`, and `.sass-cache/` out of Git.

