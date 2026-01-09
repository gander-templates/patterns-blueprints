# Pattern: Release Please Workflow

## Overview

Automated release management workflow using Google's Release Please action. Automatically generates releases, changelogs, and publishes to npm based on Conventional Commits. Includes npm provenance and SLSA build attestations for supply chain security.

## Context

Use Release Please when:
- Publishing npm packages
- Want automated semantic versioning
- Use Conventional Commits
- Need CHANGELOG generation
- Want npm provenance/SLSA attestations
- GitHub-hosted repository

Do NOT use when:
- Manual version control preferred
- Not using Conventional Commits
- Not publishing to npm
- Private packages without releases

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager) | Total: 3 projects

Missing in: nms-lib, nms-frigate-calc, osm-helper (MEDIUM priority adoption)

## Dependencies

### Required
- GitHub repository with main/master branch
- package.json with version field
- [Conventional Commits](https://www.conventionalcommits.org/) workflow
- GitHub Actions enabled
- npm account with publishing rights (for npm publishing)

### Recommended
- [renovate-config](../renovate-config/) - Automated dependency updates with semantic commits
- Branch protection rules
- CI/CD tests before release
- npm 11.6.4+ for provenance (GitHub Actions auto-installs)

## Configuration

### Two-Job Architecture

**Job 1: Create Release**
- Runs on every push to main branch
- Analyzes commit history (Conventional Commits)
- Creates/updates Release PR with:
  - Updated CHANGELOG.md
  - Bumped version in package.json
  - Release notes
- Creates GitHub Release when Release PR merged

**Job 2: Publish to npm**
- Runs ONLY after Release PR merged (conditional)
- Builds package
- Runs tests
- Validates with publint
- Publishes to npm with provenance
- Generates SLSA attestations
- Uploads artifacts to GitHub Release

### Conventional Commits Format

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Version Bumping:**
- `feat:` → Minor version bump (1.0.0 → 1.1.0)
- `fix:` → Patch version bump (1.0.0 → 1.0.1)
- `BREAKING CHANGE:` → Major version bump (1.0.0 → 2.0.0)
- `chore:`, `docs:`, `style:` → No version bump

### NPM Provenance

**What is it:**
- Cryptographic proof linking npm package to GitHub build
- Verifiable build metadata
- Transparency log entries

**How it works:**
```yaml
- name: Publish to NPM with provenance
  run: npm publish --provenance --access public
```

Requires:
- `id-token: write` permission
- npm 11.6.4+
- GitHub Actions OIDC

**Verification:**
```bash
npm audit signatures @gander-tools/package-name
```

### SLSA Build Attestations

**SLSA Levels:**
- **Level 1**: Documented build process
- **Level 2**: Automated build service
- **Level 3**: Provenance + attestations (this pattern)

**Attestations generated:**
1. **Build Provenance** - Links package to build
2. **SBOM** - Software Bill of Materials

```yaml
- name: Generate SLSA build provenance attestation
  uses: actions/attest-build-provenance@v3

- name: Generate SLSA SBOM attestation
  uses: actions/attest-sbom@v3
```

## Pattern Variations

### Variation A (Template/Simple) - node-project-starter

**Characteristics:**
- Node.js 22
- Simpler workflow structure
- Basic publishing
- Minimal security scanning

**When to use:**
- Template projects
- Libraries without complex security needs
- Projects preferring simplicity

**Reference:** `files/release-please-simple.yml`

### Variation B (Production/Hardened) - osm-tagging-schema-mcp

**Characteristics:**
- Node.js 24 (latest LTS)
- Harden Runner security
- Version validation checks
- SBOM generation
- Comprehensive attestations
- dist/ artifact sharing

**When to use:**
- Production packages
- Security-critical libraries
- MCP servers
- CLI tools

**Reference:** `files/release-please-production.yml`

### Variation C (Monorepo) - diff-voyager

**Characteristics:**
- Workspace-aware
- Multiple release configurations
- Per-package versioning

**When to use:**
- Monorepos with multiple packages
- Independent package versioning

## Known Issues

1. **Initial Release PR Creation**
   - First run creates Release PR even with no conventional commits
   - **Solution:** Close initial PR if not ready

2. **Merge Conflicts in Release PR**
   - Can occur if main branch updated during PR lifetime
   - **Solution:** Close and let Release Please create new PR

3. **npm Publish Failures**
   - Network issues or npm registry downtime
   - **Solution:** Re-run workflow or manually publish

4. **Provenance Requires npm 11.6.4+**
   - Older npm versions don't support `--provenance`
   - **Solution:** Use Node.js 24 which includes npm 11.6.4+

## Limitations

- GitHub-only (no GitLab/Bitbucket support)
- Requires Conventional Commits discipline
- Cannot customize CHANGELOG format extensively
- Release PR must be merged (squash merge not ideal)

## Integration

### Package.json Configuration

```json
{
  "version": "1.0.0",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/org/repo.git"
  }
}
```

### Release Please Config

**release-please-config.json:**
```json
{
  "$schema": "https://raw.githubusercontent.com/googleapis/release-please/main/schemas/config.json",
  "packages": {
    ".": {
      "release-type": "node",
      "changelog-sections": [
        {"type": "feat", "section": "Features"},
        {"type": "fix", "section": "Bug Fixes"},
        {"type": "chore", "section": "Miscellaneous"}
      ]
    }
  }
}
```

### GitHub Secrets Required

**NPM_TOKEN:**
- npm access token for publishing
- Create at: https://www.npmjs.com/settings/tokens
- Type: Automation token
- Add to: Repository secrets

**Note:** With npm Trusted Publishers (npm 11.6.4+), NPM_TOKEN may not be needed.

### Workflow File

**.github/workflows/release-please.yml:**
- See `files/release-please-simple.yml` or `files/release-please-production.yml`

## Conventional Commits Examples

### Feature (Minor Bump)

```
feat(auth): add OAuth2 login support

Implements OAuth2 authentication flow with Google provider.
```

### Bug Fix (Patch Bump)

```
fix(validation): handle null values in tag validation

Previously threw error on null values, now returns validation error.
```

### Breaking Change (Major Bump)

```
feat(api)!: change search endpoint response format

BREAKING CHANGE: search results now return array of objects
instead of array of strings. Update client code accordingly.
```

### No Version Bump

```
chore(deps): update @biomejs/biome to 2.3.8

docs: update installation instructions
```

## Workflow Sequence

1. **Developer commits** with conventional format → Push to feature branch
2. **Create PR** to main branch
3. **Merge PR** → Release Please analyzes commits
4. **Release Please creates Release PR** with version bump + CHANGELOG
5. **Review Release PR** → Verify version and CHANGELOG
6. **Merge Release PR** → Triggers release + npm publish
7. **GitHub Release created** + npm package published + Attestations generated

## Security Features

### Harden Runner

```yaml
- name: Harden Runner
  uses: step-security/harden-runner@v2
  with:
    disable-sudo: 'true'
    egress-policy: 'audit'
```

Monitors network egress and restricts sudo access.

### SHA-Pinned Actions

All actions use commit SHA instead of tags:

```yaml
uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6.0.1
```

Prevents tag manipulation attacks.

### OIDC Token Publishing

No long-lived secrets needed:

```yaml
permissions:
  id-token: write
```

GitHub generates short-lived token for npm publish.

## Verification

### Verify npm Provenance

```bash
npm audit signatures @gander-tools/package-name
```

### Verify SLSA Attestations

```bash
gh attestation verify oci://ghcr.io/gander-tools/package:version --owner gander-tools
```

### View Transparency Log

Visit: https://search.sigstore.dev/

## Migration from Manual Releases

1. **Install Release Please:**
   - Add workflow file
   - Add release-please-config.json

2. **Adopt Conventional Commits:**
   - Update CONTRIBUTING.md
   - Train team on format
   - Consider commitlint for enforcement

3. **First Release:**
   - Let Release Please create initial Release PR
   - Review and adjust CHANGELOG
   - Merge to trigger first automated release

4. **Remove Manual Release Steps:**
   - Delete manual release scripts
   - Update documentation

## Best Practices

1. **Squash vs Merge Commits:**
   - Prefer merge commits for Release PRs
   - Squash loses individual commit context

2. **Commit Discipline:**
   - Use conventional format consistently
   - Include scope for better changelogs
   - Write clear descriptions

3. **Release PR Review:**
   - Always review generated CHANGELOG
   - Verify version bump is correct
   - Check for breaking changes

4. **npm Provenance:**
   - Always publish with `--provenance`
   - Enables package verification
   - Builds user trust

## Troubleshooting

### Release PR Not Created

**Causes:**
- No conventional commits since last release
- Release Please not configured correctly

**Solution:**
- Check commit history
- Verify release-please-config.json

### npm Publish Failed

**Causes:**
- Package already exists at version
- npm credentials invalid
- Network issues

**Solution:**
- Check npm registry
- Verify NPM_TOKEN or OIDC setup
- Re-run workflow

### Provenance Generation Failed

**Causes:**
- npm version < 11.6.4
- Missing id-token permission

**Solution:**
- Use Node.js 24
- Add `id-token: write` to permissions

## References

- [Release Please Documentation](https://github.com/googleapis/release-please)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [npm Provenance](https://docs.npmjs.com/generating-provenance-statements)
- [SLSA Framework](https://slsa.dev/)
- Related patterns: [renovate-config](../renovate-config/), [package-json-structure](../package-json-structure/)
