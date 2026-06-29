---
layout: post
title: "Migrate from wordpress to github pages"
date: 2026-06-29 14:15:54 +0800
categories: 
 - Web
 - Linux
author: "Xiao Bo Jiang"
render_with_liquid: false
---
# Why?
Earlier I am using wordpress, which is very nice, but it costs few hundreds every year for cloud computing and maintaining the domain name (iotcolon.com). Later, I found github pages well suited my request on writing technical posts.
Why not make such migration? and it naturally link to my github.
What basically needed is to create a repo with my username, and leverage github pages and actions to host the website.
This post will talk about how to make the migration, most of repetitive actions are batched. so do not need to worry. 

# WordPress to Jekyll + GitHub Pages Migration Summary

This post summarizes the major steps used to migrate a WordPress site to a free GitHub Pages site powered by Jekyll.

## Goal

Move from a paid/self-hosted WordPress website to a static website hosted on GitHub Pages:

```text
WordPress export
→ Jekyll posts
→ GitHub repository named username.github.io
→ GitHub Pages deployment
```

Target repository:

```text
xiaobojiang.github.io
```

Target site URL:

```text
https://xiaobojiang.github.io/
```

---

## 1. Create and verify a basic Jekyll site

Create a clean Jekyll project locally:

```bash
cd ~/MyGit
jekyll new xiaobojiang.github.io
cd xiaobojiang.github.io
bundle install
bundle exec jekyll serve
```

Open:

```text
http://127.0.0.1:4000/
```

The default Jekyll site uses the Minima theme and creates files such as:

```text
_config.yml
Gemfile
index.markdown
about.markdown
_posts/
```

Edit `_config.yml` to update the site title, description, footer, email, and social links.

---

## 2. Create the GitHub Pages repository

Create a GitHub repository with this exact name pattern:

```text
YOUR_GITHUB_USERNAME.github.io
```

For example:

```text
xiaobojiang.github.io
```

Connect the local Jekyll project to GitHub:

```bash
git init
git add .
git commit -m "Initial Jekyll site"
git branch -M main
git remote add origin git@github.com:xiaobojiang/xiaobojiang.github.io.git
git push -u origin main
```

In GitHub repository settings, enable GitHub Pages:

```text
Repository → Settings → Pages
```

Initially, branch deployment can be used:

```text
Source: Deploy from a branch
Branch: main
Folder: /root
```

Later, this was changed to GitHub Actions because the imported technical posts needed Jekyll 4 behavior.

---

## 3. Export WordPress content

In WordPress Admin:

```text
Dashboard → Tools → Export → All content → Download Export File
```

This produces a WordPress WXR XML file, for example:

```text
xiao039stechnote.WordPress.2026-06-23.xml
```

The XML contains posts, pages, metadata, comments, categories, tags, and attachment references. It does not contain the actual image bytes. Image records look like this:

```xml
<wp:attachment_url><![CDATA[https://www.iotcolon.com/wp-content/uploads/2023/10/image-3.png]]></wp:attachment_url>
<wp:meta_key><![CDATA[_wp_attached_file]]></wp:meta_key>
<wp:meta_value><![CDATA[2023/10/image-3.png]]></wp:meta_value>
```

That is normal. WordPress exports references to media, while the real uploaded files live under:

```text
wp-content/uploads/
```

Useful check:

```bash
grep "<wp:post_type>" ~/Downloads/xiao039stechnote.WordPress.2026-06-23.xml | sort | uniq -c
```

You should see entries such as:

```text
<wp:post_type><![CDATA[post]]></wp:post_type>
<wp:post_type><![CDATA[page]]></wp:post_type>
<wp:post_type><![CDATA[attachment]]></wp:post_type>
```

---

## 4. Import WordPress XML into Jekyll

Install the Jekyll importer and dependency:

```bash
gem install jekyll-import
gem install open_uri_redirections
```

Run the importer:

```bash
cd ~/MyGit/xiaobojiang.github.io
jekyll-import wordpressdotcom \
  --source "/Users/xiaobo/Downloads/xiao039stechnote.WordPress.2026-06-23.xml"
```

If image downloading is too slow or not desired, use:

```bash
jekyll-import wordpressdotcom \
  --no-fetch-images \
  --source "/Users/xiaobo/Downloads/xiao039stechnote.WordPress.2026-06-23.xml"
```

After import, posts appear under:

```text
_posts/
```

For example:

```text
_posts/2023-04-17-protecting-your-application-with-oauth-proxy-and-azure-sso.html
```

---

## 5. Handle images

The importer may try to download images automatically from WordPress media URLs. If that works, it is fine.

A safer long-term method is to copy WordPress uploads manually:

```text
wp-content/uploads/
```

into the Jekyll project:

```text
assets/images/
```

Example:

```bash
mkdir -p assets/images
cp -R ~/Downloads/uploads/* assets/images/
```

Then replace WordPress image URLs in imported posts:

```bash
find _posts -name "*.html" -type f -exec sed -i '' \
's#https://www.iotcolon.com/wp-content/uploads/#/assets/images/#g' {} +
```

Repeat for Markdown files if needed:

```bash
find _posts -name "*.md" -type f -exec sed -i '' \
's#https://www.iotcolon.com/wp-content/uploads/#/assets/images/#g' {} +
```

---

## 6. Fix imported WordPress front matter

Some WordPress importer output had nested author metadata like this:

```yaml
author:
  login: xiaobo
  email: a@b.c
  display_name: xiaobo Jiang
  first_name: xiaobo
  last_name: Jiang
```

The Minima layout expected a string, so it rendered the whole author hash on the page.

Fix by converting nested author data to a simple string:

```yaml
author: "xiaobo Jiang"
```

Script used:

```bash
ruby - <<'RUBY'
Dir.glob("_posts/*.{md,markdown,html}").each do |file|
  lines = File.readlines(file)
  next unless lines[0]&.strip == "---"

  closing = nil
  lines[1..].each_with_index do |line, idx|
    if line.strip == "---"
      closing = idx + 1
      break
    end
  end
  next unless closing

  front = lines[1...closing]
  body = lines[(closing + 1)..] || []

  new_front = []
  i = 0

  while i < front.length
    line = front[i]

    if line =~ /^author:\s*$/
      block = []
      i += 1
      while i < front.length && front[i] =~ /^\s+/
        block << front[i]
        i += 1
      end

      display_name = block.find { |l| l =~ /^\s*display_name:\s*(.+?)\s*$/ }
      author_name = display_name ? display_name.match(/^\s*display_name:\s*(.+?)\s*$/)[1].strip : "Xiao Bo Jiang"
      author_name = author_name.gsub(/^["']|["']$/, "")

      new_front << "author: \"#{author_name}\"\n"
      next
    else
      new_front << line
      i += 1
    end
  end

  File.write(file, "---\n" + new_front.join + "---\n" + body.join)
  puts "fixed #{file}"
end
RUBY
```

---

## 7. Remove incorrect root permalinks

The importer created many posts with:

```yaml
permalink: "/"
```

That caused posts to overwrite the homepage because every post tried to generate as the site root.

Find bad permalinks:

```bash
grep -R -n '^permalink:' . \
  --exclude-dir=.git \
  --exclude-dir=_site \
  --exclude-dir=.jekyll-cache
```

Remove root permalinks from posts:

```bash
find _posts -type f \( -name "*.md" -o -name "*.markdown" -o -name "*.html" \) \
  -exec sed -i '' '/^permalink: "\/"$/d' {} +

find _posts -type f \( -name "*.md" -o -name "*.markdown" -o -name "*.html" \) \
  -exec sed -i '' '/^permalink: \/$/d' {} +
```

The homepage is the only file that should own `/`:

```yaml
---
layout: page
title: Articles
permalink: /
---
```

---

## 8. Build a proper landing page with all articles

Replace `index.markdown` with an article index:

```markdown
---
layout: page
title: Articles
permalink: /
---

Welcome to Xiao's technical notes.

## All Articles

<ul>
{% for post in site.posts %}
  <li>
    <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    —
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
```

Important: do not add this to `index.markdown`:

```yaml
render_with_liquid: false
```

The homepage needs Liquid enabled so the `{% for post in site.posts %}` loop can run.

Verify locally:

```bash
bundle exec jekyll clean
bundle exec jekyll build
ls -la _site/index.html
grep -n "All Articles" _site/index.html
bundle exec jekyll serve
```

Open:

```text
http://127.0.0.1:4000/
```

---

## 9. Disable Liquid rendering for imported technical posts

Imported technical posts may contain code snippets like:

```text
{{.Repository}}
{{.ID}}
{{ find_result.files|map(attribute='path')|list }}
```

Jekyll treats `{{ ... }}` as Liquid syntax, even inside some code examples. This caused build failures.

For imported posts, add this to post front matter:

```yaml
render_with_liquid: false
```

Bulk add it to posts:

```bash
ruby - <<'RUBY'
Dir.glob("_posts/*.{md,markdown,html}").each do |file|
  text = File.read(file)

  next unless text.start_with?("---\n")
  next if text.include?("\nrender_with_liquid:")

  parts = text.split(/^---\s*$/, 3)
  next unless parts.size >= 3

  front = parts[1]
  body = parts[2]

  File.write(file, "---\n#{front}render_with_liquid: false\n---#{body}")
  puts "updated #{file}"
end
RUBY
```

This works with Jekyll 4. GitHub Pages’ default builder may use an older Jekyll environment, so the deployment was later changed to GitHub Actions.

---

## 10. Remove generated output from Git

Do not commit generated output folders:

```text
_site/
.jekyll-cache/
.sass-cache/
```

Add them to `.gitignore`:

```bash
cat >> .gitignore <<'EOF'
_site/
.jekyll-cache/
.sass-cache/
EOF
```

Remove them from Git tracking:

```bash
git rm -r --cached _site 2>/dev/null || true
git rm -r --cached _posts/_site 2>/dev/null || true
rm -rf _site _posts/_site
```

---

## 11. Switch GitHub Pages to GitHub Actions deployment

GitHub Pages default branch deployment used:

```text
github-pages v232
jekyll v3.10.0
minima 2.5.1
```

But the local site used newer Jekyll behavior, especially `render_with_liquid: false`. To make deployment match local behavior, switch GitHub Pages to GitHub Actions.

Create:

```text
.github/workflows/pages.yml
```

Workflow:

```yaml
name: Build and deploy Jekyll site

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.3'
          bundler-cache: true

      - name: Build with Jekyll
        run: bundle exec jekyll build

      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./_site

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Then in GitHub:

```text
Repository → Settings → Pages → Build and deployment → Source → GitHub Actions
```

Commit and push:

```bash
git add .github/workflows/pages.yml
git commit -m "Deploy Jekyll with GitHub Actions"
git push origin main
```

Check deployment:

```text
Repository → Actions → Build and deploy Jekyll site
```

When green, test:

```text
https://xiaobojiang.github.io/
```

---

## 12. Final verification checklist

Before shutting down WordPress hosting, verify:

- Homepage shows the article list.
- Individual posts open correctly.
- Images load correctly.
- Code blocks render without Liquid errors.
- Author metadata no longer renders as a hash.
- `_site/` is not tracked by Git.
- GitHub Actions deployment is green.
- No secrets remain in posts or Git history.
- Old WordPress media URLs are either intentionally preserved or replaced with local `/assets/images/` paths.

Useful commands:

```bash
bundle exec jekyll clean
bundle exec jekyll build
bundle exec jekyll serve

git status
git ls-files | grep '_site'
grep -R -n '^permalink: "/"$' _posts
grep -R -n -i 'client_secret\|client-secret\|azure.*secret' . \
  --exclude-dir=.git \
  --exclude-dir=_site \
  --exclude-dir=.jekyll-cache
```

---

## References

- WordPress export creates an XML export file from selected content filters: https://wordpress.org/documentation/article/tools-export-screen/
- WordPress.com export is used to transfer posts, pages, and comments to another platform: https://wordpress.com/support/export/
- `jekyll-import wordpressdotcom` supports `--source`, `--no-fetch-images`, and `--assets_folder`: https://import.jekyllrb.com/docs/wordpressdotcom/
- Jekyll importer notes that the database-based WordPress importer converts posts/front matter but does not import images or external files: https://import.jekyllrb.com/docs/wordpress/
- Jekyll uses Liquid, where `{{ variable }}` outputs variables and `{% ... %}` performs logic: https://jekyllrb.com/docs/liquid/
- Jekyll notes that Liquid can be processed inside code blocks; use raw/endraw or `render_with_liquid: false` in Jekyll 4: https://jekyllrb.com/docs/liquid/tags/
- GitHub Pages supports custom workflows with GitHub Actions: https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages
- Jekyll’s GitHub Actions docs explain that GitHub Pages default builds run in a restricted environment and GitHub Actions gives more control: https://jekyllrb.com/docs/continuous-integration/github-actions/
