---
layout: default
title: How It Works
---

# How It Works

**Navigation:** [Home](.) | [Getting Started](getting-started) | [Architecture](architecture) | [Generated Projects](generated-project) | [Customisation](customisation) | [Updating](updating) | [FAQ](faq)

---

Genesis follows a strict four-phase workflow. Each phase completes before the next begins, and you have the opportunity to review and adjust at key decision points.

![Genesis Workflow](images/genesis-workflow.svg)

## Phase 1: Interview

Genesis reads your opening message and extracts as much information as it can. It gathers four things:

1. **Project name**: a kebab-case identifier (e.g. `widget-api`, `data-pipeline`, `cli-tool`)
2. **Purpose**: a one-sentence description of what the project does
3. **Tech stack**: language, framework, and runtime
4. **Key integrations**: databases, APIs, auth providers, message queues, external services

The goal is to minimise questions. If your opening message covers three or four of these, Genesis asks only what remains (1-2 questions). If your message is vague, it asks up to 4 targeted questions.

### Example: Detailed Opening

**You:** "Create a Go REST API called inventory-service with PostgreSQL and JWT auth"

Genesis extracts: name (`inventory-service`), stack (Go), integrations (PostgreSQL, JWT). It might ask:

> What is the primary purpose of inventory-service? (e.g. "manage warehouse stock levels for an e-commerce platform")

### Example: Vague Opening

**You:** "I need a new backend service"

Genesis asks:

> 1. What should the project be called?
> 2. What will it do? (one sentence)
> 3. What tech stack? (language and framework)
> 4. Any integrations? (databases, APIs, auth, etc.)

### What Genesis Already Knows

Genesis reads your `environment.md` (platform, shell, package manager) and `personalisation.md` (locale, output style, role) before the interview. It never re-asks information captured during first-time setup.

## Context-Aware Scaffolding

Genesis adjusts the size and complexity of the generated scaffold based on the user's Claude plan tier. This prevents the scaffold infrastructure from consuming too much of the available context window.

### Scaffold Profiles

| Profile | Plan Tier | Context Window | Domain Agents | Dynamic Skills | CLAUDE.md Style |
|---------|-----------|----------------|---------------|----------------|-----------------|
| **lean** | Pro | 200k | 1-2 | 1-2 | Condensed |
| **standard** | Max | 200k | 2-3 | 2-3 | Standard |
| **full** | ProMax / API | 1M | 3-4 | 3-4 | Detailed |

The scaffold profile is set during first-time setup and stored in `environment.md`. It can be overridden at any time by editing the file or by requesting a different profile during the plan phase.

### What Changes by Profile

- **lean:** Uses a condensed CLAUDE.md template (merged sections, no structure tree), consolidates memory into a single file, selects only the most critical domain agent for the project type
- **standard:** Uses the full CLAUDE.md template, full memory files, moderate agent/skill selection
- **full:** Uses the detailed CLAUDE.md template with extended context, full memory files, comprehensive agent/skill selection

Every generated project includes a statusline that shows context usage percentage, so users can monitor their budget in real time.

## Phase 2: Plan

Once Genesis has all four pieces of information, it consults the stack profiles and agent catalogue, then presents a structured plan:

```
Project: inventory-service
Path: ~/claude/inventory-service/
Stack: Go, net/http (stdlib), PostgreSQL, JWT

Agents (domain):
- api-designer -- design REST endpoints and request/response schemas
- data-modeller -- design database schemas and relationships
- auth-specialist -- implement JWT authentication flows
- security-reviewer -- audit code for security vulnerabilities

Agents (workflow):
- test-runner -- run and analyse test results
- code-reviewer -- review code quality and correctness
- doc-writer -- generate and update documentation

Skills (base):
- /test -- run go test ./...
- /lint -- run golangci-lint
- /review -- trigger code review
- /commit -- stage and commit changes

Skills (dynamic):
- /endpoint -- scaffold a new API endpoint
- /migrate -- create a database migration

MCP Servers:
- postgres (PostgreSQL connection)

Folder Structure:
cmd/inventory-service/
internal/handlers/
internal/models/
internal/services/
internal/db/
migrations/
docs/
```

### Adjustments

You can request changes before confirming:

- "Add a Redis integration"
- "Remove the security-reviewer agent"
- "Change the folder structure to use pkg/ instead of internal/"

Genesis adjusts the plan and re-presents it. Only after you confirm does it proceed to generation.

## Phase 3: Generate

Generation creates the entire project in a single pass. Genesis reads its templates and reference files, then writes every file.

### What Gets Created

**Infrastructure files:**

| File | Purpose |
|------|---------|
| `CLAUDE.md` | The project brain: rules, standards, stack-specific conventions, testing mandates |
| `.claude/settings.json` | Permissions (which Bash commands Claude can run), formatter hooks, stop hooks |
| `.claude/agents/*.md` | One file per agent with role-specific instructions |
| `.claude/skills/*/SKILL.md` | One directory per skill with invocation instructions |
| `.mcp.json` | MCP server configurations for external integrations |

**Memory files** (written to `~/.claude/projects/-home-xeeva-claude-<name>/memory/`):

| File | Purpose |
|------|---------|
| `MEMORY.md` | Index pointing to other memory files |
| `user-profile.md` | User role, preferences, and interaction style |
| `project-context.md` | Project name, purpose, stack, and key decisions |

**Application boilerplate:**

| File | Purpose |
|------|---------|
| Entry point(s) | `main.go`, `src/main.py`, `src/index.ts`, etc. |
| Package config | `go.mod`, `pyproject.toml`, `package.json`, `Cargo.toml`, etc. |
| Test setup | Test framework config and a sample test |
| `docs/README.md` | Project documentation |
| `docs/architecture.md` | High-level architecture description |
| `.gitignore` | Stack-appropriate ignores |
| Stack-specific config | `tsconfig.json`, `.golangci.yml`, `ruff.toml`, etc. |

### Template System

Genesis uses templates from `.claude/skills/genesis/templates/` as starting points. Each template contains placeholders (e.g. `{{PROJECT_NAME}}`, `{{STACK}}`) that are filled with project-specific values. The templates are:

- `CLAUDE.md.tmpl`: the project's CLAUDE.md skeleton
- `settings.json.tmpl`: permissions and hooks skeleton
- `agent.md.tmpl`: agent file skeleton
- `skill.md.tmpl`: skill file skeleton
- `mcp.json.tmpl`: MCP configuration skeleton
- `memory-user.md.tmpl`: user profile memory
- `memory-project.md.tmpl`: project context memory

### Stack Profiles

Genesis consults `.claude/skills/genesis/references/stack-profiles.md` for stack-specific conventions: folder structures, linter commands, test frameworks, permission entries, error handling patterns, and more.

### Agent Selection

Genesis consults `.claude/skills/genesis/references/agent-catalogue.md` to select domain agents. The three workflow agents (test-runner, code-reviewer, doc-writer) are always included. Domain agents are selected based on project type, capped at 3-4 to avoid overwhelming the workspace.

## Phase 4: Finalise

After all files are written, Genesis completes the setup:

### 4.1 Git Initialisation

```bash
cd ~/claude/<name> && git init && git add -A && git commit -m "Initial scaffold from Genesis"
```

The project starts with a clean git history and a single commit containing the full scaffold.

### 4.2 Registry Update

Genesis maintains a project registry in its own memory files. Each generated project is logged with its name, path, stack, date, and status.

### 4.3 Summary

Genesis prints a summary of everything it created:

```
Project created: ~/claude/inventory-service/
Stack: Go, net/http, PostgreSQL, JWT
Agents: 7 (api-designer, data-modeller, auth-specialist, security-reviewer, test-runner, code-reviewer, doc-writer)
Skills: 6 (/test, /lint, /review, /commit, /endpoint, /migrate)
MCP: postgres

Next steps:
  cd ~/claude/inventory-service/
  claude
```

## Complete Example: Python FastAPI Project

Here is a complete session from start to finish.

### Opening Message

> Create a Python FastAPI service called user-service for managing user accounts. It needs PostgreSQL for storage and JWT for authentication.

### Interview (Phase 1)

Genesis has all four pieces of information: name (`user-service`), purpose (managing user accounts), stack (Python/FastAPI), and integrations (PostgreSQL, JWT). No follow-up questions needed.

### Plan (Phase 2)

```
Project: user-service
Path: ~/claude/user-service/
Stack: Python, FastAPI, PostgreSQL, JWT

Agents (domain):
- api-designer -- design REST endpoints and request/response schemas
- data-modeller -- design database schemas and relationships
- auth-specialist -- implement JWT authentication flows
- security-reviewer -- audit code for security vulnerabilities

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
- /migrate -- create an Alembic migration

MCP Servers:
- postgres (PostgreSQL connection)

Folder Structure:
src/user_service/
  __init__.py
  main.py
  routes/
  services/
  models/
tests/
  conftest.py
migrations/
docs/
```

### Generation (Phase 3)

Genesis creates all files at `~/claude/user-service/`:

- `CLAUDE.md` with Python/FastAPI-specific rules, ruff configuration, pytest mandates
- `.claude/settings.json` with Python permissions (`python`, `pip`, `pytest`, `ruff`, `uv`)
- `.claude/agents/` with 7 agent files
- `.claude/skills/` with 6 skill directories
- `.mcp.json` with PostgreSQL MCP server config
- Memory files at `~/.claude/projects/-home-xeeva-claude-user-service/memory/`
- `pyproject.toml`, `src/user_service/main.py`, `tests/conftest.py`, sample test
- `docs/README.md`, `docs/architecture.md`
- `.gitignore` with Python-specific ignores

### Finalisation (Phase 4)

```
Project created: ~/claude/user-service/
Stack: Python, FastAPI, PostgreSQL, JWT
Agents: 7 (api-designer, data-modeller, auth-specialist, security-reviewer, test-runner, code-reviewer, doc-writer)
Skills: 6 (/test, /lint, /review, /commit, /endpoint, /migrate)
MCP: postgres

Next steps:
  cd ~/claude/user-service/
  claude
```

You are now ready to start building.
