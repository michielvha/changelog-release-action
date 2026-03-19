# changelog-release-action

Composite GitHub Action that generates a changelog from conventional commits using [git-cliff](https://git-cliff.org) and creates a GitHub Release.

## Usage

### Docker components (default)

```yaml
- name: Changelog & GitHub Release
  uses: michielvha/changelog-release-action@main
  with:
    version: ${{ github.ref_name }}
```

### Helm charts

```yaml
- name: Changelog & GitHub Release
  uses: michielvha/changelog-release-action@main
  with:
    version: ${{ github.ref_name }}
    type: helm
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `version` | Yes | — | Release version (tag name) |
| `type` | No | `docker` | Artifact type: `docker` or `helm` |

## Requirements

The calling workflow must have `contents: write` permission:

```yaml
permissions:
  contents: write
```

The action automatically fetches full git history and tags — no need for `fetch-depth: 0` on the checkout step.

## How it works

1. Fetches full git history and tags (handles shallow clones).
2. Runs git-cliff with the bundled `cliff.toml` to generate a changelog between the current and previous tag.
3. Appends an artifact footer (Docker pull or Helm install command).
4. Creates a GitHub Release with the combined body.
