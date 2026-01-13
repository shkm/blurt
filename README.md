# blurt

You want to write something and share it with the world. But you'd need to set up a blog. Woe! Who has the time?

Hold up there, sunshine. Blurt's got your back. This is everything you _really_ need to write and publish stuff on the internet.

## Quick start
1. [Create a repo from this template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template#creating-a-repository-from-a-template)
2. On your repo, go to settings -> Pages -> Source: select GitHub Actions
3. Clone your repo

Now just write your posts (see docs below), commit and push. After a short while, your repo should have a deployment (check the right side).

That's it!

## Requirements

- **pandoc** - for the actual heavy lifting
- **python3** (optional) - for the dev server (`start` command)
- **fswatch** (optional) - for instant rebuilds during development

## Usage

```bash
# Create a new post
./blurt new "My Post Title"

# Create and open in $EDITOR
./blurt new -e "My Post Title"

# Build the site
./blurt build

# Watch, rebuild, and serve at http://localhost:8000
./blurt start
```

## Structure

```
├── blurt         # CLI
├── config.yaml       # Settings
├── templates/
│   ├── index.html    # Index page template
│   └── post.html     # Post page template
├── static/           # Static files (copied to dist/)
├── posts/            # Markdown posts
└── dist/             # Generated site
```

## Templates

Templates use pandoc's variable syntax:

- `$blog_title$` - from config.yaml
- `$title$` - post title from frontmatter
- `$date$` - post date from frontmatter
- `$body$` - converted markdown content

## Posts

Posts are markdown files with YAML frontmatter:

```markdown
---
title: My Post Title
date: 2026-01-13
---

Post content here.
```

Output goes to `dist/my-post-title/index.html` for clean URLs.
