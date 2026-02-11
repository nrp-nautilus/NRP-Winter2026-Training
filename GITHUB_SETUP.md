# GitHub Pages Setup Instructions

This document explains how to configure GitHub Pages for this repository and the workflow for keeping branches in sync.

## Repository Structure

- **Root directory**: Contains only the training materials (`.ipynb` and `.md` files) and `README.md`
- **`pages/` directory**: Contains all the Software Carpentry lesson template files for GitHub Pages deployment

## GitHub Pages Configuration

### Option 1: Deploy from gh-pages branch (Recommended)

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select:
   - **Branch**: `gh-pages`
   - **Folder**: `/ (root)`
4. Click **Save**

The GitHub Actions workflow (`.github/workflows/pages.yml`) will automatically:
- Build the Jekyll site from the `pages/` directory when you push to `gh-pages`
- Deploy it to GitHub Pages

### Option 2: Deploy from main branch

If you prefer to deploy from the `main` branch:

1. Go to **Settings** → **Pages**
2. Under **Source**, select:
   - **Branch**: `main`
   - **Folder**: `/pages`
3. Click **Save**

**Note**: If using this option, you'll need to manually merge `gh-pages` to `main` when you want to update the site.

## Workflow: Merging gh-pages to main

The repository includes a workflow (`.github/workflows/merge-gh-pages.yml`) that automatically merges `gh-pages` into `main` whenever you push to `gh-pages`. This keeps both branches in sync.

### Manual Merge (if needed)

If you need to manually merge `gh-pages` to `main`:

```bash
git checkout main
git merge gh-pages
git push origin main
```

## Typical Workflow

1. **Make changes** on the `gh-pages` branch
2. **Commit and push** to `gh-pages`:
   ```bash
   git checkout gh-pages
   git add .
   git commit -m "Update training materials"
   git push origin gh-pages
   ```
3. **Automatic actions**:
   - The merge workflow will automatically merge `gh-pages` → `main`
   - The pages workflow will automatically build and deploy the site

## Enabling GitHub Actions

If GitHub Actions are not enabled:

1. Go to **Settings** → **Actions** → **General**
2. Under **Workflow permissions**, select:
   - **Read and write permissions**
   - **Allow GitHub Actions to create and approve pull requests**
3. Click **Save**

## Verifying Deployment

After pushing to `gh-pages`:

1. Go to **Actions** tab in your repository
2. You should see two workflows running:
   - `Deploy GitHub Pages` - builds and deploys the site
   - `Merge gh-pages to main` - syncs branches
3. Once complete, your site will be available at:
   `https://<username>.github.io/<repository-name>/`

## Troubleshooting

### Pages not deploying

- Check that GitHub Pages is enabled in Settings → Pages
- Verify the source branch is set correctly
- Check the Actions tab for any workflow errors
- Ensure the `pages/` directory contains valid Jekyll files

### Merge conflicts

If the automatic merge fails:
1. Check the Actions tab for the error
2. Manually resolve conflicts:
   ```bash
   git checkout main
   git merge gh-pages
   # Resolve conflicts
   git add .
   git commit
   git push origin main
   ```

### Jekyll build errors

- Check that `pages/_config.yml` is properly configured
- Verify all required Jekyll files are in the `pages/` directory
- Check the Actions logs for specific build errors

