# Improvement Proposals

This directory contains per-repository improvement proposals based on pattern analysis. Each proposal identifies gaps, anti-patterns, and opportunities for modernization.

## Repository Proposals

### gander-tools

| Repository | Priority | Status | Key Issues |
|------------|----------|--------|------------|
| [osm-helper](gander-tools/osm-helper.md) | **HIGH** | Proposed | No .github directory, missing all patterns |
| [nms-frigate-calc](gander-tools/nms-frigate-calc.md) | **HIGH** | Proposed | Minimal CI/CD, likely ESLint+Prettier, missing patterns |
| [nms-lib](gander-tools/nms-lib.md) | **MEDIUM** | Proposed | Missing Renovate, Release Please |
| [osm-tagging-schema-mcp](gander-tools/osm-tagging-schema-mcp.md) | **MEDIUM** | Proposed | Workflow consolidation opportunities |
| [diff-voyager](gander-tools/diff-voyager.md) | **LOW** | Proposed | Missing Lefthook only |

### gander-templates

| Repository | Priority | Status | Key Issues |
|------------|----------|--------|------------|
| [node-project-starter](gander-templates/node-project-starter.md) | **LOW** | Proposed | Node.js version modernization, documentation |

## Priority Levels

### HIGH Priority
**Critical gaps affecting security, quality, or maintainability**

- Missing GitHub Actions infrastructure
- No security scanning
- No automated tooling (linting, formatting)
- Anti-patterns present (ESLint+Prettier)

**Action:** Immediate attention recommended

### MEDIUM Priority
**Important improvements for automation and consistency**

- Missing automated dependency management
- Missing release automation
- Partial pattern adoption
- Workflow optimization opportunities

**Action:** Plan for next iteration

### LOW Priority
**Nice-to-have improvements for consistency**

- Minor missing patterns
- Documentation improvements
- Template modernization
- Non-critical optimizations

**Action:** Consider when convenient

## Improvement Categories

### Configuration
- Adding or updating configuration files
- Pattern adoption
- Tooling setup

### Workflow
- GitHub Actions workflows
- CI/CD pipeline improvements
- Automation additions

### Security
- Dependency scanning
- Vulnerability detection
- Supply chain security

### Performance
- CI/CD optimization
- Build improvements
- Caching strategies

### Deprecation
- Removing anti-patterns
- Modernizing old approaches
- Migration to new tools

## How to Use Improvement Proposals

### 1. Review Your Repository's Proposal

Navigate to your repository's improvement file:
- `improvements/gander-tools/{repo-name}.md`
- `improvements/gander-templates/{repo-name}.md`

### 2. Understand Priority Levels

Focus on HIGH priority items first:
- Security vulnerabilities
- Missing critical infrastructure
- Anti-patterns affecting quality

### 3. Follow Implementation Priority

Each proposal includes phased implementation:

**Phase 1: Essential**
- Critical fixes
- Foundation patterns
- Security basics

**Phase 2: Automation**
- CI/CD workflows
- Dependency management
- Git hooks

**Phase 3: Nice-to-Have**
- Additional workflows
- Documentation
- Optimizations

### 4. Check Pattern Documentation

For each improvement:
1. Read referenced pattern documentation in [patterns/](../patterns/)
2. Review pattern CHANGELOG for breaking changes
3. Check pattern suggestions for customization options
4. Copy reference files from pattern's `files/` directory

### 5. Test Before Deploying

- Start with feature branch
- Test pattern adoption locally
- Verify CI/CD passes
- Get team feedback

### 6. Gradual Rollout

Don't adopt all patterns at once:
- Week 1: Core tooling (Biome, TypeScript)
- Week 2: Git hooks (Lefthook)
- Week 3: CI/CD workflows
- Week 4: Dependency automation

## Common Improvement Paths

### Path A: No Patterns → Full Adoption

**Repositories:** osm-helper, nms-frigate-calc

**Steps:**
1. Add `.github/` directory structure
2. Add `biome.json` + `tsconfig.json`
3. Update `package.json` structure
4. Add `dependency-review` workflow
5. Add `renovate.json`
6. Add `lefthook.yml`
7. Add `release-please` workflow
8. Add supporting workflows

**Timeline:** 2-4 weeks

### Path B: Partial Adoption → Complete

**Repositories:** nms-lib, osm-tagging-schema-mcp

**Steps:**
1. Add missing core patterns (Renovate, Release Please)
2. Add security workflows
3. Optimize existing workflows
4. Align configurations with patterns

**Timeline:** 1-2 weeks

### Path C: Maintenance & Optimization

**Repositories:** diff-voyager, node-project-starter

**Steps:**
1. Add remaining minor patterns
2. Documentation improvements
3. Configuration alignment
4. Version modernization

**Timeline:** Few days

## Anti-Patterns to Address

### ESLint + Prettier → Biome
**Affected:** nms-frigate-calc (suspected), possibly others

**Migration:**
1. Install Biome: `npm install --save-dev @biomejs/biome`
2. Remove ESLint/Prettier: `npm uninstall eslint prettier`
3. Add `biome.json` configuration
4. Run initial format: `npx biome check --write .`
5. Update scripts and Git hooks

### Node.js < 20 → 22+
**Affected:** Check all repositories

**Migration:**
1. Update `package.json` engines field
2. Update GitHub Actions workflows
3. Update `.nvmrc` if present
4. Test locally with new Node version

### Manual Dependencies → Renovate
**Affected:** osm-helper, nms-frigate-calc, nms-lib, osm-helper

**Migration:**
1. Add `renovate.json` configuration
2. Enable Renovate GitHub App
3. Review and merge first Renovate PRs
4. Configure auto-merge rules

## Tracking Progress

### Update Improvement Proposals

As improvements are implemented:
1. Change status from "Proposed" to "Implemented"
2. Add implementation date
3. Note any deviations from plan
4. Document lessons learned

### Adoption Metrics

Track pattern adoption across repositories:
- Update pattern README adoption counts
- Update this index with implementation status
- Celebrate milestones (50%, 75%, 100% adoption)

## Contributing

### Report Issues with Proposals

If an improvement proposal has errors:
1. Open issue in patterns-blueprints repository
2. Reference specific proposal file
3. Suggest correction

### Propose New Improvements

When discovering new opportunities:
1. Follow format in existing proposals
2. Categorize by priority
3. Reference applicable patterns
4. Submit PR to patterns-blueprints

## Related Documentation

- [Patterns](../patterns/) - Pattern documentation and reference configs
- [CLAUDE.md](../CLAUDE.md) - Repository guidance and workflows
- [Templates](../templates/) - Pre-configured project templates (coming soon)

## Statistics (as of 2026-01-09)

- **Total Repositories Analyzed:** 6
- **HIGH Priority:** 2 repositories (33%)
- **MEDIUM Priority:** 2 repositories (33%)
- **LOW Priority:** 2 repositories (33%)
- **Total Proposed Improvements:** ~50 across all repositories
- **Average Improvements per Repo:** ~8

## Quick Reference

### Find Your Repository

**gander-tools:**
- [osm-helper](gander-tools/osm-helper.md) - HIGH
- [nms-frigate-calc](gander-tools/nms-frigate-calc.md) - HIGH
- [nms-lib](gander-tools/nms-lib.md) - MEDIUM
- [osm-tagging-schema-mcp](gander-tools/osm-tagging-schema-mcp.md) - MEDIUM
- [diff-voyager](gander-tools/diff-voyager.md) - LOW

**gander-templates:**
- [node-project-starter](gander-templates/node-project-starter.md) - LOW

### By Improvement Type

**Security Focus:**
- osm-helper - Add all security patterns
- nms-frigate-calc - Add dependency scanning

**Automation Focus:**
- nms-lib - Add Renovate + Release Please
- All repositories - Review automation gaps

**Modernization Focus:**
- node-project-starter - Node.js version strategy
- nms-frigate-calc - Replace ESLint+Prettier with Biome
