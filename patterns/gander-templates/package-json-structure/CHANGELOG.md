# Changelog - Package.json Structure Pattern

All notable changes to this pattern will be documented in this file.

## [2026-01-09] - Initial Pattern Documentation

**Added:** Initial package-json-structure pattern documentation based on analysis of 6 projects
**Reason:** Establish comprehensive metadata, exports, and security standards
**Impact:** 6 projects (100% adoption) - documents universal consensus
**Migration:** No migration needed - pattern documents existing best practices

### Pattern Coverage

Documentation covers 14 critical areas:

1. **Basic Metadata** - name, version, description
2. **Module System** - type, main, module, types
3. **Modern Exports** - Package exports API
4. **Files Distribution** - What gets published
5. **Engines** - Node.js version requirements
6. **Scripts Organization** - dev, build, test, quality
7. **Author & Contact** - Author, maintainer info
8. **Repository Links** - GitHub integration
9. **License** - Open source or proprietary
10. **Keywords** - npm search optimization
11. **Binary Executables** - CLI tools
12. **Dependencies** - Production vs development
13. **Overrides** - Dependency resolution
14. **Workspaces** - Monorepo configuration

### Configuration Consensus

**ES Modules:**
- All 6 projects use `"type": "module"`
- Modern JavaScript standard
- Better tree-shaking

**Dual Format Exports:**
- Libraries provide both ESM and CJS
- Uses `exports` field for explicit entry points
- TypeScript declarations included

**Scoped Packages:**
- Organization prefixes: `@gander-tools/`, `@gander-templates/`
- Professional branding
- Namespace management

**Node.js Versions:**
- New projects: `^22.0.0 || ^24.0.0` (strict)
- Older projects: `>=20` (permissive)
- Trend: Moving to strict version ranges

### Pattern Variations Identified

**Library Variant:**
- Dual format (ESM + CJS)
- Minimal files included
- prepublishOnly validation
- Examples: node-project-starter, nms-lib

**CLI Tool Variant:**
- Binary executable
- ES module only
- Includes documentation
- Example: osm-tagging-schema-mcp

**Application Variant:**
- Private package
- No exports needed
- Application-specific scripts
- Examples: nms-frigate-calc

**Monorepo Variant:**
- Workspaces configuration
- Root-level orchestration
- Shared devDependencies
- Example: diff-voyager

### Key Decisions Documented

**Why `files` field:**
- Whitelist approach (safer than blacklist)
- Smaller package size
- No accidental exposure

**Why scoped names:**
- Prevent npm namespace conflicts
- Organization branding
- Better discoverability

**Why `exports`:**
- Explicit entry points
- Prevents deep imports
- Better tooling support
- Future-proof

**Why lifecycle scripts:**
- `prepare` - Auto-install Git hooks
- `prepublishOnly` - Pre-publish validation
- Automate quality gates

### Security Best Practices

- Never include secrets in published packages
- Use `files` whitelist to control distribution
- Validate with publint before publishing
- Enable npm provenance (GitHub Actions)

### Breaking Changes

None - This is the initial pattern documentation.

### Notes

- Pattern is mature and universally adopted
- Significant evolution from Node.js < 18 CommonJS era
- ES modules now standard across all projects
- Package exports improving TypeScript/bundler integration
