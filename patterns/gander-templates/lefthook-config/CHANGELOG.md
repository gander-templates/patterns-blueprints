# Changelog - Lefthook Configuration Pattern

All notable changes to this pattern will be documented in this file.

## [2026-01-09] - Initial Pattern Documentation

**Added:** Initial lefthook-config pattern documentation based on analysis of 3 projects
**Reason:** Establish automated Git hooks for code quality enforcement
**Impact:** 3 projects using pattern, 3 projects should adopt (HIGH priority)
**Migration:** See [Husky to Lefthook Migration Guide](../../migrations/husky-to-lefthook.md) (to be created)

### Pattern Configuration

All adopting projects use identical configuration:

**Pre-commit hooks:**
- Format with Biome (auto-fix + stage)
- Lint with Biome (auto-fix + stage)
- Validate package-lock.json integrity

**Pre-push hooks:**
- TypeScript type checking
- Run all tests
- Build verification

### Key Benefits

**Speed:**
- Parallel command execution
- Only processes staged files (pre-commit)
- Faster than Husky

**Safety:**
- Prevents committing unformatted code
- Catches type errors before push
- Ensures tests pass before push
- Validates lockfile integrity

**Developer Experience:**
- Auto-fixes formatting issues
- Stages fixed files automatically
- Clear error messages
- Easy to skip when needed (LEFTHOOK=0)

### Adoption Analysis

**Currently Using (3/6):**
- node-project-starter (template source)
- osm-tagging-schema-mcp (MCP server)
- nms-lib (library)

**Should Adopt (3/6):**
- diff-voyager (monorepo, missing hooks)
- nms-frigate-calc (calculator, no quality checks)
- osm-helper (helper, no quality checks)

### Integration Requirements

For pattern to work, project must have:
- `npm run typecheck` script
- `npm run test` script
- `npm run build` script
- `npm run prepare` script (runs lefthook install)
- Biome or equivalent formatter/linter

### Breaking Changes

None - This is the initial pattern documentation.

### Notes

- Pattern is stable across 3 projects
- Zero configuration drift detected
- Lefthook 1.x - 2.x used (1.10.6 - 2.0.4)
- Perfect candidate for wider adoption
