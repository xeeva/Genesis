---
layout: default
title: Genesis
---

## The Problem

Claude Code is powerful, but getting the most out of it requires significant upfront configuration. A well-equipped project needs:

- A **CLAUDE.md** with coding standards, stack conventions, testing rules, and error handling patterns
- **Agents** tailored to your domain: API designers, data modellers, security reviewers, each with role-specific instructions
- **Skills** for common workflows: running tests, linting, code review, committing
- **Hooks** that auto-format code after edits and remind you to test before stopping
- **Permissions** scoped to your stack's specific tools
- **MCP server configs** for your databases, APIs, and external services
- **Memory files** seeding Claude with your profile, preferences, and project context

Most developers skip this setup entirely, or cobble together a minimal CLAUDE.md and miss the rest. The result: Claude works, but not nearly as well as it could. You lose the specialised agents, the automated workflows, the domain-specific skills, and the contextual memory that make Claude Code truly productive.

## The Solution

Genesis is a Claude Code project that creates other Claude Code projects. You describe what you want to build, and Genesis scaffolds everything listed above in under two minutes, tailored to your stack and your Claude plan tier.

Every generated project is ready for productive work from the very first session. No manual configuration. No copy-pasting boilerplate. No guesswork about which agents or skills to set up.

Genesis itself is a Claude Code project. It uses the same agents, skills, and memory system it generates for others, making it both a tool and a reference implementation.

## Quick Start

**Linux / macOS / WSL:**
```bash
git clone https://github.com/xeeva/Genesis.git ~/claude/genesis
cd ~/claude/genesis
claude
```

**Windows (PowerShell):**
```powershell
git clone https://github.com/xeeva/Genesis.git $HOME\claude\genesis
cd $HOME\claude\genesis
claude
```

On first launch, Genesis guides you through environment detection and personalisation. Then describe what you want to build:

> "Create a Python FastAPI service called widget-api with PostgreSQL and Redis"

Genesis asks 1-2 follow-up questions, presents a plan, then generates everything. See the [getting started guide](getting-started) for a detailed walkthrough.

## How It Works

Genesis follows a four-phase workflow:

1. **Interview** -- extracts project details from your description, asks only what is missing (2-4 questions max)
2. **Plan** -- presents a structured generation plan with agents, skills, MCP servers, and folder layout for your approval
3. **Generate** -- creates the entire project directory with all infrastructure and boilerplate
4. **Finalise** -- initialises git, updates the project registry, and provides next steps

![Genesis Workflow](images/genesis-workflow.svg)

Read the [detailed walkthrough](how-it-works) for examples of each phase.

## What Gets Generated

Every generated project includes:

- **CLAUDE.md** -- project brain with stack-specific rules, coding standards, and testing mandates
- **Agents** -- domain and workflow agents (test-runner, code-reviewer, doc-writer, plus 2-4 domain-specific)
- **Skills** -- base skills (/test, /lint, /review, /commit) plus domain-specific skills
- **Settings** -- permissions, formatter hooks, and stop hooks
- **MCP servers** -- configs for your integrations (database, GitHub, Playwright, AWS)
- **Memory** -- user profile and project context seeded into Claude's memory
- **Application boilerplate** -- entry points, package config, test setup, documentation
- **Git repo** -- stack-appropriate .gitignore with initial commit

See [generated project anatomy](generated-project) for a file-by-file breakdown.

## Available Skills

- `/genesis` -- bootstrap a new project (main entry point)
- `/registry` -- view all projects created by Genesis
- `/validate` -- check a generated project is complete and well-formed
- `/update` -- pull the latest Genesis updates from the remote repository

## Supported Stacks

Genesis has built-in profiles for:

- **Node.js / TypeScript** -- ESLint, Prettier, vitest/jest, ESM
- **Python** -- ruff, pytest, mypy/pyright, PEP 621
- **Go** -- gofmt, golangci-lint, table-driven tests
- **Rust** -- rustfmt, clippy, cargo test, thiserror/anyhow
- **Ruby** -- RuboCop, RSpec/Minitest, Bundler
- **Java / Kotlin** -- Gradle/Maven, Spotless, JUnit 5

Each profile includes idiomatic folder structures, linter/formatter hooks, test configuration, error patterns, and permission sets. See [customisation](customisation) to learn how to add your own.

## First-Time Setup

On first launch, Genesis checks for `personalisation.md` and `environment.md`. If either is missing, it guides you through:

1. **Environment** -- platform (Linux/macOS/WSL/Windows), shell, package manager
2. **Personalisation** -- locale preferences, output style, role, experience level
3. **Prerequisite check** -- verifies required tools are installed

These files are gitignored and preserved across Genesis updates. Your preferences travel with you; Genesis updates never overwrite them.

## Updating

Pull the latest improvements using the `/update` skill, or manually with `git pull`. Your personalisation and environment configuration are always preserved. See [updating](updating) for details.

## Contributing

Genesis welcomes contributions: new stack profiles, agent archetypes, templates, and documentation improvements. See the [contributing guide](https://github.com/xeeva/Genesis/blob/main/CONTRIBUTING.md) for details.

---

**Copyright (c) 2026 David Summers. All rights reserved.**

[MIT Licence](https://github.com/xeeva/Genesis/blob/main/LICENSE) | [Disclaimer](https://github.com/xeeva/Genesis/blob/main/DISCLAIMER.md)
