# Changelog - TypeScript Base Configuration Pattern

All notable changes to this pattern will be documented in this file.

## [2026-01-09] - Initial Pattern Documentation

**Added:** Initial tsconfig-base pattern documentation based on analysis of 6 projects
**Reason:** Establish consistent TypeScript configuration across all projects
**Impact:** 6 projects (100% of TypeScript projects) already following this pattern
**Migration:** No migration needed - pattern documents existing consensus

### Configuration Consensus

All 6 projects share:
- Strict type checking enabled (`strict: true`)
- ES2022 target and library
- Source maps and declarations enabled
- `noUncheckedIndexedAccess: true` for safer code
- `skipLibCheck: true` for performance

### Pattern Variations Identified

**Library/Bundler Variant:**
- Used in: node-project-starter, nms-lib
- `module: "ESNext"`, `moduleResolution: "bundler"`
- Target: npm packages consumed by bundlers

**Node.js Variant:**
- Used in: osm-tagging-schema-mcp, diff-voyager, nms-frigate-calc, osm-helper
- `module: "NodeNext"` or `"ESNext"`
- Target: CLI tools, Node.js applications

### Key Decisions

**Why ES2022:**
- Balances modern features with broad Node.js support
- Node.js 18+ fully supports ES2022
- Avoids unnecessary polyfills

**Why `noUncheckedIndexedAccess`:**
- Prevents common runtime errors
- Forces explicit null/undefined handling
- Small verbosity cost for significant safety gain

**Why `bundler` resolution:**
- Modern standard for library development
- Better tree-shaking support
- Matches expectations of modern bundlers

### Breaking Changes

None - This is the initial pattern documentation.

### Notes

- Pattern is mature and stable
- No significant drift across projects
- TypeScript 5.x consistently used (5.7.3 - 5.9.3)
- All projects prioritize type safety over convenience
