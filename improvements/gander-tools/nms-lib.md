# Improvements for nms-lib

Last updated: 2026-01-09

## Summary

**Priority: MEDIUM** - Repository has partial pattern adoption (Biome, Lefthook) but missing automated dependency management and release workflows. Inconsistent tooling (has Lefthook but no Biome in some areas).

## Configuration Improvements

### [2026-01-09] Add Renovate Dependency Management
**Priority:** High
**Category:** Config
**Status:** Proposed

**Current:** Manual dependency updates
**Proposed:** Add `renovate.json` configuration
**Reason:** Automated security patches and dependency updates
**Files:** `renovate.json`
**Pattern:** [renovate-config](../patterns/gander-templates/renovate-config/)

### [2026-01-09] Review Biome Configuration
**Priority:** Medium
**Category:** Config
**Status:** Proposed

**Current:** Has Biome, but verify completeness
**Proposed:** Ensure Biome configuration aligns with pattern
**Reason:** Consistency across projects
**Files:** `biome.json`
**Pattern:** [biome-config](../patterns/gander-templates/biome-config/)

## Workflow Improvements

### [2026-01-09] Add Release Please Workflow
**Priority:** High
**Category:** Workflow
**Status:** Proposed

**Current:** Has publish.yml and pkg-pr-new.yml, but no automated release management
**Proposed:** Add Release Please workflow for automated releases
**Reason:** Semantic versioning, automatic CHANGELOG, consistent with other libraries
**Files:** `.github/workflows/release-please.yml`, `release-please-config.json`
**Pattern:** [release-please-workflow](../patterns/gander-templates/release-please-workflow/)
**Note:** Review existing publish workflow for integration

### [2026-01-09] Add Auto PR Workflow
**Priority:** Low
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
**Priority:** Medium
**Category:** Security
**Status:** Proposed

**Current:** No dependency security scanning detected
**Proposed:** Add dependency-review workflow
**Reason:** Security vulnerability detection before merge
**Files:** `.github/workflows/dependency-review.yml`
**Pattern:** [dependency-review-workflow](../patterns/gander-templates/dependency-review-workflow/)

## Implementation Priority

**Phase 1 (Essential):**
1. Add `renovate.json` - Highest priority for security
2. Add `release-please` workflow - Automate version management
3. Add `dependency-review` workflow - Security scanning

**Phase 2 (Quality):**
4. Review `biome.json` alignment with pattern
5. Review `lefthook.yml` alignment with pattern

**Phase 3 (Nice-to-Have):**
6. Add `auto-pr` workflow
7. Add `labeler` workflow

## Migration Notes

- Library already has good foundation (Biome, Lefthook)
- Focus on adding missing automation (Renovate, Release Please)
- Verify existing publish.yml integrates well with Release Please
- Consider consolidating publish workflows

## Estimated Impact

- **Security:** Significant improvement with Renovate and dependency-review
- **Release Management:** Major improvement with Release Please automation
- **Maintenance:** Reduced manual work for versioning and publishing
- **Consistency:** Better alignment with other gander projects
