# Pattern: Auto PR Workflow for Claude Branches

## Overview

Automatically creates Pull Requests when code is pushed to `claude/*` branches. Streamlines Claude Code workflow by eliminating manual PR creation step.

## Context

Use this workflow when:
- Using Claude Code for development
- Want automated PR creation for AI-assisted development
- Follow `claude/*` branch naming convention
- GitHub repository with GitHub Actions

## Adoption (as of 2026-01-09)

Used in: [node-project-starter](https://github.com/gander-templates/node-project-starter), [osm-tagging-schema-mcp](https://github.com/gander-tools/osm-tagging-schema-mcp), [diff-voyager](https://github.com/gander-tools/diff-voyager) | Total: 3 projects

## Configuration

### Trigger

```yaml
on:
  push:
    branches:
      - 'claude/**'
```

Activates on any push to branches matching `claude/*` pattern.

### Workflow Steps

1. **Extract branch metadata** - Gets branch name and session ID
2. **Check if PR exists** - Avoids duplicate PRs
3. **Get commit messages** - Collects commits for PR body
4. **Create PR** - Uses first commit title as PR title
5. **Add labels** - Automatically adds "claude" and "automated" labels

### PR Format

**Title:** First commit message from branch
**Body:**
```markdown
## Changes

- Commit message 1
- Commit message 2
- ...

---
*This PR was automatically created from branch `claude/feature-name`*
```

**Labels:** `claude`, `automated`

## Integration

### With Claude Code

Claude Code automatically creates `claude/*` branches. This workflow detects those branches and creates PRs.

### Required Permissions

```yaml
permissions:
  contents: read
  pull-requests: write
```

## Known Issues

1. **PR title from oldest commit**
   - Uses first commit (oldest) as title
   - **Workaround:** Manually edit PR title if needed

2. **Large commit history**
   - Limits to 20 commits in PR body
   - Prevents excessive PR descriptions

## Installation

1. Copy `files/auto-pr.yml` to `.github/workflows/auto-pr.yml`
2. Commit and push to repository
3. Workflow activates automatically

## References

- [Claude Code Documentation](https://claude.com/claude-code)
- [GitHub Actions](https://docs.github.com/actions)
- Related patterns: [labeler-workflow](../labeler-workflow/)
