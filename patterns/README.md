# Gander Patterns Library

This directory contains reusable configuration patterns, workflows, and best practices extracted from Gander projects.

## Pattern Organization

### By Organization

- **[gander-templates/](gander-templates/)** - Patterns from template repositories (source of truth)
- **gander-tools/** - Patterns from tool repositories (coming soon)
- **external/** - External inspirations and references (coming soon)

## Available Patterns

### Core Tooling Patterns

| Pattern | Adoption | Description |
|---------|----------|-------------|
| [biome-config](gander-templates/biome-config/) | 4/6 (67%) | BiomeJS configuration for formatting and linting |
| [tsconfig-base](gander-templates/tsconfig-base/) | 6/6 (100%) | TypeScript compiler configuration |
| [lefthook-config](gander-templates/lefthook-config/) | 3/6 (50%) | Git hooks automation with Lefthook |

### Dependency Management Patterns

| Pattern | Adoption | Description |
|---------|----------|-------------|
| [renovate-config](gander-templates/renovate-config/) | 3/6 (50%) | Automated dependency updates with Renovate |
| [package-json-structure](gander-templates/package-json-structure/) | 6/6 (100%) | Modern package.json structure and metadata |

### CI/CD Workflow Patterns

| Pattern | Adoption | Description |
|---------|----------|-------------|
| [release-please-workflow](gander-templates/release-please-workflow/) | 3/6 (50%) | Automated release management |
| [auto-pr-workflow](gander-templates/auto-pr-workflow/) | 3/6 (50%) | Automatic PR creation for Claude branches |
| [labeler-workflow](gander-templates/labeler-workflow/) | 3/6 (50%) | Automatic PR/Issue labeling |
| [dependency-review-workflow](gander-templates/dependency-review-workflow/) | 3/6 (50%) | Dependency security scanning |

## Pattern Maturity Levels

### Stable (≥3 projects)
Patterns adopted by 3+ projects are considered stable and ready for production use:
- ✅ All 9 documented patterns meet stability threshold

### Experimental (2 projects)
Patterns used in 2 projects, marked experimental until 3rd adoption.
- Currently none in this category

### Proposed (1 project)
Patterns found in 1 project, not yet documented as patterns.

## How to Adopt Patterns

### 1. Review Pattern Documentation

Each pattern includes:
- **README.md** - Overview, context, configuration
- **CHANGELOG.md** - Pattern evolution and breaking changes
- **suggestions.md** - Customization guidance
- **files/** - Reference configuration files

### 2. Check Pattern Dependencies

Some patterns require others:
- `lefthook-config` → Requires `biome-config` for format/lint commands
- `release-please-workflow` → Recommends `renovate-config` for semantic commits
- `auto-pr-workflow` → Works with `labeler-workflow`

### 3. Start with Core Patterns

Recommended adoption order:

**Phase 1: Foundation**
1. `tsconfig-base` - TypeScript configuration
2. `biome-config` - Code quality tooling
3. `package-json-structure` - Proper metadata

**Phase 2: Automation**
4. `renovate-config` - Dependency automation
5. `lefthook-config` - Git hooks
6. `dependency-review-workflow` - Security scanning

**Phase 3: Release Management**
7. `release-please-workflow` - Automated releases
8. `auto-pr-workflow` - PR automation
9. `labeler-workflow` - Organization

### 4. Customize for Your Project

Patterns are starting points, not rigid rules:
- Choose appropriate variant (strict vs. minimal)
- Adjust for project type (library vs. application)
- Add project-specific customizations

## Pattern Variations

Many patterns have multiple variants:

### biome-config
- **Strict** - 100+ explicit rules (libraries)
- **Minimal** - Recommended rules only (applications)

### release-please-workflow
- **Simple** - Basic workflow (templates)
- **Production** - Full security features (production packages)

### package-json-structure
- **Library** - Dual format exports (ESM + CJS)
- **CLI Tool** - Binary executable
- **Application** - Private package

See individual pattern documentation for variant details.

## Contributing Patterns

Patterns are documented when they meet criteria:
- ✅ Used in ≥3 projects (≥2 for experimental)
- ✅ Stable for ≥3 months
- ✅ Solves general problem
- ✅ Not project-specific

See [CLAUDE.md](../CLAUDE.md) for full contribution guidelines.

## Pattern Updates

Patterns evolve over time. Check CHANGELOG.md for:
- **Last Updated** date
- **Breaking Changes** section
- **Migration** guides

Subscribe to repository for pattern update notifications.

## Related Documentation

- [Improvements](../improvements/) - Per-repository improvement proposals
- [Templates](../templates/) - Pre-configured project templates (coming soon)
- [Migrations](../migrations/) - Breaking change migration guides (coming soon)
- [CLAUDE.md](../CLAUDE.md) - Repository guidance and workflows

## Quick Reference

### Finding a Pattern

**By Technology:**
- TypeScript → `tsconfig-base`, `package-json-structure`
- Code Quality → `biome-config`, `lefthook-config`
- Dependencies → `renovate-config`, `dependency-review-workflow`
- Releases → `release-please-workflow`
- CI/CD → All workflow patterns

**By Use Case:**
- Starting new project → Start with Phase 1 patterns
- Improving code quality → `biome-config` + `lefthook-config`
- Automating releases → `release-please-workflow`
- Security → `dependency-review-workflow` + `renovate-config`

## Statistics (as of 2026-01-09)

- **Total Patterns:** 9
- **Average Adoption:** 60% (3.6/6 projects)
- **100% Adoption:** 2 patterns (tsconfig-base, package-json-structure)
- **Organizations:** 2 (gander-templates, gander-tools)
- **Analyzed Repositories:** 6

## Pattern Evolution

Patterns are **living documentation** that evolve with:
- Technology updates (new Node.js versions, tool updates)
- Best practice changes (security improvements, performance)
- Community feedback (adoption experience, issues)

Patterns are versioned through CHANGELOG.md, not semantic versioning.
