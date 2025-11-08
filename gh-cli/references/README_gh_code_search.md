# gh_code_search.py

A Python wrapper for `gh search code` with enhanced filtering and formatting capabilities.

## Features

- **All `gh search code` options**: Supports all native GitHub CLI search filters
- **Custom filtering**: Exclude forks, private repos, or filter by match count
- **Multiple output formats**: JSON, pretty-printed table, or summary statistics
- **Sorting**: Sort results by match count, repository, or file path
- **Error handling**: Graceful handling of rate limits, timeouts, and other errors

## Requirements

- Python 3.8+
- GitHub CLI (`gh`) installed and authenticated

## Installation

```bash
chmod +x gh_code_search.py
```

## Usage

```bash
./gh_code_search.py [OPTIONS] QUERY
```

### Basic Examples

Search for "hello world" in Python files:
```bash
./gh_code_search.py "hello world" --language python
```

Search in a specific repository:
```bash
./gh_code_search.py "error handling" --repo microsoft/vscode
```

Search with multiple filters:
```bash
./gh_code_search.py "TODO" --extension md --exclude-forks --limit 10
```

### Output Formats

**Pretty format** (default):
```bash
./gh_code_search.py "React component" --language typescript --output pretty
```

**Summary statistics**:
```bash
./gh_code_search.py "hello world" --language python --output summary
```

**JSON format**:
```bash
./gh_code_search.py "class.*Component" --language typescript --output json
```

### Advanced Filtering

Exclude forks and sort by match count:
```bash
./gh_code_search.py "authentication" --exclude-forks --sort-by matches
```

Filter by minimum number of matches:
```bash
./gh_code_search.py "TODO" --min-matches 3 --output summary
```

Search in multiple repositories:
```bash
./gh_code_search.py "bug fix" --repo user/repo1 --repo user/repo2
```

## Options

### GitHub Search Filters

| Option | Description |
|--------|-------------|
| `-L, --limit` | Maximum number of results (default: 30) |
| `--language` | Filter by programming language |
| `--filename` | Filter by filename |
| `--extension` | Filter by file extension |
| `-R, --repo` | Filter by repository (can specify multiple) |
| `--owner` | Filter by owner (can specify multiple) |
| `--match` | Restrict search to "file" or "content" |
| `--size` | Filter by file size range (e.g., "10..100" in KB) |

### Custom Filters

| Option | Description |
|--------|-------------|
| `--exclude-forks` | Exclude results from forked repositories |
| `--exclude-private` | Exclude results from private repositories |
| `--min-matches` | Minimum number of text matches per file |

### Output Options

| Option | Description |
|--------|-------------|
| `-o, --output` | Output format: json, pretty, summary (default: pretty) |
| `--sort-by` | Sort by: matches, repo, path |

## Error Handling

The script handles common errors gracefully:

- **Rate Limit**: Displays helpful message when GitHub API rate limit is exceeded
- **Timeout**: Suggests simplifying query when search times out
- **Invalid Query**: Validates query before execution

## Exit Codes

- `0`: Success
- `1`: Error (rate limit, timeout, invalid query, etc.)
- `130`: Cancelled by user (Ctrl+C)

## Examples by Use Case

### Find security issues
```bash
./gh_code_search.py "eval(" --language javascript --exclude-forks --sort-by matches
```

### Analyze TODO comments
```bash
./gh_code_search.py "TODO" --extension py --owner yourorg --output summary
```

### Find code patterns
```bash
./gh_code_search.py "class.*extends.*Component" --language typescript --limit 50 --output json
```

### Repository-specific search
```bash
./gh_code_search.py "database connection" --repo myorg/myrepo --output pretty
```
