# Deploy to GitHub Pages Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Deploy the Telestrations Sketchpad Vite app to GitHub and host it as a GitHub Page accessible at https://username.github.io/repo-name/

**Architecture:** Initialize a local git repository, create a new GitHub repository, push the source code to the main branch, configure Vite for GitHub Pages deployment (setting the base path), create a GitHub Actions workflow to build the app and deploy to the gh-pages branch, and enable GitHub Pages to serve from the gh-pages branch.

**Tech Stack:** Vite, npm, Git, GitHub, GitHub Actions

### Task 1: Initialize Git Repository

**Files:**
- Modify: (none - this is a repo-level operation)

**Step 1: Initialize git in the project directory**

Run: `git init`
Expected: Initializes a new git repository in the current directory.

**Step 2: Verify git initialization**

Run: `git status`
Expected: Shows untracked files including index.html, package.json, etc.

**Step 3: Commit initial setup**

Run: `git add . && git commit -m "Initial commit: Telestrations Sketchpad Vite app"`
Expected: Creates the first commit with all project files.

### Task 2: Configure Vite for GitHub Pages

**Files:**
- Create: `vite.config.js`

**Step 1: Create Vite config file**

Create `vite.config.js` with the following content:

```javascript
import { defineConfig } from 'vite'

export default defineConfig({
  base: '/repo-name/'  // Replace 'repo-name' with your actual GitHub repository name
})
```

**Step 2: Verify the config file exists**

Run: `ls -la vite.config.js`
Expected: Shows the newly created vite.config.js file.

**Step 3: Commit the config**

Run: `git add vite.config.js && git commit -m "feat: configure Vite base path for GitHub Pages"`
Expected: Commits the Vite config.

### Task 3: Create GitHub Repository

**Files:**
- Modify: (none - uses GitHub CLI)

**Step 1: Create new GitHub repository**

Run: `gh repo create repo-name --public --source=. --remote=origin --push`
Expected: Creates a new public repository named 'repo-name' (replace with your desired name), sets origin remote, and pushes the current branch.

**Step 2: Verify repository creation and push**

Run: `git remote -v`
Expected: Shows origin remote pointing to the new GitHub repository.

### Task 4: Create GitHub Actions Workflow for Deployment

**Files:**
- Create: `.github/workflows/deploy.yml`

**Step 1: Create the workflows directory**

Run: `mkdir -p .github/workflows`
Expected: Creates the necessary directories.

**Step 2: Create the deploy workflow file**

Create `.github/workflows/deploy.yml` with the following content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

**Step 3: Commit the workflow**

Run: `git add .github/workflows/deploy.yml && git commit -m "feat: add GitHub Actions workflow for Pages deployment"`
Expected: Commits the workflow file.

**Step 4: Push the workflow**

Run: `git push origin main`
Expected: Pushes the commit with the workflow to GitHub, triggering the deployment.

### Task 5: Enable GitHub Pages

**Files:**
- Modify: (GitHub repository settings - manual step)

**Step 1: Enable GitHub Pages in repository settings**

Go to the GitHub repository settings, scroll to "Pages" section, select "Deploy from a branch", choose "gh-pages" branch and "/ (root)" folder, then save.

**Step 2: Verify deployment**

Wait a few minutes, then visit https://username.github.io/repo-name/ to see the deployed app.

**Step 3: Commit any necessary updates if base path was incorrect**

If the repository name was different, update `vite.config.js` with the correct base path, commit, and push to trigger redeployment.