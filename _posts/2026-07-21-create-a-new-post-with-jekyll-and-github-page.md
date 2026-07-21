---
layout: post
title: "Create A new Post with Jekyll and github page"
date: 2026-07-21 14:21:57 +0800
categories:
  # Enable applicable categories by uncommenting one or more lines below.
  # - AI
  - Cloud
  # - Devops
  # - Internet of Things
  # - IoT
  # - Kubernetes
  # - Life
  # - Linux
  # - One Line Tip
  # - Product
  # - Python
  # - Robotics
  # - Technology
  - Web
author: "Xiao"
render_with_liquid: false
---

With effort mentioned in previous posts, you have a site which is built with Jekyll and deployed to GitHub Pages through GitHub Actions.

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

## How to create a new post
First build a helper script to create a Jekyll article template.
````bash
#!/usr/bin/env bash
set -euo pipefail

if [ "$#" -eq 0 ]; then
  echo "Usage:"
  echo "  bin/newpost \"Post Title\""
  echo
  echo "Example:"
  echo "  bin/newpost \"Use Legacy SCP Protocol\""
  exit 1
fi

title="$*"

slug=$(
  printf "%s" "$title" |
  tr '[:upper:]' '[:lower:]' |
  sed -E 's/[^a-z0-9]+/-/g; s/^-+//; s/-+$//'
)

if [ -z "$slug" ]; then
  echo "Error: could not generate a slug from title."
  exit 1
fi

year=$(date +%Y)
month=$(date +%m)
day=$(date +%d)
date_prefix=$(date +%Y-%m-%d)
datetime=$(date "+%Y-%m-%d %H:%M:%S %z")

post="_posts/${date_prefix}-${slug}.md"
imgdir="assets/images/${year}/${month}/${slug}"

if [ -e "$post" ]; then
  echo "Error: post already exists:"
  echo "  $post"
  exit 1
fi

mkdir -p _posts
mkdir -p "$imgdir"

cat > "$post" <<POST
---
layout: post
title: "$title"
date: $datetime
categories:
  # Enable applicable categories by uncommenting one or more lines below.
  # - AI
  # - Cloud
  # - Devops
  # - Internet of Things
  # - IoT
  # - Kubernetes
  # - Life
  # - Linux
  # - One Line Tip
  # - Product
  # - Python
  # - Robotics
  # - Technology
  # - Web
author: "Xiao Bo Jiang"
render_with_liquid: false
---

Write a short introduction here.

## Background

Explain the context or problem.

## Steps

\`\`\`bash
echo "example command"
\`\`\`

## Images

Put images in:

\`\`\`text
$imgdir/
\`\`\`

Then reference them like this:

\`\`\`markdown
![Screenshot](/$imgdir/screenshot.png)
\`\`\`

## Takeaway

Summarize the main point.
POST

echo "Created post:"
echo "  $post"
echo
echo "Created image folder:"
echo "  $imgdir"
echo
echo "Next:"
echo "  1. Edit $post"
echo "  2. Copy images into $imgdir/"
echo "  3. Run: bundle exec jekyll serve"
````

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
author: "Xiao"
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
https://{yourname}.github.io/
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
cd ~/MyGit/{yourname}.github.io

bin/newpost "Article Title"

# Edit the generated post
nano _posts/YYYY-MM-DD-article-title.md

# Add images if needed
cp ~/Downloads/image.png assets/images/YYYY/MM/article-title/

# Preview
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