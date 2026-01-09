# Pattern: Lefthook Git Hooks Configuration

## Overview

Lefthook is a fast, language-agnostic Git hooks manager that automates code quality checks before commits and pushes. This pattern establishes consistent pre-commit formatting/linting and pre-push testing across Gander projects, preventing broken code from entering version control.

## Context

Use Lefthook when:
- You want automated code quality enforcement
- Need fast, parallel hook execution
- Want to prevent committing unformatted/failing code
- Team needs consistent Git workflow
- Already using [biome-config](../biome-config/) or similar tools

Do NOT use Lefthook when:
- Project has no automated tooling (nothing to run in hooks)
- Team strongly prefers manual workflow
- CI/CD is sole quality gate (though hooks + CI is better)

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [nms-lib](https://github.com/gander-tools/nms-lib) | Total: 3 projects

Missing in: diff-voyager, nms-frigate-calc, osm-helper (HIGH priority adoption candidates)

## Dependencies

### Required
- Lefthook package (`npm install --save-dev lefthook`)
- Node.js project with package.json
- Git repository

### Recommended
- [biome-config](../biome-config/) - For format and lint commands
- [tsconfig-base](../tsconfig-base/) - For typecheck command
- Test framework (Vitest, Jest, Node test runner)
- Build tool (tsc, tsdown, esbuild)

## Configuration

### Pre-commit Hooks

Run before commit is created. All commands run in **parallel** for speed.

**1. Format Check with Auto-fix**
- Glob: `*.{ts,js,json}`
- Command: `npx biome check --write {staged_files}`
- Feature: `stage_fixed: true` (automatically stages formatted files)

**2. Lint Check with Auto-fix**
- Glob: `*.{ts,js}`
- Command: `npx biome lint --write {staged_files}`
- Feature: `stage_fixed: true`

**3. Lockfile Validation**
- Trigger: When `package-lock.json` is staged
- Command: `npm install --package-lock-only --ignore-scripts`
- Purpose: Ensure lockfile integrity

### Pre-push Hooks

Run before push to remote. All commands run in **parallel** for speed.

**1. TypeScript Type Checking**
- Command: `npm run typecheck` (tsc --noEmit)
- Purpose: Catch type errors before CI

**2. Run All Tests**
- Command: `npm test`
- Purpose: Prevent pushing broken tests

**3. Build Verification**
- Command: `npm run build`
- Purpose: Ensure project builds successfully

## Pattern Variations

### Variation A (Full) - node-project-starter, osm-tagging-schema-mcp, nms-lib

**Configuration:** All hooks enabled (format, lint, lockfile, typecheck, test, build)

**When to use:**
- Libraries and packages
- Projects with comprehensive test suites
- High code quality requirements

**Reference:** `files/lefthook.yml`

### Variation B (Minimal)

**Configuration:** Only pre-commit format/lint, skip pre-push

**When to use:**
- Rapid prototyping
- Projects without tests yet
- CI/CD handles all validation

```yaml
pre-commit:
  parallel: true
  commands:
    format:
      glob: "*.{ts,js,json}"
      run: npx biome check --write {staged_files}
      stage_fixed: true
```

### Variation C (CI-Only)

**Configuration:** No pre-push hooks, only pre-commit formatting

**When to use:**
- Very large test suites (slow pre-push)
- Team prefers fast local workflow
- CI is fast enough to catch issues

## Known Issues

1. **Slow Tests Block Pushes**
   - Large test suites can make `git push` slow
   - **Workaround:** Move tests to CI-only, keep only fast checks in pre-push
   - Or use `LEFTHOOK=0 git push` to skip hooks when needed

2. **Lockfile Validation Fails**
   - Can occur if `package.json` and lockfile are out of sync
   - **Solution:** Run `npm install` before committing

3. **Windows Path Issues**
   - Some commands may need adjusted path separators
   - **Workaround:** Use forward slashes in glob patterns

4. **Hook Installation**
   - Hooks not installed if `npm install` run with `--ignore-scripts`
   - **Solution:** Run `npm run prepare` or `npx lefthook install` manually

## Limitations

- Requires npm scripts to exist (typecheck, test, build)
- Pre-push hooks can slow down pushes if checks are slow
- Cannot skip individual hooks easily (use `LEFTHOOK=0` to skip all)
- Parallel execution means error output can be interleaved

## Integration

### Package.json Setup

```json
{
  "scripts": {
    "prepare": "lefthook install",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "build": "tsc"
  }
}
```

**Important:** `prepare` script ensures hooks install automatically on `npm install`.

### Installation

```bash
# Install package
npm install --save-dev lefthook

# Install hooks (automatic with prepare script)
npm run prepare

# Or manually
npx lefthook install
```

### Skipping Hooks

When you need to bypass hooks:

```bash
# Skip all hooks for one push
LEFTHOOK=0 git push

# Skip all hooks for one commit
LEFTHOOK=0 git commit -m "WIP"

# Skip specific hook
git commit --no-verify
```

Use sparingly - hooks exist for good reasons!

### CI/CD Integration

Run same checks in CI:

```yaml
# .github/workflows/test.yml
- name: Typecheck
  run: npm run typecheck

- name: Test
  run: npm test

- name: Build
  run: npm run build
```

## Performance Tips

### Speed Up Pre-commit

Only check staged files (already configured):

```yaml
format:
  glob: "*.{ts,js,json}"
  run: npx biome check --write {staged_files}  # Fast!
```

### Speed Up Pre-push

Skip slow tests in hooks, run in CI:

```yaml
pre-push:
  commands:
    quick-test:
      run: npm run test:unit  # Only fast unit tests
```

### Parallel Execution

Lefthook runs commands in parallel by default:

```yaml
pre-commit:
  parallel: true  # Enable parallel execution (default)
```

## Migration from Husky

Lefthook is faster and simpler than Husky:

```bash
# 1. Remove Husky
npm uninstall husky
rm -rf .husky

# 2. Install Lefthook
npm install --save-dev lefthook

# 3. Create lefthook.yml (see files/lefthook.yml)

# 4. Install hooks
npm run prepare
```

## Team Adoption

### Communication
- Explain hooks prevent broken code
- Show time saved catching issues early
- Emphasize parallel execution speed

### Onboarding
- Document in README
- Include in setup instructions
- Show how to skip hooks when needed

### Troubleshooting

**Hooks not running?**
- Check: `ls -la .git/hooks/`
- Fix: `npx lefthook install`

**Too slow?**
- Profile: Time individual commands
- Optimize: Remove slow commands from pre-push
- Consider: Move to CI-only

## References

- [Lefthook Official Documentation](https://github.com/evilmartians/lefthook)
- [Git Hooks Documentation](https://git-scm.com/docs/githooks)
- Related patterns: [biome-config](../biome-config/), [tsconfig-base](../tsconfig-base/)
