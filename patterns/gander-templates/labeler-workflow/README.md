# Pattern: Labeler Workflow

## Overview

Automatically applies labels to Pull Requests and Issues based on file changes, PR size, draft status, and branch patterns. Uses `srvaroa/labeler` action for intelligent labeling.

## Context

Use this workflow when:
- Want automated PR/Issue labeling
- Need file-based categorization (tests, docs, workflows, etc.)
- Want PR size indicators (small/medium/large)
- Team uses labels for organization

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager) | Total: 3 projects

## Configuration

### Label Categories

**File-Based Labels:**
- `documentation` - Changes to \*.md files
- `tests` - Changes to test files
- `workflows` - Changes to .github/workflows/
- `dependencies` - Changes to package.json, package-lock.json
- `core` - Changes to src/ (main code)
- `docker` - Changes to Dockerfile, docker-compose.yml

**Size Labels:**
- `size/small` - < 10 lines changed
- `size/medium` - 10-100 lines changed
- `size/large` - 100-500 lines changed
- `size/xlarge` - > 500 lines changed

**State Labels:**
- `work-in-progress` - Draft PRs
- `claude code` - PRs from claude/* branches

### Labeler Configuration

**.github/labeler.yml:**
```yaml
documentation:
  - changed-files:
    - any-glob-to-any-file: '**/*.md'

tests:
  - changed-files:
    - any-glob-to-any-file: 'tests/**/*'
    - any-glob-to-any-file: '**/*.test.ts'

workflows:
  - changed-files:
    - any-glob-to-any-file: '.github/workflows/**'

dependencies:
  - changed-files:
    - any-glob-to-any-file: 'package.json'
    - any-glob-to-any-file: 'package-lock.json'
```

## Integration

### Trigger Events

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review, converted_to_draft]
```

Runs on PR creation, updates, and status changes.

### Required Permissions

```yaml
permissions:
  contents: read
  pull-requests: write
  issues: write
```

## Benefits

1. **Automatic Categorization** - Files changes determine labels
2. **Size Awareness** - Quick visual indicator of PR scope
3. **Draft Detection** - Marks WIP PRs automatically
4. **Branch Pattern Detection** - Identifies Claude Code PRs

## Installation

1. Copy `files/labeler.yml` to `.github/workflows/labeler.yml`
2. Copy `files/.github-labeler.yml` to `.github/labeler.yml`
3. Create labels in GitHub repository (or let workflow create them)
4. Commit and push

## References

- [srvaroa/labeler](https://github.com/srvaroa/labeler)
- [GitHub Labels](https://docs.github.com/issues/using-labels-and-milestones-to-track-work)
- Related patterns: [auto-pr-workflow](../auto-pr-workflow/)
