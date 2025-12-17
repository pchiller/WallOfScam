# Deploy to GitHub Pages

## Steps to deploy:

1. **Build the static site:**
   ```bash
   npm run build
   ```

2. **Push the `out` folder to GitHub Pages:**
   - The build creates an `out` folder with static files
   - Push this to your `gh-pages` branch or configure GitHub Pages to serve from the `out` folder

3. **Using GitHub Actions (Recommended):**
   - Create `.github/workflows/deploy.yml` with the workflow below
   - Push to main branch and GitHub Actions will automatically build and deploy

## GitHub Actions Workflow:

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ['main']
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-pages-artifact@v2
        with:
          path: ./out

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/deploy-pages@v2
        id: deployment
```

## Manual Deployment:

If you prefer manual deployment:

```bash
npm run build
cd out
git init
git add -A
git commit -m 'deploy'
git push -f git@github.com:username/repo.git main:gh-pages
```

Then enable GitHub Pages in your repository settings to serve from the `gh-pages` branch.
