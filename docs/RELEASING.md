# Release Process

This document describes how to create a release for TIDAL Hi-Fi.

## Creating a Release

The project uses GitHub Actions to automatically build and publish releases when a version tag is pushed.

### Automated Release (Recommended)

1. Update the version in `package.json` and `package-lock.json`:
   ```bash
   npm version <major|minor|patch>
   ```

2. Update `CHANGELOG.md` with the changes for the new version

3. Commit the changes:
   ```bash
   git add package.json package-lock.json CHANGELOG.md
   git commit -m "Release v<version>"
   ```

4. Create and push a git tag:
   ```bash
   git tag v<version>
   git push origin v<version>
   ```

5. The GitHub Actions workflow will automatically:
   - Build the .deb package
   - Create a GitHub release
   - Upload the .deb file to the release

### Manual Release Trigger

You can also trigger a release manually through the GitHub Actions UI:

1. Go to the Actions tab in the GitHub repository
2. Select the "Publish Release" workflow
3. Click "Run workflow"
4. Enter the tag name (e.g., `v5.20.1`)
5. Click "Run workflow"

## Build Artifacts

The release workflow builds the following artifacts:

- `.deb` package for Debian/Ubuntu Linux distributions

## Requirements

The build process requires:
- Ubuntu latest (GitHub Actions runner)
- Node.js 22.12.0
- System dependencies: libarchive-tools, build-essential

## Troubleshooting

If the build fails:

1. Check the GitHub Actions logs for error messages
2. Ensure all dependencies are correctly installed
3. Verify that the version tag format is correct (should start with 'v')
4. Make sure the package.json version matches the tag version
