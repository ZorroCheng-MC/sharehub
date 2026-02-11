---
---

# ShareHub

ShareHub is a free, self-hosted platform for publishing and sharing documents — powered by GitHub Pages, no backend required.

**Two ways to share your content:**

1. **Publish** — push markdown or HTML files to your repo and they become web pages at your GitHub Pages URL. Add `access: private` to any document's frontmatter to password-protect it.

2. **Share via URL** — compress a note into a single URL and send it to anyone. The content lives entirely in the link (no server storage). Recipients can add inline comments and send back an annotated URL for you to merge.

---

## Quick Start

Download the [latest release](https://github.com/ZorroCheng-MC/sharehub/releases/latest) (core platform only, no sample content):

```bash
# Download and extract
gh release download --repo ZorroCheng-MC/sharehub --pattern "sharehub-core-*.zip"
unzip sharehub-core-*.zip -d my-sharehub
cd my-sharehub

# Initialize git
git init
git add .
git commit -m "Initial commit from sharehub template"
```

### Deploy to GitHub Pages

```bash
# Create your repo
gh repo create my-sharehub --public --source=. --push

# Enable GitHub Pages
gh repo edit --enable-pages --pages-branch main
```

Edit `_config.yml`:
```yaml
baseurl: "/my-sharehub"  # Change to YOUR repo name (empty string if using custom domain)
url: "https://YOUR_USERNAME.github.io"
```

Your site will be live at `https://YOUR_USERNAME.github.io/my-sharehub` after the GitHub Actions workflow completes.

---

## Core Files

These files make up the platform — everything else is personal content:

```
sharehub/
├── _config.yml          # Jekyll configuration
├── _layouts/            # Page layouts with access control
│   ├── default.html
│   ├── private_protected.html
│   └── universal.html
├── .github/workflows/   # GitHub Pages deployment
├── index.html           # Document listing with search/filter
├── share.html           # Serverless note sharing page
├── Gemfile              # Ruby dependencies
├── .gitignore
├── README.md
├── documents/           # YOUR documents go here
└── images/              # YOUR images go here
```

---

## Publishing Documents

### Public Document (default)

Place in `documents/` with frontmatter:

```yaml
---
title: "My Document"
date: 2026-01-15
---

Content here...
```

### Private Document

Add `access: private` — password is "maco":

```yaml
---
title: "Confidential Report"
access: private
---

Private content...
```

### Publish and Push

```bash
git add documents/my-document.md
git commit -m "Add: my document"
git push
```

Document URL: `https://your-domain/documents/my-document.html`

---

## Sharing Notes via URL

### How It Works

`share.html` encodes note content directly into the URL fragment using zlib compression + Base64. No server storage — the recipient decodes the URL client-side.

**URL format**: `https://your-domain/share#<compressed-base64-data>`

### Using with kf-claude

If you use the [kf-claude](https://github.com/ZorroCheng-MC/kf-claude) Claude Code plugin:

```
/kf-claude:share my-note.md
```

This compresses the note and copies the share URL to your clipboard.

The public decoder is hosted at `https://sharehub.zorro.hk/share` — all kf-claude users can use this without deploying their own instance. To use your own decoder, set `share_base_url` in `.claude/config.local.json`:

```json
{
  "share_base_url": "https://your-domain/share"
}
```

### Annotation Workflow

1. **Share** — generate a share URL for your note
2. **Annotate** — recipient opens URL, adds inline comments, gets a new URL with annotations
3. **Merge** — open the annotated URL and click "Merge" to see annotations inline with the original content
4. **Export** — download the merged result as markdown

### URL Length Limits

| Length | Status |
|--------|--------|
| < 4,000 chars | Safe for all platforms |
| 4,000–8,000 chars | May be truncated by some messaging apps |
| > 8,000 chars | Will likely be truncated — use `/publish` instead |

CRC32 checksums detect corruption. If a URL is truncated, the decoder shows an error with guidance.

---

## Access Control

| Frontmatter | Behavior |
|-------------|----------|
| (none) | Public — anyone can view |
| `access: private` | Password-protected ("maco") |

The index page shows only public documents by default. After entering the password, private documents become visible with a lock icon.

---

## Custom Domain

To use a custom domain instead of `github.io`:

1. Set up DNS (CNAME or A record pointing to GitHub Pages)
2. Add domain in repo Settings > Pages > Custom domain
3. Update `_config.yml`:
   ```yaml
   baseurl: ""
   url: "https://your-domain.com"
   ```

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Document not appearing | Check frontmatter syntax, wait 1-5 min for rebuild |
| Private doc accessible without password | Verify `access: private` in frontmatter |
| Share URL shows error | URL may be corrupted in transit — check CRC error message |
| Images not loading | Use relative paths: `./images/filename.jpg` |

---

## License

MIT
