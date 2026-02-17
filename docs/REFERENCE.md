# Workflow Reference

Technical reference for GitHub Agentic Workflows structure, configuration, and best practices.

## Workflow Anatomy

Each agentic workflow consists of two parts:

### 1. YAML Front Matter (Configuration)

Located between `---` markers at the top of the `.md` file:

```yaml
---
description: |
  Human-readable description of what the workflow does.
  Supports multi-line strings.

on:
  schedule: daily           # or: hourly, weekly, monthly
  workflow_dispatch:        # Allow manual trigger

permissions:
  contents: read            # Access level needed
  issues: read
  pull-requests: read

network: defaults           # Network policy (defaults = firewall enabled)

tools:
  github:
    lockdown: false         # Allow reading 3rd-party data in public repos
  bash: true               # Enable bash command execution
  web-fetch: true          # Enable HTTP requests

safe-outputs:
  create-issue:
    title-prefix: "[prefix] "
    labels: [label1, label2]
  create-pull-request:
    draft: true
    labels: [automation, docs]

timeout-minutes: 15         # Execution timeout
engine: copilot            # AI model (copilot is default)
---
```

### 2. Markdown Job Description

The human-readable instructions for the AI agent. Written in Markdown format below the front matter. This guides what the AI should do.

## Configuration Reference

### Triggers (`on`)

Controls when the workflow runs:

```yaml
on:
  schedule: daily          # Runs daily
  schedule: weekly         # Runs weekly
  schedule: hourly         # Runs hourly
  workflow_dispatch:       # Allow manual trigger
  push:
    branches: [main]       # Run on push to main
  issues:
    types: [opened]        # Run when issue opened
  pull_request:
    types: [opened]        # Run when PR opened
```

### Permissions

Principle of least privilege—request only needed access:

```yaml
permissions:
  contents: read           # Read repository code/config
  issues: read             # Read issues (not write)
  pull-requests: read      # Read PRs (not write)
  # For write access (rare):
  contents: write          # Modify code
  issues: write            # Create/modify issues
  pull-requests: write     # Create/modify PRs
```

### Network Policy

```yaml
network: defaults          # Firewall enabled (recommended)
network: public           # Allow all outbound (use with caution)
```

### Tools Available

```yaml
tools:
  github:                  # GitHub API access
    lockdown: false        # In public repos, can read 3rd-party data
    toolsets: [all]       # or [default] for limited access
  bash: true              # Execute bash commands
  web-fetch: true         # HTTP requests (GET, POST, etc.)
```

### Safe Outputs

Controls how the workflow can modify GitHub resources:

```yaml
safe-outputs:
  create-issue:
    title-prefix: "[prefix] "
    labels: [label1, label2]
    max: 1                # Max issues per run
  
  create-pull-request:
    draft: true           # Create as draft
    labels: [automation]
    max: 1                # Max PRs per run
  
  add-comment:
    max: 5                # Max comments per run
  
  update-issue:
    max: 1                # Max issues updated per run
```

### Engine Selection

```yaml
engine: copilot           # Default, uses GitHub Copilot
engine: gpt-4            # Alternative (if available)
```

## Common Workflow Patterns

### Pattern 1: Scheduled Report

Runs on a schedule, gathers information, creates an issue:

```yaml
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

safe-outputs:
  create-issue:
    labels: [report]
```

**Use cases**: Daily summaries, weekly reports, health checks

### Pattern 2: Event-Triggered Classification

Runs when an event occurs, analyzes content, applies labels:

```yaml
on:
  issues:
    types: [opened]

permissions:
  issues: read

safe-outputs:
  update-issue:
    max: 1
  add-comment:
    max: 1
```

**Use cases**: Issue triage, PR review routing, spam detection

### Pattern 3: Code Change Analysis

Runs on push, analyzes diffs, creates PR with updates:

```yaml
on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

safe-outputs:
  create-pull-request:
    draft: true
    labels: [automation]
```

**Use cases**: Documentation updates, dependency scanning, code quality analysis

## Writing Effective Job Descriptions

The job description tells the AI what to do. Write clear, specific instructions:

### Structure

1. **Purpose**: What is the workflow's main goal?
2. **Input**: What data should the AI analyze?
3. **Processing**: What steps should it follow?
4. **Output**: What should the result be?
5. **Quality criteria**: How is success measured?

### Example: Issue Triage

```markdown
# Issue Triage Agent

When a new issue is opened:

1. Analyze the issue title and body
2. Classify the issue type: bug, feature, question, docs, or chore
3. Assess priority: critical, high, medium, or low
4. Apply appropriate labels
5. Post a helpful comment acknowledging the submission

## Classification Guide

- **Bug**: Problem with existing functionality
- **Feature**: Request for new capability
- **Question**: User asking for help
- **Docs**: Documentation missing or unclear
- **Chore**: Maintenance, refactoring, updates

## Priority Assessment

- **Critical**: Breaks core functionality or affects security
- **High**: Significant impact on usability or stability
- **Medium**: Nice-to-have improvements or minor bugs
- **Low**: Polish, edge cases, or low-impact items
```

### Tips for Clear Instructions

- **Be specific**: "Apply labels: bug, high-priority" not "label it"
- **Provide examples**: Show the format you want
- **Set constraints**: "Max 1 comment per issue", "Check for duplicates first"
- **Define terms**: If "critical" means something specific, explain it
- **Progressive disclosure**: Start with what's important, add details later
- **Avoid ambiguity**: "Bug" not "issue"—issues are GitHub's term

## Compile and Deploy

### Initial Compile

Generate `.lock.yml` from `.md`:

```bash
gh aw compile <workflow-name>.md
```

### After Editing

Always recompile after editing a `.md` file:

```bash
gh aw compile daily-repo-status.md
gh aw compile triage-new-issues.md
gh aw compile update-docs.md
```

### Deployment

Commit both files to version control:

```bash
git add .github/workflows/<name>.md
git add .github/workflows/<name>.lock.yml
git commit -m "feat: add <name> workflow"
git push
```

## Monitoring and Debugging

### View Workflow Runs

```bash
gh run list --workflow=<name>.lock.yml
gh run view <run-id>
```

### View Logs

```bash
gh run view <run-id> --log
```

### Manual Trigger

```bash
gh workflow run <name>.lock.yml
```

### Check Status

```bash
gh workflow list
gh workflow view <name>.lock.yml
```

## Best Practices

### Do ✅

- **Start simple**: Begin with clear, focused instructions
- **Test frequently**: Run manually before relying on automated triggers
- **Document assumptions**: Explain what the workflow expects to find
- **Set timeouts**: Prevent runaway workflows with `timeout-minutes`
- **Limit operations**: Use `max:` in safe-outputs to prevent spam
- **Review output**: Always review AI-generated content before merging
- **Version control**: Commit both `.md` and `.lock.yml` files
- **Iterate**: Refine instructions based on real-world results

### Don't ❌

- **Don't over-constrain**: Leave room for AI to be helpful
- **Don't forget to compile**: Changes to `.md` won't take effect without compilation
- **Don't request excessive permissions**: Use principle of least privilege
- **Don't ignore errors**: Review failed workflow logs to improve instructions
- **Don't automate merges**: Always require human review of AI-generated PRs
- **Don't mix concerns**: Keep workflows focused on one task
- **Don't set impossible timeouts**: Allow enough time for real work

## Troubleshooting

### Workflow doesn't run at all

**Checklist**:
- ✓ `.lock.yml` file is in `.github/workflows/`
- ✓ GitHub Actions are enabled in repository settings
- ✓ YAML syntax is valid (check workflow logs)
- ✓ Permissions allow workflow execution
- ✓ Trigger conditions are met

### Workflow produces wrong output

**Solution**: Review and improve the job description:
- Add more specific examples
- Clarify edge cases
- Provide templates for the desired output
- Add explicit constraints

### Workflow times out

**Solutions**:
- Increase `timeout-minutes`
- Simplify instructions
- Reduce scope (fewer issues to process, etc.)
- Check for infinite loops in instructions

### Too many duplicate outputs

**Solutions**:
- Lower `max:` limits in safe-outputs
- Add deduplication logic to instructions
- Review and refine the workflow's logic

## Integration Examples

### Integrate with Labels

Ensure labels exist before workflow uses them:

```bash
# Create labels
gh label create bug --color ff0000
gh label create feature --color 0075ca
gh label create docs --color 0366d6
```

### Integrate with Teams

Assign triaged issues to team members:

```markdown
Add job description instruction:
"Assign to the appropriate team member based on the issue type"
```

## Additional Resources

- [GitHub Agentic Workflows Docs](https://github.github.com/gh-aw/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Markdown Syntax Guide](https://www.markdownguide.org/)
- [YAML Specification](https://yaml.org/)
