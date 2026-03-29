---
layout: default
title: Getting Started
---

# Getting Started

[Home](.) | [How It Works](how-it-works) | [Architecture](architecture) | [Projects](generated-project) | [Plans](plans-and-context) | [Customise](customisation) | [Update](updating) | [FAQ](faq)

---

## Prerequisites

### Claude Code CLI (v1.x+)

Claude Code is the runtime that powers Genesis. Install it via npm:

```bash
npm install -g @anthropic-ai/claude-code
```

You will need an Anthropic API key or a Claude Pro/Team subscription with Claude Code access.

### git (v2.x+)

Genesis initialises every generated project as a git repository.

| Platform | Command |
|----------|---------|
| Debian / Ubuntu | `sudo apt install git` |
| Fedora | `sudo dnf install git` |
| Arch | `sudo pacman -S git` |
| macOS | `brew install git` |
| Windows (Scoop) | `scoop install git` |
| Windows (Chocolatey) | `choco install git` |

### Node.js (v18+)

Required by Claude Code and MCP servers.

| Platform | Command |
|----------|---------|
| Debian / Ubuntu | `sudo apt install nodejs npm` or use [nvm](https://github.com/nvm-sh/nvm) |
| Fedora | `sudo dnf install nodejs npm` |
| Arch | `sudo pacman -S nodejs npm` |
| macOS | `brew install node` |
| Windows (Scoop) | `scoop install nodejs` |
| Windows | [Official installer](https://nodejs.org/) or [nvm-windows](https://github.com/coreybutler/nvm-windows) |

### Supported Shell

Genesis works with bash, zsh, and PowerShell. Most Linux and macOS systems have bash or zsh by default. On Windows, PowerShell is the default.

## Cloning the Repository

```bash
git clone https://github.com/xeeva/Genesis.git ~/claude/genesis
cd ~/claude/genesis
```

Genesis expects to live at `~/claude/genesis/`. This is the conventional location and is referenced in internal paths.

## First-Time Setup

Launch Claude in the Genesis directory:

```bash
claude
```

On first launch, Genesis detects that `personalisation.md` and `environment.md` do not yet exist and guides you through setup.

### Step 1: Environment Detection

Genesis asks about your platform:

- **Operating system**: Linux, macOS, Windows, or WSL
- **Shell**: bash, zsh, or PowerShell
- **Package manager**: apt, brew, dnf, pacman, scoop, etc.

Genesis also asks which Claude plan you are on (Pro, Max, ProMax, or API key). This determines the **scaffold profile**, which controls how much infrastructure Genesis generates for each project:

| Plan | Scaffold Profile | What You Get |
|------|-----------------|--------------|
| Pro | lean | Condensed CLAUDE.md, 1-2 domain agents, 1-2 dynamic skills. Optimised to leave maximum context for your work. |
| Max | standard | Full CLAUDE.md, 2-3 domain agents, 2-3 dynamic skills. A solid balance of infrastructure and context headroom. |
| ProMax | full | Detailed CLAUDE.md, 3-4 domain agents, 3-4 dynamic skills, extended context sections. Takes full advantage of the 1M context window. |
| API | ask | Genesis asks for your context budget and recommends a profile. |

The ProMax plan with its 1M context window provides the best Genesis experience, with room for comprehensive infrastructure and extended working sessions without compaction.

It writes these answers to `environment.md` in the Genesis root. This file is gitignored and preserved across updates. You can change your scaffold profile at any time by editing this file.

### Step 2: Personalisation

Genesis presents its defaults:

- Australian English locale (colour, behaviour, organisation)
- No em dashes
- Terse output style
- Mandatory testing

You can keep these defaults or customise them. If you customise, Genesis asks about your locale, output verbosity, role, and experience level. It writes these to `personalisation.md`, also gitignored and preserved across updates.

### Step 3: Prerequisite Check

Genesis verifies that `claude`, `git`, and `node` are available on your PATH. If anything is missing, it provides installation instructions tailored to your platform. Missing tools do not block setup; Genesis simply warns you.

### Setup Complete

After setup, Genesis displays:

> Setup complete. You can re-run setup at any time by deleting `personalisation.md` or `environment.md` and starting a new session.

You are now ready to create your first project.

## Your First Project

With setup complete, describe what you want to build. Here is an example session:

### You say:

> Create a Python FastAPI service called widget-api with PostgreSQL and Redis

### Genesis interviews (Phase 1):

Genesis extracts the project name (`widget-api`), stack (Python/FastAPI), and integrations (PostgreSQL, Redis) from your message. It might ask one follow-up question:

> Does widget-api need authentication? If so, what provider (JWT, OAuth2, API keys)?

### You respond:

> JWT authentication

### Genesis presents a plan (Phase 2):

```
Project: widget-api
Path: ~/claude/widget-api/
Stack: Python, FastAPI, PostgreSQL, Redis, JWT

Agents (domain):
- api-designer -- design REST endpoints and request/response schemas
- data-modeller -- design database schemas and relationships
- auth-specialist -- implement JWT authentication flows

Agents (workflow):
- test-runner -- run and analyse test results
- code-reviewer -- review code quality and correctness
- doc-writer -- generate and update documentation

Skills (base):
- /test -- run pytest
- /lint -- run ruff check and format
- /review -- trigger code review
- /commit -- stage and commit changes

Skills (dynamic):
- /endpoint -- scaffold a new API endpoint
- /migrate -- create a database migration

MCP Servers:
- postgres (PostgreSQL connection)

Folder Structure:
src/widget_api/
tests/
migrations/
docs/
```

### You confirm:

> Looks good, go ahead

### Genesis generates (Phase 3):

Genesis creates the entire project directory at `~/claude/widget-api/` with all files, then initialises git with an initial commit.

### Genesis summarises (Phase 4):

```
Project created: ~/claude/widget-api/
Stack: Python, FastAPI, PostgreSQL, Redis, JWT
Agents: 6 (api-designer, data-modeller, auth-specialist, test-runner, code-reviewer, doc-writer)
Skills: 6 (/test, /lint, /review, /commit, /endpoint, /migrate)
MCP: postgres

Next steps:
  cd ~/claude/widget-api/
  claude
```

### Start working:

```bash
cd ~/claude/widget-api/
claude
```

Your new project is ready. Claude already knows the stack, the coding standards, the testing requirements, and has agents ready to help with API design, data modelling, and authentication.

## Troubleshooting

### "command not found: claude"

The Claude Code CLI is not installed or not on your PATH. Install it with:

```bash
npm install -g @anthropic-ai/claude-code
```

If you installed it but it is not found, check that your npm global bin directory is on your PATH.

### "Target directory already exists"

Genesis only creates greenfield projects. If `~/claude/<name>/` already exists, Genesis refuses to overwrite it. Either choose a different name or remove the existing directory first.

### Setup runs every time

Check that `personalisation.md` and `environment.md` exist in `~/claude/genesis/`. If they are missing, Genesis re-runs setup. These files should not be deleted unless you want to reconfigure.

### MCP server connection failures

MCP servers require the relevant service to be running. For example, the PostgreSQL MCP server needs a PostgreSQL instance accessible at the configured connection string. Genesis configures the MCP server entry but does not start the service itself.

### Windows-specific issues

- Use WSL for the best experience. Native Windows support exists but some tools have quirks.
- If using WSL, keep projects in the WSL filesystem (`~/`) rather than on `/mnt/c/` for better performance.
- Configure `git config --global core.autocrlf input` to prevent line ending issues in WSL.
- Configure `git config --global core.autocrlf true` on native Windows.
