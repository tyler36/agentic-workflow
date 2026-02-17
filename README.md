# GitHub Agentic Workflows Demo

A reference implementation demonstrating three AI-powered GitHub Agentic Workflows for repository automation.

## Overview

[GitHub Agentic Workflows](https://github.github.com/gh-aw/) enable AI agents to automate repository management tasks. Define workflows in Markdown, compile to GitHub Actions YAML, and let AI handle repetitive work.

This repository includes three production-ready workflow examples:

### Workflows Included

1. **Daily Repo Status** — Automated daily activity summaries as GitHub issues
2. **Issue Triage Agent** — Smart issue classification, labeling, and acknowledgment
3. **Update Docs** — Keeps documentation synchronized with code changes

See [Workflow Documentation](./docs/WORKFLOWS.md) for detailed information about each workflow.

## Quick Start

### Prerequisites

- GitHub CLI with Agentic Workflows extension:
  ```bash
  gh extension install github/gh-aw
  ```

### Install Workflows

Copy the workflow files from this repository, or add them to your own:

```bash
gh aw add-wizard githubnext/agentics/daily-repo-status
gh aw add-wizard githubnext/agentics/triage-new-issues
gh aw add-wizard githubnext/agentics/update-docs
```

### Compile and Deploy

After copying workflow files, compile them to executable YAML:

```bash
gh aw compile daily-repo-status.md
gh aw compile triage-new-issues.md
gh aw compile update-docs.md
```

Commit the generated `.lock.yml` files to your repository.

## Documentation

- **[Getting Started](./docs/GETTING_STARTED.md)** — Step-by-step setup and customization guide
- **[Workflow Details](./docs/WORKFLOWS.md)** — In-depth documentation of each workflow
- **[Agentic Workflows Docs](https://github.github.com/gh-aw/)** — Official GitHub documentation

## Key Features

- **Markdown-based definitions** — Write workflows as readable Markdown, not complex YAML
- **AI-powered intelligence** — Workflows make smart decisions, not just execute scripts
- **GitHub-native** — Integrates directly with issues, PRs, discussions, and discussions
- **Safe by default** — Read-only by default with controlled output operations
- **Easy customization** — Edit `.md` files to adapt workflows to your needs

## Workflow Files

All workflow files are located in `.github/workflows/`:

| File | Purpose | Trigger |
|------|---------|---------|
| `daily-repo-status.md` | Generate daily activity reports | Daily schedule |
| `triage-new-issues.md` | Auto-classify new issues | Issue opened |
| `update-docs.md` | Sync docs with code changes | Push to main |

Each `.md` file compiles to a corresponding `.lock.yml` file that GitHub Actions executes.

## Examples

### View Daily Report

The daily-repo-status workflow runs automatically and creates issues like:
- Repository activity summary
- Key metrics and highlights
- Actionable recommendations

### Triage in Action

When someone opens an issue, triage-new-issues automatically:
- Classifies it (bug, feature, question, docs, chore)
- Assesses priority (critical, high, medium, low)
- Applies appropriate labels
- Posts an acknowledgment comment

### Documentation Updates

When code is merged to main, update-docs:
- Analyzes the changes
- Identifies what documentation is needed
- Creates draft pull requests with suggestions

## Customization

Edit any `.md` workflow file to customize behavior:

1. Modify YAML front matter for configuration (schedule, labels, permissions)
2. Update job descriptions and AI instructions
3. Run `gh aw compile <filename>` to regenerate

See [Getting Started](./docs/GETTING_STARTED.md#step-5-customize-for-your-needs) for detailed customization examples.

## Learn More

- [Official Agentic Workflows Documentation](https://github.github.com/gh-aw/)
- [Diátaxis Framework](https://diataxis.fr/) — Documentation structure methodology
- [Google Developer Style Guide](https://developers.google.com/style)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
