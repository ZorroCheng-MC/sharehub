---
title: "Workflow Test Document"
---

# Workflow Test Document

This document tests the enhanced GitHub Action that automatically extracts titles from headings.

## Features

The action should:
1. Detect this file has no front matter
2. Extract "Workflow Test Document" from the `# ` heading
3. Add front matter with the title
4. Commit and push automatically

## Expected Result

After the workflow runs, this file should have:
```yaml
---
title: "Workflow Test Document"
---
```

And Jekyll should convert it to `.html` format immediately!
