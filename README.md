# Multi-Repository Traffic Statistics Tracker

This repository tracks GitHub traffic statistics (views, clones, referrers, stars, forks) for **my repositories** using **jgehrcke/github-repo-stats**, providing long-term analytics beyond GitHub's 14-day limit.

## Tracked Repositories

- MeherBhaskar/temporal-debug-skill
- MeherBhaskar/agent-rigor
- MeherBhaskar/diffwhisperer
- MeherBhaskar/OptiMindTune
- MeherBhaskar/meherbhaskar.github.io

## Live Traffic Report

View the live traffic report at:
**https://MeherBhaskar.github.io/temporal-debug-stats/**

The report includes charts for:
- Views (unique + total)
- Clones (unique + total)
- Referrers
- Top paths
- Stars over time
- Forks over time

## Setup (for maintainers)

### 1. Create Data Repository
Create a separate repository to store the stats data (e.g., `temporal-debug-stats`).

### 2. Create GitHub Personal Access Token (PAT)
1. Go to [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Name: "multi-repo-traffic-stats"
4. Select scopes: `repo` (full control of private repositories)
5. Copy the token

### 3. Add Secret to Repository
1. Go to this repo's **Settings > Secrets and variables > Actions**
2. Click "New repository secret"
3. Name: `GHRS_GITHUB_API_TOKEN`
4. Value: Your PAT from step 2

### 4. Enable GitHub Pages
1. Go to **Settings > Pages**
2. Source: "Deploy from a branch"
3. Branch: `github-repo-stats` / `/(root)`
4. Save

### 5. Configure Tracked Repositories
Edit `.github/workflows/repo-stats.yml` and modify the `matrix.repository` array to list all repositories you want to track:

```yaml
strategy:
  matrix:
    repository:
      - MeherBhaskar/temporal-debug-skill
      - MeherBhaskar/agent-rigor
      # Add more repos here
```

### 6. Run the Workflow
The workflow runs daily at 23:00 UTC automatically, or trigger manually:
- Go to **Actions > Repository Traffic Stats > Run workflow**

## Reports

Once the workflow runs and GitHub Pages is enabled, view the traffic report at:
**https://MeherBhaskar.github.io/temporal-debug-stats/**

## Multi-Repository Support

To add more repositories, edit `.github/workflows/repo-stats.yml` and add to the `matrix.repository` array (one repo per line, format: `owner/repo`).

The workflow uses a matrix strategy with `fail-fast: false` and `continue-on-error: true` so that one failing repo doesn't stop the others.

## Manual Stats Collection

```bash
# Install
pip install github-repo-stats

# Configure
export GHRS_GITHUB_API_TOKEN=your_token_here
export GHRS_FILES_ROOT_PATH=/path/to/data/dir

# Run for a specific repo
github-repo-stats MeherBhaskar/temporal-debug-skill
```

## API Limits

- 5,000 requests/hour for authenticated requests
- Traffic API limited to last 14 days per request
- This tool stores daily snapshots to work around the 14-day limit

## Alternative: GitHub's Built-in Insights

For basic stats without setup:
- **Insights > Traffic** (14-day window)
- **Insights > Contributors** (commit activity)