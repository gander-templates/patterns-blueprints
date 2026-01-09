# Lefthook Configuration - Customization Suggestions

## Choosing What to Include

### Minimal Setup (Format Only)

For projects just starting with hooks:

```yaml
pre-commit:
  parallel: true
  commands:
    format:
      glob: "*.{ts,js,json}"
      run: npx biome check --write {staged_files}
      stage_fixed: true
```

Benefits:
- Fast (only formatting)
- Non-intrusive
- Good first step

### Standard Setup (Pre-commit + Quick Pre-push)

For most projects:

```yaml
pre-commit:
  parallel: true
  commands:
    format:
      glob: "*.{ts,js,json}"
      run: npx biome check --write {staged_files}
      stage_fixed: true

pre-push:
  parallel: true
  commands:
    typecheck:
      run: npm run typecheck
```

Benefits:
- Catches most issues
- Reasonably fast
- Good balance

### Full Setup (All Checks)

For libraries and high-quality codebases:

Use reference configuration (files/lefthook.yml)

Benefits:
- Maximum safety
- All issues caught locally
- Matches CI checks

## Common Customizations

### Add Commit Message Linting

```yaml
commit-msg:
  commands:
    lint-commit-msg:
      run: npx commitlint --edit {1}
```

Requires: `@commitlint/cli` and `@commitlint/config-conventional`

### Skip Tests on Pre-push

If tests are slow:

```yaml
pre-push:
  commands:
    typecheck:
      run: npm run typecheck
    build:
      run: npm run build
    # Test removed - run in CI only
```

### Only Check Changed Files in Tests

For faster testing:

```yaml
pre-push:
  commands:
    test:
      run: npm run test:related {staged_files}
```

Requires test framework that supports file filtering.

### Add Security Audit

```yaml
pre-commit:
  commands:
    audit:
      run: npm audit --audit-level=high
```

Warning: May slow down commits.

### Skip Hooks for WIP Commits

Create git alias:

```bash
git config --global alias.wip '!git commit -m "WIP" --no-verify'
```

Usage: `git wip` skips hooks automatically.

### Project-Specific Scripts

If your project has different script names:

```yaml
pre-push:
  commands:
    typecheck:
      run: npm run type-check  # Custom script name
    test:
      run: npm run test:ci    # CI test script
```

## Performance Optimization

### Cache Build Results

For faster builds:

```yaml
pre-push:
  commands:
    build:
      run: npm run build
      cache: true  # Cache build results (experimental)
```

### Skip Slow Commands Conditionally

```yaml
pre-push:
  commands:
    test:
      run: |
        if [ "$SKIP_TESTS" != "1" ]; then
          npm test
        fi
```

Usage: `SKIP_TESTS=1 git push`

### Parallel with Limits

Limit parallel execution:

```yaml
pre-commit:
  parallel: true
  piped: true  # Stream output instead of buffering
```

## Different Configurations per Branch

```yaml
pre-push:
  commands:
    test:
      run: npm test
      exclude_branches:
        - wip/*
        - draft/*
```

Or only on main:

```yaml
pre-push:
  commands:
    comprehensive-test:
      run: npm run test:all
      only_branches:
        - main
        - master
```

## Hook Debugging

### Verbose Output

```bash
LEFTHOOK_VERBOSE=1 git commit -m "test"
```

### Dry Run

Test configuration without running commands:

```bash
lefthook run pre-commit --dry-run
```

### Manual Trigger

Run hooks manually:

```bash
lefthook run pre-commit
lefthook run pre-push
```

## Integration with Different Tools

### With Prettier (instead of Biome)

```yaml
pre-commit:
  commands:
    format:
      glob: "*.{ts,js,json}"
      run: npx prettier --write {staged_files}
      stage_fixed: true
```

### With ESLint (instead of Biome)

```yaml
pre-commit:
  commands:
    lint:
      glob: "*.{ts,js}"
      run: npx eslint --fix {staged_files}
      stage_fixed: true
```

### With Stylelint (for CSS)

```yaml
pre-commit:
  commands:
    style-lint:
      glob: "*.{css,scss}"
      run: npx stylelint --fix {staged_files}
      stage_fixed: true
```

## Monorepo Configuration

For workspaces/monorepos:

```yaml
pre-commit:
  parallel: true
  commands:
    format-backend:
      glob: "packages/backend/**/*.ts"
      run: npx biome check --write {staged_files}
      stage_fixed: true

    format-frontend:
      glob: "packages/frontend/**/*.{ts,vue}"
      run: npx prettier --write {staged_files}
      stage_fixed: true
```

## CI/CD Synchronization

Ensure hooks match CI:

**lefthook.yml:**
```yaml
pre-push:
  commands:
    typecheck:
      run: npm run typecheck
    test:
      run: npm test
    build:
      run: npm run build
```

**.github/workflows/test.yml:**
```yaml
- name: Typecheck
  run: npm run typecheck

- name: Test
  run: npm test

- name: Build
  run: npm run build
```

Benefit: Local checks match CI checks exactly.

## Team Guidelines

### When to Skip Hooks

Acceptable:
- WIP commits on feature branches
- Emergency hotfixes (with follow-up fix)
- Merge commits with conflicts

Not acceptable:
- "I'm in a hurry" (hooks are fast)
- "Tests are annoying" (fix tests, don't skip)
- Regular workflow (hooks should be fast enough)

### Communicating with Team

Add to README:

```markdown
## Development Workflow

This project uses Lefthook for automated code quality checks.

**Pre-commit:** Formats and lints code automatically
**Pre-push:** Runs type checking, tests, and build

**To skip hooks (emergency only):**
\`\`\`bash
LEFTHOOK=0 git push
\`\`\`
```

## Troubleshooting

### Hooks Not Running

```bash
# Check installation
ls -la .git/hooks/

# Reinstall
npx lefthook install

# Verify configuration
lefthook version
```

### Hooks Running But Failing

```bash
# Test commands individually
npm run typecheck
npm test
npm run build

# Check glob patterns
lefthook run pre-commit --verbose
```

### Performance Issues

```bash
# Profile hook execution
time lefthook run pre-commit

# Identify slow commands
# Remove or optimize slowest ones
```

## Related Patterns

- [biome-config](../biome-config/) - Formatting and linting tool
- [tsconfig-base](../tsconfig-base/) - TypeScript type checking
- [package-json-structure](../package-json-structure/) - Required scripts
