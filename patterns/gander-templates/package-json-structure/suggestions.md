# Package.json Structure - Customization Suggestions

## Choosing Package Type

### Library/Package

For npm packages consumed by other projects:

```json
{
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  },
  "files": ["dist", "README.md", "LICENSE"]
}
```

### CLI Tool

For command-line applications:

```json
{
  "type": "module",
  "bin": {
    "my-tool": "dist/index.js"
  },
  "files": ["dist", "README.md", "LICENSE", "CHANGELOG.md"]
}
```

Remember: Add shebang `#!/usr/bin/env node` to entry file.

### Application (Private)

For applications not published to npm:

```json
{
  "name": "my-app",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "start": "node dist/index.js"
  }
}
```

### Monorepo Root

For workspace-based monorepos:

```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "build": "npm run build --workspaces",
    "test": "npm run test --workspaces"
  }
}
```

## Metadata Customization

### Multiple Authors

```json
{
  "author": "Primary Author <email@example.com>",
  "contributors": [
    "Contributor 1 <email1@example.com>",
    "Contributor 2 <email2@example.com>"
  ]
}
```

### Funding Links

```json
{
  "funding": {
    "type": "github",
    "url": "https://github.com/sponsors/username"
  }
}
```

Or multiple funding sources:

```json
{
  "funding": [
    {
      "type": "github",
      "url": "https://github.com/sponsors/username"
    },
    {
      "type": "patreon",
      "url": "https://patreon.com/username"
    }
  ]
}
```

### Custom Fields

Add project-specific metadata:

```json
{
  "mcpName": "io.github.gander-tools/package-name",
  "customField": "value"
}
```

## Module System Variations

### ESM-Only (Modern)

For Node.js 18+ projects:

```json
{
  "type": "module",
  "exports": {
    ".": "./dist/index.js"
  }
}
```

### CJS-Only (Legacy)

For maximum compatibility:

```json
{
  "main": "./dist/index.js"
}
```

Note: Prefer ESM unless legacy support required.

### Dual Package (ESM + CJS)

Best for libraries:

```json
{
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

## Exports Patterns

### Multiple Entry Points

```json
{
  "exports": {
    ".": "./dist/index.js",
    "./utils": "./dist/utils.js",
    "./types": "./dist/types.js"
  }
}
```

Usage: `import { foo } from 'package/utils'`

### Conditional Exports

```json
{
  "exports": {
    ".": {
      "node": "./dist/node.js",
      "browser": "./dist/browser.js",
      "default": "./dist/index.js"
    }
  }
}
```

### Block Deep Imports

```json
{
  "exports": {
    ".": "./dist/index.js",
    "./package.json": "./package.json"
  }
}
```

Only exported paths can be imported.

## Scripts Organization

### Standard Development Scripts

```json
{
  "scripts": {
    "dev": "tsx --watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "clean": "rm -rf dist"
  }
}
```

### Testing Scripts

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:unit": "vitest run tests/unit",
    "test:integration": "vitest run tests/integration",
    "test:coverage": "vitest run --coverage"
  }
}
```

### Quality Scripts

```json
{
  "scripts": {
    "check": "biome check .",
    "format": "biome format --write .",
    "lint": "biome lint .",
    "typecheck": "tsc --noEmit"
  }
}
```

### CI/CD Scripts

```json
{
  "scripts": {
    "ci": "npm run typecheck && npm test && npm run build",
    "validate": "npm run check && npm run ci"
  }
}
```

### Composite Scripts

Chain multiple scripts:

```json
{
  "scripts": {
    "prebuild": "npm run clean",
    "build": "tsc",
    "postbuild": "npm run validate"
  }
}
```

## Engine Constraints

### Strict Versions

For production tools:

```json
{
  "engines": {
    "node": "^22.0.0 || ^24.0.0",
    "npm": "^10.0.0 || ^11.0.0"
  }
}
```

Prevents installation on unsupported versions.

### Permissive Versions

For libraries:

```json
{
  "engines": {
    "node": ">=18"
  }
}
```

Allows wider compatibility.

### Enforce Engines

Add to `.npmrc`:

```
engine-strict=true
```

Blocks installation on incompatible Node versions.

## Dependency Management

### Version Pinning Strategies

**Caret (Flexible):**
```json
{
  "dependencies": {
    "package": "^1.2.3"
  }
}
```
Allows: 1.2.3, 1.2.4, 1.3.0, but not 2.0.0

**Tilde (Conservative):**
```json
{
  "dependencies": {
    "package": "~1.2.3"
  }
}
```
Allows: 1.2.3, 1.2.4, but not 1.3.0

**Exact (Strict):**
```json
{
  "dependencies": {
    "package": "1.2.3"
  }
}
```
Allows: Only 1.2.3

### Peer Dependencies

For plugins/extensions:

```json
{
  "peerDependencies": {
    "react": "^18.0.0"
  },
  "peerDependenciesMeta": {
    "react": {
      "optional": true
    }
  }
}
```

### Optional Dependencies

For non-critical dependencies:

```json
{
  "optionalDependencies": {
    "fsevents": "^2.3.0"
  }
}
```

### Bundle Dependencies

Include dependencies in package tarball:

```json
{
  "bundleDependencies": [
    "critical-package"
  ]
}
```

## Files Distribution

### Minimal Distribution

Only essentials:

```json
{
  "files": [
    "dist"
  ]
}
```

### Include Documentation

```json
{
  "files": [
    "dist",
    "README.md",
    "LICENSE",
    "CHANGELOG.md",
    "docs"
  ]
}
```

### Test Package Before Publishing

```bash
# Create tarball
npm pack

# Check contents
tar -tzf package-name-1.0.0.tgz

# Test installation
npm install ./package-name-1.0.0.tgz
```

## Security Configuration

### Private Package

Prevent accidental publishing:

```json
{
  "private": true
}
```

### Publish Configuration

Control publishing:

```json
{
  "publishConfig": {
    "access": "public",
    "registry": "https://registry.npmjs.org/"
  }
}
```

### NPM Ignore vs Files

Prefer `files` whitelist over `.npmignore` blacklist.

## Validation

### Pre-publish Checks

```json
{
  "scripts": {
    "prepublishOnly": "npm run validate && publint --strict"
  }
}
```

### Package Validation Tools

```bash
# Validate exports
npx publint

# Check TypeScript types
npx @arethetypeswrong/cli

# Validate package.json
npm pkg get
```

## Migration Strategies

### From CommonJS to ESM

**Step 1:** Add `"type": "module"`

**Step 2:** Update imports:
```javascript
// Before
const foo = require('./foo')

// After
import foo from './foo.js'
```

**Step 3:** Update exports:
```javascript
// Before
module.exports = foo

// After
export default foo
```

**Step 4:** Add file extensions to imports

### From Unscoped to Scoped

**Step 1:** Rename package:
```json
{
  "name": "@org/package-name"
}
```

**Step 2:** Publish with public access:
```json
{
  "publishConfig": {
    "access": "public"
  }
}
```

**Step 3:** Deprecate old package:
```bash
npm deprecate old-package-name "Moved to @org/package-name"
```

## Monorepo Configuration

### Workspace Packages

```json
{
  "workspaces": [
    "packages/*",
    "apps/*"
  ]
}
```

### Workspace-Specific Scripts

```json
{
  "scripts": {
    "build:backend": "npm run build --workspace=@org/backend",
    "test:all": "npm run test --workspaces"
  }
}
```

### Shared Dependencies

Install in root for all workspaces:

```bash
npm install --save-dev typescript -w
```

## Related Patterns

- [tsconfig-base](../tsconfig-base/) - TypeScript configuration
- [biome-config](../biome-config/) - Quality scripts integration
- [lefthook-config](../lefthook-config/) - Prepare script for hooks
- [release-please-workflow](../release-please-workflow/) - Version management
