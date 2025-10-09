# Template Sync Workflow

This GitHub Actions workflow syncs your repository with the main template repository (`prafful-s/synch-main`). It can be triggered manually from the Actions tab.

## Setup Instructions

### 1. Template Repository Configuration

This workflow is pre-configured to sync with the main template repository:

```yaml
git remote add template https://github.com/prafful-s/synch-main.git
```

If you created this repository using "Use this template" from `prafful-s/synch-main`, no configuration changes are needed.

### 2. Required Permissions

The workflow requires the following permissions:
- `contents: write` - To create branches and push changes
- `pull-requests: write` - To create pull requests

These are already configured in the workflow file.

### 3. Repository Settings

Make sure your repository has the following settings:
- GitHub Actions are enabled
- The default branch is `main` (or update the workflow to match your default branch)

## How It Works

1. **Manual Trigger**: Trigger the workflow manually from the "Actions" tab in your repository
2. **Update Check**: It fetches the latest changes from the template repository (`prafful-s/synch-main`)
3. **Smart Sync**: Only creates a pull request if there are actual updates
4. **File Protection**: Preserves your custom files (`paths.json`, `fstab.yaml`) during sync
5. **Pull Request**: Creates a PR with all the template changes for review and merging

## Workflow Features

- ✅ **Automatic Detection**: Only runs when there are actual updates
- ✅ **Pull Request Creation**: Creates PRs for easy review and merging
- ✅ **Detailed Information**: Includes commit hashes and sync timestamps
- ✅ **Manual Override**: Can be triggered manually when needed
- ✅ **Safe Merging**: Uses `--allow-unrelated-histories` for initial syncs

## Customization Options

### Add Automatic Schedule (Optional)
By default, the workflow runs manually. To add automatic syncing, add a schedule trigger:

```yaml
on:
  workflow_dispatch: # Manual triggering
  schedule:
    - cron: '0 12 * * *'  # 12:00 PM UTC daily
    # - cron: '0 9 * * 1'   # 9:00 AM UTC every Monday
    # - cron: '0 0 1 * *'   # Midnight UTC on the 1st of every month
```

### Change Branch Names
To use a different base branch, update the workflow:

```yaml
git checkout -b "$BRANCH_NAME"
git merge template/develop --no-edit --allow-unrelated-histories  # Change 'main' to 'develop'
```

### Add More Files to Ignore During Sync
To preserve additional custom files during sync, update the `IGNORE_FILES` array in the workflow:

```yaml
IGNORE_FILES=(
  "paths.json"
  "fstab.yaml"
)
```

### Add Custom Logic
You can add additional steps before or after the sync:

```yaml
- name: Run tests
  run: npm test

- name: Build project
  run: npm run build
```

## Troubleshooting

### Permission Issues
If you encounter permission errors:
1. Check that the repository has Actions enabled
2. Verify the `GITHUB_TOKEN` has the required permissions
3. Ensure the template repository is public or accessible

### Merge Conflicts
If there are merge conflicts:
1. The workflow will create a PR with conflicts
2. Resolve conflicts manually in the PR
3. Merge the PR when ready

### Template Repository Not Found
Make sure:
1. The template repository URL is correct
2. The repository exists and is accessible
3. The branch name matches (default is `main`)

## Security Notes

- The workflow uses the default `GITHUB_TOKEN` which is automatically provided
- No additional secrets are required for public template repositories
- For private template repositories, you may need to configure additional authentication
