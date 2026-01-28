# github-skills

A Claude Code plugin containing skills for GitHub automation via the gh CLI.
These skills simplify interacting with the GitHub API for use in Claude Code.
The gh CLI handles all authentication, so you don't have to worry about leaking keys.

> **Warning:** This was entirely made by Claude, use at your own risk.
> Please be mindful of the risks involved in pulling arbitrary code from GitHub,
> including prompt injection and arbitrary code execution.

## Installation

### As a Plugin (Recommended)

Install directly from GitHub:
```
/plugin install JamesPrial/github-skills
```

Once installed, both skills are available as:
- `/github-skills:gh-cli` - GitHub CLI utilities
- `/github-skills:github-actions-writer` - GitHub Actions workflow generator

### Manual Installation

Clone the repository and use with the `--plugin-dir` flag:
```bash
git clone https://github.com/JamesPrial/github-skills.git
claude --plugin-dir /path/to/github-skills
```

## Skills Included

### gh-cli

GitHub CLI utilities for automating repetitive workflows. Review the SKILL.md and references for more info.

- **gh_code_search.py** - Search for code on GitHub (e.g., searching for leaked credentials or finding examples)
- **gh_failed_run.py** - Extract error messages from your most recent GitHub Actions run
- **gh_pages_deploy.py** - Handle the tedium of deploying to GitHub Pages

### github-actions-writer

A skill for creating and optimizing GitHub Actions CI/CD workflows with best practices and security built-in.
Includes templates and validation tools.

## Dependencies

- Python 3.9+
- gh CLI (GitHub CLI)

## Setup

1. Install the [gh CLI](https://cli.github.com/)
2. Run `gh auth login` and follow the instructions
3. Install this plugin using one of the methods above

## Usage

After installation, tell Claude to explicitly use the skill for best results:
- "Use gh-cli to search for examples of X on GitHub"
- "Use github-actions-writer to create a CI workflow for my Node.js project"

## Project Structure

```
github-skills/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/
│   ├── gh-cli/              # GitHub CLI utilities skill
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── references/
│   └── github-actions-writer/  # GitHub Actions skill
│       ├── SKILL.md
│       ├── scripts/
│       ├── assets/templates/
│       └── references/
└── README.md
```
