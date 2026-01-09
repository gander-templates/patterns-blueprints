# Pattern: Renovate Dependency Management

## Overview

Renovate is an automated dependency update tool that creates pull requests for dependency updates. This pattern establishes intelligent auto-merge strategies, grouped updates, and semantic commit conventions for keeping dependencies current with minimal manual intervention.

## Context

Use Renovate when:
- You want automated dependency updates
- Need to stay current with security patches
- Want grouped, logical PRs (not one PR per dependency)
- Team can review and merge automated PRs
- Repository is active (not archived)

Do NOT use Renovate when:
- Project is archived or inactive
- Dependencies are intentionally pinned
- Team prefers manual dependency management
- Using alternative tools (Dependabot with custom config)

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager) | Total: 3 projects

Missing in: nms-lib, nms-frigate-calc, osm-helper (MEDIUM-HIGH priority adoption candidates)

## Dependencies

### Required
- GitHub repository with package.json
- Renovate GitHub App installed on repository
- Or self-hosted Renovate instance

### Recommended
- CI/CD pipeline for automated testing
- [release-please-workflow](../release-please-workflow/) for version management
- Branch protection rules (require CI passing)

## Configuration

### Base Configuration

**Extends:**
- `config:recommended` - Renovate's recommended defaults
- `:semanticCommits` - Use conventional commit format
- `:semanticCommitTypeAll(chore)` - All updates use `chore:` type

**Schedule:**
- Monday mornings before 6am (low-traffic time)
- Avoids interrupting active development

**Rate Limiting:**
- Max 10 concurrent PRs
- Max 5 PRs per hour
- Prevents overwhelming PR queue

### Auto-merge Strategy

**Patch Updates** (e.g., 1.0.0 → 1.0.1):
- Auto-merge: **Yes**
- Reason: Bug fixes, security patches
- Risk: Very low

**Minor devDependencies** (e.g., 1.0.0 → 1.1.0):
- Auto-merge: **Yes**
- Reason: Development tooling, not production
- Risk: Low (tests will catch issues)

**Minor dependencies** (e.g., 1.0.0 → 1.1.0):
- Auto-merge: **No**
- Reason: Could affect production behavior
- Requires: Manual review

**Major updates** (e.g., 1.0.0 → 2.0.0):
- Auto-merge: **No**
- Reason: Breaking changes likely
- Requires: Manual review and testing

### Grouped Updates

**BiomeJS:**
- Groups: `@biomejs/*` packages
- Reason: Related tooling, update together

**Vitest:**
- Groups: `vitest`, `@vitest/*` packages
- Reason: Testing framework, update together

**TypeScript:**
- Groups: `typescript`, `@types/*` packages
- Reason: Type definitions match TypeScript version

### Lock File Maintenance

**Enabled:** Yes
**Auto-merge:** Yes
**Schedule:** Monday mornings
**Purpose:** Keep package-lock.json fresh without dependency updates

## Pattern Variations

### Variation A (Standard) - node-project-starter, osm-tagging-schema-mcp, diff-voyager

**Configuration:**
- Auto-merge patches + minor devDependencies
- Grouped updates (Biome, Vitest, TypeScript)
- Monday morning schedule
- Semantic commits

**When to use:**
- Active development projects
- Projects with good CI/CD
- Teams comfortable with automation

**Reference:** `files/renovate.json`

### Variation B (Conservative)

**Configuration:**
- No auto-merge (manual review all updates)
- Same grouping
- Less frequent schedule (monthly)

**When to use:**
- Critical production systems
- Projects with slow CI
- Teams preferring manual control

```json
{
  "extends": ["config:recommended"],
  "packageRules": [
    {
      "matchUpdateTypes": ["patch", "minor"],
      "automerge": false
    }
  ],
  "schedule": ["before 6am on the first day of the month"]
}
```

### Variation C (Aggressive)

**Configuration:**
- Auto-merge everything except majors
- Daily schedule
- Higher rate limits

**When to use:**
- Internal tools
- Rapid development phase
- Strong CI/CD safety net

```json
{
  "extends": ["config:recommended"],
  "packageRules": [
    {
      "matchUpdateTypes": ["patch", "minor"],
      "automerge": true
    }
  ],
  "schedule": ["before 6am every weekday"],
  "prConcurrentLimit": 20
}
```

## Known Issues

1. **Auto-merge with Breaking Changes**
   - Patch updates can sometimes include breaking changes (semver violations)
   - **Workaround:** Pin problematic packages, report to maintainer
   - **Mitigation:** Strong CI/CD catches issues before merge

2. **Renovate Bot Permissions**
   - May lack permissions to auto-merge on protected branches
   - **Solution:** Grant Renovate bypass permissions or disable auto-merge

3. **Rate Limit Exhaustion**
   - Too many updates can hit GitHub API limits
   - **Solution:** Adjust `prConcurrentLimit` and `prHourlyLimit`

4. **False Positive Updates**
   - Sometimes proposes updates that aren't real changes
   - **Solution:** Use `ignoreDeps` for problematic packages

## Limitations

- Cannot understand business logic (auto-merge relies on CI)
- May create noise if many dependencies update frequently
- Requires CI/CD to be reliable (fails = no auto-merge)
- GitHub-specific features (not all available on GitLab/Bitbucket)

## Integration

### GitHub App Installation

1. Install [Renovate App](https://github.com/apps/renovate)
2. Grant repository access
3. Add `renovate.json` to repository root
4. Renovate creates initial PR

### With CI/CD

Renovate respects branch protection:

```yaml
# .github/workflows/test.yml
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test
```

Auto-merge only happens if CI passes.

### With Release Please

Renovate updates trigger new releases:

```
Renovate updates dependency → CI passes → Auto-merge → Release Please → New version
```

Perfect automation pipeline!

### Package.json Scripts

No special scripts needed. Renovate works with:
- `npm install` / `npm ci`
- Standard package.json format

## Monitoring and Maintenance

### Renovate Dashboard

GitHub repositories have a Renovate dashboard:
- Issues tab → "Dependency Dashboard"
- Shows pending updates, rate limits, errors

### Common Maintenance Tasks

**Review Failed Auto-merge:**
- Check PR for CI failures
- Fix issues or merge manually

**Adjust Auto-merge Rules:**
- If too aggressive: Disable minor devDependencies auto-merge
- If too conservative: Enable minor dependencies auto-merge

**Handle Major Updates:**
- Review breaking changes
- Update code if needed
- Merge when ready

### Renovate Configuration Validation

Use Renovate config validator:

```bash
npx -p renovate -c 'renovate-config-validator'
```

Or use GitHub Action:

```yaml
name: Validate Renovate Config

on:
  pull_request:
    paths:
      - renovate.json

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: rinchsan/renovate-config-validator@v1
```

## Migration from Dependabot

Renovate is more flexible than Dependabot:

```bash
# 1. Disable Dependabot
# Remove .github/dependabot.yml

# 2. Install Renovate GitHub App

# 3. Add renovate.json (see files/renovate.json)

# 4. Close existing Dependabot PRs
# Renovate will create new ones
```

## Best Practices

1. **Start Conservative:** Begin with manual review, enable auto-merge gradually
2. **Monitor First Week:** Watch for issues, adjust configuration
3. **Trust CI/CD:** Ensure comprehensive test coverage before auto-merge
4. **Group Related:** Always group related packages (frameworks, tooling)
5. **Use Semantic Commits:** Integrates with Release Please and CHANGELOG generators

## References

- [Renovate Official Documentation](https://docs.renovatebot.com/)
- [Renovate Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Semantic Commit Format](https://www.conventionalcommits.org/)
- Related patterns: [release-please-workflow](../release-please-workflow/), [package-json-structure](../package-json-structure/)
