# GitHub Actions Failed Run Analyzer

A Python script that analyzes the most recent failed GitHub Actions workflow run and provides detailed error information.

## Prerequisites

- Python 3.6+
- [GitHub CLI (gh)](https://cli.github.com/) installed and authenticated

## Installation

1. Ensure you have the GitHub CLI installed:
   ```bash
   gh --version
   ```

2. Make sure you're authenticated:
   ```bash
   gh auth status
   ```

3. Make the script executable:
   ```bash
   chmod +x gh_failed_run.py
   ```

## Usage

### Basic Usage

Analyze the most recent failed run in the current repository:
```bash
./gh_failed_run.py
```

Or with Python:
```bash
python3 gh_failed_run.py
```

### Analyze a Specific Repository

```bash
./gh_failed_run.py --repo owner/repo-name
```

### Pretty-Print JSON Output

For more readable output with indentation:
```bash
./gh_failed_run.py --pretty
```

### Combined Example

```bash
./gh_failed_run.py --repo microsoft/vscode --pretty
```

## Output Format

The script outputs JSON with the following structure:

```json
{
  "run": {
    "number": 12345,
    "url": "https://github.com/owner/repo/actions/runs/12345",
    "workflow": "CI Tests",
    "conclusion": "failure",
    "status": "completed",
    "created_at": "2025-11-07T10:30:00Z",
    "branch": "main",
    "commit": "abc123...",
    "event": "push"
  },
  "failed_jobs": [
    {
      "name": "build",
      "conclusion": "failure",
      "status": "completed",
      "started_at": "2025-11-07T10:30:05Z",
      "completed_at": "2025-11-07T10:35:22Z",
      "error_excerpts": [
        "Error: Module 'foo' not found",
        "npm ERR! Failed at the build script",
        "Process completed with exit code 1"
      ]
    }
  ],
  "repository": "owner/repo-name"
}
```

## Features

- **Automatic Detection**: Finds the most recent failed run without needing a run ID
- **Error Extraction**: Intelligently extracts error messages from logs using pattern matching
- **Multiple Job Support**: Analyzes all failed jobs in a run
- **Flexible Repository Selection**: Works with current repository or any specified repository
- **Clean Output**: Removes ANSI color codes and timestamps from error excerpts

## Error Patterns Detected

The script looks for common error indicators:
- Lines containing "error", "failed", "failure"
- Exception messages
- Exit code messages
- Timeout messages
- Panic messages

## Piping to Other Tools

Since the output is JSON, you can easily pipe it to other tools:

```bash
# Extract just the error excerpts
./gh_failed_run.py | jq '.failed_jobs[].error_excerpts[]'

# Get the run URL
./gh_failed_run.py | jq -r '.run.url'

# Count failed jobs
./gh_failed_run.py | jq '.failed_jobs | length'

# Save to file
./gh_failed_run.py --pretty > failed_run_analysis.json
```

## Troubleshooting

### "gh CLI is not installed or not in PATH"
Install the GitHub CLI from https://cli.github.com/

### "No failed runs found"
This means there are no failed workflow runs in the repository. The script only looks for runs with `failure` status.

### "Error running gh command"
Make sure you're authenticated with `gh auth login` and have access to the repository you're trying to analyze.

## Exit Codes

- `0`: Success
- `1`: Error (gh CLI not found, command failed, JSON parsing error)

## Advanced Usage

### Filter Error Excerpts with grep

```bash
./gh_failed_run.py | jq -r '.failed_jobs[].error_excerpts[]' | grep -i "module"
```

### Monitor Multiple Repositories

```bash
for repo in owner/repo1 owner/repo2 owner/repo3; do
  echo "Checking $repo..."
  ./gh_failed_run.py --repo "$repo" --pretty
done
```

### Integration with CI/CD

You can use this script in your CI/CD pipeline to automatically analyze and report failures:

```bash
#!/bin/bash
if ! ./gh_failed_run.py > failure_report.json; then
  echo "Failed to analyze run"
  exit 1
fi

# Send to monitoring system, Slack, etc.
curl -X POST https://hooks.slack.com/... -d @failure_report.json
```
