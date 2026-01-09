# Improvements for nms-frigate-calc

Last updated: 2026-01-09

## Summary

**Priority: HIGH** - Repository missing most standard patterns. Has minimal GitHub Actions (only deploy.yml), no automated tooling, likely using outdated ESLint+Prettier setup.

## Configuration Improvements

### [2026-01-09] Add Biome Configuration
**Priority:** High
**Category:** Config
**Status:** Proposed

**Current:** Likely using ESLint + Prettier (anti-pattern)
**Proposed:** Replace with `biome.json` configuration
**Reason:** Faster, simpler, single tool for formatting and linting
**Files:** `biome.json`, remove `.eslintrc.*`, `.prettierrc.*`
**Pattern:** [biome-config](../patterns/gander-templates/biome-config/)
**Migration:** Remove ESLint/Prettier packages, add Biome, run initial format

### [2026-01-09] Add Lefthook Git Hooks
**Priority:** High
**Category:** Config
**Status:** Proposed

**Current:** No Git hooks automation
**Proposed:** Add `lefthook.yml` with pre-commit and pre-push hooks
**Reason:** Prevents committing unformatted/broken code
**Files:** `lefthook.yml`, update `package.json` scripts
**Pattern:** [lefthook-config](../patterns/gander-templates/lefthook-config/)

### [2026-01-09] Add Renovate Dependency Management
**Priority:** High
**Category:** Security
**Status:** Proposed

**Current:** Manual dependency updates
**Proposed:** Add `renovate.json` configuration
**Reason:** Automated security patches, dependency updates
**Files:** `renovate.json`
**Pattern:** [renovate-config](../patterns/gander-templates/renovate-config/)

### [2026-01-09] Review TypeScript Configuration
**Priority:** Medium
**Category:** Config
**Status:** Proposed

**Current:** Has tsconfig.json
**Proposed:** Verify alignment with tsconfig-base pattern
**Reason:** Consistency, strict type checking
**Files:** `tsconfig.json`
**Pattern:** [tsconfig-base](../patterns/gander-templates/tsconfig-base/)

### [2026-01-09] Review Package.json Structure
**Priority:** Medium
**Category:** Config
**Status:** Proposed

**Current:** Has package.json
**Proposed:** Verify modern structure (exports, engines, metadata)
**Reason:** Better compatibility, security
**Files:** `package.json`
**Pattern:** [package-json-structure](../patterns/gander-templates/package-json-structure/)

## Workflow Improvements

### [2026-01-09] Add Release Please Workflow
**Priority:** Medium
**Category:** Workflow
**Status:** Proposed

**Current:** No automated release process
**Proposed:** Add Release Please workflow
**Reason:** Semantic versioning, automatic CHANGELOG
**Files:** `.github/workflows/release-please.yml`, `release-please-config.json`
**Pattern:** [release-please-workflow](../patterns/gander-templates/release-please-workflow/)
**Note:** Evaluate if calculator needs formal releases or if deploy.yml is sufficient

### [2026-01-09] Add Auto PR Workflow
**Priority:** Medium
**Category:** Workflow
**Status:** Proposed

**Current:** Manual PR creation
**Proposed:** Add auto-pr workflow for claude/* branches
**Reason:** Streamlines Claude Code workflow
**Files:** `.github/workflows/auto-pr.yml`
**Pattern:** [auto-pr-workflow](../patterns/gander-templates/auto-pr-workflow/)

### [2026-01-09] Add Labeler Workflow
**Priority:** Low
**Category:** Workflow
**Status:** Proposed

**Current:** Manual labeling
**Proposed:** Add automatic PR/Issue labeling
**Reason:** Better organization
**Files:** `.github/workflows/labeler.yml`, `.github/labeler.yml`
**Pattern:** [labeler-workflow](../patterns/gander-templates/labeler-workflow/)

### [2026-01-09] Add Dependency Review Workflow
**Priority:** High
**Category:** Security
**Status:** Proposed

**Current:** Only deploy.yml exists, no security scanning
**Proposed:** Add dependency-review workflow
**Reason:** Security vulnerability detection
**Files:** `.github/workflows/dependency-review.yml`
**Pattern:** [dependency-review-workflow](../patterns/gander-templates/dependency-review-workflow/)

### [2026-01-09] Add CI Test Workflow
**Priority:** High
**Category:** Workflow
**Status:** Proposed

**Current:** No CI testing detected
**Proposed:** Add test workflow
**Reason:** Automated testing before deployment
**Files:** `.github/workflows/test.yml`
**Note:** May need to add tests first if none exist

## Security Improvements

### [2026-01-09] Security Scanning
**Priority:** High
**Category:** Security
**Status:** Proposed

**Current:** No security scanning
**Proposed:** Add dependency-review workflow
**Reason:** Catch vulnerabilities before deployment
**Files:** `.github/workflows/dependency-review.yml`

## Anti-Pattern Deprecation

### [2026-01-09] Remove ESLint + Prettier
**Priority:** High
**Category:** Deprecation
**Status:** Proposed

**Current:** Likely using ESLint + Prettier (assumption)
**Proposed:** Migrate to Biome
**Reason:** Biome is faster, simpler, and official replacement
**Files:** Remove `.eslintrc.*`, `.prettierrc.*`, eslint/prettier packages
**Migration:** Run `npm uninstall eslint prettier`, add Biome, format codebase

## Implementation Priority

**Phase 1 (Essential):**
1. Add `biome.json` (replace ESLint/Prettier)
2. Add `lefthook.yml`
3. Add `renovate.json`
4. Add `dependency-review` workflow

**Phase 2 (Quality):**
5. Add test workflow (may require adding tests)
6. Review `tsconfig.json` alignment
7. Review `package.json` structure

**Phase 3 (Nice-to-Have):**
8. Add `release-please` workflow (if versioning needed)
9. Add `auto-pr` workflow
10. Add `labeler` workflow

## Migration Notes

- **ESLint/Prettier Migration:** Run Biome check after migration, commit formatting changes separately
- **Deploy Workflow:** Keep existing deploy.yml, add quality checks before deployment
- **Testing:** May need to add test infrastructure before test workflow
- Consider gradual rollout to minimize disruption

## Estimated Impact

- **Code Quality:** Major improvement with Biome and Git hooks
- **Security:** Significant improvement with dependency scanning and Renovate
- **Maintenance:** Reduced overhead with automated tooling
- **Deployment:** More reliable with CI checks before deploy
