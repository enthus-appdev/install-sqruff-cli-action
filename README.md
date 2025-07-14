# install-sqruff-cli-action

GitHub Action to install [sqruff](https://github.com/quarylabs/sqruff), a fast SQL linter and formatter written in Rust.

## Features

- Automatically detects and installs the latest version of sqruff
- Supports specifying a specific version
- Handles GitHub API rate limiting with optional token
- Architecture detection (supports x86_64 and ARM64)
- Comprehensive error handling and debugging output
- Verifies installation before completing

## Usage

### Basic usage (installs latest version)

```yaml
- uses: quarylabs/install-sqruff-cli-action@main
```

### With GitHub token (recommended to avoid rate limiting)

```yaml
- uses: quarylabs/install-sqruff-cli-action@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Install specific version

```yaml
- uses: quarylabs/install-sqruff-cli-action@main
  with:
    version: v0.28.0
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version` | Version of Sqruff CLI to install (e.g., `v0.28.0`) | No | `latest` |
| `github-token` | GitHub token for API requests (helps avoid rate limiting) | No | - |

## Example workflow

```yaml
name: Lint SQL

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install sqruff
        uses: quarylabs/install-sqruff-cli-action@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Lint SQL files
        run: sqruff lint --dialect postgres .
```

## Troubleshooting

### API Rate Limit Exceeded

If you see an error about GitHub API rate limits, make sure to provide the `github-token` input:

```yaml
with:
  github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Architecture Not Supported

This action currently supports:
- Linux x86_64 (amd64)
- Linux ARM64 (aarch64)

Other architectures are not supported at this time.

## License

This project is licensed under the MIT License.
