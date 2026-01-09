# Changelog - Biome Configuration Pattern

All notable changes to this pattern will be documented in this file.

## [2026-01-09] - Initial Pattern Documentation

**Added:** Initial biome-config pattern documentation based on analysis of 4 projects
**Reason:** Establish Biome as standard tooling replacement for ESLint + Prettier
**Impact:** 4 projects already using pattern, 2 projects could adopt (nms-frigate-calc, osm-helper)
**Migration:** See [Pattern Migration Guide](../../migrations/eslint-to-biome.md) (to be created)

### Pattern Variations Identified

**Strict Variant:**
- Used in: node-project-starter
- 100+ explicit lint rules
- Maximum code quality enforcement
- Target: Public libraries and packages

**Minimal Variant:**
- Used in: osm-tagging-schema-mcp, diff-voyager
- Recommended rules + minimal overrides
- Faster iteration, less restrictive
- Target: Internal tools and applications

**Library Minimal Variant:**
- Used in: nms-lib
- Strict formatting + minimal linting
- Focus on consistency
- Target: Established libraries

### Configuration Commonalities

All variants share:
- Tab indentation (2-space width)
- 100-character line width
- Double quotes for strings
- Always semicolons
- Trailing commas everywhere
- VCS integration enabled
- Organize imports enabled

### Breaking Changes

None - This is the initial pattern documentation.

### Notes

- Pattern is stable and battle-tested across 4 production projects
- No significant configuration drift detected
- All projects use BiomeJS 2.x (2.3.8 - 2.3.11)
