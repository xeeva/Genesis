---
layout: default
title: Genesis
---

# Genesis

**The Claude that builds Claudes.** Bootstrap fully-equipped Claude Code projects in under two minutes.

---

**Navigation:** [Getting Started](getting-started) | [How It Works](how-it-works) | [Architecture](architecture) | [Generated Projects](generated-project) | [Customisation](customisation) | [Updating](updating) | [FAQ](faq)

---

## What is Genesis?

Genesis is a Claude Code project that creates other Claude Code projects. You describe what you want to build, and Genesis scaffolds a fully-equipped development environment: agents, skills, hooks, memory, MCP server configs, and application boilerplate, all tailored to your chosen stack.

Every generated project is ready for productive work from the very first Claude session. There is no manual configuration, no copy-pasting of boilerplate, and no guesswork about which agents or skills to set up. Genesis handles all of it.

Genesis itself is a Claude Code project. It uses the same agents, skills, and memory system it generates for others, making it both a tool and a reference implementation for how to structure Claude Code workspaces.

## Why Genesis?

Setting up a Claude Code project properly takes effort. You need to write a CLAUDE.md with project rules, configure permissions and hooks in settings.json, create agents tailored to your domain, set up skills for common workflows, configure MCP servers for your integrations, and seed memory with project context. Genesis automates all of this.

Instead of spending an hour configuring your environment, you spend two minutes describing your project and reviewing a plan. Genesis does the rest.

## Quick Start

```bash
git clone https://github.com/xeeva/Genesis.git ~/claude/genesis
cd ~/claude/genesis
claude
```

On first launch, Genesis guides you through environment detection and personalisation. Then describe what you want to build:

> "Create a Python FastAPI service called widget-api with PostgreSQL and Redis"

Genesis asks 1-2 follow-up questions, presents a plan, then generates everything. See the [getting started guide](getting-started) for a detailed walkthrough.

## How It Works

Genesis follows a four-phase workflow:

![Genesis Workflow](images/genesis-workflow.svg)

1. **Interview**: extracts project details from your description, asks only what is missing (2-4 questions max)
2. **Plan**: presents a structured generation plan with agents, skills, MCP servers, and folder layout for your approval
3. **Generate**: creates the entire project directory with all infrastructure and boilerplate
4. **Finalise**: initialises git, updates the project registry, and provides next steps

Read the [detailed walkthrough](how-it-works) for examples of each phase.

## What Gets Generated

Every generated project includes:

| Component | Description |
|-----------|-------------|
| `CLAUDE.md` | Project brain with stack-specific rules, coding standards, and testing mandates |
| `.claude/agents/` | Domain and workflow agents (test-runner, code-reviewer, doc-writer, plus 2-4 domain-specific) |
| `.claude/skills/` | Base skills (/test, /lint, /review, /commit) plus domain-specific skills |
| `.claude/settings.json` | Permissions, formatter hooks, and stop hooks |
| `.mcp.json` | MCP server configs for your integrations (database, GitHub, Playwright, AWS) |
| Memory files | User profile and project context seeded into Claude's memory |
| Application boilerplate | Entry points, package config, test setup, documentation |
| `.gitignore` | Stack-appropriate ignores |
| Git repo | Initialised with an initial commit |

See [generated project anatomy](generated-project) for a file-by-file breakdown.

## Available Skills

| Skill | Description |
|-------|-------------|
| `/genesis` | Bootstrap a new project (main entry point) |
| `/registry` | View all projects created by Genesis |
| `/validate` | Check a generated project is complete and well-formed |
| `/update` | Pull the latest Genesis updates from the remote repository |

## Supported Stacks

Genesis has built-in profiles for:

- **Node.js / TypeScript**: ESLint, Prettier, vitest/jest, ESM
- **Python**: ruff, pytest, mypy/pyright, PEP 621
- **Go**: gofmt, golangci-lint, table-driven tests
- **Rust**: rustfmt, clippy, cargo test, thiserror/anyhow
- **Ruby**: RuboCop, RSpec/Minitest, Bundler
- **Java / Kotlin**: Gradle/Maven, Spotless, JUnit 5

Each profile includes idiomatic folder structures, linter/formatter hooks, test configuration, error patterns, and permission sets. See [customisation](customisation) to learn how to add your own.

## First-Time Setup

On first launch, Genesis checks for `personalisation.md` and `environment.md`. If either is missing, it guides you through:

1. **Environment**: platform (Linux/macOS/WSL/Windows), shell, package manager
2. **Personalisation**: locale preferences, output style, role, experience level
3. **Prerequisite check**: verifies required tools are installed

These files are gitignored and preserved across Genesis updates. Your preferences travel with you; Genesis updates never overwrite them.

## Updating Genesis

Pull the latest improvements using the `/update` skill, or manually with `git pull`. Your personalisation and environment configuration are always preserved. See [updating](updating) for details.

## Contributing

Genesis welcomes contributions: new stack profiles, agent archetypes, templates, and documentation improvements. See the [contributing guide](https://github.com/xeeva/Genesis/blob/main/CONTRIBUTING.md) for details.

## Author

Created by [David Summers](https://github.com/xeeva).

## Licence

[MIT](https://github.com/xeeva/Genesis/blob/main/LICENSE)
