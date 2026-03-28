---
name: registry
description: View the registry of all projects created by Genesis. Use when the user says "registry", "list projects", "what projects", or "show projects".
user-invocable: true
allowed-tools: Read, Glob, Grep
---

# Project Registry

Display the registry of all projects bootstrapped by Genesis.

## Steps

1. Read `~/.claude/projects/-home-xeeva-claude-genesis/memory/project-registry.md`
2. Display the registry table to the user
3. If the user asks to filter (by stack, date, or name), apply the filter and show matching entries

## Output Format

Display as a formatted table:

```
| Project | Path | Stack | Created | Description |
|---------|------|-------|---------|-------------|
| ...     | ...  | ...   | ...     | ...         |
```

If the registry is empty, say: "No projects have been created yet. Use /genesis to create your first project."

If the registry file doesn't exist, say: "The project registry hasn't been initialised yet. It will be created when you scaffold your first project."
