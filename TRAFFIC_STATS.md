# Repository Traffic Statistics

This repository uses **jgehrcke/github-repo-stats** for long-term traffic tracking beyond GitHub's 14-day limit.

## Setup

### 1. Create a GitHub Personal Access Token (PAT)

1. Go to [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Give it a name like "temporal-debug-stats"
4. Select scopes: `repo` (full control of private repositories)
4. Copy the token

### 2. Add Secret to Repository

1. Go to this repo's **Settings > Secrets and variables > Actions**
2. Click "New repository secret"
3. Name: `GHRS_GITHUB_API_TOKEN`
4. Value: Your PAT from step 1

### 3. Enable GitHub Pages (for the stats report)

1. Go to **Settings > Pages**
2. Source: "Deploy from a branch"
3. Branch: `github-repo-stats` / `/(root)`
5. Save

### 4. Run the Workflow

The workflow runs daily at 23:00 UTC automatically, or you can trigger it manually:
- Go to **Actions > Repository Traffic Stats > Run workflow**

## Reports

Once the workflow runs and GitHub Pages is enabled, you can view the traffic reports at:

```
https://MeherBhaskar.github.io/temporal-debug-stats/
```

The reports include charts for:
- Views (unique + total)
- Clones (unique + total)
- Referrers
- Top paths
- Stars over time
- Forks over time

## Multi-Repository Support

To track multiple repositories (e.g., for microservices), edit the workflow's `additional-repos` input:

```yaml
with:
  repository: MeherBhaskar/temporal-debug-skill
  ghtoken: ${{ secrets.GHRS_GITHUB_API_TOKEN }}
  databranch: github-repo-stats
  additional-repos: |
    MeherBhaskar/temporal-debug-skill
    owner/repo-2
    owner/repo-3
```

## Manual Stats Collection

If you prefer manual collection, you can run the tool locally:

```bash
# Install
pip install github-repo-stats

# Configure
export GHRS_GITHUB_API_TOKEN=your_token_here
export GHRS_FILES_ROOT_PATH=/path/to/data/dir

# Run
github-repo-stats MeherBhaskar/temporal-debug-skill
```

## API Limits

GitHub's REST API has rate limits:
- 5,000 requests/hour for authenticated requests
- Traffic API is limited to the last 14 days per request
- This tool works around the 14-day limit by storing daily snapshots

## Alternative: GitHub's Built-in Insights

For basic stats without setup, use GitHub's built-in:
- **Insights > Traffic** (14-day window)
- **Insights > Contributors** (for commit activity)