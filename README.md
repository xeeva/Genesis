<div align="center">

<!-- Hero Banner -->
<img src="docs/images/genesis-workflow.svg" alt="Genesis Workflow" width="100%" />

<br/>

# ⚡ G E N E S I S

### The Claude that builds Claudes

**Bootstrap fully-equipped Claude Code projects in under two minutes.**

<br/>

[![GitHub stars](https://img.shields.io/github/stars/xeeva/Genesis?style=for-the-badge&logo=github&logoColor=white&labelColor=0d1117&color=58a6ff)](https://github.com/xeeva/Genesis)
[![License: MIT](https://img.shields.io/badge/Licence-MIT-F7DF1E?style=for-the-badge&logo=opensourceinitiative&logoColor=black)](LICENSE)
[![Docs](https://img.shields.io/badge/Docs-GitHub%20Pages-A371F7?style=for-the-badge&logo=readthedocs&logoColor=white)](https://xeeva.github.io/Genesis)
[![Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-FF6B35?style=for-the-badge&logo=anthropic&logoColor=white)](https://docs.anthropic.com/en/docs/claude-code)

<br/>

<table>
<tr>
<td>

```
 ██████╗ ███████╗███╗   ██╗███████╗███████╗██╗███████╗
██╔════╝ ██╔════╝████╗  ██║██╔════╝██╔════╝██║██╔════╝
██║  ███╗█████╗  ██╔██╗ ██║█████╗  ███████╗██║███████╗
██║   ██║██╔══╝  ██║╚██╗██║██╔══╝  ╚════██║██║╚════██║
╚██████╔╝███████╗██║ ╚████║███████╗███████║██║███████║
 ╚═════╝ ╚══════╝╚═╝  ╚═══╝╚══════╝╚══════╝╚═╝╚══════╝
```

</td>
</tr>
</table>

---

**You describe it. Genesis builds it. Claude runs it.**

</div>

<br/>

## What is Genesis?

Genesis is a **Claude Code project that creates other Claude Code projects**. You describe what you want to build, and Genesis scaffolds a fully-equipped development environment -- agents, skills, hooks, memory, MCP server configs, and application boilerplate -- all tailored to your chosen stack.

> Every generated project is ready for productive work from the very first Claude session. No manual configuration. No copy-pasting boilerplate. No guesswork.

Genesis itself is a Claude Code project. It uses the same agents, skills, and memory system it generates for others, making it both a tool and a reference implementation.

<br/>

## Quick Start

```bash
git clone https://github.com/xeeva/Genesis.git ~/claude/genesis
cd ~/claude/genesis
claude
```

Then describe what you want to build:

> *"Create a Python FastAPI service called widget-api with PostgreSQL and Redis"*

Genesis asks 1-2 follow-up questions, presents a plan, then generates everything.

<br/>

## How It Works

Genesis follows a strict **four-phase workflow**:

<table>
<tr>
<td align="center" width="25%">

### 🔍 Interview
Extract project details from your description. Ask only what is missing.

**1-4 questions max**

</td>
<td align="center" width="25%">

### 📐 Plan
Present agents, skills, MCP servers, and folder layout for approval.

**Review before generating**

</td>
<td align="center" width="25%">

### 🏗️ Generate
Create the entire project directory with all infrastructure and boilerplate.

**Single-pass generation**

</td>
<td align="center" width="25%">

### 🚀 Finalise
Initialise git, update registry, provide next steps.

**Ready to code**

</td>
</tr>
</table>

See the [detailed walkthrough](https://xeeva.github.io/Genesis/how-it-works) for examples.

<br/>

## What Gets Generated

<table>
<tr>
<th align="left">Component</th>
<th align="left">Description</th>
</tr>
<tr>
<td><code>CLAUDE.md</code></td>
<td>Project brain with stack-specific rules, coding standards, and testing mandates</td>
</tr>
<tr>
<td><code>.claude/agents/</code></td>
<td>Domain and workflow agents (test-runner, code-reviewer, doc-writer, plus 2-4 domain-specific)</td>
</tr>
<tr>
<td><code>.claude/skills/</code></td>
<td>Base skills (<code>/test</code>, <code>/lint</code>, <code>/review</code>, <code>/commit</code>) plus domain-specific skills</td>
</tr>
<tr>
<td><code>.claude/settings.json</code></td>
<td>Permissions, formatter hooks, and stop hooks</td>
</tr>
<tr>
<td><code>.mcp.json</code></td>
<td>MCP server configs for your integrations (database, GitHub, Playwright, AWS)</td>
</tr>
<tr>
<td><code>Memory files</code></td>
<td>User profile and project context seeded into Claude's memory</td>
</tr>
<tr>
<td><code>Application code</code></td>
<td>Entry points, package config, test setup, documentation</td>
</tr>
<tr>
<td><code>.gitignore</code> + Git repo</td>
<td>Stack-appropriate ignores with initial commit</td>
</tr>
</table>

<br/>

<div align="center">

<img src="docs/images/generated-project-arch.svg" alt="Generated Project Architecture" width="100%" />

</div>

<br/>

## Supported Stacks

<table>
<tr>
<td align="center" width="16%">

**Node.js / TS**

ESLint, Prettier
vitest/jest, ESM

</td>
<td align="center" width="16%">

**Python**

ruff, pytest
mypy/pyright

</td>
<td align="center" width="16%">

**Go**

gofmt, golangci-lint
table-driven tests

</td>
<td align="center" width="16%">

**Rust**

rustfmt, clippy
cargo test

</td>
<td align="center" width="16%">

**Ruby**

RuboCop
RSpec/Minitest

</td>
<td align="center" width="16%">

**Java / Kotlin**

Gradle/Maven
Spotless, JUnit 5

</td>
</tr>
</table>

Each profile includes idiomatic folder structures, linter/formatter hooks, test configuration, error patterns, and permission sets.

<br/>

## Available Skills

| Skill | Description |
|-------|-------------|
| `/genesis` | Bootstrap a new project (main entry point) |
| `/registry` | View all projects created by Genesis |
| `/validate` | Check a generated project is complete and well-formed |
| `/update` | Pull the latest Genesis updates from the remote repository |

<br/>

## Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) | v1.x+ | AI coding assistant (the runtime) |
| [git](https://git-scm.com/) | v2.x+ | Version control |
| [Node.js](https://nodejs.org/) | v18+ | Required by Claude Code and MCP servers |

You also need a supported shell: bash, zsh, or PowerShell.

<details>
<summary><strong>Platform-specific installation</strong></summary>

| Platform | Install tools via |
|----------|-------------------|
| Linux (Debian/Ubuntu) | `apt install git nodejs npm` |
| Linux (Fedora) | `dnf install git nodejs npm` |
| macOS | `brew install git node` |
| WSL | Use your Linux distro's package manager |
| Windows | [Scoop](https://scoop.sh/) or the official installers |

Claude Code CLI: `npm install -g @anthropic-ai/claude-code`

</details>

<br/>

## First-Time Setup

On first launch, Genesis checks for two configuration files: `personalisation.md` and `environment.md`. If either is missing, it guides you through setup:

1. **Environment detection**: platform, shell, package manager
2. **Personalisation**: locale preferences, output style, role, experience level
3. **Prerequisite check**: verifies that required tools are installed

These files are gitignored and preserved across Genesis updates. Re-run setup at any time by deleting either file.

<br/>

## Updating Genesis

```
/update
```

Or manually: `cd ~/claude/genesis && git pull origin main`

Your `personalisation.md` and `environment.md` are always preserved. Only the core scaffold is updated.

<br/>

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

<br/>

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding stacks, agents, templates, and more.

<br/>

<div align="center">

---

## Disclaimer

Genesis is an AI-powered code generation tool provided **"as is" without warranty of any kind**. You use this tool entirely at your own risk. The author accepts no responsibility for the correctness, security, or suitability of any generated output. Review all generated code before use. See [DISCLAIMER.md](DISCLAIMER.md) for the full terms.

---

**Copyright (c) 2026 David Summers. All rights reserved.**

[MIT Licence](LICENSE) | [Disclaimer](DISCLAIMER.md) | [Contributing](CONTRIBUTING.md)

<br/>

<sub>Genesis is a Claude Code project that bootstraps Claude Code projects. It's Claudes all the way down.</sub>

</div>
