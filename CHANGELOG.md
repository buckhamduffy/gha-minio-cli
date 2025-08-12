# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial release of the MinIO CLI GitHub Action
- Weekly cache rotation for MinIO client binary
- Support for alias configuration with endpoint and credentials
- Optional bucket creation with `--ignore-existing` flag
- Optional SHA256 checksum verification (enabled by default)
- Automatic addition of `mc` to PATH for subsequent workflow steps
- Version selection support (latest or specific version)
- Custom download URL override capability
- Comprehensive input validation and error handling

### Features
- **Caching**: Weekly cache rotation prevents stale binaries
- **Security**: Uses GitHub Secrets for credentials
- **Flexibility**: Configurable install path and cache directory
- **Reliability**: Checksum verification for downloaded binaries
- **Compatibility**: Optimized for Ubuntu runners (Linux AMD64)
- **Outputs**: Provides `mc-path` and `mc-version` for downstream steps

### Technical Details
- Uses composite action approach for maximum compatibility
- Implements proper error handling with `set -euo pipefail`
- Supports both latest and specific version downloads
- Constructs version-aware download URLs
- Graceful fallback when checksums are unavailable

## [1.0.0] - TBD

### Added
- First stable release
- Complete documentation and examples
- Automated testing with MinIO service
- Release automation with major version tag management

---

## Usage Examples

### Basic Usage
```yaml
- uses: buckhamduffy/gha-minio-cli@v1
  with:
    alias: minio
    url: http://localhost:9000
    access_key: ${{ secrets.MINIO_ACCESS_KEY }}
    secret_key: ${{ secrets.MINIO_SECRET_KEY }}
```

### Advanced Usage
```yaml
- uses: buckhamduffy/gha-minio-cli@v1
  with:
    alias: production
    url: https://s3.amazonaws.com
    access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
    secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    bucket: my-production-bucket
    version: "RELEASE.2024-01-16T16-06-34Z"
    verify_checksum: true
```

---

## Migration Guide

### From Internal Action
If you were previously using this action from within the same repository:

**Old:**
```yaml
uses: ./.github/actions/setup-minio-client
```

**New:**
```yaml
uses: buckhamduffy/gha-minio-cli@v1
```

All inputs and outputs remain the same - only the `uses` reference needs to change.

---

## Support

- **Platform**: Linux (Ubuntu runners)
- **Architecture**: AMD64
- **MinIO Client**: All official releases
- **GitHub Actions**: Compatible with all runner versions

For issues, feature requests, or contributions, please visit:
https://github.com/buckhamduffy/gha-minio-cli