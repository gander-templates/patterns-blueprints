# Pattern: Package.json Structure and Metadata

## Overview

Comprehensive package.json configuration pattern covering module system, exports, metadata, scripts organization, security, and distribution settings. This pattern establishes consistent structure across all npm packages, libraries, and tools.

## Context

Use this pattern when:
- Publishing npm packages (public or private)
- Building libraries for distribution
- Creating CLI tools
- Any Node.js/TypeScript project with package.json

Package.json is the contract between your project and:
- npm registry (publishing)
- Users/consumers (installation)
- Build tools (bundling)
- CI/CD systems (automation)

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager), [nms-lib](https://github.com/gander-tools/nms-lib), [nms-frigate-calc](https://github.com/gander-tools/nms-frigate-calc), [osm-helper](https://github.com/gander-tools/osm-helper) | Total: 6 projects (100% adoption)

## Dependencies

### Required
- Node.js 20+ (package.json is standard)
- npm 10+ (for modern features)

### Recommended for Publishing
- Scoped package name (`@org/package-name`)
- README.md file
- LICENSE file
- CHANGELOG.md file

## Configuration

### 1. Basic Metadata

**Name and Version:**
```json
{
  "name": "@gander-tools/package-name",
  "version": "1.0.0",
  "description": "Clear, concise description (< 140 chars)"
}
```

**Scoped Names:**
- Use organization prefix: `@gander-tools/`, `@gander-templates/`
- Prevents npm namespace conflicts
- Professional branding

**Version:**
- Follow Semantic Versioning (semver)
- Updated by release-please (do not edit manually)

### 2. Module System (Critical)

**ES Modules (Recommended):**
```json
{
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

**Key fields:**
- `type: "module"` - Use ES modules, not CommonJS
- `main` - CommonJS entry (for legacy compatibility)
- `module` - ES module entry (for bundlers)
- `types` - TypeScript declarations entry

### 3. Modern Exports (Package Exports)

**Dual Format (ESM + CJS):**
```json
{
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

**Benefits:**
- Explicit entry points
- Prevents deep imports
- Better tree-shaking
- TypeScript integration

**Multiple Entry Points:**
```json
{
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js"
    },
    "./utils": {
      "types": "./dist/utils.d.ts",
      "import": "./dist/utils.js"
    }
  }
}
```

### 4. Files for Distribution

**Include Only Necessary Files:**
```json
{
  "files": [
    "dist",
    "README.md",
    "LICENSE",
    "CHANGELOG.md"
  ]
}
```

**Automatically excluded:**
- node_modules
- .git
- .github (unless explicitly included)
- Tests, source files (unless in `files`)

**Why limit files:**
- Smaller package size
- Faster installation
- No accidental exposure of source code

### 5. Engines (Node.js Version)

**Strict Version Range:**
```json
{
  "engines": {
    "node": "^22.0.0 || ^24.0.0",
    "npm": "^10.0.0 || ^11.0.0"
  }
}
```

**Permissive Version Range:**
```json
{
  "engines": {
    "node": ">=20"
  }
}
```

**Best practice:**
- Use strict ranges for production tools
- Use permissive ranges for libraries
- Match CI/CD Node version

### 6. Scripts Organization

**Development Scripts:**
```json
{
  "scripts": {
    "dev": "tsdown --watch",
    "build": "npm run build:tsdown",
    "build:tsdown": "tsdown",
    "typecheck": "tsc --noEmit"
  }
}
```

**Testing Scripts:**
```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest run --coverage"
  }
}
```

**Code Quality Scripts:**
```json
{
  "scripts": {
    "check": "biome check .",
    "format": "biome format --write .",
    "lint": "biome lint ."
  }
}
```

**Lifecycle Scripts:**
```json
{
  "scripts": {
    "prepare": "lefthook install",
    "prepublishOnly": "npm run build && publint --strict"
  }
}
```

**Key lifecycle hooks:**
- `prepare` - Runs after `npm install` (install Git hooks)
- `prepublishOnly` - Runs before `npm publish` (validation)

### 7. Author and Contact Information

**Individual Author:**
```json
{
  "author": "Adam Gąsowski",
  "author": {
    "name": "Adam Gąsowski",
    "email": "user@example.com",
    "url": "https://github.com/gander"
  }
}
```

**Organization:**
```json
{
  "author": "Gander Tools <team@example.com>"
}
```

### 8. Repository, Bugs, Homepage

**GitHub Repository:**
```json
{
  "repository": {
    "type": "git",
    "url": "git+https://github.com/gander-tools/package-name.git"
  },
  "bugs": {
    "url": "https://github.com/gander-tools/package-name/issues"
  },
  "homepage": "https://github.com/gander-tools/package-name#readme"
}
```

**Benefits:**
- Links from npm page to GitHub
- Issue tracker integration
- Documentation access

### 9. License

**Open Source:**
```json
{
  "license": "MIT"
}
```

**Common licenses:**
- `MIT` - Permissive (most libraries)
- `GPL-3.0` - Copyleft (viral license)
- `Apache-2.0` - Patent grant
- `UNLICENSED` - Private/proprietary

**Must match LICENSE file in repository**

### 10. Keywords (npm Search)

**Search Optimization:**
```json
{
  "keywords": [
    "typescript",
    "library",
    "mcp",
    "model-context-protocol",
    "openstreetmap"
  ]
}
```

**Best practices:**
- 5-10 relevant keywords
- Include technology stack
- Include use case
- Include domain/category

### 11. Binary Executables (CLI Tools)

**Single Executable:**
```json
{
  "bin": "dist/index.js"
}
```

**Named Executable:**
```json
{
  "bin": {
    "osm-tagging-schema-mcp": "dist/index.js"
  }
}
```

**Must have shebang in file:**
```javascript
#!/usr/bin/env node
```

### 12. Dependencies Management

**Production Dependencies:**
```json
{
  "dependencies": {
    "@modelcontextprotocol/sdk": "~1.25.0"
  }
}
```

**Development Dependencies:**
```json
{
  "devDependencies": {
    "@biomejs/biome": "^2.3.8",
    "typescript": "^5.7.3",
    "vitest": "^3.0.5"
  }
}
```

**Version Ranges:**
- `^1.2.3` - Caret (allow minor/patch updates)
- `~1.2.3` - Tilde (allow patch updates only)
- `1.2.3` - Exact (no updates)

**osm-tagging-schema-mcp uses tilde (`~`) for predictability**

### 13. Overrides (Dependency Resolution)

**Force Specific Version:**
```json
{
  "overrides": {
    "esbuild": "^0.27.2"
  }
}
```

Use case: Fix vulnerability in transitive dependency

### 14. Workspaces (Monorepos)

**Workspace Configuration:**
```json
{
  "workspaces": [
    "packages/*"
  ]
}
```

Used by: diff-voyager (monorepo with shared, backend, frontend)

## Pattern Variations

### Variation A (Library/Package) - node-project-starter, nms-lib

**Characteristics:**
- Dual format exports (ESM + CJS)
- TypeScript declarations
- Minimal files included
- Strict engines
- prepublishOnly validation

**Reference:** `files/package-library.json`

### Variation B (CLI Tool) - osm-tagging-schema-mcp

**Characteristics:**
- Binary executable
- ES module only (no CJS needed)
- Includes documentation in package
- Runtime dependencies

**Reference:** `files/package-tool.json`

### Variation C (Application) - diff-voyager, nms-frigate-calc

**Characteristics:**
- Private package (not published)
- No exports needed
- May skip TypeScript declarations
- Application-specific scripts

### Variation D (Monorepo Root) - diff-voyager

**Characteristics:**
- Workspaces configuration
- Root-level scripts delegate to packages
- Shared devDependencies
- Private package

## Known Issues

1. **Exports vs Main Confusion**
   - Old tools use `main`, new tools use `exports`
   - **Solution:** Include both for compatibility
   - `exports` takes precedence if supported

2. **Type Declarations Not Found**
   - `types` field must match actual .d.ts location
   - **Solution:** Verify path after build

3. **Binary Not Executable**
   - Missing shebang line
   - **Solution:** Add `#!/usr/bin/env node` to entry file

4. **Package Size Too Large**
   - Including source files/tests
   - **Solution:** Use `files` whitelist, not `.npmignore`

## Limitations

- `exports` not supported in Node.js < 12
- Some older bundlers ignore `exports`
- `bin` requires file to be executable (chmod +x on Unix)

## Integration

### With TypeScript

```json
{
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "scripts": {
    "build": "tsc"
  }
}
```

Ensure tsconfig.json outputs declarations.

### With Modern Bundlers

```json
{
  "type": "module",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js"
    }
  }
}
```

Bundlers prefer `exports` over `main`.

### With npm Publishing

```json
{
  "scripts": {
    "prepublishOnly": "npm run build && npm test && publint --strict"
  }
}
```

Validates package before publishing.

## Validation

### Check Package Configuration

```bash
# Validate package.json
npm pkg get

# Test package installation locally
npm pack
npm install ./package-name-1.0.0.tgz

# Validate exports
npx publint
npx @arethetypeswrong/cli
```

### Check Published Package

```bash
# After publishing
npm view @gander-tools/package-name

# Test installation
npm install @gander-tools/package-name
```

## Security Best Practices

1. **Never include secrets in package.json**
   - No API keys, tokens, passwords
   - Use environment variables

2. **Limit files published**
   - Use `files` whitelist
   - Don't include `.env`, `credentials.json`

3. **Validate dependencies**
   - Run `npm audit`
   - Use Renovate/Dependabot

4. **Enable npm provenance**
   - Publish with `--provenance` flag
   - Requires GitHub Actions OIDC

## References

- [npm package.json Documentation](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [Node.js Package Entry Points](https://nodejs.org/api/packages.html#package-entry-points)
- [Semantic Versioning](https://semver.org/)
- [publint](https://publint.dev/) - Package validation tool
- Related patterns: [tsconfig-base](../tsconfig-base/), [release-please-workflow](../release-please-workflow/)
