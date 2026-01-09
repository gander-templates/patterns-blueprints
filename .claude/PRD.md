# Product Requirements Document: patterns-blueprints

## Executive Summary

The `patterns-blueprints` project is a centralized knowledge base repository within the `gander-templates` organization containing patterns, configurations, and best practices from all Gander projects. Its purpose is to document proven solutions, propose improvements, and facilitate the creation of new projects through ready-made templates.

## Problem Statement

Currently, technical knowledge is scattered across multiple repositories in different organizations (gander-actions, gander-tools, gander-mcp, etc.). There is no centralized location:
- Makes it difficult to find proven patterns
- Causes code and configuration duplication
- Prevents systematic solution improvements
- Complicates bootstrapping of new projects

## Goals & Success Metrics

### Primary Goals
1. Create centralized repository of patterns and configurations
2. Automate pattern discovery and documentation
3. Systematically propose improvements for existing projects
4. Facilitate new project creation through templates

### Success Metrics
- All patterns from ≥3 projects are documented
- Minimum 5 ready templates for common use cases
- Claude Code can find patterns for new projects in <5 minutes
- Improvement proposals generated during each session

## Target Users

- **Primary**: You (as maintainer of all Gander projects)
- **Secondary**: Claude Code (as AI assistant analyzing and proposing changes)
- **Future**: Potential contributors to open-source projects

## Technical Requirements

### Repository Structure

```
patterns-blueprints/
├── README.md                          # Overview, Quick Start, Decision Matrix
├── CLAUDE.md                          # Instructions for Claude Code
├── LICENSE                            # MIT
├── .github/
│   └── workflows/                     # (minimal - no CI/CD for docs)
├── patterns/                          # Atomic building blocks
│   ├── gander-templates/
│   │   ├── typescript-base/
│   │   │   ├── README.md             # Description, usage, adoption
│   │   │   ├── CHANGELOG.md          # Evolution history
│   │   │   ├── suggestions.md        # Improvement proposals
│   │   │   └── files/
│   │   │       ├── tsconfig.json
│   │   │       ├── package.json
│   │   │       └── ...
│   │   ├── biome-config/
│   │   ├── renovate-setup/
│   │   └── ...
│   ├── gander-actions/
│   ├── gander-tools/
│   ├── gander-mcp/
│   └── external/                      # (optional) External inspirations
│       └── vercel/
├── templates/                         # Pre-configured stacks
│   ├── typescript-npm-library/
│   │   ├── README.md                 # What, when to use, how to customize
│   │   └── patterns.md               # List of included patterns
│   ├── typescript-cli-tool/
│   ├── github-action/
│   └── api-server/
├── improvements/                      # Per-repo suggestions
│   ├── gander-templates/
│   │   ├── node-project-starter.md
│   │   └── ...
│   ├── gander-actions/
│   ├── gander-tools/
│   └── ...
├── migrations/                        # Breaking change guides
│   ├── biome-config/
│   │   └── v1-to-v2.md
│   └── ...
└── docs/
    ├── onboarding-example.md         # Real scenario: node-project-starter
    └── ...
```

### Pattern Documentation Structure

**README.md** template:
```markdown
# Pattern: {Name}

## Overview
Brief description of what this pattern solves.

## Context
When to use this pattern vs alternatives.

## Adoption (as of YYYY-MM-DD)
Used in: [repo1](url), [repo2](url) | Total: X projects

## Dependencies
### Required
- [Pattern A]
- [Pattern B]

### Recommended
- [Pattern C]

## Configuration
Key customization points and options.

## Pattern Variations
### Variation A (strict)
Used in: libraries
Reason: Higher quality standards

### Variation B (loose)
Used in: internal tools
Reason: Faster iteration

## Known Issues
- Issue description with workaround

## Limitations
- What this pattern doesn't support

## References
- [External docs]
- [Related patterns]
```

**CHANGELOG.md** format:
```markdown
## [2026-01-09] - Security improvement
Changed: Updated Node version to 22
Reason: Security patches and performance
Impact: 5 projects affected
Migration: Update .nvmrc and CI workflows
```

**suggestions.md** format:
```markdown
## Priority: High | Medium | Low
## Category: Config | Workflow | Security | Performance
## Date: 2026-01-09
## Status: Proposed | Accepted | Rejected | Implemented

### Current State
Description of current implementation

### Proposed Change
What should be changed and why

### Affected Projects
- project1
- project2

### Implementation Guide
Step-by-step how to apply this change

### Breaking Changes
(if any)

### Effort: Simple | Medium | Complex
### Risk: Low | Medium | High
```

### Improvements Documentation

**improvements/{org}/{repo}.md** format:
```markdown
# Improvements for {repo-name}

Last updated: 2026-01-09

## Configuration Improvements

### [2026-01-09] Update Node.js version
**Priority:** High
**Category:** Config
**Status:** Proposed

Current: Node 20
Proposed: Node 22
Reason: Security + performance
Files: .nvmrc, package.json engines

### [2026-01-05] Modernize Biome config
...

## Workflow Improvements

### [2026-01-09] Add SLSA provenance
...

## Security Improvements

## Performance Improvements
```

### CLAUDE.md Content Structure

```markdown
# Claude Code Instructions for patterns-blueprints

## Purpose
This repository is a living knowledge base of patterns, configurations, and best practices from all Gander projects. Your role is to analyze projects, document patterns, propose improvements, and suggest new templates.

## Core Principles

1. **Manual Process**: All analysis happens during Claude Code sessions. No automated background jobs.
2. **Privacy First**: NEVER store source information about private repositories in documentation.
3. **Evidence-Based**: Document only patterns used in ≥2 projects (unless explicitly marked as experimental).
4. **Living Documentation**: Patterns evolve. Always check dates and propose updates.

## Repository Scan Priority

Always analyze in this order:
1. `gander-templates/*` - Source of truth for templates
2. `gander-actions/*`, `gander-tools/*` - Stable, production solutions
3. `gander-apis/*`, `gander-mcp/*` - Newer, specialized patterns
4. Private repos - Only for variation discovery, NEVER document source

## Pattern Discovery Workflow

### Step 1: Identify Candidate Patterns

Scan these files across repositories:
- `/.github/workflows/*.yml`
- `/.github/ISSUE_TEMPLATE/*`
- `/.github/dependabot.yml`
- `/.github/renovate.json`
- `biome.json`
- `tsconfig.json`
- `package.json` (scripts, engines, dependencies structure)
- `renovate.json`
- `release-please-config.json`
- `lefthook.yml`
- `.nvmrc`
- `.dockerignore`
- `.gitignore`
- `LICENSE`
- `SECURITY.md`

### Step 2: Determine Pattern Maturity

Document pattern if:
- ✅ Used in ≥3 projects (≥2 for experimental flag)
- ✅ Stable for ≥3 months without structural changes
- ✅ Not project-specific (no hardcoded names/paths)
- ✅ Solves a general problem

Flag as `## Template Candidate` if:
- Pattern could be extracted to new template
- Would benefit multiple future projects

### Step 3: Document Pattern

Create structure:
```
/patterns/{organization}/{pattern-name}/
  README.md (use template from PRD)
  CHANGELOG.md (initial entry)
  suggestions.md (empty initially)
  files/ (actual config files)
```

In README.md:
- List ALL projects using this pattern with links
- Document variations with context
- Add last updated date
- Include comments in files/ only for non-obvious decisions

### Step 4: Detect Pattern Variations

Classify differences as:
- **Variation**: Structural modification used in ≥2 projects → Document as pattern variant
- **Customization**: Project-specific tweaks in 1 project → Ignore
- **Drift**: Accidental differences from pattern → Flag in improvements/

When multiple variations exist:
- Document all variants with context
- Propose unified version based on newest/best practices
- Explain trade-offs in "Pattern Variations" section

## Improvement Proposal Workflow

### For Each Analyzed Repository

Create/update `/improvements/{org}/{repo-name}.md` with:
1. Group proposals thematically (Config, Workflow, Security, Performance)
2. Use template from PRD
3. Set realistic priorities based on impact vs effort
4. Reference relevant patterns that could be adopted

### Priority Guidelines
- **High**: Security issues, deprecated dependencies, breaking drift from patterns
- **Medium**: Performance improvements, modernization, pattern adoption
- **Low**: Nice-to-haves, optimization, consistency improvements

## New Template Detection

When you find patterns that could become templates:

1. Check if ≥3 projects share similar stack/structure
2. Verify no existing template covers this use case
3. Create proposal in improvements/ with:
   ```markdown
   ## NEW TEMPLATE PROPOSAL: {name}
   
   **Based on projects:** [proj1], [proj2], [proj3]
   **Stack:** TypeScript, Node 22, Bun, Biome
   **Purpose:** {what problem it solves}
   **Included Patterns:**
   - pattern1
   - pattern2
   
   **Justification:**
   Why this deserves dedicated template vs using existing ones
   ```

## Anti-Patterns to Flag

When you encounter these, flag in improvements/ as `## Deprecation Notice`:

- ESLint + Prettier (use Biome instead)
- npm/yarn (prefer Bun)
- Node.js < 20 (use 22+)
- Manual CHANGELOG (use release-please)
- Manual versioning (use release-please)
- Inconsistent GitHub workflow naming
- Missing security scanning
- No provenance/SLSA attestation

## Secret Detection

**CRITICAL SECURITY RULE:**

Before documenting any file:
- Scan for: API keys, tokens, passwords, private URLs, email addresses
- Replace with:
  - `${VAR_NAME}` for environment variables
  - `***` for tokens
  - `example.com` for URLs
  - `user@example.com` for emails
- Add comment: `# Replace with your actual value`

If you detect potential secret, STOP and ERROR. Do not proceed.

## Pattern Update Guidelines

Update "Last Updated" date only when:
- ✅ Files in files/ directory change
- ✅ Major updates to README (changed best practices)
- ✅ Dependencies section modified
- ❌ NOT for: typo fixes, formatting, minor clarifications

Consider update "significant" if it would require action in adopting projects.

## Adoption Tracking

Update adoption metrics by:
1. Scanning all repos for pattern usage
2. Adding/removing repos from "Adoption" section
3. Updating "Total: X projects" count
4. Adding date stamp: `(as of 2026-01-09)`

Format: `Used in: [repo1](full-github-url), [repo2](url) | Total: X projects`

## Template Composition

When creating `/templates/{name}/`:

1. **README.md** must include:
   - What type of project this is for
   - When to choose this vs other templates
   - Required patterns (with links)
   - Optional patterns (with rationale)
   - Customization checklist
   - Post-setup steps

2. **patterns.md** lists:
   ```markdown
   ## Included Patterns
   
   ### Core
   - [typescript-base](../../patterns/gander-templates/typescript-base/)
   - [biome-config](../../patterns/gander-templates/biome-config/)
   
   ### Optional
   - [renovate-setup](../../patterns/gander-templates/renovate-setup/)
   ```

## Decision Matrix Maintenance

Keep `/README.md` Decision Matrix current:

| Project Type | Required Patterns | Optional Patterns |
|--------------|-------------------|-------------------|
| NPM Library | typescript-base, npm-publishing, automated-release | biome-strict, slsa-provenance |
| CLI Tool | typescript-base, bin-executable, npm-publishing | commander-setup |
| GitHub Action | typescript-base, action-metadata, docker-build | renovate-config |
| API Server | typescript-base, express-setup, error-handling | rate-limiting, auth-jwt |

Update when:
- New template created
- Pattern relationships change
- New common use case identified

## Changelog Management

For each pattern, maintain CHANGELOG.md:

```markdown
## [YYYY-MM-DD] - {Reason for change}

**Changed:** {what changed}
**Reason:** {why}
**Impact:** {number} projects affected
**Migration:** {how to update}

### Breaking Changes
- List any breaking changes
- Link to migration guide if complex
```

For major/breaking changes, create:
`/migrations/{pattern-name}/v{old}-to-v{new}.md`

## Comments in Config Files

Add comments ONLY for:
- ✅ Non-obvious decisions (why this specific value)
- ✅ Required vs optional sections
- ✅ Common customization points
- ✅ Links to docs for complex options
- ❌ NOT obvious configurations

Example GOOD:
```json
{
  "compilerOptions": {
    // Using bundler resolution for better Bun compatibility
    "moduleResolution": "bundler",
    
    // Stricter than default - required for library publishing
    "noUncheckedIndexedAccess": true
  }
}
```

Example BAD:
```json
{
  "compilerOptions": {
    // Set target to ES2022
    "target": "ES2022"  // ← obvious, don't comment
  }
}
```

## Session Workflow

During each Claude Code session:

1. **Scan phase** (5-10 min)
   - Scan repos by priority order
   - Detect new patterns
   - Update adoption metrics
   - Identify drifts

2. **Analysis phase** (10-15 min)
   - Classify findings (patterns vs variations vs drift)
   - Check pattern maturity
   - Detect template candidates
   - Identify anti-patterns

3. **Documentation phase** (15-20 min)
   - Update existing patterns
   - Create new pattern docs
   - Generate improvement proposals
   - Update templates/decision matrix

4. **Summary**
   - List changes made
   - Highlight priority improvements
   - Suggest next focus areas

## Output Guidelines

- Use English for all technical documentation
- Use English for code, comments, technical terms
- Keep markdown clean and consistent
- Always include dates in ISO format (YYYY-MM-DD)
- Link generously between related patterns
- Be specific, not generic ("Update to Node 22" not "Update Node")

## Questions to Ask User

If uncertain:
- "Found pattern X in 2 projects, mark as experimental or wait for 3rd adoption?"
- "Detected variation Y, document as separate pattern or merge with existing Z?"
- "Template candidate detected for {use case}, create now or flag for later?"
- "Found anti-pattern in {repo}, priority for fix?"

Never guess - ask for clarification.

## Continuous Improvement

This CLAUDE.md file itself evolves. If you identify:
- Missing workflows/patterns to scan
- Better heuristics for pattern detection
- Improved documentation templates
- Process inefficiencies

Propose updates to this file in `/improvements/patterns-blueprints.md`.
```

### Key Files to Analyze

According to project requirements, Claude Code should analyze:

**Configuration Files:**
- `.c8rc.json`
- `.dockerignore`
- `.gitignore`
- `.npmrc`
- `.nvmrc`
- `.test-status`
- `biome.json`
- `lefthook.yml`
- `package.json`
- `release-please-config.json`
- `renovate.json`
- `server.json`
- `tsconfig.json`

**Documentation:**
- `LICENSE`
- `README.md`
- `SECURITY.md`

**GitHub Specific:**
- Everything in `/.github/` (workflows, templates, configs)

## Implementation Plan

### Phase 1: Repository Setup (Day 1)
1. Create `patterns-blueprints` repo in `gander-templates` organization
2. Prepare base directory structure
3. Create CLAUDE.md, README.md, LICENSE
4. Setup basic .gitignore

### Phase 2: Initial Pattern Documentation (Days 2-3)
1. Start with `gander-templates/node-project-starter`
2. Extract first patterns:
   - typescript-base
   - biome-config
   - renovate-setup
   - github-workflows-base
   - npm-publishing
3. Create first template: `typescript-npm-library`

### Phase 3: Expansion (Week 1-2)
1. Analyze remaining repos from gander-templates
2. Extend with gander-actions patterns
3. Add more templates
4. Create Decision Matrix

### Phase 4: Improvements Generation (Week 2-3)
1. First complete analysis of all projects
2. Generate improvement proposals
3. Validate with real use cases

### Phase 5: Documentation & Onboarding (Week 3-4)
1. Complete onboarding-example.md for node-project-starter
2. Create migration guides for breaking changes
3. Final documentation polish

## Success Criteria

✅ **MVP Complete When:**
- ≥10 documented patterns
- ≥3 ready templates
- Complete CLAUDE.md with workflows
- Onboarding example for node-project-starter
- Decision Matrix for common use cases

✅ **Full Launch When:**
- All active repos analyzed
- Improvement proposals for each repo
- ≥2 template candidates identified
- Zero critical secrets in documentation

## Future Enhancements

**Phase 2 (Post-MVP):**
- External patterns research (Vercel, GitHub, etc.)
- Migration guides for major version bumps
- Pattern relationship visualization
- Template generator CLI tool

**Phase 3 (Long-term):**
- Integration with project generation automation
- AI-powered pattern recommendation
- Cross-project impact analysis
- Pattern adoption metrics dashboard

## Non-Goals

❌ **Explicitly NOT in Scope:**
- Automated CI/CD for this repo (it's documentation only)
- Renovate/Dependabot setup (no code dependencies)
- Automated background scanning (only manual Claude Code sessions)
- Public API for pattern access
- Storing actual private code (only configs from whitelisted files)

## Technical Constraints

- **No secrets**: Absolute prohibition on storing credentials
- **No private code**: Only configurations from whitelisted files
- **No automation**: Only manual Claude Code process
- **Read-only for private repos**: Claude Code can read but NEVER write sources

## Risk Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Secrets leak in docs | Critical | Automated secret scanning in CLAUDE.md |
| Outdated patterns | Medium | Date stamps + regular review sessions |
| Pattern proliferation | Low | Strict maturity criteria (≥3 projects) |
| Documentation drift | Medium | Clear ownership (You + Claude Code) |

## Open Questions

None - all clarified during planning rounds.

---

## Real-World Example: Onboarding with node-project-starter

```markdown
# Real Example: Understanding node-project-starter through patterns-blueprints

## Project Overview
`node-project-starter` is a TypeScript-based NPM library template with:
- Modern TypeScript 5.7+ setup
- BiomeJS for linting/formatting
- Automated releases with release-please
- SLSA Level 3 provenance
- Comprehensive testing setup

## Pattern Breakdown

This project uses the following patterns from patterns-blueprints:

### Core Patterns
1. **[typescript-base](../patterns/gander-templates/typescript-base/)**
   - Files: `tsconfig.json`, `package.json` (TypeScript config)
   - Why: Foundation for all TS projects
   - Customization: Target ES2022, strict mode enabled

2. **[biome-config](../patterns/gander-templates/biome-config/)**
   - Files: `biome.json`
   - Why: Replaces ESLint+Prettier with single tool
   - Customization: Strictness level for libraries

3. **[npm-publishing](../patterns/gander-templates/npm-publishing/)**
   - Files: `package.json` (publishConfig, files), `.npmrc`
   - Why: Proper package distribution
   - Customization: Scoped package @gander-templates

4. **[automated-release](../patterns/gander-templates/automated-release/)**
   - Files: `/.github/workflows/release-please.yml`, `release-please-config.json`
   - Why: Zero-manual versioning and changelogs
   - Customization: Release branches, changelog sections

5. **[renovate-setup](../patterns/gander-templates/renovate-setup/)**
   - Files: `renovate.json`
   - Why: Automated dependency updates
   - Customization: Extends from gander-settings/renovate-config

### Testing & Quality
6. **[node-testing](../patterns/gander-templates/node-testing/)**
   - Files: `package.json` (test scripts), `.c8rc.json`
   - Why: Consistent testing approach
   - Customization: Coverage thresholds

7. **[lefthook-hooks](../patterns/gander-templates/lefthook-hooks/)**
   - Files: `lefthook.yml`
   - Why: Pre-commit quality checks
   - Customization: Hooks for TS, Biome, tests

### Security & Provenance
8. **[slsa-provenance](../patterns/gander-templates/slsa-provenance/)**
   - Files: `/.github/workflows/release-please.yml` (slsa sections)
   - Why: Supply chain security
   - Customization: Level 3 attestation

9. **[security-policy](../patterns/gander-templates/security-policy/)**
   - Files: `SECURITY.md`
   - Why: Responsible disclosure
   - Customization: Contact info, supported versions

### Development Environment
10. **[node-version-pinning](../patterns/gander-templates/node-version-pinning/)**
    - Files: `.nvmrc`, `package.json` (engines)
    - Why: Consistent Node version across environments
    - Customization: Node 22

## How to Use This for New Project

### Step 1: Choose Template
For NPM library → Use `typescript-npm-library` template

### Step 2: Copy Base Files
```bash
cp -r patterns-blueprints/templates/typescript-npm-library/base/* my-new-project/
```

### Step 3: Customize
Follow checklist in template README:
- [ ] Update `package.json`: name, description, repository
- [ ] Update `README.md` badges with your repo name
- [ ] Configure `release-please-config.json` branches
- [ ] Add GitHub secrets if needed
- [ ] Update `LICENSE` author

### Step 4: Apply Optional Patterns
Based on your needs:
- Need CLI? Add `bin-executable` pattern
- Need Docker? Add `docker-build` pattern
- Need API? Add `express-setup` pattern

### Step 5: First Commit
```bash
git init
git add .
git commit -m "chore: initialize project from typescript-npm-library template"
```

## Evolution Tracking

`node-project-starter` was created before patterns-blueprints existed.

**If starting today, changes would include:**
1. Use consolidated `typescript-base` instead of custom config
2. Adopt `github-workflows-standard-names` for consistency
3. Add `semantic-pr-titles` pattern for better changelog
4. Consider `monorepo-support` if expanding

This demonstrates how patterns-blueprints helps standardize and improve future projects.

## Improvement Opportunities

See `/improvements/gander-templates/node-project-starter.md` for:
- Upgrading to newer pattern versions
- Adding missing optional patterns
- Aligning with latest best practices
- Breaking changes requiring migration

---

**Last Updated:** 2026-01-09
**Template Used:** typescript-npm-library
**Pattern Count:** 10 core patterns
**Maturity:** Production-ready reference implementation
```

---

## Final Summary

The `patterns-blueprints` project is a **living documentation repository** that:

✅ **Organizes** patterns from ~10+ Gander repositories in one place  
✅ **Documents** proven solutions with context and history  
✅ **Proposes** improvements for existing projects  
✅ **Facilitates** new project creation through ready templates  
✅ **Evolves** with your projects through Claude Code sessions  

**Key Features:**
- Manual process (Claude Code sessions only)
- Privacy-first (zero secrets, no private code sources)
- Evidence-based (≥3 projects per pattern)
- Template-driven (reusable stacks)
- Self-documenting (dates, adoption, variations)

**First real use case:** `node-project-starter` as reference implementation

## Repository Information

**Name:** patterns-blueprints  
**Organization:** gander-templates  
**License:** MIT  
**Language:** English (technical documentation)  
**Status:** Ready for implementation
