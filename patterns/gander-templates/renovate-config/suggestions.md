# Renovate Configuration - Customization Suggestions

## Adjusting Auto-merge Strategy

### More Conservative (Lower Risk)

Disable auto-merge for devDependencies:

```json
{
  "packageRules": [
    {
      "matchUpdateTypes": ["patch"],
      "automerge": true
    }
  ]
}
```

Only patches auto-merge. Everything else requires review.

### More Aggressive (Faster Updates)

Enable auto-merge for all non-major updates:

```json
{
  "packageRules": [
    {
      "matchUpdateTypes": ["patch", "minor"],
      "automerge": true
    }
  ]
}
```

Requires strong CI/CD test coverage!

### Never Auto-merge

For maximum control:

```json
{
  "packageRules": [
    {
      "matchUpdateTypes": ["patch", "minor", "major"],
      "automerge": false
    }
  ]
}
```

## Schedule Customization

### Daily Updates

For rapidly changing projects:

```json
{
  "schedule": ["before 6am every weekday"]
}
```

### Monthly Updates

For stable projects:

```json
{
  "schedule": ["before 6am on the first day of the month"]
}
```

### Timezone-Specific

```json
{
  "timezone": "America/New_York",
  "schedule": ["before 9am on Monday"]
}
```

## Additional Grouping Rules

### Group Frontend Dependencies

```json
{
  "packageRules": [
    {
      "matchPackagePatterns": ["^vue", "^@vue/", "^vite"],
      "groupName": "Vue Ecosystem"
    }
  ]
}
```

### Group Testing Tools

```json
{
  "packageRules": [
    {
      "matchPackagePatterns": ["vitest", "@vitest/", "playwright", "@playwright/"],
      "groupName": "Testing Tools"
    }
  ]
}
```

### Group GitHub Actions

```json
{
  "packageRules": [
    {
      "matchManagers": ["github-actions"],
      "groupName": "GitHub Actions"
    }
  ]
}
```

## Ignoring Specific Packages

### Ignore Problematic Package

```json
{
  "ignoreDeps": ["problematic-package"]
}
```

### Ignore Major Updates Only

```json
{
  "packageRules": [
    {
      "matchPackageNames": ["legacy-package"],
      "matchUpdateTypes": ["major"],
      "enabled": false
    }
  ]
}
```

## Monorepo Configuration

### Workspace-Specific Rules

```json
{
  "packageRules": [
    {
      "matchPaths": ["packages/backend/**"],
      "automerge": false
    },
    {
      "matchPaths": ["packages/frontend/**"],
      "automerge": true,
      "matchUpdateTypes": ["patch"]
    }
  ]
}
```

### Group Workspace Dependencies

```json
{
  "packageRules": [
    {
      "matchFileNames": ["packages/*/package.json"],
      "groupName": "Workspace Dependencies"
    }
  ]
}
```

## Pin Specific Versions

### Pin Exact Version

In package.json:
```json
{
  "dependencies": {
    "critical-package": "1.2.3"
  }
}
```

Renovate respects exact versions.

### Ignore Until Specific Date

```json
{
  "packageRules": [
    {
      "matchPackageNames": ["new-major-version"],
      "enabled": false,
      "schedule": ["after 2026-03-01"]
    }
  ]
}
```

## Commit Message Customization

### Custom Commit Format

```json
{
  "commitMessagePrefix": "deps:",
  "commitMessageAction": "bump",
  "commitMessageTopic": "{{depName}}",
  "commitMessageExtra": "from {{currentVersion}} to {{newVersion}}"
}
```

Result: `deps: bump typescript from 5.0.0 to 5.1.0`

### Per-Package Commit Type

```json
{
  "packageRules": [
    {
      "matchPackagePatterns": ["^@types/"],
      "semanticCommitType": "chore",
      "semanticCommitScope": "types"
    }
  ]
}
```

Result: `chore(types): update @types/node to 20.0.0`

## Security-Focused Configuration

### Prioritize Security Updates

```json
{
  "vulnerabilityAlerts": {
    "enabled": true,
    "automerge": true,
    "schedule": ["at any time"]
  }
}
```

### Separate Security PRs

```json
{
  "packageRules": [
    {
      "matchDatasources": ["npm"],
      "matchUpdateTypes": ["patch"],
      "matchDepTypes": ["dependencies"],
      "groupName": "Security Patches",
      "automerge": true
    }
  ]
}
```

## Rate Limit Tuning

### Higher Limits

For active repos with many dependencies:

```json
{
  "prConcurrentLimit": 20,
  "prHourlyLimit": 10
}
```

### Lower Limits

For quieter repos or to reduce noise:

```json
{
  "prConcurrentLimit": 3,
  "prHourlyLimit": 2
}
```

## Labeling Strategy

### Custom Labels

```json
{
  "labels": ["dependencies", "automated"],
  "packageRules": [
    {
      "matchUpdateTypes": ["major"],
      "labels": ["dependencies", "major-update", "review-required"]
    }
  ]
}
```

### Assign Reviewers

```json
{
  "reviewers": ["@team-lead"],
  "packageRules": [
    {
      "matchUpdateTypes": ["major"],
      "reviewers": ["@team-lead", "@senior-dev"]
    }
  ]
}
```

## Testing Configuration

### Validate Renovate Config

GitHub Action workflow:

```yaml
name: Validate Renovate

on:
  pull_request:
    paths:
      - renovate.json
      - .github/renovate.json5

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: rinchsan/renovate-config-validator@v1
```

### Dry Run Locally

```bash
# Install Renovate CLI
npm install -g renovate

# Dry run
renovate --dry-run --print-config
```

## Migration Strategies

### From No Automation

**Week 1:** Add Renovate, disable auto-merge
```json
{
  "extends": ["config:recommended"],
  "packageRules": [
    {
      "matchUpdateTypes": ["patch", "minor", "major"],
      "automerge": false
    }
  ]
}
```

**Week 2-3:** Review PRs, build confidence

**Week 4:** Enable patch auto-merge
```json
{
  "packageRules": [
    {
      "matchUpdateTypes": ["patch"],
      "automerge": true
    }
  ]
}
```

**Month 2:** Enable minor devDependencies

### From Dependabot

1. Keep Dependabot active initially
2. Add Renovate with different schedule
3. Compare PR quality for 2 weeks
4. Disable Dependabot
5. Remove `.github/dependabot.yml`

## Troubleshooting

### Too Many PRs

Reduce rate limits or increase grouping:

```json
{
  "prConcurrentLimit": 5,
  "packageRules": [
    {
      "matchPackagePatterns": ["*"],
      "groupName": "All Dependencies"
    }
  ]
}
```

### Auto-merge Not Working

Check:
1. Branch protection requires reviews? (Add Renovate to bypass list)
2. CI failing? (Fix tests first)
3. Renovate has merge permissions? (Check app settings)

### Updates Too Slow

Increase schedule frequency or rate limits:

```json
{
  "schedule": ["at any time"],
  "prConcurrentLimit": 15
}
```

## Related Patterns

- [release-please-workflow](../release-please-workflow/) - Automatic releases after dependency updates
- [package-json-structure](../package-json-structure/) - Dependency organization
- [lefthook-config](../lefthook-config/) - Local quality checks before Renovate PRs
