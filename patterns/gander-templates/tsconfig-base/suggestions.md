# TypeScript Configuration - Customization Suggestions

## Choosing Module Settings

### Use `bundler` Module Resolution When:
- Building npm libraries
- Consumers will use bundlers (webpack, vite, esbuild)
- Want modern ESM features
- Need tree-shaking support

```json
{
  "module": "ESNext",
  "moduleResolution": "bundler"
}
```

### Use `NodeNext` Module Resolution When:
- Building CLI tools
- Running directly in Node.js
- Need Node.js-specific resolution
- Want package.json exports support

```json
{
  "module": "NodeNext",
  "moduleResolution": "NodeNext"
}
```

## Common Customizations

### Relax Unused Variable Checks

For rapid prototyping or WIP code:

```json
{
  "noUnusedLocals": false,
  "noUnusedParameters": false
}
```

Note: Re-enable before production.

### Add Path Aliases

For cleaner imports:

```json
{
  "baseUrl": ".",
  "paths": {
    "@/*": ["./src/*"],
    "@utils/*": ["./src/utils/*"],
    "@types/*": ["./src/types/*"]
  }
}
```

Remember: Configure build tool to resolve aliases at runtime.

### Experimental Decorators

For projects using decorators:

```json
{
  "experimentalDecorators": true,
  "emitDecoratorMetadata": true
}
```

Use case: TypeORM, NestJS, Angular

### Monorepo Configuration

For workspace projects:

```json
{
  "composite": true,
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/backend" }
  ]
}
```

### Allow JavaScript

During migration from JavaScript:

```json
{
  "allowJs": true,
  "checkJs": false
}
```

Gradually enable `checkJs: true` as you add types.

## Project-Type Specific Settings

### For Libraries

```json
{
  "declaration": true,
  "declarationMap": true,
  "stripInternal": true,
  "removeComments": true
}
```

### For Applications

```json
{
  "declaration": false,
  "declarationMap": false,
  "sourceMap": true,
  "inlineSourceMap": false
}
```

### For CLI Tools

```json
{
  "module": "NodeNext",
  "moduleResolution": "NodeNext",
  "allowImportingTsExtensions": false
}
```

## Strict Mode Adoption

### Gradual Approach

Start with relaxed settings:

```json
{
  "strict": false,
  "strictNullChecks": false,
  "strictFunctionTypes": false,
  "noImplicitAny": false
}
```

Enable one at a time:

```bash
# Week 1
"noImplicitAny": true

# Week 2
"strictNullChecks": true

# Week 3
"strictFunctionTypes": true

# Week 4
"strict": true  // Enables all strict checks
```

### Skip if Time-Constrained

Focus on critical checks:

```json
{
  "strict": true,
  "noUncheckedIndexedAccess": true,
  // These are non-negotiable for safety
}
```

## Performance Tuning

### Large Projects

```json
{
  "skipLibCheck": true,
  "skipDefaultLibCheck": true,
  "incremental": true,
  "tsBuildInfoFile": "./dist/.tsbuildinfo"
}
```

### Faster Development Builds

```json
{
  "sourceMap": false,  // Disable in dev, enable in prod
  "declaration": false  // Only for libraries
}
```

## Build Tool Integration

### With tsdown

tsdown reads tsconfig.json automatically:

```json
{
  "scripts": {
    "build": "tsdown"
  }
}
```

### With esbuild

esbuild ignores some tsconfig options. Configure separately:

```javascript
// build.js
import esbuild from 'esbuild';

esbuild.build({
  entryPoints: ['src/index.ts'],
  bundle: true,
  platform: 'node',
  target: 'es2022',
  outfile: 'dist/index.js',
});
```

### With tsc (Direct)

For pure TypeScript projects:

```json
{
  "scripts": {
    "build": "tsc",
    "build:watch": "tsc --watch"
  }
}
```

## Common Pitfalls

### Path Aliases Without Runtime Resolution

**Problem:** Aliases work in TypeScript but fail at runtime

**Solution:** Use build tool that resolves aliases:
- tsdown (built-in)
- esbuild plugins
- webpack configuration
- Or runtime resolver: tsconfig-paths, module-alias

### `bundler` Resolution in Node.js

**Problem:** Code builds but crashes in Node.js

**Solution:** Use `NodeNext` for Node.js projects or bundle output

### Overly Strict Configuration

**Problem:** Development slows down due to type errors

**Solution:**
- Use `@ts-expect-error` for known issues
- Create `tsconfig.dev.json` with relaxed settings
- Balance safety vs. productivity

## Testing Configuration

Separate configuration for tests:

**tsconfig.test.json:**
```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "noUnusedLocals": false,
    "types": ["node", "vitest"]
  },
  "include": ["tests/**/*.ts"]
}
```

## Related Patterns

- [biome-config](../biome-config/) - Complements TypeScript with linting
- [package-json-structure](../package-json-structure/) - Build scripts integration
- [lefthook-config](../lefthook-config/) - Pre-commit type checking
