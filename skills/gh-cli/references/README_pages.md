# GitHub Pages Deployment Script

A simple Python script to automate GitHub Pages deployment using the GitHub CLI (`gh`). This script provides an easy-to-use interface for enabling Pages, checking deployment status, triggering rebuilds, and creating GitHub Actions workflows.

## Features

- **Enable GitHub Pages** - Configure Pages with GitHub Actions or Jekyll build type
- **Check Status** - Query Pages site information and deployment status
- **Trigger Rebuilds** - Request new Pages builds via API
- **Create Workflows** - Generate GitHub Actions workflow files for automated deployment

## Prerequisites

1. **GitHub CLI (`gh`)** - Must be installed and authenticated
   ```bash
   # Install GitHub CLI (see https://cli.github.com/)
   # macOS
   brew install gh

   # Linux
   sudo apt install gh  # Debian/Ubuntu

   # Windows
   winget install GitHub.cli

   # Authenticate
   gh auth login
   ```

2. **Python 3.6+**

## Installation

Simply download the script:

```bash
curl -O https://raw.githubusercontent.com/yourusername/yourrepo/main/gh_pages_deploy.py
chmod +x gh_pages_deploy.py
```

Or clone this repository:

```bash
git clone https://github.com/yourusername/yourrepo.git
cd yourrepo
```

## Usage

### Enable GitHub Pages

Enable Pages with GitHub Actions as the build source:

```bash
python gh_pages_deploy.py enable owner/repo
```

**Options:**
- `--branch BRANCH` - Source branch (default: `main`)
- `--path PATH` - Source path: `/` or `/docs` (default: `/`)
- `--build-type TYPE` - Build type: `workflow` (GitHub Actions) or `legacy` (Jekyll)
- `--no-https` - Disable HTTPS enforcement

**Examples:**

```bash
# Enable with default settings (main branch, GitHub Actions)
python gh_pages_deploy.py enable myusername/myrepo

# Enable with custom branch and Jekyll
python gh_pages_deploy.py enable myorg/myrepo --branch gh-pages --build-type legacy

# Enable with /docs path
python gh_pages_deploy.py enable myusername/myrepo --path /docs
```

### Check Status

Check if Pages is enabled and view configuration:

```bash
python gh_pages_deploy.py status owner/repo
```

**Options:**
- `--build-info` - Also show latest build information

**Example:**

```bash
python gh_pages_deploy.py status myusername/myrepo --build-info
```

**Sample Output:**

```
Checking GitHub Pages status for myusername/myrepo...

✓ GitHub Pages is enabled:
  URL: https://myusername.github.io/myrepo/
  Status: built
  Build type: workflow
  Source branch: main
  Source path: /
  HTTPS enforced: True

Latest build:
  Status: built
  Commit: abc123def456
  Created: 2025-01-15T10:30:00Z
```

### Trigger Rebuild

Manually trigger a new Pages build:

```bash
python gh_pages_deploy.py rebuild owner/repo
```

**Example:**

```bash
python gh_pages_deploy.py rebuild myusername/myrepo
```

### Create GitHub Actions Workflow

Generate a GitHub Actions workflow file for automated Pages deployment:

```bash
python gh_pages_deploy.py create-workflow
```

**Options:**
- `--output PATH` - Custom output path (default: `.github/workflows/pages.yml`)

**Example:**

```bash
# Create in default location
python gh_pages_deploy.py create-workflow

# Create with custom path
python gh_pages_deploy.py create-workflow --output .github/workflows/deploy.yml
```

This creates a workflow that:
1. Runs on push to `main` branch
2. Builds your site (customize the build step)
3. Uploads the built site as an artifact
4. Deploys to GitHub Pages

**After creating the workflow:**
1. Edit `.github/workflows/pages.yml` to add your build commands
2. Commit and push the workflow to your repository
3. The deployment will run automatically on the next push

## Complete Workflow Example

Here's a complete example of setting up GitHub Pages for a new repository:

```bash
# 1. Enable GitHub Pages with GitHub Actions
python gh_pages_deploy.py enable myusername/myrepo

# 2. Create the deployment workflow
python gh_pages_deploy.py create-workflow

# 3. Customize the workflow (edit .github/workflows/pages.yml)
# Add your build commands, e.g., npm install && npm run build

# 4. Commit and push
git add .github/workflows/pages.yml
git commit -m "Add GitHub Pages deployment workflow"
git push origin main

# 5. Check status
python gh_pages_deploy.py status myusername/myrepo --build-info
```

## How It Works

The script uses the GitHub CLI (`gh`) to interact with the GitHub REST API:

- **Enable Pages**: `POST /repos/{owner}/{repo}/pages`
- **Check Status**: `GET /repos/{owner}/{repo}/pages`
- **Trigger Rebuild**: `POST /repos/{owner}/{repo}/pages/builds`
- **Latest Build**: `GET /repos/{owner}/{repo}/pages/builds/latest`

All authentication is handled by the GitHub CLI, so you don't need to manage tokens manually.

## GitHub Actions Workflow

The generated workflow file (`.github/workflows/pages.yml`) uses the official GitHub Actions for Pages deployment:

- `actions/configure-pages@v5` - Configure Pages metadata
- `actions/upload-pages-artifact@v3` - Package and upload site
- `actions/deploy-pages@v4` - Deploy to Pages

This is the **official, recommended approach** for GitHub Pages deployment.

## Customizing Your Build

Edit the "Build site" step in `.github/workflows/pages.yml`:

### Static HTML Site
```yaml
- name: Build site
  run: |
    mkdir -p _site
    cp -r * _site/
```

### Node.js / npm
```yaml
- name: Build site
  run: |
    npm install
    npm run build
    mkdir -p _site
    cp -r dist/* _site/
```

### Python / MkDocs
```yaml
- name: Setup Python
  uses: actions/setup-python@v4
  with:
    python-version: '3.x'

- name: Build site
  run: |
    pip install mkdocs
    mkdocs build
    mv site _site
```

### Jekyll
```yaml
- name: Setup Ruby
  uses: ruby/setup-ruby@v1
  with:
    ruby-version: '3.1'

- name: Build site
  run: |
    gem install jekyll bundler
    bundle install
    bundle exec jekyll build -d _site
```

## Troubleshooting

### "GitHub CLI is not authenticated"

Run `gh auth login` to authenticate the GitHub CLI.

### "Failed to enable Pages: ... requires admin permissions"

You need admin access to the repository to enable GitHub Pages.

### "Pages may already be enabled"

Pages is already configured. Use `status` command to check configuration, or update settings in the repository settings.

### Workflow not deploying

1. Check that Pages source is set to "GitHub Actions" in repository settings (Settings > Pages)
2. Verify the workflow file is in `.github/workflows/` directory
3. Check workflow runs in the Actions tab
4. Ensure proper permissions in the workflow file

## API Reference

### GitHubPagesManager Class

```python
manager = GitHubPagesManager('owner/repo')

# Enable Pages
manager.enable_pages(
    branch='main',
    path='/',
    build_type='workflow',  # or 'legacy'
    https_enforced=True
)

# Check status
manager.check_status()

# Trigger rebuild
manager.trigger_rebuild()

# Get latest build info
manager.get_latest_build()
```

## Repository Settings

After enabling Pages with this script, you can verify settings at:

```
https://github.com/owner/repo/settings/pages
```

Ensure the "Source" is set to:
- **GitHub Actions** (if using `build_type='workflow'`)
- **Deploy from a branch** (if using `build_type='legacy'`)

## License

MIT License - feel free to use and modify as needed.

## Resources

- [GitHub Pages Documentation](https://docs.github.com/pages)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub REST API - Pages](https://docs.github.com/rest/pages)
- [GitHub Actions for Pages](https://github.com/actions/deploy-pages)
