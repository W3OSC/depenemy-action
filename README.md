# depenemy-action

GitHub Action for [depenemy](https://github.com/W3OSC/depenemy) - scans your dependencies for supply chain risks, reputation red flags, and behavioral issues.

Results appear automatically in your **Security > Code Scanning** tab on every push and pull request.

---

## Usage

Create `.github/workflows/depenemy.yml` in your repository:

```yaml
name: Depenemy scan
on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
      - uses: actions/checkout@v4
      - uses: W3OSC/depenemy-action@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}  # optional - unlocks R001 and R006 checks
          fail-on: error
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | GitHub token for author and contributor lookups | No | `${{ github.token }}` |
| `paths` | Space-separated paths to scan | No | `.` |
| `fail-on` | Minimum severity to fail the action (`error`, `warning`, `info`, `never`) | No | `error` |
| `output-sarif` | Path to write SARIF output file | No | `depenemy.sarif` |
| `config` | Path to `.depenemy.yml` config file | No | — |
| `ecosystems` | Comma-separated ecosystems to scan (`npm,pypi,cargo`) | No | auto-detect |
| `upload-sarif` | Automatically upload SARIF to GitHub Code Scanning | No | `true` |

## Outputs

| Output | Description |
|--------|-------------|
| `sarif-file` | Path to the generated SARIF file |
| `findings-count` | Total number of findings |
| `errors-count` | Number of error-level findings |

---

## What it detects

| ID | Name | Severity |
|----|------|----------|
| B001 | Range specifier | Warning |
| B002 | No version pinned | Error |
| B003 | Lagging version | Warning |
| R001 | Young author account | Warning |
| R002 | New package | Warning |
| R003 | Low weekly downloads | Warning |
| R004 | Low total downloads | Warning |
| R005 | No updates in 2+ years | Warning |
| R006 | Few contributors | Warning |
| R007 | Known vulnerable version | Error |
| R008 | Deprecated package | Warning |
| R009 | Typosquatting suspected | Warning |
| S001 | Install scripts | Error |
| S002 | No source repository | Warning |
| S003 | Archived repository | Warning |
| S004 | Dependency confusion | Warning |
| S005 | Known malicious package | Error |

---

## Supported ecosystems

- npm / Node.js
- Python
- Rust
- Solidity

---

## License

MIT - see [LICENSE](https://github.com/W3OSC/depenemy/blob/main/LICENSE)
