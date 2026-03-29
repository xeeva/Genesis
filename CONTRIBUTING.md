# Contributing to Genesis

Thanks for your interest in contributing to Genesis. This guide covers how to contribute and what to keep in mind.

## How to Contribute

1. **Fork** the repository on GitHub
2. **Clone** your fork: `git clone https://github.com/<you>/Genesis.git ~/claude/genesis-dev`
3. **Create a feature branch**: `git checkout -b feat/your-feature`
4. **Make your changes** following the conventions described below
5. **Test** by generating a project and verifying the output
6. **Commit** using Conventional Commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`
7. **Push** your branch and open a pull request

## Adding a New Stack Profile

Stack profiles live in `.claude/skills/genesis/references/stack-profiles.md`.

### Required Sections

Every stack profile must include all of the following:

1. **Project Config**: package manager, config files, version pinning mechanism
2. **Linting and Formatting**: tools, hook commands (PostToolUse format), configuration files
3. **Testing**: framework, file naming conventions, coverage tool
4. **Permissions**: Bash permission entries for `.claude/settings.json` (JSON string array format)
5. **Commit Style**: Conventional Commits or plain imperative
6. **Folder Structure**: idiomatic directory layout as an ASCII tree
7. **Error Patterns**: language-specific error handling conventions and anti-patterns

### Format

Follow the existing section format exactly. Use an `##` heading for the stack name and `###` headings for each subsection. Include code blocks where appropriate.

### Testing Your Stack Profile

1. Start a Genesis session: `cd ~/claude/genesis && claude`
2. Request a project using the new stack
3. Verify the generated project has correct folder structure, permissions, hooks, and configuration
4. Run `/validate` on the generated project

## Adding a New Agent Archetype

Agent definitions live in `.claude/skills/genesis/references/agent-catalogue.md`.

### Required Fields

Each agent entry must include:

- **Name** (bold, as a subheading): kebab-case, descriptive
- **Purpose**: one sentence describing the agent's role
- **Model**: `sonnet` for most agents; `opus` for security-critical or complex architectural review
- **Tools**: which Claude Code tools the agent needs (subset of: Read, Write, Edit, Grep, Glob, Bash)
- **Best for**: project types where this agent is most valuable

### Placement

Add your agent under the appropriate category heading. If no existing category fits, create a new `### Category Name` section. Update the **Selection Guidelines** at the bottom if your agent has specific inclusion criteria.

### Guidelines

- Every agent should have a clear, non-overlapping purpose with existing agents
- Prefer `sonnet` model tier unless the agent handles security or complex architecture
- Limit tools to only what the agent genuinely needs

## Adding or Modifying Templates

Templates live in `.claude/skills/genesis/templates/` and use `{{PLACEHOLDER}}` syntax.

### Conventions

- File extension: `.tmpl`
- Placeholders: uppercase with underscores (e.g. `{{PROJECT_NAME}}`)
- Include a comment at the top explaining the template's purpose
- Templates provide structure, not complete implementations; keep them minimal
- Use existing placeholder names where applicable (check other templates for conventions)

### After Adding a Template

Ensure the `claude-infra-writer` or `scaffold-writer` agent knows when and how to use it. If necessary, update the agent's instructions or the generation workflow in `.claude/skills/genesis/SKILL.md`.

## Improving Documentation

Documentation lives in `docs/` and is served via GitHub Pages with the Cayman theme.

### Conventions

- Use Australian English for all upstream contributions (colour, behaviour, organisation, initialise, licence, defence, analyse, catalogue)
- Never use em dashes; use commas, semicolons, colons, or full stops instead
- Markdown files use ATX-style headings (`#`, `##`, `###`)
- Every docs page includes YAML frontmatter with `layout: default` and `title:`
- Every docs page includes a navigation bar linking to other pages

## Testing Your Changes

The best way to test Genesis changes is end-to-end:

1. Run `cd ~/claude/genesis && claude`
2. Request a project that exercises your changes (specific stack, agent type, or template)
3. Check the generated output matches expectations
4. Run `/validate` on the generated project to verify infrastructure completeness
5. Open a Claude session in the generated project and verify it works

There is no automated test suite for Genesis; it is tested by generating projects and validating them.

## Code Style

- Australian English in all documentation and comments
- No em dashes; use commas, semicolons, colons, or full stops
- Markdown files use ATX-style headings
- YAML frontmatter in skill files must be valid
- JSON files must be valid and parseable
- Keep commit messages concise and explanatory (the "why", not the "what")

## Reporting Issues

Use the GitHub issue templates for:

- **Bug reports**: something is not working as expected
- **Feature requests**: ideas for new functionality
- **Stack requests**: request support for a new language or framework

For general questions and discussion, use [GitHub Discussions](https://github.com/xeeva/Genesis/discussions).

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## Licence

By contributing, you agree that your contributions will be licensed under the [MIT licence](LICENSE).
