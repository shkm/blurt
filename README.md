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
- **ImageMagick** (optional) - for OpenGraph image generation
- **python3** (optional) - for the dev server (`start` command)
- **fswatch** (optional) - for instant rebuilds during development
- **curl** or **wget** (optional) - for self-updating

## Docker

A Docker image is available at [shkm/blurt](https://hub.docker.com/r/shkm/blurt):

```bash
docker run --rm -v $(pwd):/repo shkm/blurt
```

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

# Update to the latest version
./blurt update

# Show version
./blurt version
```

## Structure

```
├── blurt             # CLI
├── config.yaml       # Settings
├── templates/
│   ├── index.html    # Index page template
│   ├── post.html     # Post page template
│   ├── social.html   # OpenGraph/social meta tags partial
│   └── *.html        # Other partials (optional)
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

## Partials

You can create reusable template snippets in `templates/`. For example, `templates/footer.html` can be included in other templates with `$footer()$`.

## Placeholders

The following placeholders are replaced at build time:

- `__BUILD_TIME__` - Time taken to build that specific page (e.g., `42ms`)
- `__COMMIT_SHA__` - Short git commit hash, uses `GITHUB_SHA` env var if available
- `__BLURT_VERSION__` - Blurt version number (e.g., `1.0.1`)

Example usage in a template:

```html
<footer>
  <p>Built in __BUILD_TIME__ · <a href="https://github.com/you/repo/commit/__COMMIT_SHA__">__COMMIT_SHA__</a></p>
</footer>
```

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

## Atom Feed

An Atom feed is automatically generated at `dist/feed.xml` when `site_url` is configured in `config.yaml`. The feed includes the full HTML content of each post.

Required config:

```yaml
site_url: "https://example.com"
```

Optional config for the feed:

```yaml
site_title: "My Blog"
site_author: "Your Name"
```

## OpenGraph Images

When ImageMagick is installed, blurt automatically generates OpenGraph images for social sharing. Requires the [Open Sans](https://fonts.google.com/specimen/Open+Sans) font to be installed. Images are created at:

- `dist/og.jpg` - for the index page
- `dist/<post>/og.jpg` - for each post

Images use the page title and description (if available) on a subtle gradient background. The `social.html` partial includes all necessary meta tags for OpenGraph and Twitter cards.
