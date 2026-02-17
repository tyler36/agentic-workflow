# GitHub Agentic Workflows

## Overview

This project demonstrates using [GitHub Agentic Workflows](https://github.github.com/gh-aw/).
These workflow use AI for event-triggered and scheduled jobs to improve your repository.

Key features include:

- [Automated markdown workflows](https://github.github.com/gh-aw/introduction/overview/#natural-language-to-github-actions)
- [AI-powered Decision making](https://github.github.com/gh-aw/introduction/how-they-work/)
- [GitHub integration](https://github.github.com/gh-aw/reference/tools/#github-tools-github)
- [Safety first](https://github.github.com/gh-aw/introduction/architecture)
- [Multiple AI Engines](https://github.github.com/gh-aw/reference/engines/)
- [Continuos AI](https://github.github.com/gh-aw/introduction/how-they-work/)

## Installation

1. Install the GitHub Agentic Workflow extension

```shell
gh extension install github/gh-aw
```

1. Add the sample workflow and trigger a run

```shell
gh aw add-wizard githubnext/agentics/daily-repo-status
```

1. Wait for the workflow to complete.

1. If you change a `.md` workflow, you need to regenerate the workflow `.yml`

```shell
gh aw compile
```

## Getting started

1. Create a new `.github/workflows/<filename>.md` agentic workflow.
2. Compile the markdown into `.yml` workflow.

```shell
gh aw compile <filename>
```
