# Agentic Workflows Documentation

This repository demonstrates three AI-powered automated workflows using GitHub Agentic Workflows.

## Overview

GitHub Agentic Workflows enable AI agents to automate repository management tasks. Each workflow is defined as a Markdown file (`.md`) in `.github/workflows/`, which compiles into executable GitHub Actions workflows (`.lock.yml`).

## Workflows

### 1. Daily Repo Status

**File**: `.github/workflows/daily-repo-status.md`

**Trigger**: Daily schedule + manual dispatch

**Purpose**: Creates upbeat daily status reports of repository activity.

**What it does**:
- Gathers recent activity: issues, pull requests, discussions, releases, and code changes
- Analyzes repository progress and highlights key metrics
- Generates actionable recommendations for maintainers
- Posts findings as a GitHub issue

**Configuration**:
- **Schedule**: Runs daily
- **Labels applied**: `report`, `daily-status`
- **Issue prefix**: `[repo-status] `

**Example use case**: Stay informed about repository health and community engagement without manual tracking.

---

### 2. Issue Triage Agent

**File**: `.github/workflows/triage-new-issues.md`

**Trigger**: When a new issue is opened

**Purpose**: Automatically classifies and labels new issues.

**What it does**:
- Reads issue title, body, and any code snippets
- Classifies the issue: bug, feature, question, docs, or chore
- Assesses priority level: critical, high, medium, or low
- Applies appropriate labels to the issue
- Posts a helpful acknowledgment comment

**Use cases**:
- Reduce manual triage work for repository maintainers
- Ensure consistent issue categorization
- Provide immediate feedback to issue reporters
- Improve issue tracking and searchability

---

### 3. Update Docs

**File**: `.github/workflows/update-docs.md`

**Trigger**: Every push to main branch + manual dispatch

**Purpose**: Keeps documentation synchronized with code changes.

**What it does**:
- Analyzes code diffs from the latest push
- Identifies new or modified APIs, functions, classes, and configurations
- Reviews existing documentation for accuracy and completeness
- Detects documentation gaps
- Creates draft pull requests with documentation updates

**Documentation standards applied**:
- **Structure**: Uses Diátaxis framework (tutorials, how-to guides, reference, explanation)
- **Style**: Precise, concise, developer-friendly writing
- **Standards**: Follows Google Developer Style Guide and Microsoft Writing Style Guide
- **Accessibility**: Ensures inclusive naming and internationalization-readiness

**Key principles**:
- Documentation-as-Code: treat docs gaps like failing tests
- Single source of truth for all technical content
- Progressive disclosure: high-level concepts first, detailed examples next

---

## Managing Workflows

### Viewing Workflow Runs

Each workflow can be triggered manually or runs automatically. View results in your repository's **Actions** tab.

### Customizing Workflows

To customize a workflow:

1. Edit the corresponding `.md` file in `.github/workflows/`
2. Update the YAML front matter for configuration
3. Modify the job description and instructions
4. Recompile the workflow (see below)

### Compiling Workflows

After editing a `.md` workflow file:

```bash
gh aw compile <filename>
```

Example:
```bash
gh aw compile update-docs.md
```

This generates the corresponding `.lock.yml` file that GitHub Actions executes.

---

## How Agentic Workflows Work

Each workflow:

1. **Defines execution context** via YAML front matter (triggers, permissions, tools)
2. **Provides AI instructions** in Markdown format
3. **Uses AI agents** to perform intelligent tasks
4. **Returns safe outputs** through dedicated tools (create issues, comments, PRs)
5. **Maintains audit trails** via GitHub's built-in logging

### Key Components

- **Triggers** (`on`): When the workflow runs (schedule, push, workflow_dispatch, etc.)
- **Permissions**: Minimal required access (principle of least privilege)
- **Tools**: Available integrations (GitHub API, bash, web-fetch, etc.)
- **Safe-outputs**: Controlled mechanisms to create or modify GitHub resources
- **Engine**: AI model that executes the workflow (default: Copilot)

---

## Getting Started

### Prerequisites

- GitHub repository with Actions enabled
- GitHub Agentic Workflows extension installed:
  ```bash
  gh extension install github/gh-aw
  ```

### Setup Steps

1. **Add the workflows to your repo**:
   ```bash
   gh aw add-wizard githubnext/agentics/daily-repo-status
   gh aw add-wizard githubnext/agentics/triage-new-issues
   gh aw add-wizard githubnext/agentics/update-docs
   ```

2. **Wait for initial workflow runs**

3. **Review and customize** the workflows as needed

4. **Monitor results** in your repository's Actions tab

---

## Best Practices

### Workflow Configuration

- **Start small**: Enable one workflow at a time and verify results
- **Set appropriate triggers**: Match workflow purpose to trigger event
- **Use labels consistently**: Establish label conventions across workflows
- **Review AI output**: Always review draft PRs and comments before merging

### Documentation Updates

- **Keep docs close to code**: Update docs whenever code changes
- **Use clear examples**: Include functional code samples
- **Maintain consistency**: Follow established style guides
- **Test examples**: Verify code examples work as documented

### Security

- **Principle of least privilege**: Workflows request only necessary permissions
- **Read-only by default**: Most workflows are read-only except for safe outputs
- **Audit trails**: All changes are logged and traceable

---

## Troubleshooting

### Workflow fails to run

- Check that the `.md` file is valid YAML front matter
- Verify the workflow has been compiled: `gh aw compile <filename>`
- Review workflow logs in the Actions tab

### Unexpected workflow behavior

- Verify AI instructions are clear and complete
- Check that required tools are properly configured
- Review the workflow's permissions match its needs

### Documentation not updating

- Ensure the `update-docs` workflow has recent code changes to analyze
- Check that the workflow ran successfully (view Actions logs)
- Review draft PR for suggestions

---

## Additional Resources

- [GitHub Agentic Workflows Documentation](https://github.github.com/gh-aw/)
- [Diátaxis Framework](https://diataxis.fr/)
- [Google Developer Style Guide](https://developers.google.com/style)
- [Microsoft Writing Style Guide](https://docs.microsoft.com/en-us/style-guide/)

