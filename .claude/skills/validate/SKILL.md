---
name: validate
description: Validate that a generated project's Claude infrastructure is complete and well-formed. Use when the user says "validate", "check project", or "verify".
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash
---

# Project Validator

Check that a target project directory has all expected Claude Code infrastructure files and that they are well-formed.

## Usage

The user provides a project name or path. Default to the most recently created project if none specified.

## Checks

### Required Files
- [ ] `CLAUDE.md` exists and is non-empty
- [ ] `.claude/settings.json` exists and is valid JSON
- [ ] `.claude/agents/` directory has at least 3 agent files (test-runner, code-reviewer, doc-writer)
- [ ] `.claude/skills/` directory has at least 4 skill directories (test, lint, review, commit)
- [ ] Each skill directory contains a `SKILL.md` with valid YAML frontmatter
- [ ] `docs/README.md` exists
- [ ] `docs/architecture.md` exists
- [ ] `.gitignore` exists
- [ ] Git repo is initialised (`.git/` exists)

### Memory Files
- [ ] `~/.claude/projects/-home-xeeva-claude-<name>/memory/MEMORY.md` exists
- [ ] `~/.claude/projects/-home-xeeva-claude-<name>/memory/user-profile.md` exists
- [ ] `~/.claude/projects/-home-xeeva-claude-<name>/memory/project-context.md` exists

### Content Quality
- [ ] CLAUDE.md contains "Australian English" rule
- [ ] CLAUDE.md contains "em dash" rule
- [ ] CLAUDE.md contains "fail fast" or error handling section
- [ ] CLAUDE.md contains testing section
- [ ] settings.json has permissions.allow array
- [ ] settings.json has at least one hook

### Optional Files
- [ ] `.mcp.json` exists if integrations were specified (report if missing but expected)
- [ ] Stack-specific config files exist (tsconfig.json, pyproject.toml, etc.)

## Output

Report results as a checklist:

```
Validating: ~/claude/<name>/

Required Files:        [12/12 passed]
Memory Files:          [3/3 passed]
Content Quality:       [5/6 passed]
  - MISSING: settings.json has no hooks configured
Optional Files:        [2/2 passed]

Overall: 22/23 checks passed
```

If all checks pass: "Project is fully configured and ready to use."
If checks fail: list the failures with specific remediation steps.
