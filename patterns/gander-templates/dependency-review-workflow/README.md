# Pattern: Dependency Review Workflow

## Overview

Scans Pull Requests for dependency changes and identifies vulnerabilities, license issues, and security risks before merging. Uses GitHub's Dependency Review action.

## Context

Use this workflow when:
- Want security scanning for dependency updates
- Need license compliance checking
- Publishing to npm
- Security is a priority

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager) | Total: 3 projects

## Configuration

### Trigger

```yaml
on:
  pull_request:
    branches:
      - main
```

Scans every PR to main branch for dependency changes.

### Security Checks

**Vulnerability Scanning:**
- Identifies known CVEs in dependencies
- Checks severity levels (low, moderate, high, critical)
- Fails PR if critical vulnerabilities found

**License Compliance:**
- Detects license changes
- Flags incompatible licenses
- Ensures license compliance

**Dependency Changes:**
- Shows added dependencies
- Shows removed dependencies
- Shows version updates

### GitHub Dependency Graph

Requires GitHub Dependency Graph enabled:
1. Go to repository Settings
2. Security & analysis
3. Enable Dependency graph

## Configuration Options

### Fail on Severity

```yaml
- name: Dependency Review
  uses: actions/dependency-review-action@v4
  with:
    fail-on-severity: moderate
```

Options: `low`, `moderate`, `high`, `critical`

### Deny Licenses

```yaml
with:
  deny-licenses: GPL-3.0, AGPL-3.0
```

Blocks specific licenses.

### Allow Licenses

```yaml
with:
  allow-licenses: MIT, Apache-2.0, BSD-3-Clause
```

Only permits specified licenses.

## Integration

### With Renovate

Perfect combination:
1. Renovate creates PR with dependency update
2. Dependency Review scans for issues
3. Auto-merge only if scan passes

### Required Permissions

```yaml
permissions:
  contents: read
  pull-requests: write
```

## Benefits

1. **Early Detection** - Catches vulnerabilities before merge
2. **License Compliance** - Ensures legal compliance
3. **Transparent** - Shows dependency changes in PR
4. **Automated** - No manual review needed

## Known Issues

1. **GitHub Enterprise Only Features**
   - Some features require GitHub Enterprise
   - Public repos have full functionality

2. **Dependency Graph Latency**
   - New packages may not be in database immediately
   - Rare false negatives

## Installation

1. Enable Dependency Graph in repository settings
2. Copy `files/dependency-review.yml` to `.github/workflows/dependency-review.yml`
3. Commit and push
4. Workflow activates on next PR

## References

- [Dependency Review Action](https://github.com/actions/dependency-review-action)
- [GitHub Dependency Graph](https://docs.github.com/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)
- Related patterns: [renovate-config](../renovate-config/)
