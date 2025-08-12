# Setup MinIO Client (GitHub Action)

Download, cache, install, and configure the MinIO client (`mc`) in GitHub Actions.  
Optimized for Ubuntu runners and CI use-cases where you want fast, reliable MinIO access without re-downloading on every run.

## Features

- ⚡ Weekly cache rotation (no manual version bumps)
- 🔐 Alias configuration with your endpoint & credentials
- 🪣 Optional bucket creation (`--ignore-existing`)
- 🧪 Optional checksum verification (enabled by default)
- 🛣️ Adds `mc` to `PATH` for subsequent steps
- 🧰 Outputs `mc-path` and `mc-version`

---

## Inputs

| Name            | Description                                                                 | Required | Default   |
|-----------------|-----------------------------------------------------------------------------|----------|-----------|
| `alias`         | Alias name for MinIO                                                        | Yes      |           |
| `url`           | MinIO server URL (e.g. http://minio:9000)                                   | Yes      |           |
| `access_key`    | MinIO access key (use GitHub Secrets)                                       | Yes      |           |
| `secret_key`    | MinIO secret key (use GitHub Secrets)                                       | Yes      |           |
| `bucket`        | Bucket to create if missing (optional)                                      | No       |           |
| `install_path`  | Directory to place mc and add to PATH                                       | No       | `$RUNNER_TEMP/bin` |
| `cache_root`    | Directory used for caching the mc binary                                    | No       | `$HOME/.cache/setup-minio-client` |
| `verify_checksum` | Verify SHA256 checksum of the download                                    | No       | `true`    |
| `version`       | MinIO client version to install (e.g. `2023-08-23T10-07-06Z` or `latest`)   | No       | `latest`  |
| `download_url`  | Override download URL (advanced, version-aware)                             | No       |           |

## Outputs

| Name        | Description                       |
|-------------|-----------------------------------|
| `mc-path`   | Full path to the mc binary        |
| `mc-version`| Output of `mc --version`          |

---

## Installation

To use this action in your GitHub workflow, reference it with a specific version tag:

```yaml
uses: buckhamduffy/gha-minio-cli@v1
```

For the latest features, you can also use:
- `@main` - Latest development version (not recommended for production)
- `@v1.0.0` - Specific release version

---

## Usage

### Basic Example (Latest Version)

```yaml
- name: Setup MinIO Client
  uses: buckhamduffy/gha-minio-cli@v1
  with:
    alias: minio
    url: http://localhost:9000
    access_key: ${{ secrets.MINIO_ACCESS_KEY }}
    secret_key: ${{ secrets.MINIO_SECRET_KEY }}
```

### Specify a Version

```yaml
- name: Setup MinIO Client (specific version)
  uses: buckhamduffy/gha-minio-cli@v1
  with:
    alias: minio
    url: http://localhost:9000
    access_key: ${{ secrets.MINIO_ACCESS_KEY }}
    secret_key: ${{ secrets.MINIO_SECRET_KEY }}
    version: 2023-08-23T10-07-06Z
```

### With Optional Bucket Creation

```yaml
- name: Setup MinIO Client and bucket
  uses: buckhamduffy/gha-minio-cli@v1
  with:
    alias: minio
    url: http://localhost:9000
    access_key: ${{ secrets.MINIO_ACCESS_KEY }}
    secret_key: ${{ secrets.MINIO_SECRET_KEY }}
    bucket: my-bucket
```

---

## How Version Selection Works

- By default, the action installs the **latest** MinIO client.
- To install a specific version, set the `version` input to the desired release string (e.g. `2023-08-23T10-07-06Z`).
- The action constructs the download URL as:
  - Latest: `https://dl.min.io/client/mc/release/linux-amd64/mc`
  - Specific: `https://dl.min.io/client/mc/release/linux-amd64/mc.RELEASE.{version}`

You can override the download URL entirely with `download_url` if you need a custom or pre-release binary.

---

## Example: Use the MinIO Client

After setup, you can use `mc` in subsequent steps:

```yaml
- name: List buckets
  run: mc ls minio
```

---

## Publishing Releases

This action follows semantic versioning. To create a new release:

1. Update the version in any relevant files
2. Create a git tag: `git tag v1.0.0`
3. Push the tag: `git push origin v1.0.0`
4. Create a GitHub release from the tag
5. Update the major version tag: `git tag -f v1 && git push origin v1 --force`

This allows users to reference `@v1` for the latest v1.x.x release.

---

## Development

To test this action locally:

1. Use the test workflow in `.github/workflows/test.yml`
2. The workflow sets up a MinIO service and tests various scenarios
3. You can also test against your own MinIO instance by setting the appropriate secrets

---

## License

[MIT](./LICENSE)
- 🏷️ **Version selection**: Install a specific MinIO client version or always get the latest (default)