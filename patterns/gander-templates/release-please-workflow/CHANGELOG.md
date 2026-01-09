# Changelog - Release Please Workflow Pattern

All notable changes to this pattern will be documented in this file.

## [2026-01-09] - Initial Pattern Documentation

**Added:** Initial release-please-workflow pattern documentation based on analysis of 3 projects
**Reason:** Automate release management with semantic versioning and npm publishing
**Impact:** 3 projects using pattern, 3 projects should adopt
**Migration:** Replaces manual versioning and CHANGELOG maintenance

### Pattern Configuration

**Two-Job Architecture:**
1. **Create Release** - Analyzes commits, creates Release PR
2. **Publish npm** - Builds, tests, publishes with provenance

**Conventional Commits:**
- `feat:` → Minor bump
- `fix:` → Patch bump
- `BREAKING CHANGE:` → Major bump
- Other types → No bump

**Security Features:**
- npm provenance (cryptographic proof)
- SLSA Level 3 attestations
- SHA-pinned actions
- Harden Runner (production variant)

### Pattern Variations Identified

**Simple Variant (node-project-starter):**
- Node.js 22
- Basic workflow structure
- Minimal security features
- Suitable for templates

**Production Variant (osm-tagging-schema-mcp):**
- Node.js 24 (latest LTS)
- Harden Runner security
- Version validation
- SBOM generation
- Comprehensive attestations
- dist/ artifact sharing (NPM + Docker)

**Monorepo Variant (diff-voyager):**
- Workspace-aware releases
- Per-package versioning
- Multiple release configurations

### Key Benefits

**Automation:**
- Zero-touch version management
- Automatic CHANGELOG generation
- Semantic versioning enforcement
- npm publishing on merge

**Security:**
- npm provenance verification
- SLSA build attestations
- Supply chain transparency
- Cryptographic proof of origin

**Developer Experience:**
- Review Release PR before merge
- Clear release notes
- Predictable versioning
- No manual CHANGELOG editing

### Adoption Analysis

**Currently Using (3/6):**
- node-project-starter (template source)
- osm-tagging-schema-mcp (production MCP)
- diff-voyager (active development)

**Should Adopt (3/6):**
- nms-lib (library, needs releases)
- nms-frigate-calc (manual versioning burden)
- osm-helper (no release process)

### Integration Requirements

**Prerequisites:**
- Conventional Commits discipline
- GitHub Actions enabled
- npm publishing rights
- Branch protection (recommended)

**Configuration Files:**
- `.github/workflows/release-please.yml`
- `release-please-config.json` (optional, for customization)
- `.release-please-manifest.json` (auto-generated)

### Breaking Changes

None - This is the initial pattern documentation.

### Notes

- Pattern significantly reduces release management overhead
- Conventional Commits adoption is critical success factor
- npm provenance becoming industry standard
- SLSA attestations provide supply chain security
- All 3 projects report positive experience
