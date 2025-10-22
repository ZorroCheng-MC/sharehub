# ShareHub Fork and Deployment Guide

This guide will help you fork the ShareHub repository and deploy your own instance using GitHub Pages.

## Prerequisites

- A GitHub account
- Git installed on your computer
- GitHub CLI (`gh`) installed on your computer

## Step 1: Install GitHub CLI

### macOS
```bash
brew install gh
```

### Windows
Download from: https://cli.github.com/

### Linux
```bash
# Debian/Ubuntu
sudo apt install gh

# Fedora/CentOS
sudo dnf install gh
```

## Step 2: Authenticate with GitHub
```bash
gh auth login
```

Follow the prompts to authenticate with your GitHub account.

## Step 3: Fork and Clone the Repository
```bash
# Fork the original repository and clone it to your local machine
gh repo fork tommyjns/sharehub --clone

# Navigate into the cloned repository
cd sharehub
```

This will:
- Create a fork under your GitHub account
- Clone the fork to your local machine
- Automatically set up the git remotes

## Step 4: Enable GitHub Pages
```bash
# Enable GitHub Pages to deploy from the main branch
gh api -X POST /repos/YOUR_USERNAME/sharehub/pages \
  -f source[branch]=main \
  -f source[path]=/
```

**Replace `YOUR_USERNAME` with your actual GitHub username.**

### Alternative: If you need to use a docs folder
```bash
gh api -X POST /repos/YOUR_USERNAME/sharehub/pages \
  -f source[branch]=main \
  -f source[path]=/docs
```

## Step 5: Verify GitHub Pages Deployment
```bash
# Check GitHub Pages status
gh api /repos/YOUR_USERNAME/sharehub/pages
```

Your ShareHub instance will be available at:
```
https://YOUR_USERNAME.github.io/sharehub/
```

## Step 6: Make Changes (Optional)

If you want to customize your ShareHub:
```bash
# Make your changes to the files
# Then commit and push

git add .
git commit -m "Customize ShareHub"
git push origin main
```

GitHub Pages will automatically rebuild and deploy your changes within a few minutes.

## Troubleshooting

### GitHub Pages not enabling
If the API call fails, you can enable GitHub Pages manually:
1. Go to your repository on GitHub.com
2. Click **Settings** → **Pages**
3. Under "Source", select **main** branch
4. Click **Save**

### Finding your clone location
If you're not sure where the repository was cloned:
```bash
pwd  # Shows your current directory
```

The repository is cloned to your current working directory when you ran the fork command.

### Checking remotes
Verify your git remotes are set up correctly:
```bash
git remote -v
```

You should see:
- `origin` pointing to your fork
- `upstream` pointing to the original tommyjns/sharehub repository

## Updating Your Fork

To sync your fork with the original repository:
```bash
# Fetch updates from the original repository
git fetch upstream

# Merge updates into your main branch
git checkout main
git merge upstream/main

# Push updates to your fork
git push origin main
```

## Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [Original ShareHub Repository](https://github.com/tommyjns/sharehub)

---

**Note:** It may take a few minutes for your GitHub Pages site to become available after enabling it for the first time.
