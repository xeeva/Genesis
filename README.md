```
 ██████╗ ███████╗███╗   ██╗███████╗███████╗██╗███████╗
██╔════╝ ██╔════╝████╗  ██║██╔════╝██╔════╝██║██╔════╝
██║  ███╗█████╗  ██╔██╗ ██║█████╗  ███████╗██║███████╗
██║   ██║██╔══╝  ██║╚██╗██║██╔══╝  ╚════██║██║╚════██║
╚██████╔╝███████╗██║ ╚████║███████╗███████║██║███████║
 ╚═════╝ ╚══════╝╚═╝  ╚═══╝╚══════╝╚══════╝╚═╝╚══════╝
```

**The Claude that builds Claudes.** Bootstrap fully-equipped Claude Code projects in under two minutes.

[![GitHub stars](https://img.shields.io/github/stars/xeeva/Genesis)](https://github.com/xeeva/Genesis)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/docs-GitHub%20Pages-brightgreen)](https://xeeva.github.io/Genesis)

---

## What is Genesis?

Genesis is a Claude Code project that creates other Claude Code projects. You describe what you want to build, and Genesis scaffolds a fully-equipped development environment: agents, skills, hooks, memory, MCP server configs, and application boilerplate, all tailored to your chosen stack.

Every generated project is ready for productive work from the very first Claude session. There is no manual configuration, no copy-pasting of boilerplate, and no guesswork about which agents or skills to set up. Genesis handles all of it.

Genesis itself is a Claude Code project. It uses the same agents, skills, and memory system it generates for others, making it both a tool and a reference implementation for how to structure Claude Code workspaces.

## Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) | v1.x+ | AI coding assistant (the runtime) |
| [git](https://git-scm.com/) | v2.x+ | Version control |
| [Node.js](https://nodejs.org/) | v18+ | Required by Claude Code and MCP servers |

You also need a supported shell: bash, zsh, or PowerShell.

**Platform-specific installation:**

| Platform | Install tools via |
|----------|-------------------|
| Linux (Debian/Ubuntu) | `apt install git nodejs npm` |
| Linux (Fedora) | `dnf install git nodejs npm` |
| macOS | `brew install git node` |
| WSL | Use your Linux distro's package manager |
| Windows | [Scoop](https://scoop.sh/) or the official installers |

Claude Code CLI installation: `npm install -g @anthropic-ai/claude-code`

## Quick Start

```bash
git clone https://github.com/xeeva/Genesis.git ~/claude/genesis
cd ~/claude/genesis
claude
```

Then describe what you want to build:

> "Create a Python FastAPI service called widget-api with PostgreSQL and Redis"

Genesis asks 1-2 follow-up questions, presents a plan, then generates everything.

## First-Time Setup

On first launch, Genesis checks for two configuration files: `personalisation.md` and `environment.md`. If either is missing, it guides you through setup:

1. **Environment detection**: platform (Linux/macOS/WSL/Windows), shell, package manager
2. **Personalisation**: locale preferences, output style, role, experience level
3. **Prerequisite check**: verifies that required tools are installed

These files are created from the `.example` templates, gitignored, and preserved across Genesis updates. You can re-run setup at any time by deleting either file and starting a new session.

## How It Works

Genesis follows a four-phase workflow:

1. **Interview**: extracts project details from your description, asks only what is missing (2-4 questions max)
2. **Plan**: presents a structured generation plan with agents, skills, MCP servers, and folder layout for your approval
3. **Generate**: creates the entire project directory with all infrastructure and boilerplate
4. **Finalise**: initialises git, updates the project registry, and provides next steps

See the [detailed walkthrough](https://xeeva.github.io/Genesis/how-it-works) for examples.

## What Gets Generated

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

Each profile includes idiomatic folder structures, linter/formatter hooks, test configuration, error patterns, and permission sets.

## Updating Genesis

Pull the latest improvements using the update skill:

```
/update
```

Or manually:

```bash
cd ~/claude/genesis && git pull origin main
```

Your `personalisation.md` and `environment.md` are always preserved. Only the core scaffold (templates, agents, skills, references) is updated. After updating, check whether the `.example` files have new configuration options you might want to add to your own files.

## Project Structure

```
genesis/
├── CLAUDE.md                          # Master bootstrapper instructions
├── personalisation.md.example         # Template for user preferences
├── environment.md.example             # Template for platform config
├── .claude/
│   ├── settings.json                  # Permissions and hooks
│   ├── agents/                        # 5 specialised agents
│   │   ├── interviewer.md
│   │   ├── planner.md
│   │   ├── scaffold-writer.md
│   │   ├── claude-infra-writer.md
│   │   └── finaliser.md
│   └── skills/
│       ├── genesis/                   # Main bootstrap skill
│       │   ├── SKILL.md
│       │   ├── templates/             # File templates for generation
│       │   └── references/            # Stack profiles and agent catalogue
│       ├── registry/                  # Project registry viewer
│       ├── validate/                  # Project validator
│       └── update/                    # Self-update skill
├── docs/                              # Documentation and GitHub Pages
├── CONTRIBUTING.md                    # Contribution guidelines
├── CODE_OF_CONDUCT.md                 # Contributor Covenant
└── LICENSE                            # MIT licence
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding stacks, agents, templates, and more.

## Author

Created by **David Summers**.

## Licence

[MIT](LICENSE)
