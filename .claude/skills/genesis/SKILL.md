---
name: genesis
description: Bootstrap a new Claude Code project with full infrastructure. Use this when the user wants to create a new project, scaffold a project, or says "new project", "create", "bootstrap", or "build me".
user-invocable: true
---

# Genesis: Project Bootstrapper

You are executing the Genesis bootstrap workflow. Follow these phases strictly.

## Phase 1: Interview

Read the user's request carefully. Extract everything you can before asking questions.

**Required information:**
1. Project name (kebab-case)
2. Purpose (one sentence)
3. Tech stack (language, framework, runtime)
4. Key integrations (databases, APIs, auth, external services)

**Rules:**
- If the user provided 3+ of these, ask only what's missing (1-2 questions max)
- If the user was vague, ask up to 4 targeted questions
- Never re-ask what was already stated
- Use AskUserQuestion for a clean interview experience

## Phase 2: Plan

Once you have all information, produce this plan and present it:

```
Project: <name>
Path: ~/claude/<name>/
Stack: <language, framework, key dependencies>

Agents (domain):
- <agent-name> -- <one-line purpose>

Agents (workflow):
- test-runner -- run and analyse test results
- code-reviewer -- review code quality and correctness
- doc-writer -- generate and update documentation

Skills (base):
- /test -- run the test suite
- /lint -- run linters and formatters
- /review -- trigger code review
- /commit -- stage and commit changes

Skills (dynamic):
- /<skill-name> -- <one-line purpose>

MCP Servers:
- <server-name> (or "None" if no integrations)

Folder Structure:
<tree of top-level directories>
```

Wait for user confirmation. If they request changes, adjust and re-present.

## Phase 3: Generate

After confirmation, create the project. Read the templates and references before generating:

1. Read `.claude/skills/genesis/templates/` for all template files
2. Read `.claude/skills/genesis/references/stack-profiles.md` for stack-specific conventions
3. Read `.claude/skills/genesis/references/agent-catalogue.md` for agent definitions

**Generation order:**

### 3.1 Directory structure
```bash
mkdir -p ~/claude/<name>/.claude/agents
mkdir -p ~/claude/<name>/.claude/skills/<skill-name> (for each skill)
mkdir -p ~/claude/<name>/docs
mkdir -p ~/claude/<name>/<stack-specific-dirs>
```

### 3.2 CLAUDE.md
Use `templates/CLAUDE.md.tmpl` as the skeleton. Fill all placeholders with project-specific content. Ensure every section from the Global Rules in Genesis's CLAUDE.md is present. This is the most important file -- it governs the entire project.

### 3.3 .claude/settings.json
Use `templates/settings.json.tmpl` as skeleton. Configure:
- Permissions appropriate to the stack (consult stack-profiles.md)
- PostToolUse hooks for the stack's formatter and linter
- Stop hook for test/commit reminders

Write valid JSON, not a template. Replace all placeholders with actual values.

### 3.4 Agents
For each agent in the plan:
- Use `templates/agent.md.tmpl` as skeleton
- Fill with role-specific instructions from agent-catalogue.md
- Write to `~/claude/<name>/.claude/agents/<agent-name>.md`

### 3.5 Skills
For each skill in the plan:
- Use `templates/skill.md.tmpl` as skeleton
- Write a clear description that tells Claude when to invoke it
- Include the specific commands or workflow for the stack
- Write to `~/claude/<name>/.claude/skills/<skill-name>/SKILL.md`

### 3.6 .mcp.json (if needed)
Use `templates/mcp.json.tmpl` as skeleton. Only include servers for integrations mentioned in the plan. Write valid JSON.

### 3.7 Memory files
Create the memory directory at `~/.claude/projects/-home-xeeva-claude-<name>/memory/`

Write three files:
- `user-profile.md` from `templates/memory-user.md.tmpl` (copy verbatim)
- `project-context.md` from `templates/memory-project.md.tmpl` (fill placeholders)
- `MEMORY.md` index file pointing to both

### 3.8 Application boilerplate
Consult stack-profiles.md for the appropriate structure. Generate:
- Entry point file(s)
- Package/project config (package.json, pyproject.toml, go.mod, Cargo.toml, etc.)
- Test configuration and a sample test
- `.gitignore` appropriate to the stack
- `docs/README.md` with project name, purpose, setup instructions, and usage
- `docs/architecture.md` with high-level architecture description
- Any stack-specific config files (tsconfig.json, ruff.toml, .golangci.yml, etc.)

## Phase 4: Finalise

1. Initialise git and create the first commit:
```bash
cd ~/claude/<name> && git init && git add -A && git commit -m "Initial scaffold from Genesis"
```

2. Update the project registry. Read the current registry from Genesis memory, append the new project entry, and write it back.

3. Print a summary:
```
Project created: ~/claude/<name>/
Stack: <stack>
Agents: <count> (<names>)
Skills: <count> (<names>)
MCP: <servers or "none">

Next steps:
  cd ~/claude/<name>/
  claude
```

## Important Notes

- All generated content must use Australian English
- Never use em dashes in any generated file
- If the target directory already exists, refuse and explain
- Write valid, parseable JSON for settings.json and .mcp.json
- Write valid YAML frontmatter for all skill files
- Every generated CLAUDE.md must include all Global Rules from Genesis's CLAUDE.md
