---
---

# Share Hub - Tag-Based Document Sharing Portal

> **Jekyll-based publishing platform with password protection**
>
> Publish documents to GitHub Pages with optional password protection using frontmatter tags.

---

## 🚀 Quick Start

### What is Sharehub?

Sharehub is a **Jekyll-based document publishing platform** that lets you:

- Publish markdown and HTML documents to GitHub Pages
- Protect sensitive documents with password authentication
- Organize content with subfolders and tags
- Control access via simple frontmatter tags (not folder structure)

### Initial Setup (5 minutes)

**Step 1: Clone this template repository**

```bash
cd ~/Documents
git clone https://github.com/ZorroCheng-MC/sharehub.git
cd sharehub

# Remove template's git history
rm -rf .git
git init
git add .
git commit -m "Initial commit: My document portal"
```

**Step 2: Create your GitHub repository**

```bash
# Using GitHub CLI (recommended)
gh repo create my-sharehub --public --source=. --push

# Or manually at github.com/new, then:
# git remote add origin https://github.com/YOUR_USERNAME/my-sharehub.git
# git push -u origin main
```

**Step 3: Enable GitHub Pages**

```bash
# Using GitHub CLI
gh repo edit --enable-pages --pages-branch main

# Or manually: Settings → Pages → Source: "main" branch → Save
```

**Step 4: Configure Jekyll baseurl**

Edit `_config.yml` and change baseurl to match your repository name:

```yaml
baseurl: "/my-sharehub"  # IMPORTANT: Change to YOUR repo name
```

Commit and push:

```bash
git add _config.yml
git commit -m "Configure Jekyll baseurl"
git push
```

**Step 5: Verify deployment**

1. Go to: `https://github.com/YOUR_USERNAME/my-sharehub/actions`
2. Wait for "Deploy Jekyll site to Pages" workflow to complete (green checkmark)
3. Visit: `https://YOUR_USERNAME.github.io/my-sharehub`

**✅ Setup complete!** Your sharehub is ready to publish documents.

---

## 📖 Complete Documentation

### Overview

Share Hub is a Jekyll-based document sharing portal with tag-based access control. Documents can be public or private based on tags in their front matter, not their folder location. All documents are stored in a single `documents/` folder for better organization.

## 🆕 Tag-Based Protection System
Unlike traditional folder-based protection, Share Hub uses front matter tags to control access:
- **Public files**: No `access` tag needed (default)
- **Private files**: Add `access: private` in front matter
- **Password**: "maco" for all private documents

## Repository Structure
```
sharehub/
├── documents/          # All documents (both public and private)
│   ├── *.md           # Markdown files
│   ├── *.html         # HTML files
│   └── [subfolders]/  # Optional organization by topic
├── _layouts/          # Jekyll layouts (DO NOT MODIFY)
│   └── universal.html # Universal layout with protection logic
└── index.html         # Main listing page
```

## File Upload Guidelines

### 1. Public Documents
Documents are public by default. Simply upload to `documents/` folder.

**For Markdown files (.md):**
```yaml
---
title: "Document Title"
date: 2025-01-15
---

Your content here...
```

**For HTML files (.html):**
```yaml
---
title: "Document Title"
---
<!DOCTYPE html>
<html>
...
</html>
```

### 2. Private Documents
Add `access: private` tag to make any document password-protected.

**For Markdown files (.md):**
```yaml
---
title: "Confidential Document"
date: 2025-01-15
access: private
---

Your confidential content here...
```

**For HTML files (.html):**
```yaml
---
title: "Confidential Document"
access: private
---
<!DOCTYPE html>
<html>
...
</html>
```

### 3. Organizing with Subfolders
Create subfolders within `documents/` for topical organization:
```
documents/
├── reports/
│   ├── quarterly_report.md
│   └── annual_summary.html
├── presentations/
│   └── product_launch.md
└── internal/
    └── strategy.md (with access: private)
```

## Key Features

### Index Page Behavior
- **Before login**: Shows only public documents
- **After login**: Shows all documents (public + private)
- **Visual indicators**: 
  - 🔒 Lock icon for private files
  - Folder badges for files in subfolders
- **Default view**: HTML/MD files only (checkbox to show all file types)
- **Sorting**: Alphabetical by title/folder name

### Document Access
- Files without `access: private` → Publicly accessible
- Files with `access: private` → Password required ("maco")
- Session-based: Password remembered until browser closed

## Git Workflow

### Step 1: Pull Latest Changes
```bash
git pull origin main
```

### Step 2: Add Your Document
Place your file in `documents/` folder with appropriate front matter.

### Step 3: Commit Changes
```bash
git add documents/[filename]
git commit -m "Add [type]: [description]"
```

Example commit messages:
- "Add public report: Q1 2025 marketing analysis"
- "Add private document: Confidential financial data"

### Step 4: Push to Repository
```bash
git push origin main
```

## Quick Guide for Adding Files

### 📄 **Public Document (Default)**
Simply place your file in `documents/` folder with minimal front matter:

**Simplest (title from filename):**
```yaml
---
---
Content here...
```

**With custom title (optional):**
```yaml
---
title: "Custom Title"
---
Content here...
```

### 🔒 **Private Document**
Just add `access: private` - that's all you need:

**Simplest:**
```yaml
---
access: private
---
Private content...
```

**With custom title (optional):**
```yaml
---
title: "Custom Title"
access: private
---
Private content...
```

### 🎯 **Automatic Fallbacks**
- **No title?** → Uses filename (e.g., `my-report.md` → "my report")
- **No date?** → Uses file modification time
- **No author?** → Not required

### 📁 **Using Subfolders**
Organize with folders - they'll show as badges:
```
documents/
├── Reports/Q1/financial.md     → Shows [Reports/Q1]
├── Presentations/demo.html     → Shows [Presentations]
└── strategy.md                 → No badge
```

### ⚡ **That's It!**
- **Password**: "maco" for all private files
- **Default**: Files are public unless tagged private
- **Git**: `git add`, `commit`, and `push` to publish
- **Wait**: 1-5 minutes for GitHub Pages to update

## URL Structure After Upload
Documents are accessible at:

**GitHub Pages (default):**
- `https://[username].github.io/[repository]/documents/[filename]`

**Custom Domain:**
- `https://[your-domain]/documents/[filename]`

**Relative paths (always work):**
- From index: `./documents/[filename]`
- From anywhere: `/documents/[filename]`

## Migration from Folder-Based System

If you have existing files in `public/` and `private/` folders:

1. Move all files to `documents/` folder
2. Add `access: private` to front matter of previously private files
3. Remove old `public/` and `private/` folders
4. Commit and push changes

## Important Notes

1. **Default Access**: Files are public unless `access: private` is specified
2. **Password**: All private files use password "maco"
3. **Jekyll Processing**: GitHub Pages automatically converts .md to HTML
4. **Build Time**: Changes take 1-5 minutes to appear after pushing
5. **File Size**: Keep files under 100MB for optimal performance
6. **Front Matter**: Required for all .md and .html files

## Troubleshooting

### Document Not Appearing
- Verify YAML front matter syntax
- Check file is in `documents/` folder
- Wait 5 minutes for GitHub Pages rebuild
- Clear browser cache

### Access Issues
- Private files need `access: private` in front matter
- Password is case-sensitive: "maco"
- Session storage keeps you logged in until browser closes

### Formatting Issues
- Validate YAML syntax (use spaces, not tabs)
- Check markdown formatting
- Ensure HTML is well-formed

## Advanced Features

### Custom Passwords (Future)
While currently all private files use "maco", the system is designed to support custom passwords per file:
```yaml
---
access: private
password_hash: "SHA256_HASH_HERE"
---
```

### Tags and Categories
Use tags for better organization:
```yaml
---
title: "Document Title"
tags: [report, financial, q1-2025]
category: finance
access: private
---
```

## Support
For issues or questions, check the repository issues page or contact the administrator.
