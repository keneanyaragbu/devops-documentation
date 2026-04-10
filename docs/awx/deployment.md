# Documentation Site Deployment Guide

## Overview
This documentation site is built using MkDocs with the Material
theme and deployed to GitHub Pages for free hosting.

## Tools Used
| Tool | Purpose |
|---|---|
| MkDocs | Static site generator |
| Material for MkDocs | Professional theme |
| GitHub Pages | Free hosting |
| Git | Version control |

## Prerequisites
- Python 3.x installed on local machine
- Git installed
- GitHub account

## Step 1 - Install MkDocs
```bash
pip install mkdocs
pip install mkdocs-material
```

## Step 2 - Configure PATH on Windows
MkDocs scripts may not be on PATH automatically on Windows.
Add an alias in Git Bash:
```bash
echo "alias mkdocs='python -m mkdocs'" >> ~/.bashrc
source ~/.bashrc
mkdocs --version
```

## Step 3 - Create MkDocs Project
```bash
mkdocs new awx-documentation
cd awx-documentation
```

## Step 4 - Configure mkdocs.yml
Update mkdocs.yml with Material theme and navigation:
```yaml
site_name: Kenechukwu DevOps Projects
theme:
  name: material
  palette:
    - scheme: default
      primary: indigo
      accent: cyan
nav:
  - Home: index.md
  - AWX Project:
    - Overview: awx/overview.md
    - Installation: awx/installation.md
    - Configuration: awx/configuration.md
    - Troubleshooting: awx/troubleshooting.md
    - Deployment: awx/deployment.md
```

## Step 5 - Add Custom Styling
Create custom CSS file:
```bash
mkdir docs/stylesheets
nano docs/stylesheets/extra.css
```

Add to mkdocs.yml:
```yaml
extra_css:
  - stylesheets/extra.css
```

## Step 6 - Preview Locally
```bash
mkdocs serve
```
Open browser at:
http://localhost:8000

## Step 7 - Push Source Code to GitHub
```bash
git init
git add .
git commit -m "Initial documentation site"
git branch -M main
git push -u origin main
```

## Step 8 - Deploy to GitHub Pages
```bash
mkdocs gh-deploy
```

This command:
- Builds documentation into HTML
- Creates gh-pages branch on GitHub
- Pushes built site to gh-pages branch
- Makes site live on GitHub Pages

## Step 9 - Enable GitHub Pages
1. Go to GitHub repo
2. Click Settings
3. Click Pages on left menu
4. Under Source select:
   - Branch: gh-pages
   - Folder: / (root)
5. Click Save

## Live Site
Your documentation is now live at:
https://keneanyaragbu.github.io/devops-documentation/

## Updating the Site
Every time you make changes:
```bash
# Update source code
git add .
git commit -m "Update documentation"
git push origin main

# Redeploy live site
mkdocs gh-deploy
```

## Important Notes
- Never use emojis in .md files when using nano on Windows
  as they cause UTF-8 encoding errors
- Always restart mkdocs serve after changing mkdocs.yml
- Content changes in .md files require mkdocs serve restart
  on Windows due to file watching limitations
