---
layout: default
title: FAQ
---

# FAQ and Troubleshooting

**Navigation:** [Home](.) | [Getting Started](getting-started) | [How It Works](how-it-works) | [Architecture](architecture) | [Generated Projects](generated-project) | [Customisation](customisation) | [Updating](updating)

---

## Frequently Asked Questions

### What platforms are supported?

Genesis supports Linux, macOS, Windows (via WSL), and native Windows. It detects your platform during first-time setup and tailors commands, paths, and dependency guidance accordingly.

For the best experience, use Linux or macOS. WSL is fully supported and recommended over native Windows. Native Windows works but some Unix tools may need alternatives or installation via Scoop or Chocolatey.

### Can I use Genesis on Windows without WSL?

Yes, but with caveats. Genesis supports native Windows with PowerShell as the shell. However:

- Some tools assumed by stack profiles (e.g. `make`, `grep`, `sed`) may not be available natively. Install them via [Scoop](https://scoop.sh/) or [Chocolatey](https://chocolatey.org/).
- Path separators use backslashes. Most Node.js tools handle this automatically, but shell scripts may need adjustment.
- The target directory becomes `%USERPROFILE%\claude\<name>\` instead of `~/claude/<name>/`.
- Line endings: configure `git config --global core.autocrlf true`.

WSL is recommended if you want a seamless experience.

### How do I change my locale after setup?

Edit `personalisation.md` in the Genesis root directory (`~/claude/genesis/personalisation.md`). Change the **Locale** field under **Language and Locale** to your preferred locale (e.g. British English, American English). The change takes effect on your next Claude session and applies to all future generated projects.

Existing projects are not affected; they have their own copy of the locale rules in their `CLAUDE.md`. To change the locale in an existing project, edit that project's `CLAUDE.md` directly.

### Does updating Genesis affect my existing projects?

No. Each generated project is completely independent. It has its own `CLAUDE.md`, agents, skills, settings, and memory files that were written at generation time. Updating Genesis only changes the templates and references used for future projects.

### Can I add my own agents and skills to Genesis?

Yes. See the [customisation guide](customisation) for detailed instructions on:

- Adding a new agent to the catalogue (`.claude/skills/genesis/references/agent-catalogue.md`)
- Adding a new skill to the selection guidelines (`CLAUDE.md`)
- Creating new templates (`.claude/skills/genesis/templates/`)
- Adding a new stack profile (`.claude/skills/genesis/references/stack-profiles.md`)

If your addition is generally useful, consider [contributing it back](https://github.com/xeeva/Genesis/blob/main/CONTRIBUTING.md).

### What if a generated project has issues?

Run the `/validate` skill to check whether the project's Claude infrastructure is complete and well-formed:

```bash
cd ~/claude/genesis
claude
```

Then:

```
/validate ~/claude/<project-name>
```

This checks for the presence and validity of CLAUDE.md, settings.json, agents, skills, memory files, and MCP configuration. It reports any missing or malformed files.

If the issue is with application boilerplate rather than Claude infrastructure, open a session in the generated project and describe the problem to Claude.

### How do I contribute a new stack?

1. Fork the Genesis repository
2. Add a new section to `.claude/skills/genesis/references/stack-profiles.md` following the existing format
3. Include all required sections: project config, linting/formatting, testing, permissions, commit style, folder structure, error patterns
4. Test by generating a project with the new stack
5. Run `/validate` on the generated project
6. Open a pull request

See [CONTRIBUTING.md](https://github.com/xeeva/Genesis/blob/main/CONTRIBUTING.md) for full guidelines.

### Can I use Genesis for non-greenfield projects?

No. Genesis is designed for new projects only. If the target directory already exists, Genesis refuses to overwrite it. This is a deliberate safety measure. Genesis scaffolds a complete project from scratch; it does not modify existing codebases.

If you want to add Claude Code infrastructure to an existing project, you can generate a throwaway project with the same stack and copy the relevant files (CLAUDE.md, agents, skills, settings.json) into your existing project, adjusting as needed.

### How many agents should a project have?

Genesis limits domain agents to 3-4 per project to avoid overwhelming the workspace. Combined with the 3 workflow agents that are always included, a typical project has 6-7 agents total. You can add more agents manually after generation if needed.

### How does Genesis handle different Claude plans?

Genesis detects your Claude plan tier during first-time setup and assigns a scaffold profile (lean, standard, or full). The profile controls how many agents and skills are generated, what CLAUDE.md template is used, and how memory files are structured. Pro users get a lean scaffold that minimises context usage. ProMax and API users get the full scaffold with all available infrastructure.

### Can I change my scaffold profile after setup?

Yes. Edit `environment.md` in the Genesis root directory and change the `Scaffold profile` value under `## Claude Plan` to `lean`, `standard`, or `full`. The change takes effect on the next project you create. You can also override the profile during the plan phase by asking for a different level of scaffold complexity.

### Does Genesis support monorepos or multi-package projects?

Genesis generates single-project scaffolds. For monorepos, you can generate each package separately and then restructure the workspace. Alternatively, describe a monorepo layout during the interview phase; Genesis will do its best to accommodate, though this is not its primary use case.

---

## Common Errors and Fixes

### "command not found: claude"

The Claude Code CLI is not installed or not on your PATH.

**Fix:** `npm install -g @anthropic-ai/claude-code`

If installed but not found, ensure your npm global bin directory is on your PATH:

```bash
npm config get prefix   # shows the npm prefix
# Add <prefix>/bin to your PATH in .bashrc/.zshrc
```

### "Target directory already exists"

Genesis only creates greenfield projects.

**Fix:** Either choose a different project name, or remove the existing directory:

```bash
rm -rf ~/claude/<name>
```

### "git: not a git repository"

You are not in the Genesis directory.

**Fix:** Ensure you are in `~/claude/genesis/` before starting a Claude session:

```bash
cd ~/claude/genesis
claude
```

### MCP server connection failures

MCP servers require the underlying service to be running. For example, the PostgreSQL MCP server needs a PostgreSQL instance at the configured connection string.

**Fix:** Start the required service before using MCP features. For local development, Docker Compose is often the easiest approach:

```bash
docker compose up -d
```

### "Permission denied" when running tools

Claude Code needs explicit permission to run Bash commands. Permissions are configured in `.claude/settings.json`.

**Fix:** Check that the required permission entries are present in `settings.json`. For example, a Python project needs:

```json
"Bash(python:*)", "Bash(pip:*)", "Bash(pytest:*)", "Bash(ruff:*)"
```

### Setup runs every time I start a session

Genesis checks for `personalisation.md` and `environment.md` on every launch.

**Fix:** Verify both files exist in `~/claude/genesis/`:

```bash
ls ~/claude/genesis/personalisation.md ~/claude/genesis/environment.md
```

If they are missing, Genesis re-runs first-time setup. Do not delete these files unless you want to reconfigure.

### Formatter hook runs on every file save

The PostToolUse hook runs the stack's formatter whenever Claude edits a file of the matching type. This is intentional; it keeps code consistently formatted.

If you find it disruptive, you can adjust the hook configuration in `.claude/settings.json` of the generated project.

### "No such file or directory" for memory files

Memory files are stored outside the project directory at `~/.claude/projects/-home-xeeva-claude-<name>/memory/`. If they are missing, Claude will not have the user profile and project context.

**Fix:** Regenerate the project, or manually create the memory directory and files. Check the templates in `.claude/skills/genesis/templates/` for the expected format.
