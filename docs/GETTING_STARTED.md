# Getting Started with Agentic Workflows

This guide walks you through setting up and using GitHub Agentic Workflows in your repository.

## What Are Agentic Workflows?

Agentic Workflows are AI-powered automation tools that run on GitHub. They perform intelligent tasks like triaging issues, generating status reports, and keeping documentation in sync with code changes—all without manual intervention.

This repository demonstrates three workflows you can use or customize:
1. **Daily Repo Status**: Automated daily activity summaries
2. **Issue Triage Agent**: Smart issue classification and labeling
3. **Update Docs**: Documentation automation synchronized with code changes

## Prerequisites

Before getting started, ensure you have:

- **GitHub Account**: Access to a repository where you can enable Actions
- **GitHub CLI**: [Install gh](https://github.com/cli/cli#installation)
- **Agentic Workflows Extension**: Install with:
  ```bash
  gh extension install github/gh-aw
  ```

## Step 1: Install the Extension

If you haven't already, install the GitHub Agentic Workflows extension:

```bash
gh extension install github/gh-aw
```

Verify installation:
```bash
gh aw --version
```

## Step 2: Copy Workflows to Your Repository

Option A: **Use the Add Wizard** (recommended for new setup)

The add-wizard guides you through setup:

```bash
gh aw add-wizard githubnext/agentics/daily-repo-status
gh aw add-wizard githubnext/agentics/triage-new-issues
gh aw add-wizard githubnext/agentics/update-docs
```

Option B: **Copy from This Repository**

Clone or copy the workflow files from `.github/workflows/`:
- `daily-repo-status.md`
- `triage-new-issues.md`
- `update-docs.md`

## Step 3: Compile Workflows

After adding workflow files, compile them to generate the executable YAML:

```bash
gh aw compile daily-repo-status.md
gh aw compile triage-new-issues.md
gh aw compile update-docs.md
```

This creates:
- `daily-repo-status.lock.yml`
- `triage-new-issues.lock.yml`
- `update-docs.lock.yml`

Commit these `.lock.yml` files to your repository.

## Step 4: Test Your Workflows

### Test Daily Repo Status

Trigger manually:
```bash
gh workflow run daily-repo-status.lock.yml
```

Check results in your repository's **Actions** tab. A new issue with the `[repo-status]` label should appear within minutes.

### Test Issue Triage

Open a new issue in your repository. The triage workflow runs automatically when a new issue is created. Look for:
- Automatic labels (bug, feature, question, docs, chore)
- Priority labels (critical, high, medium, low)
- A helpful comment from the bot

### Test Update Docs

Trigger manually:
```bash
gh workflow run update-docs.lock.yml
```

Or, make a code change and push to main. The workflow analyzes the diff and may create a draft pull request with documentation updates.

## Step 5: Customize for Your Needs

### Customizing Workflow Behavior

Each workflow can be customized by editing its `.md` file:

1. Edit `.github/workflows/<workflow-name>.md`
2. Modify the YAML front matter for settings (schedule, labels, etc.)
3. Update the job description and AI instructions
4. Run `gh aw compile <workflow-name>.md` to regenerate

### Example: Change Daily Report Schedule

Edit `.github/workflows/daily-repo-status.md`:

```markdown
---
on:
  schedule: daily  # or 'weekly', 'hourly', etc.
...
```

### Example: Add Custom Labels for Issues

Edit `.github/workflows/triage-new-issues.md`:

```markdown
---
safe-outputs:
  add-comment:
    max: 1
  update-issue:
    max: 1
---
```

Add to the job description what labels you want applied.

## Step 6: Monitor and Refine

### View Workflow Results

Check your repository's **Actions** tab to see:
- Workflow run history
- Success/failure status
- Detailed logs for debugging
- Execution time and resource usage

### Review Generated Content

- **Daily Reports**: Check new issues with `[repo-status]` label
- **Triage Results**: Review labels on recent issues
- **Documentation Updates**: Review draft pull requests

### Make Adjustments

Based on results, you can:
- Adjust workflow instructions for better output quality
- Change trigger conditions (schedule, event types)
- Add or remove labels from classification
- Update documentation standards

## Common Use Cases

### Use Case: Team Collaboration

Enable all three workflows to:
- Keep your team informed with daily status reports
- Automatically categorize community issues
- Ensure documentation stays in sync with development

```bash
gh aw add-wizard githubnext/agentics/daily-repo-status
gh aw add-wizard githubnext/agentics/triage-new-issues
gh aw add-wizard githubnext/agentics/update-docs
```

### Use Case: Documentation-Focused Project

Enable just the update-docs workflow for a repository where documentation quality is critical:

```bash
gh aw add-wizard githubnext/agentics/update-docs
```

### Use Case: Community Repository

Enable triage and daily status for repositories with active communities:

```bash
gh aw add-wizard githubnext/agentics/triage-new-issues
gh aw add-wizard githubnext/agentics/daily-repo-status
```

## Troubleshooting

### Workflow doesn't run

**Problem**: Workflow file exists but doesn't execute

**Solutions**:
- Verify the `.lock.yml` file is in `.github/workflows/`
- Check that GitHub Actions are enabled in repository settings
- Review workflow file for YAML syntax errors
- Check the Actions tab for error logs

### Generated content is incorrect or unhelpful

**Problem**: The AI agent produces unexpected results

**Solutions**:
- Review and refine the job description in the `.md` file
- Add more specific constraints or examples
- Run `gh aw compile` to regenerate after editing
- Check the workflow logs to understand the AI's reasoning

### Compilation fails

**Problem**: `gh aw compile` returns an error

**Solutions**:
- Verify the YAML front matter is valid (use online YAML validators)
- Ensure all required front matter fields are present
- Check that indentation is correct (use 2 spaces, not tabs)
- Review error message for specific issues

### Too many workflow runs

**Problem**: Workflows running too frequently or unexpectedly

**Solutions**:
- Check the `on:` trigger conditions in front matter
- Verify `schedule:` values (e.g., `daily`, `weekly`)
- Disable workflow temporarily: uncheck in Settings > Actions
- Adjust `max:` fields in `safe-outputs` to limit operations

## Next Steps

- **Learn more**: Read [Workflow Details](./WORKFLOWS.md) for in-depth information
- **Explore examples**: Check this repository's workflow files for examples
- **Get help**: Visit [GitHub Agentic Workflows docs](https://github.github.com/gh-aw/)
- **Share**: Discuss workflows in [GitHub Discussions](https://github.com/github/gh-aw/discussions)

