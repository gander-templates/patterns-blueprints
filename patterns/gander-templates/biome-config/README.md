# Pattern: Biome Configuration

## Overview

BiomeJS is a fast, all-in-one toolchain for formatting, linting, and organizing JavaScript/TypeScript code. This pattern provides standardized Biome configurations used across Gander projects, replacing ESLint + Prettier with a single, performant tool.

## Context

Use Biome when:
- Starting a new TypeScript/JavaScript project
- Migrating from ESLint + Prettier to reduce tooling complexity
- Need fast, consistent formatting and linting
- Want VCS integration (Git-aware formatting)

Do NOT use Biome when:
- Project requires ESLint plugins not yet supported by Biome
- Team has strong preference for existing ESLint rules

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager), [nms-lib](https://github.com/gander-tools/nms-lib) | Total: 4 projects

## Dependencies

### Required
- `@biomejs/biome` package (npm install --save-dev @biomejs/biome)
- Node.js 18+ (for Biome CLI)

### Recommended
- [lefthook-config](../lefthook-config/) - Git hooks for automatic formatting
- Pre-commit hook running `biome check --write`

## Configuration

### Core Settings

**Formatter:**
- Indentation: Tabs (2-space width)
- Line width: 100 characters
- Line ending: LF (Unix-style)

**JavaScript/TypeScript:**
- Quote style: Double quotes
- Semicolons: Always
- Trailing commas: All
- Arrow parentheses: Always
- Bracket spacing: True

**VCS Integration:**
- Enabled with Git
- Uses `.gitignore` for file exclusion
- Default branch: `main`

**Files:**
- Includes: `**/*.ts`, `**/*.js`, `**/*.json`
- Excludes: `node_modules`, `dist`, `build`, `coverage`, `.git`, `*.min.js`

**Assist:**
- Organize imports: Enabled (automatically sorts imports)

## Pattern Variations

### Variation A (Strict) - node-project-starter

**Purpose:** Library/package development with high quality standards

**Additional rules:**
- 100+ explicit lint rules covering complexity, correctness, style, suspicious
- All rules set to "error" severity
- Strictest TypeScript checking

**When to use:**
- Public npm packages
- Libraries intended for reuse
- Projects requiring maximum code quality

**Reference:** `files/biome.json`

### Variation B (Minimal) - osm-tagging-schema-mcp, diff-voyager

**Purpose:** Internal tools and applications with faster iteration

**Configuration:**
- Uses `"recommended": true` base rules
- Only 2-3 custom overrides
- Focuses on critical issues

**When to use:**
- Internal tools
- Rapid prototyping
- Applications (not libraries)
- Teams preferring less restrictive linting

**Reference:** `files/biome-minimal.json`

### Variation C (Library with minimal lint) - nms-lib

**Purpose:** Library with minimal linting but strict formatting

**Configuration:**
- Same formatter settings as other variants
- Minimal linter rules (only recommended)
- Focus on consistency over strictness

**When to use:**
- Libraries with established patterns
- Projects migrating from ESLint
- Teams wanting gradual adoption

## Known Issues

1. **Migration from ESLint**
   - Not all ESLint plugins have Biome equivalents
   - Some custom rules may need rewriting
   - **Workaround:** Run Biome alongside ESLint during migration

2. **IDE Integration**
   - May conflict with existing Prettier/ESLint extensions
   - **Workaround:** Disable Prettier and ESLint extensions when using Biome

3. **JSON Formatting**
   - JSON files use space indentation (2 spaces) regardless of main config
   - **Limitation:** Biome enforces this for JSON spec compliance

## Limitations

- Does not support all ESLint plugins
- Cannot customize indentation per file type (except JSON)
- No support for `.prettierignore` (use `.gitignore` or Biome's `files.ignore`)

## Integration

### With Lefthook (Recommended)

```yaml
pre-commit:
  parallel: true
  commands:
    format:
      glob: "*.{ts,js,json}"
      run: npx biome check --write {staged_files}
      stage_fixed: true
```

### Package.json Scripts

```json
{
  "scripts": {
    "check": "biome check .",
    "format": "biome format --write .",
    "lint": "biome lint ."
  }
}
```

### VS Code Integration

Install extension: `biomejs.biome`

Add to `.vscode/settings.json`:
```json
{
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnSave": true
}
```

## References

- [BiomeJS Official Documentation](https://biomejs.dev/)
- [BiomeJS vs ESLint/Prettier Comparison](https://biomejs.dev/blog/biome-v1/)
- [Migration Guide from ESLint](https://biomejs.dev/guides/migrate-eslint-prettier/)
- Related patterns: [lefthook-config](../lefthook-config/), [package-json-structure](../package-json-structure/)
