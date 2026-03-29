---
layout: default
title: Customisation
---

# Customisation

**Navigation:** [Home](.) | [Getting Started](getting-started) | [How It Works](how-it-works) | [Architecture](architecture) | [Generated Projects](generated-project) | [Plans and Context](plans-and-context) | [Updating](updating) | [FAQ](faq)

---

Genesis is designed to be extended. You can add new stack profiles, agent archetypes, templates, and skills without modifying the core workflow.

## Adding a New Stack Profile

Stack profiles live in `.claude/skills/genesis/references/stack-profiles.md`. To add support for a new language or framework, append a new section following the existing format.

### Required Sections

Every stack profile must include:

1. **Project Config**: package manager, config files, version pinning
2. **Linting and Formatting**: tools, hook commands, configuration
3. **Testing**: framework, file conventions, coverage tools
4. **Permissions**: Bash permission entries for `settings.json`
5. **Commit Style**: Conventional Commits or plain imperative
6. **Folder Structure**: idiomatic directory layout
7. **Error Patterns**: language-specific error handling conventions

### Example: Adding Elixir

```markdown
## Elixir

### Project Config
- `mix.exs` for project configuration and dependencies
- `.tool-versions` for runtime version pinning (via asdf)

### Linting and Formatting
- `mix format` for formatting (built-in)
- Credo for linting
- Hook: `mix format` on PostToolUse for `.ex`, `.exs` files

### Testing
- ExUnit (built-in)
- Test files: `test/**/*_test.exs`
- Coverage: `mix test --cover`

### Permissions
\`\`\`json
"Bash(mix:*)", "Bash(elixir:*)", "Bash(iex:*)"
\`\`\`

### Commit Style
- Conventional Commits

### Folder Structure
\`\`\`
lib/
  <app_name>/
  <app_name>.ex
test/
config/
priv/
\`\`\`

### Error Patterns
- Use tagged tuples: `{:ok, result}` and `{:error, reason}`
- Pattern match on error tuples at call sites
- Never silently ignore error tuples
```

## Adding a New Agent to the Catalogue

Agent definitions live in `.claude/skills/genesis/references/agent-catalogue.md`. Add your agent under the appropriate category (or create a new category).

### Required Fields

Each agent entry needs:

- **Name**: kebab-case identifier (used as the filename)
- **Purpose**: one sentence describing what it does
- **Model**: `sonnet` for most agents; `opus` for security-critical or complex architectural review
- **Tools**: which Claude Code tools the agent needs (Read, Write, Edit, Grep, Glob, Bash)
- **Best for**: project types where this agent is most valuable

### Example: Adding a Monitoring Agent

```markdown
**monitoring-designer**
- **Purpose:** Design observability strategy: metrics, logging, tracing, and alerting
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **Best for:** Production services, microservices, cloud-native applications
```

After adding the agent, update the **Selection Guidelines** section at the bottom of the catalogue if needed.

## Creating New Templates

Templates live in `.claude/skills/genesis/templates/` and use `{{PLACEHOLDER}}` syntax.

### Conventions

- File extension: `.tmpl` (e.g. `dockerfile.tmpl`)
- Placeholders are uppercase with underscores: `{{PROJECT_NAME}}`, `{{STACK_LANGUAGE}}`
- Comments explaining the purpose of each section help the `claude-infra-writer` agent fill them correctly
- Templates should be minimal; they provide structure, not complete implementations

### Adding a Template

1. Create the file in `.claude/skills/genesis/templates/`
2. Use existing placeholders where applicable (check other templates for the naming conventions)
3. Ensure the `claude-infra-writer` agent or `scaffold-writer` agent knows when and how to use it by documenting the template's purpose in a comment at the top

### Existing Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{{PROJECT_NAME}}` | The project name in kebab-case |
| `{{STACK}}` | The primary language/framework |
| `{{PURPOSE}}` | One-sentence project description |
| `{{PERMISSIONS}}` | Stack-appropriate Bash permissions |
| `{{HOOKS}}` | Formatter and linter hook configuration |
| `{{AGENTS}}` | List of agents for the project |
| `{{SKILLS}}` | List of skills for the project |

## Adding New Skills

Skills live in `.claude/skills/<skill-name>/SKILL.md`. Each skill needs a YAML frontmatter section and a Markdown body with instructions.

### Skill File Format

```markdown
---
name: skill-name
description: One sentence describing when to invoke this skill.
user-invocable: true
---

# Skill Title

Instructions for Claude when this skill is invoked.

## Steps

1. Step one
2. Step two
3. Step three
```

### Adding a Skill to Genesis

To add a new skill that Genesis offers to generated projects:

1. Define the skill in the **Skill Selection Guidelines** section of `CLAUDE.md`
2. Map it to appropriate project types (API, frontend, CLI, data, etc.)
3. The `claude-infra-writer` agent will generate the skill's `SKILL.md` with stack-specific instructions based on its description

### Adding a Skill to Genesis Itself

To add a new skill that runs within Genesis:

1. Create `.claude/skills/<skill-name>/SKILL.md`
2. Add the skill to the banner in `CLAUDE.md` (the `Skills:` line in the welcome box)
3. Update the Available Skills table in the root `README.md` and `docs/index.md`

## Modifying Global Rules

Global rules live in the `CLAUDE.md` file under the **Global Rules** section. These rules are included verbatim in every generated project's `CLAUDE.md`.

### What You Can Change

- **Error handling**: adjust the error handling philosophy
- **Code standards**: modify clean code thresholds (nesting levels, function length)
- **Testing**: change testing requirements or conventions
- **Documentation**: adjust documentation mandates
- **Commit style**: change the default commit conventions

### What is Sourced from personalisation.md

Language/locale and output style rules are read from `personalisation.md`, not hardcoded in `CLAUDE.md`. To change these, edit your `personalisation.md` directly.

### Caution

Changes to global rules affect all future generated projects. Existing projects are not affected; they have their own copy of the rules in their `CLAUDE.md`.

## Overriding the Scaffold Profile

The scaffold profile (lean, standard, full) is normally derived from your Claude plan tier during first-time setup. You can override it in two ways:

1. **Permanently:** Edit `environment.md` and change the `Scaffold profile` value under `## Claude Plan`
2. **Per-project:** During the plan phase, tell Genesis you want a different profile (e.g. "keep it lean" or "give me the full scaffold")

This is useful for API key users who have varying context budgets, or for any user who prefers a lighter or heavier scaffold regardless of their plan tier.

## Contributing Back Upstream

If you have added a useful stack profile, agent, template, or skill, consider contributing it back to Genesis.

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Make your changes following the existing format and conventions
4. Test by generating a project that uses your addition
5. Run `/validate` on the generated project
6. Open a pull request

See [CONTRIBUTING.md](https://github.com/xeeva/Genesis/blob/main/CONTRIBUTING.md) for full guidelines.

### Style Requirements

- All documentation uses Australian English (or your configured locale for local use; Australian English for upstream contributions)
- Never use em dashes; use commas, semicolons, colons, or full stops
- Follow existing formatting conventions in whichever file you are editing
