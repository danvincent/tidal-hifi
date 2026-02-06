# Testing the Release Workflow

This document provides instructions for testing the new release workflow.

## How to Test

### Option 1: Create a Real Release (Recommended for Production)

1. Update the version in package.json:
   ```bash
   npm version patch  # or minor, or major
   ```

2. Update CHANGELOG.md with release notes

3. Commit and push:
   ```bash
   git add package.json package-lock.json CHANGELOG.md
   git commit -m "Prepare release v5.20.2"
   git push origin master
   ```

4. Create and push a tag:
   ```bash
   git tag v5.20.2
   git push origin v5.20.2
   ```

5. The workflow will automatically trigger and create a release at:
   https://github.com/danvincent/tidal-hifi/releases

### Option 2: Test via Manual Trigger

1. Go to https://github.com/danvincent/tidal-hifi/actions
2. Select "Publish Release" workflow from the left sidebar
3. Click "Run workflow" button (top right)
4. Enter a tag name (e.g., "v5.20.1-test")
5. Click "Run workflow"

Note: The manual trigger requires the tag to already exist in the repository.

### Option 3: Test with a Pre-release Tag

1. Create a test tag:
   ```bash
   git tag v5.20.2-beta.1
   git push origin v5.20.2-beta.1
   ```

2. Manually edit the release in GitHub UI to mark it as "pre-release" if needed

## What to Verify

After the workflow runs, verify:

1. ✅ The workflow completed successfully (green checkmark)
2. ✅ A new release appears at https://github.com/danvincent/tidal-hifi/releases
3. ✅ The release includes a .deb file as an asset
4. ✅ The .deb file name follows the pattern: `tidal-hifi_<version>_amd64.deb`
5. ✅ The release notes are auto-generated from commits
6. ✅ The release is properly tagged with the version

## Troubleshooting

### Workflow Fails at Build Step

- Check that package.json and all dependencies are correct
- Ensure electron-builder configuration is valid
- Review the Actions log for specific error messages

### Workflow Fails at Release Creation

- Verify the repository has the correct permissions
- Ensure the tag format is correct (must start with 'v')
- Check that GITHUB_TOKEN has write permissions

### .deb File is Missing

- Check the build logs to see if the .deb was created
- Verify the `dist/` directory contains the .deb file
- Ensure the file glob pattern `dist/*.deb` matches your file

## Expected Build Time

The workflow should complete in approximately 3-5 minutes:
- Dependency installation: ~1 minute
- TypeScript compilation: ~30 seconds
- Electron packaging: ~2-3 minutes
- Release creation: ~10 seconds

## Next Steps After Testing

Once verified:
1. Clean up any test releases/tags if created
2. Document the release process for maintainers
3. Consider adding additional artifact types (rpm, AppImage, etc.) if needed
