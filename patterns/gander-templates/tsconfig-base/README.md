# Pattern: TypeScript Base Configuration

## Overview

Standardized TypeScript compiler configuration for all Gander TypeScript projects. This pattern establishes strict type checking, modern module resolution, and consistent build settings across libraries, tools, and applications.

## Context

Use this pattern when:
- Starting any new TypeScript project
- Building libraries for npm publication
- Creating command-line tools
- Developing Node.js applications
- Need consistent TypeScript behavior across projects

TypeScript configuration should match project type:
- **Libraries:** `module: "ESNext"`, `moduleResolution: "bundler"`
- **Node.js apps:** `module: "NodeNext"`, `moduleResolution: "NodeNext"`
- **Tools:** Mix of both depending on distribution method

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager), [nms-lib](https://github.com/gander-tools/nms-lib), [nms-frigate-calc](https://github.com/gander-tools/nms-frigate-calc), [osm-helper](https://github.com/gander-tools/osm-helper) | Total: 6 projects (100% adoption)

## Dependencies

### Required
- TypeScript 5.x (`npm install --save-dev typescript`)
- Node.js 20+ (for modern TypeScript features)

### Recommended
- Build tool: tsdown, pkgroll, tsc, or esbuild
- [biome-config](../biome-config/) - Linting and formatting

## Configuration

### Core Compiler Options

**Language and Environment:**
- `target: "ES2022"` - Modern JavaScript features
- `lib: ["ES2022"]` - Standard library for ES2022

**Modules:**
- `module: "ESNext"` - Latest ECMAScript modules
- `moduleResolution: "bundler"` - Bundler-friendly resolution (for libraries)
- `resolveJsonModule: true` - Import JSON files as modules

**Emit:**
- `declaration: true` - Generate .d.ts files
- `declarationMap: true` - Source maps for declarations
- `sourceMap: true` - Enable debugging
- `outDir: "./dist"` - Output directory
- `removeComments: true` - Smaller output

**Interop Constraints:**
- `esModuleInterop: true` - CommonJS/ES module compatibility
- `allowSyntheticDefaultImports: true` - Default import flexibility
- `forceConsistentCasingInFileNames: true` - Cross-platform consistency
- `isolatedModules: true` - Ensure module can be transpiled in isolation

**Type Checking (Strict Mode):**
- `strict: true` - Enable all strict type checking
- `noUnusedLocals: true` - Flag unused variables
- `noUnusedParameters: true` - Flag unused parameters
- `noImplicitReturns: true` - All code paths must return
- `noFallthroughCasesInSwitch: true` - Explicit switch fallthrough
- `noUncheckedIndexedAccess: true` - Safer array/object access
- `noImplicitOverride: true` - Explicit method overrides

**Performance:**
- `skipLibCheck: true` - Faster compilation (skip .d.ts checks)

### Path Aliases (Optional)

```json
{
  "baseUrl": ".",
  "paths": {
    "@/*": ["./src/*"]
  }
}
```

## Pattern Variations

### Variation A (Library/Bundler) - node-project-starter, nms-lib

**Purpose:** npm packages, libraries intended for bundlers

**Key settings:**
- `module: "ESNext"`
- `moduleResolution: "bundler"`
- `target: "ES2022"`
- Path aliases: Enabled

**When to use:**
- Building npm libraries
- Output will be bundled by consumers
- Need tree-shaking support
- Want modern module features

**Reference:** `files/tsconfig.json`

### Variation B (Node.js Native) - osm-tagging-schema-mcp, diff-voyager

**Purpose:** Node.js applications and CLI tools

**Key settings:**
- `module: "NodeNext"` or `"ESNext"`
- `moduleResolution: "NodeNext"` or `"bundler"`
- `target: "ES2022"`
- Stricter resolution for Node.js

**When to use:**
- CLI tools
- Node.js servers
- Scripts meant to run directly in Node
- Need full ES modules support in Node

### Common Across All Variations

All projects use:
- Strict type checking enabled
- `noUncheckedIndexedAccess: true` - Critical for safety
- `skipLibCheck: true` - Performance optimization
- Source maps and declarations enabled
- ES2022 target and lib

## Known Issues

1. **Path Aliases in Output**
   - TypeScript doesn't resolve path aliases in emitted .js files
   - **Workaround:** Use build tools (tsdown, esbuild) that handle path resolution
   - Or use runtime path resolution (tsconfig-paths, module-alias)

2. **`noUncheckedIndexedAccess` Strictness**
   - Makes array access return `T | undefined`
   - Can be verbose for known-safe array access
   - **Workaround:** Use `.at()` method or explicit checks
   - Trade-off: Safer code vs. more type assertions

3. **`isolatedModules` Limitations**
   - Cannot use `const enum`
   - Cannot re-export types with same name
   - **Workaround:** Use regular enums or type imports

## Limitations

- Path aliases require build tool support for runtime resolution
- `bundler` module resolution not suitable for direct Node.js execution
- Strict mode can be challenging for JavaScript migrations

## Integration

### Package.json Scripts

```json
{
  "scripts": {
    "build": "tsc",
    "typecheck": "tsc --noEmit",
    "dev": "tsc --watch"
  }
}
```

### With Modern Build Tools

**tsdown** (recommended for libraries):
```bash
npm install --save-dev tsdown
# Uses tsconfig.json automatically
```

**esbuild:**
```bash
npm install --save-dev esbuild
esbuild src/index.ts --bundle --platform=node --outfile=dist/index.js
```

### VS Code Integration

TypeScript works automatically in VS Code. Ensure `.vscode/settings.json`:

```json
{
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true
}
```

## Upgrade Path

### From Loose Configuration

1. Start with `strict: false`
2. Enable strict options one by one:
   - `strictNullChecks`
   - `strictFunctionTypes`
   - `noImplicitAny`
3. Fix type errors incrementally
4. Enable full `strict: true`

### From JavaScript

1. Set `allowJs: true` and `checkJs: false`
2. Rename files `.js` → `.ts` incrementally
3. Add types gradually
4. Remove `allowJs` when fully migrated

## References

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TSConfig Reference](https://www.typescriptlang.org/tsconfig)
- [Module Resolution](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution)
- Related patterns: [biome-config](../biome-config/), [package-json-structure](../package-json-structure/)
