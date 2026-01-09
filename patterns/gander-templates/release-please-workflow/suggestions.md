# Release Please Workflow - Customization Suggestions

## Choosing Workflow Variant

### Simple Variant

For templates and basic libraries:

```yaml
jobs:
  release-please:
    runs-on: ubuntu-latest
    steps:
      - uses: googleapis/release-please-action@v4
        with:
          release-type: node
          token: ${{ secrets.GITHUB_TOKEN }}
```

### Production Variant

For production packages with security requirements:

```yaml
jobs:
  create-release:
    runs-on: ubuntu-latest
    steps:
      - name: Harden Runner
        uses: step-security/harden-runner@v2

      - uses: googleapis/release-please-action@v4

  publish-npm:
    needs: create-release
    if: needs.create-release.outputs.release_created
    steps:
      - name: Generate SBOM
      - name: Create tarball for attestation
      - name: Generate SLSA attestations
      - name: Publish to NPM with provenance
```

## Conventional Commits Customization

### Custom Changelog Sections

**release-please-config.json:**
```json
{
  "packages": {
    ".": {
      "changelog-sections": [
        {"type": "feat", "section": "Features"},
        {"type": "fix", "section": "Bug Fixes"},
        {"type": "perf", "section": "Performance"},
        {"type": "docs", "section": "Documentation", "hidden": false},
        {"type": "chore", "section": "Miscellaneous"},
        {"type": "refactor", "hidden": true}
      ]
    }
  }
}
```

### Custom Version Bump Rules

```json
{
  "packages": {
    ".": {
      "bump-minor-pre-major": true,
      "bump-patch-for-minor-pre-major": true
    }
  }
}
```

## npm Publishing Customization

### Public Scoped Package

```yaml
- name: Publish to NPM
  run: npm publish --provenance --access public
```

### Private Package

```yaml
- name: Publish to NPM
  run: npm publish --provenance --access restricted
```

### Registry Configuration

For private registries:

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: 24
    registry-url: 'https://npm.pkg.github.com'

- name: Publish to GitHub Packages
  run: npm publish --provenance
  env:
    NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Release Branches

### Multiple Release Branches

Support releases from multiple branches:

```yaml
on:
  push:
    branches:
      - main
      - v1.x
      - v2.x
```

**release-please-config.json:**
```json
{
  "packages": {
    ".": {
      "release-type": "node"
    }
  },
  "branches": ["main", "v1.x", "v2.x"]
}
```

### Beta/Pre-release Support

```json
{
  "packages": {
    ".": {
      "prerelease": true,
      "prerelease-type": "beta"
    }
  }
}
```

## Monorepo Configuration

### Multiple Packages

**release-please-config.json:**
```json
{
  "packages": {
    "packages/backend": {
      "release-type": "node",
      "package-name": "@org/backend"
    },
    "packages/frontend": {
      "release-type": "node",
      "package-name": "@org/frontend"
    }
  }
}
```

### Workspace Publishing

```yaml
- name: Publish all workspaces
  run: npm publish --workspaces --provenance --access public
```

## CHANGELOG Customization

### Include Contributors

```json
{
  "packages": {
    ".": {
      "changelog-notes-type": "github",
      "include-component-in-tag": false
    }
  }
}
```

### Custom CHANGELOG Template

Create `.github/release-please-template.txt`:

```
## {{version}} ({{date}})

{{#each commits}}
* {{subject}} ({{author}})
{{/each}}
```

## Pre-publish Checks

### Add Validation Steps

```yaml
publish-npm:
  steps:
    - name: Install dependencies
      run: npm ci

    - name: Run tests
      run: npm test

    - name: Type check
      run: npm run typecheck

    - name: Validate package
      run: npx publint --strict

    - name: Build
      run: npm run build

    - name: Publish to NPM
      run: npm publish --provenance
```

### Dry Run Testing

Test publish locally:

```bash
npm publish --dry-run
```

## Security Enhancements

### Add Vulnerability Scanning

```yaml
- name: Scan for vulnerabilities
  uses: aquasecurity/trivy-action@master
  with:
    scan-type: 'fs'
    scan-ref: '.'
```

### Add SBOM Generation

```yaml
- name: Generate SBOM
  run: |
    npx @cyclonedx/cyclonedx-npm --output-file sbom.json

- name: Generate SLSA SBOM attestation
  uses: actions/attest-sbom@v3
  with:
    subject-path: ${{ env.TARBALL }}
    sbom-path: sbom.json
```

### Code Signing

```yaml
- name: Sign package
  uses: sigstore/gh-action-sigstore-python@v2
  with:
    inputs: ./${{ env.TARBALL }}
```

## Notification Customization

### Slack Notification

```yaml
- name: Notify Slack
  if: success()
  uses: slackapi/slack-github-action@v1
  with:
    payload: |
      {
        "text": "Released version ${{ needs.create-release.outputs.version }}"
      }
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

### Discord Notification

```yaml
- name: Notify Discord
  if: success()
  uses: sarisia/actions-status-discord@v1
  with:
    webhook: ${{ secrets.DISCORD_WEBHOOK }}
    title: "Package Published"
    description: "Version ${{ needs.create-release.outputs.version }}"
```

## Commit Message Enforcement

### Add commitlint

**.commitlintrc.json:**
```json
{
  "extends": ["@commitlint/config-conventional"]
}
```

**package.json:**
```json
{
  "scripts": {
    "commitlint": "commitlint --edit"
  }
}
```

**lefthook.yml:**
```yaml
commit-msg:
  commands:
    lint-commit-msg:
      run: npx commitlint --edit {1}
```

## Troubleshooting

### Release PR Not Updating

**Problem:** New commits not reflected in Release PR

**Solution:**
1. Close existing Release PR
2. Let Release Please create fresh PR

### Version Conflict

**Problem:** package.json version doesn't match release

**Solution:**
- Never manually edit version in package.json
- Let Release Please manage versions

### npm Publish Timeout

**Problem:** Publish step hangs

**Solution:**
```yaml
- name: Publish to NPM
  timeout-minutes: 5
  run: npm publish --provenance
```

## Migration Strategies

### From Manual Releases

**Week 1:** Add Release Please, keep manual releases
```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:  # Allow manual trigger
```

**Week 2-3:** Run in parallel, compare outputs

**Week 4:** Remove manual release process

### From Semantic Release

1. Remove semantic-release config
2. Add Release Please workflow
3. Add release-please-config.json
4. First release creates baseline

## Best Practices

1. **Always Review Release PR**
   - Check CHANGELOG accuracy
   - Verify version bump
   - Confirm breaking changes documented

2. **Use Scoped Commits**
   ```
   feat(auth): add OAuth support
   fix(api): handle null responses
   ```

3. **Document Breaking Changes**
   ```
   feat!: change API response format

   BREAKING CHANGE: Returns array instead of object
   ```

4. **Keep Commits Focused**
   - One logical change per commit
   - Clear, descriptive messages

5. **Test Before Merge**
   - PR checks must pass
   - Manual testing when needed

## Related Patterns

- [renovate-config](../renovate-config/) - Dependency updates with semantic commits
- [package-json-structure](../package-json-structure/) - Version and repository configuration
- [lefthook-config](../lefthook-config/) - Commit message validation
