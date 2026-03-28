# Genesis

Genesis is a Claude Code project bootstrapper. It scaffolds fully-equipped Claude Code projects with opinionated configuration, agents, skills, hooks, memory, and application boilerplate.

## How It Works

1. Enter the Genesis directory and start a Claude session
2. Describe the project you want to build
3. Genesis interviews you briefly (2-4 questions) to fill in gaps
4. It presents a generation plan for your approval
5. On confirmation, it creates the entire project at `~/claude/<project-name>/`

## Usage

```bash
cd ~/claude/genesis/
claude
```

Then describe your project:

> "Create a Python FastAPI service called widget-api with PostgreSQL and Redis"

Genesis will ask 1-2 follow-up questions, present a plan, then generate everything.

## What Gets Generated

Every project includes:

- **CLAUDE.md** with project-specific rules, Australian English, testing mandate, and coding standards
- **.claude/agents/** with domain-specific and workflow agents (test-runner, code-reviewer, doc-writer)
- **.claude/skills/** with base skills (/test, /lint, /review, /commit) plus domain-specific skills
- **.claude/settings.json** with stack-appropriate permissions and hooks
- **.mcp.json** for relevant integrations (database, GitHub, etc.)
- **Memory files** with user profile and project context
- **Application boilerplate** (entry points, config, test setup)
- **Documentation** (docs/README.md, docs/architecture.md)
- **Git repo** initialised with an initial commit

## Skills

| Skill | Description |
|-------|-------------|
| `/genesis` | Bootstrap a new project (main entry point) |
| `/registry` | View all projects created by Genesis |
| `/validate` | Check a generated project is complete and well-formed |

## Project Structure

```
genesis/
├── CLAUDE.md                          # Master bootstrapper instructions
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
│       └── validate/                  # Project validator
└── docs/
    └── README.md                      # This file
```
