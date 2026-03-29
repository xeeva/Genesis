---
layout: default
title: Claude Plans and Context
---

# Claude Plans and Context

[Home](.) | [Start](getting-started) | [How It Works](how-it-works) | [Architecture](architecture) | [Projects](generated-project) | [Customise](customisation) | [Update](updating) | [FAQ](faq)

---

Genesis is designed to work across all Claude plans that support Claude Code. It automatically adapts the size and complexity of the generated scaffold to make the best use of your available context window.

## How Context Works in Claude Code

Every Claude Code session has a **context window**: the total amount of text (measured in tokens) that Claude can consider at once. This includes the system prompt, your CLAUDE.md, settings, loaded agents, loaded skills, memory files, conversation history, file contents you have read, and Claude's own responses.

When the context window fills up, Claude Code automatically **compacts** (summarises) the conversation to free up space. You can also trigger this manually with `/compact`. While compaction preserves the key points, some detail is lost, so larger context windows allow for longer, more nuanced working sessions.

## Plan Comparison

| Feature | Pro | Max | ProMax |
|---------|-----|-----|--------|
| **Context window** | 200k tokens | 200k tokens | 1M tokens |
| **Scaffold profile** | lean | standard | full |
| **Domain agents** | 1-2 per project | 2-3 per project | 3-4 per project |
| **Dynamic skills** | 1-2 per project | 2-3 per project | 3-4 per project |
| **CLAUDE.md template** | Condensed | Standard | Detailed |
| **Memory files** | Consolidated (1 file) | Full (3 files) | Full (3 files) |
| **Scaffold cost** | ~5-8k tokens (2.5-4%) | ~10-15k tokens (5-7.5%) | ~15-25k tokens (1.5-2.5%) |
| **Effective working context** | ~190k tokens | ~185k tokens | ~975k tokens |
| **Session length before compaction** | Moderate | Moderate | Extended |
| **Rate limits** | Standard | Higher | Highest |

Note: API key users have variable context windows depending on the model. Genesis asks for the context budget during setup.

## What Each Plan Gets

### Pro: Lean Scaffold

The Pro plan provides 200k tokens of context. Genesis generates a lean scaffold that typically consumes 2.5-4% of that budget, leaving approximately 190k tokens for your work.

**What is included:**
- Full CLAUDE.md with project rules, standards, and conventions (condensed format)
- The three mandatory workflow agents: test-runner, code-reviewer, doc-writer
- One or two domain agents: the single most critical agent for your project type (e.g. api-designer for an API, data-modeller for a database project)
- Four base skills: /test, /lint, /review, /commit
- One or two domain-specific skills
- Consolidated memory (user profile and project context in a single MEMORY.md)
- Statusline showing context usage percentage
- Full application scaffold (entry points, package config, test setup, docs)

**What this means in practice:**
You have everything you need to build your project productively. The condensed CLAUDE.md still contains all the coding standards, testing rules, and stack-specific conventions. You have a focused set of agents covering the most important domain concern. For longer or more complex sessions, you may need to use `/compact` to free up context. The statusline helps you monitor when this is needed.

**Tip:** If you find yourself frequently hitting context limits, consider upgrading to Max or ProMax for more headroom.

### Max: Standard Scaffold

The Max plan provides 200k tokens of context (same as Pro) but with higher rate limits and more messages per day. Genesis generates a standard scaffold that consumes 5-7.5% of the context budget.

**What is included:**
Everything in the lean scaffold, plus:
- Full CLAUDE.md template with all sections clearly separated (language, error handling, code standards, testing, documentation, each as their own section)
- Two or three domain agents covering primary and secondary concerns (e.g. api-designer + data-modeller for an API with a database)
- Two or three domain-specific skills
- Full memory files: separate MEMORY.md index, user-profile.md, and project-context.md

**What this means in practice:**
More specialised help during development. With two or three domain agents, you have focused assistance for the main aspects of your project without needing to handle everything in the main conversation. The higher rate limits also mean you can iterate faster and generate more projects per day.

### ProMax: Full Scaffold

The ProMax plan provides 1M tokens of context, five times the Pro/Max window. Genesis generates a comprehensive scaffold that consumes just 1.5-2.5% of this budget, leaving approximately 975k tokens for your work.

**What is included:**
Everything in the standard scaffold, plus:
- Detailed CLAUDE.md with extended context sections and additional project-specific rules
- Three or four domain agents covering primary, secondary, and supporting concerns (e.g. api-designer + data-modeller + auth-specialist + security-reviewer for a secure API with a database)
- Three or four domain-specific skills
- The widest possible selection from the agent catalogue

**What this means in practice:**
The best Genesis experience. You get comprehensive, specialised assistance across all aspects of your project. The 1M context window means you can:
- Work in extended sessions without compaction, maintaining full conversation context
- Read and analyse large files or multiple files simultaneously
- Conduct thorough code reviews that consider the full codebase context
- Run complex multi-step workflows without losing earlier context
- Use all available agents without worrying about context overhead

This is the plan the author uses and tests against.

### API Key Users

API key users are not on a subscription plan and are billed per token. Genesis asks for the context budget during setup and recommends a scaffold profile accordingly:
- 200k context or below: standard profile (default)
- Above 200k: full profile
- User can override to any profile regardless of context budget

## Context Budget Breakdown

Here is what consumes context in a generated project, so you can understand where your tokens go:

| Component | Loaded When | Estimated Cost | Present in Lean |
|-----------|------------|----------------|-----------------|
| CLAUDE.md | Every message | ~2-4k tokens | Yes (condensed, ~2k) |
| settings.json | Every message | ~300-500 tokens | Yes |
| MEMORY.md (index) | Every message | ~200-400 tokens | Yes (consolidated) |
| user-profile.md | Every message | ~200-400 tokens | No (inline in MEMORY.md) |
| project-context.md | Every message | ~300-600 tokens | No (inline in MEMORY.md) |
| Workflow agent (each) | When spawned | ~600-800 tokens | Yes (3 agents) |
| Domain agent (each) | When spawned | ~800-1200 tokens | Yes (1-2 agents) |
| Skill (each) | When invoked | ~400-800 tokens | Yes (5-6 skills) |
| MCP tool schemas | Every message (if configured) | ~500-2000 tokens | Yes (if needed) |

The "Every message" components are always in context. Agents and skills are loaded on demand, so they only consume context when you use them.

## Monitoring Context Usage

Every generated project includes a statusline that shows the current model and context usage percentage:

```
[Claude Sonnet 4.6] ctx: 34%
```

This updates in real time as you work. When usage gets high:
- **50-60%:** Consider whether you need everything currently in context
- **60-75%:** Run `/compact` to summarise the conversation and free up space
- **75%+:** Claude Code will auto-compact soon; run `/compact` proactively for a cleaner summary

You can also run `/context` at any time for a detailed breakdown of what is consuming your context window.

## Changing Your Profile

### After a Plan Upgrade

If you upgrade your Claude plan, update your Genesis configuration to match:

1. Open `environment.md` in the Genesis root directory
2. Update the `## Claude Plan` section:

```markdown
## Claude Plan

- **Tier:** ProMax
- **Context window:** 1000000
- **Scaffold profile:** full
```

3. New projects will use the updated profile. Existing projects keep their original scaffold.

### Per-Project Override

During the plan phase (Phase 2), you can request a different profile for a specific project:

- "Keep this one lean, it is a simple script"
- "Give me the full scaffold, I want all the agents"

Genesis adjusts the plan accordingly without changing your default profile.

### Enriching Existing Projects

If you upgrade and want to add infrastructure to an existing project:

1. Generate a new throwaway project with the same stack at the higher profile
2. Copy the additional agent files from `.claude/agents/`
3. Copy any additional skill directories from `.claude/skills/`
4. Update the project's CLAUDE.md to list the new agents and skills

Alternatively, write new agent and skill files directly. The templates in Genesis's `.claude/skills/genesis/templates/` show the expected format.
