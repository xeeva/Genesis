---
layout: default
title: Updating Genesis
---

# Updating Genesis

Genesis receives updates through its git repository. Updates bring new stack profiles, improved templates, additional agents, bug fixes, and documentation improvements.

## Using the /update Skill

The recommended way to update Genesis is through the `/update` skill during a Claude session:

```bash
cd ~/claude/genesis
claude
```

Then invoke:

```
/update
```

The skill performs these steps:

1. **Checks for local changes**: warns you if there are uncommitted modifications that might conflict
2. **Fetches updates**: pulls the latest changes from the remote repository
3. **Shows what changed**: lists incoming commits with highlights for notable additions
4. **Asks for confirmation**: you review the changes before applying
5. **Applies the update**: runs `git pull origin main`
6. **Reports results**: summarises what was updated

## Manual Update

If you prefer to update manually:

```bash
cd ~/claude/genesis
git fetch origin main
git log HEAD..origin/main --oneline
git pull origin main
```

This gives you the same control: fetch first, review the incoming changes, then pull.

## What Gets Preserved

The following files are gitignored and **never** overwritten by updates:

| File | Contains |
|------|----------|
| `personalisation.md` | Your locale, output style, role, experience level, project defaults |
| `environment.md` | Your platform, shell, package manager, paths |
| `.claude/settings.local.json` | Local Claude Code permission overrides |

These files are yours. Genesis updates will never touch them.

## What Gets Updated

| Component | Location |
|-----------|----------|
| Master instructions | `CLAUDE.md` |
| Agents | `.claude/agents/*.md` |
| Skills | `.claude/skills/*/SKILL.md` |
| Templates | `.claude/skills/genesis/templates/*.tmpl` |
| References | `.claude/skills/genesis/references/*.md` |
| Example configs | `personalisation.md.example`, `environment.md.example` |
| Documentation | `docs/`, `README.md`, `CONTRIBUTING.md` |
| Settings | `.claude/settings.json` |

## Checking for New Configuration Options

After updating, the `.example` files may contain new configuration options that were not present in your personal files. To check:

1. Compare your `personalisation.md` with `personalisation.md.example`
2. Compare your `environment.md` with `environment.md.example`
3. Add any new fields you want to your personal files

You can do this manually or ask Claude to compare them for you during a session.

## Previously Generated Projects

Updates to Genesis do **not** affect previously generated projects. Each generated project is independent; it has its own `CLAUDE.md`, agents, skills, and settings that were written at generation time.

If you want a generated project to benefit from newer templates or agents, you have two options:

1. **Regenerate**: create a new project with the same name (after removing the old directory)
2. **Manual update**: copy specific improvements from Genesis's updated templates into the existing project

## Rollback

If an update causes problems, you can revert to the previous state:

```bash
cd ~/claude/genesis
git log --oneline -5        # find the commit before the update
git reset --hard <commit>   # revert to that commit
```

Your `personalisation.md` and `environment.md` are unaffected by rollback since they are not tracked by git.

If you are unsure which commit to revert to, look for the commit just before the most recent `git pull`:

```bash
git reflog
```

This shows the full history of HEAD movements, including the pull operation.

## Update Frequency

There is no required update schedule. Update when you want new features or improvements. Genesis works perfectly well on any version; updates are purely additive.

To check whether updates are available without applying them:

```bash
cd ~/claude/genesis
git fetch origin main
git rev-list HEAD..origin/main --count
```

A count of `0` means you are up to date. Any other number indicates available updates.
