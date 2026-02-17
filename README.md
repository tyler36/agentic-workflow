# GitHub Agentic Workflows

A demonstration repository showcasing [GitHub Agentic Workflows](https://github.github.com/gh-aw/)—AI-powered automation for repository management and documentation.

## What Are Agentic Workflows?

Agentic Workflows combine GitHub Actions with AI agents to automate complex repository tasks. Rather than traditional imperative workflows, you write task descriptions in Markdown, and the AI agent autonomously executes the work—reading code, analyzing context, making decisions, and creating pull requests.

**Key capabilities:**

- 🤖 **AI-Powered Agents**: Natural language task descriptions instead of imperative YAML
- 📝 **Markdown Workflows**: Write workflow logic in `.md` files, compiled to `.lock.yml`
- 🧠 **Contextual Awareness**: Agents understand code, issues, PRs, and repository history
- 🔒 **Safety-First**: Built-in sandboxing, credential protection, and audit trails
- 🚀 **Multiple AI Engines**: Claude, GPT, and other large language models
- ⏰ **Event & Schedule Triggered**: Respond to pushes, issues, PRs, or scheduled intervals

## Workflows in This Repository

This project includes three example workflows:

### 1. **Update Docs** (`update-docs.md`)
Keeps documentation synchronized with code changes.

- **Triggers**: Every push to `main` branch
- **Actions**: Analyzes code diffs, identifies changes, creates draft documentation PRs
- **Output**: Draft pull requests with documentation updates
- **Use case**: Ensure docs stay current when code changes

### 2. **Daily Repo Status** (`daily-repo-status.md`)
Generates upbeat daily status reports for repository activity.

- **Triggers**: Daily schedule (configurable)
- **Actions**: Gathers recent activity, analyzes trends, creates status issues
- **Output**: GitHub issue with productivity insights and highlights
- **Use case**: Keep team informed of repository progress

### 3. **Issue Triage** (`triage-new-issues.md`)
Automatically classifies and triages new issues.

- **Triggers**: When new issues are opened
- **Actions**: Classifies issues, assesses priority, applies labels, posts response
- **Output**: Labeled issues with helpful acknowledgment comments
- **Use case**: Ensure consistent issue handling and faster response times

## Getting Started

### Prerequisites

- GitHub CLI (`gh`) installed
- Access to the GitHub Agentic Workflows extension
- Repository with Actions enabled

### Installation

1. **Install the Agentic Workflows extension:**

````shell
gh extension install github/gh-aw
````

2. **Verify installation:**

````shell
gh aw --version
````

### Running Workflows

#### Option A: Use Existing Workflows

The three workflows in this repository are already configured and active:

- `update-docs` runs on every push to `main`
- `daily-repo-status` runs daily at midnight (configurable)
- `triage-new-issues` runs when issues are opened

#### Option B: Create a New Workflow

1. **Create a workflow definition** (`.md` file):

````shell
cat > .github/workflows/my-workflow.md << 'EOF'
---
description: "My custom workflow"
on:
  push:
    branches: [main]
---

# My Workflow

Your task description here...
EOF
````

2. **Compile the workflow** to generate the executable `.lock.yml`:

````shell
gh aw compile my-workflow.md
````

3. **Commit and push**:

````shell
git add .github/workflows/my-workflow.*
git commit -m "Add: my-workflow"
git push
````

## Understanding Workflow Files

Each workflow consists of two files:

### `.md` File (Source)
Human-readable task description with YAML frontmatter:

- **Frontmatter**: Configures triggers, permissions, AI engine, and safe-outputs
- **Description**: Details what the agent should do
- **Format**: Plain Markdown with natural language instructions

**Example structure:**
````markdown
---
description: "Workflow purpose"
on:
  push:
    branches: [main]
permissions: read-all
tools:
  github:
    toolsets: [all]
---

# Workflow Title

Your natural language task here...
````

### `.lock.yml` File (Compiled)
Generated executable workflow (auto-created by `gh aw compile`):

- Contains the compiled AI instructions and configuration
- Do **not** edit directly—regenerate from `.md` instead
- Committed to version control for reproducibility

**Workflow:** `.md` source → `gh aw compile` → `.lock.yml` executable

## How It Works

1. **Event Trigger**: Workflow is triggered by configured event (push, schedule, issue opened, etc.)
2. **Agent Initialization**: AI agent reads task description from `.md` frontmatter and body
3. **Context Analysis**: Agent examines repository state, code, issues, discussions relevant to the task
4. **Autonomous Execution**: Agent performs work—reading files, analyzing code, making decisions
5. **Safe Outputs**: Agent creates pull requests, issues, or comments using safe-output tools
6. **Logging**: Actions records agent reasoning and decisions

Example flow for "Update Docs" workflow:

````
Push to main
    ↓
Workflow triggers
    ↓
Agent reads diff
    ↓
Agent identifies code changes
    ↓
Agent reviews existing docs
    ↓
Agent detects gaps
    ↓
Agent creates/updates markdown
    ↓
Agent opens draft PR
````

## Customization

### Modify Workflow Behavior

Edit the `.md` file (not the `.lock.yml`):

````shell
# Edit the task description
vim .github/workflows/update-docs.md

# Compile to generate new .lock.yml
gh aw compile update-docs.md

# Commit both files
git add .github/workflows/update-docs.md .github/workflows/update-docs.lock.yml
git commit -m "Update: update-docs workflow"
git push
````

### Change Trigger Conditions

Modify the `on:` section in the frontmatter:

````markdown
---
on:
  push:
    branches: [main, develop]  # Add develop branch
  workflow_dispatch:           # Also allow manual triggers
---
````

### Adjust Permissions

Control what the agent can do:

````markdown
---
permissions:
  contents: read           # Read repository code
  issues: write            # Create/update issues
  pull-requests: write     # Create pull requests
---
````

## Architecture & Safety

### Design Philosophy

- **Autonomous but bounded**: Agents make decisions within explicit constraints
- **Transparent reasoning**: All agent actions are logged and auditable
- **Credential protection**: No secrets exposed to agents; they use safe-output tools instead
- **Sandbox isolation**: Agents run in isolated containers with firewall constraints

### Permissions Model

- Workflows declare required permissions upfront
- Agents cannot escalate privileges or access restricted resources
- All external actions go through safe-output tools (create PR, add comment, etc.)
- Audit trails record every agent decision

## Troubleshooting

### Workflow Doesn't Trigger

**Symptom**: Workflow runs but takes no action.

**Solutions**:
1. Check the `on:` conditions match your event
2. Verify branch name matches (case-sensitive)
3. Ensure workflow is not disabled in repository settings
4. Review agent logs in the Actions UI

### Compilation Errors

**Symptom**: `gh aw compile` fails.

**Solutions**:
1. Verify YAML frontmatter is valid: `---` on separate lines
2. Check for trailing whitespace in YAML keys
3. Ensure descriptions don't exceed character limits
4. Run: `gh aw compile <filename> --debug`

### Agent Creates Unexpected Results

**Symptom**: Agent generates incorrect documentation or classifies issues wrong.

**Solutions**:
1. Review agent reasoning in the workflow run logs
2. Refine task description to be more specific
3. Add examples or explicit rules to the task
4. Consider reducing agent scope (one task per workflow)

### Safe-Output Tool Errors

**Symptom**: "Draft pull request creation failed."

**Solutions**:
1. Check repository has Actions enabled
2. Verify workflow has `pull-requests: write` permission
3. Ensure the branch doesn't already exist
4. Check for rate limiting (wait 1 hour, retry)

## Best Practices

### Writing Clear Task Descriptions

✅ **Good**: "Review code changes in the last commit and update API documentation in the docs/ directory to reflect new parameters."

❌ **Poor**: "Update docs"

### Minimizing Agent Scope

✅ **Good**: One workflow per clear responsibility (docs, triage, status)

❌ **Poor**: One mega-workflow trying to handle 5 different tasks

### Testing Workflows

1. Create the workflow locally
2. Test with `workflow_dispatch` trigger first (manual run)
3. Review the run logs and any created PRs
4. Refine the task description if needed
5. Then enable automatic triggers

### Version Control

- Always commit both `.md` and `.lock.yml` files
- `.lock.yml` is compiled output, don't edit directly
- Use meaningful commit messages: "Update: <workflow-name> - <what changed>"

## Additional Resources

- [GitHub Agentic Workflows Documentation](https://github.github.com/gh-aw/)
- [Available Tools Reference](https://github.github.com/gh-aw/reference/tools/)
- [AI Engine Options](https://github.github.com/gh-aw/reference/engines/)
- [Safe-Outputs Guide](https://github.github.com/gh-aw/reference/safe-outputs/)

## Contributing

To improve these workflows:

1. Create a feature branch: `git checkout -b feature/workflow-enhancement`
2. Edit the `.md` workflow file
3. Run: `gh aw compile <filename>`
4. Test the changes
5. Create a pull request with your improvements

## License

This project is provided as a demonstration and reference implementation.
