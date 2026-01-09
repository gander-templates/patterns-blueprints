# Biome Configuration - Customization Suggestions

## Choosing a Variant

### Use Strict Variant When:
- Building a public npm package
- Code will be reused across multiple projects
- Maximum code quality is priority
- Team has bandwidth for stricter linting

### Use Minimal Variant When:
- Building internal tools
- Rapid prototyping or MVP development
- Team prefers less restrictive rules
- Migrating from no linter/formatter

### Use Library Minimal Variant When:
- Library with established patterns
- Gradual adoption from ESLint
- Want consistency without strict enforcement

## Common Customizations

### Disable Specific Rules

If a rule is too strict for your project:

```json
{
  "linter": {
    "rules": {
      "suspicious": {
        "noExplicitAny": "off"
      }
    }
  }
}
```

### Add Project-Specific Ignores

```json
{
  "files": {
    "includes": ["**"],
    "ignore": [
      "**/generated/**",
      "**/fixtures/**",
      "scripts/temp-*.ts"
    ]
  }
}
```

### Adjust Line Width

For wider monitors or different preferences:

```json
{
  "formatter": {
    "lineWidth": 120
  }
}
```

### Change Indentation Style

If your team prefers spaces:

```json
{
  "formatter": {
    "indentStyle": "space",
    "indentWidth": 2
  }
}
```

Note: Consider team consistency before changing this.

### Per-Language Overrides

```json
{
  "javascript": {
    "formatter": {
      "quoteStyle": "single"
    }
  },
  "json": {
    "formatter": {
      "indentWidth": 4
    }
  }
}
```

## Migration Tips

### From ESLint + Prettier

1. **Install Biome:**
   ```bash
   npm install --save-dev @biomejs/biome
   npm uninstall eslint prettier
   ```

2. **Copy Configuration:**
   - Start with minimal variant
   - Add strict rules gradually

3. **Update Pre-commit Hooks:**
   - Remove ESLint/Prettier hooks
   - Add Biome check (see lefthook-config pattern)

4. **IDE Setup:**
   - Uninstall ESLint/Prettier extensions
   - Install Biome extension
   - Set as default formatter

5. **Run Initial Format:**
   ```bash
   npx biome check --write .
   ```

6. **Commit Formatting Changes:**
   - Separate commit for Biome adoption
   - Separate commit for code changes

### Gradual Adoption

Run Biome alongside ESLint initially:

```json
{
  "scripts": {
    "lint": "npm run lint:biome && npm run lint:eslint",
    "lint:biome": "biome check .",
    "lint:eslint": "eslint ."
  }
}
```

## Performance Optimization

### Large Monorepos

Use `ignoreUnknown` to skip unrecognized files:

```json
{
  "files": {
    "ignoreUnknown": true
  }
}
```

### CI/CD Optimization

Run Biome checks before expensive operations:

```yaml
- name: Check code quality
  run: npm run check  # Fast Biome check

- name: Run tests      # Only if Biome passes
  run: npm test
```

## Team Adoption

### Communication
- Explain why Biome (performance, simplicity)
- Show before/after comparisons
- Highlight reduced configuration complexity

### Training
- Share this documentation
- Run demo session showing Biome features
- Document any project-specific customizations

### Rollout Strategy
1. Add Biome to single project (pilot)
2. Gather team feedback
3. Adjust configuration based on feedback
4. Rollout to remaining projects
5. Document lessons learned

## Related Patterns

- [lefthook-config](../lefthook-config/) - Git hooks for automatic Biome checks
- [package-json-structure](../package-json-structure/) - Scripts organization
- [tsconfig-base](../tsconfig-base/) - TypeScript configuration that complements Biome
