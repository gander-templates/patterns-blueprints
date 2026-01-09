# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is a living knowledge base of patterns, configurations, and best practices from all Gander projects. Your role is to analyze projects, document patterns, propose improvements, and suggest templates.

## Core Principles

1. **Manual Process**: All analysis happens during Claude Code sessions. No automated background jobs.
2. **Privacy First**: NEVER store source information about private repositories in documentation.
3. **Evidence-Based**: Document only patterns used in ≥2 projects (unless explicitly marked as experimental).
4. **Living Documentation**: Patterns evolve. Always check dates and propose updates.

## Repository Structure

```
patterns-blueprints/
├── patterns/               # Atomic building blocks
│   ├── gander-templates/
│   ├── gander-actions/
│   ├── gander-tools/
│   ├── gander-mcp/
│   └── external/          # External inspirations
├── templates/             # Pre-configured stacks
│   ├── typescript-npm-library/
│   ├── typescript-cli-tool/
│   └── github-action/
├── improvements/          # Per-repo suggestions
│   ├── gander-templates/
│   ├── gander-actions/
│   └── gander-tools/
├── migrations/            # Breaking change guides
└── docs/                  # Additional documentation
```

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
- `.c8rc.json`
- `.dockerignore`
- `.gitignore`
- `.npmrc`
- `.nvmrc`
- `biome.json`
- `lefthook.yml`
- `package.json` (scripts, engines, dependencies structure)
- `release-please-config.json`
- `renovate.json`
- `tsconfig.json`
- `LICENSE`
- `README.md`
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
  README.md
  CHANGELOG.md
  suggestions.md
  files/
```

In README.md, use this template:

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

```markdown
# Improvements for {repo-name}

Last updated: YYYY-MM-DD

## Configuration Improvements

### [YYYY-MM-DD] Title
**Priority:** High | Medium | Low
**Category:** Config | Workflow | Security | Performance
**Status:** Proposed | Accepted | Rejected | Implemented

Current: [description]
Proposed: [description]
Reason: [justification]
Files: [affected files]

## Workflow Improvements

## Security Improvements

## Performance Improvements
```

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
4. Adding date stamp: `(as of YYYY-MM-DD)`

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

## License & Privacy

- **Documentation License**: This repository's documentation is proprietary (see LICENSE.md)
- **Referenced Projects**: Have their own licenses (typically MIT)
- **Private Repos**: Never document source code, only configuration patterns
- **Security**: Zero tolerance for secrets in documentation
