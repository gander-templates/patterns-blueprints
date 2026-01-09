# Improvements for node-project-starter

Last updated: 2026-01-09

## Summary

**Priority: LOW** - Template repository serving as source of truth for patterns. Consider modernization to Node.js 22+ to match production projects.

## Configuration Improvements

### [2026-01-09] Update Node.js Version Requirement
**Priority:** Medium
**Category:** Config
**Status:** Proposed

**Current:** `"engines": { "node": ">=20" }`
**Proposed:** Consider strict range like `"node": "^22.0.0 || ^24.0.0"`
**Reason:** Production projects use stricter ranges, template drift
**Files:** `package.json`
**Pattern:** [package-json-structure](../patterns/gander-templates/package-json-structure/)

**Considerations:**
- Template currently permissive (`>=20`) for wider compatibility
- Production projects use strict (`^22 || ^24`) for predictability
- Trade-off: Compatibility vs. Consistency

**Recommendation:** Document both approaches in pattern, let users choose based on project type

### [2026-01-09] Review Biome Configuration Variant
**Priority:** Low
**Category:** Config
**Status:** Proposed

**Current:** Uses "strict" variant with 100+ explicit rules
**Proposed:** Verify this is appropriate default for templates
**Reason:** Balance strictness with usability
**Files:** `biome.json`
**Pattern:** [biome-config](../patterns/gander-templates/biome-config/)

**Considerations:**
- Strict variant appropriate for libraries
- May be overkill for simple projects
- Users can customize after template usage

**Recommendation:** Keep strict as default, document minimal variant in pattern

## Workflow Improvements

### [2026-01-09] Review Workflow Versions
**Priority:** Low
**Category:** Workflow
**Status:** Proposed

**Current:** Workflows use specific Node.js version (22)
**Proposed:** Ensure alignment with package.json engines
**Reason:** Consistency between local and CI environments
**Files:** `.github/workflows/*.yml`

### [2026-01-09] Document Template Customization
**Priority:** Medium
**Category:** Documentation
**Status:** Proposed

**Current:** Template includes comprehensive setup
**Proposed:** Add TEMPLATE_USAGE.md guide
**Reason:** Help users customize template for their needs
**Files:** `TEMPLATE_USAGE.md` (new)

**Content should cover:**
- How to customize patterns for project type
- What to keep vs. remove
- Optional vs. required configurations
- Next steps after template usage

## Documentation Improvements

### [2026-01-09] Add Pattern Decision Matrix
**Priority:** Low
**Category:** Documentation
**Status:** Proposed

**Current:** README describes features
**Proposed:** Add decision guide for pattern selection
**Reason:** Help users understand when to use which patterns
**Files:** `docs/PATTERN_SELECTION.md` (new)

### [2026-01-09] Cross-Reference with patterns-blueprints
**Priority:** Low
**Category:** Documentation
**Status:** Proposed

**Current:** Template is standalone
**Proposed:** Add links to patterns-blueprints repository
**Reason:** Connect template with pattern documentation
**Files:** `README.md`

## Implementation Priority

**Phase 1 (Template Improvement):**
1. Add `TEMPLATE_USAGE.md` guide
2. Review Node.js version strategy
3. Cross-reference patterns-blueprints

**Phase 2 (Documentation):**
4. Add pattern decision matrix
5. Review Biome strict variant documentation

## Migration Notes

- Template serves as source of truth, changes affect all new projects
- Any breaking changes should be carefully considered
- Focus on improving usability and documentation over config changes
- Template already excellent, mostly documentation improvements

## Estimated Impact

- **Usability:** Significant improvement with usage guide
- **Consistency:** Better alignment with production projects
- **Discoverability:** Improved with cross-references to patterns
- **Decision Making:** Easier pattern selection with decision matrix

## Notes

**Template vs. Production Drift:**

Current drift between template defaults and production usage:
- **Node.js version:** Template `>=20`, production `^22 || ^24`
- **Biome config:** Template strict (100+ rules), production minimal
- **Workflow Node:** Template 22, production 24

**Recommendation:**
- Document both approaches in patterns
- Let template remain flexible (permissive defaults)
- Add clear guidance on when to use strict vs. permissive
- Users can tighten constraints based on project needs

**Template Philosophy:**
- Provide comprehensive starting point
- Allow easy customization
- Document choices and alternatives
- Balance strictness with usability
