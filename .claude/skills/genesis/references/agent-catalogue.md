# Agent Catalogue

Reference document for selecting which agents to generate in a new project. The planner agent consults this to decide which agents are appropriate for a given project type.

---

## Context Budget Guidelines

Generated agent files consume context tokens every time they are loaded. The scaffold profile (from `environment.md`) determines how many agents to include.

| Scaffold Profile | Workflow Agents | Domain Agents | Total Agent Budget |
|-----------------|----------------|---------------|-------------------|
| **lean** (Pro, 200k) | 3 (always) | 1-2 | ~3-5k tokens |
| **standard** (Max, 200k) | 3 (always) | 2-3 | ~4-7k tokens |
| **full** (ProMax/API, 1M) | 3 (always) | 3-4 | ~5-10k tokens |

Estimated cost per agent file: ~600-800 tokens (workflow), ~800-1200 tokens (domain).

**Lean profile prioritisation:** When limited to 1-2 domain agents, pick the single most impactful agent for the project type:
- API project: api-designer
- Database project: data-modeller
- Frontend project: component-architect
- CLI project: cli-designer
- Data/ML project: pipeline-architect
- Library project: api-surface-reviewer
- Security-sensitive: add security-reviewer only if the project explicitly handles auth or sensitive user data

---

## Workflow Agents (always included)

These agents are included in every generated project regardless of domain.

### test-runner
- **Purpose:** Run the project's test suite, analyse results, report failures with context
- **Model:** sonnet
- **Tools:** Bash, Read, Grep, Glob
- **When to invoke:** After code changes, before commits, when debugging test failures

### code-reviewer
- **Purpose:** Review code for quality, style, correctness, security, and adherence to project standards
- **Model:** sonnet
- **Tools:** Read, Grep, Glob
- **When to invoke:** Before merging, after completing a feature, on request

### doc-writer
- **Purpose:** Generate and update documentation, README, architecture docs, inline comments
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **When to invoke:** After adding new features, when architecture changes, on request

---

## Domain Agents (selected based on project type)

### API and Web Service Projects

**api-designer**
- **Purpose:** Design REST/GraphQL endpoints, request/response schemas, authentication flows
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **Best for:** API-first projects, microservices, backend services
- **Profiles:** lean, standard, full

**auth-specialist**
- **Purpose:** Implement and review authentication and authorisation patterns
- **Model:** opus
- **Tools:** Read, Write, Edit, Grep, Glob, Bash
- **Best for:** Projects with user authentication, OAuth, JWT, RBAC
- **Profiles:** standard, full

**performance-analyst**
- **Purpose:** Identify bottlenecks, suggest optimisations, review query performance
- **Model:** sonnet
- **Tools:** Read, Grep, Glob, Bash
- **Best for:** High-traffic services, data-heavy applications
- **Profiles:** full

### Database Projects

**data-modeller**
- **Purpose:** Design database schemas, relationships, indices, and constraints
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **Best for:** Projects with relational databases, complex data models
- **Profiles:** lean, standard, full

**migration-writer**
- **Purpose:** Write database migration files, validate migration safety, plan rollbacks
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Bash
- **Best for:** Projects using migration frameworks (Alembic, Flyway, Goose, Prisma)
- **Profiles:** standard, full

### Frontend Projects

**component-architect**
- **Purpose:** Design component hierarchy, state management, routing, and layout patterns
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **Best for:** React, Vue, Svelte, Angular applications
- **Profiles:** lean, standard, full

**accessibility-reviewer**
- **Purpose:** Review UI for WCAG compliance, keyboard navigation, screen reader support
- **Model:** sonnet
- **Tools:** Read, Grep, Glob
- **Best for:** User-facing web applications
- **Profiles:** standard, full

**ui-reviewer**
- **Purpose:** Review component design, responsiveness, consistency, and UX patterns
- **Model:** sonnet
- **Tools:** Read, Grep, Glob
- **Best for:** Frontend applications with design systems
- **Profiles:** full

### Data and ML Projects

**pipeline-architect**
- **Purpose:** Design data pipelines, ETL workflows, scheduling, and monitoring
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob, Bash
- **Best for:** Data engineering, ETL, streaming projects
- **Profiles:** lean, standard, full

**model-evaluator**
- **Purpose:** Evaluate ML model performance, suggest improvements, review training pipelines
- **Model:** opus
- **Tools:** Read, Grep, Glob, Bash
- **Best for:** Machine learning projects
- **Profiles:** standard, full

### Infrastructure Projects

**infra-reviewer**
- **Purpose:** Review infrastructure-as-code, Dockerfiles, CI/CD pipelines, cloud configs
- **Model:** sonnet
- **Tools:** Read, Grep, Glob
- **Best for:** DevOps, platform engineering, cloud-native projects
- **Profiles:** standard, full

**security-reviewer**
- **Purpose:** Audit code for security vulnerabilities, review dependencies, check configurations
- **Model:** opus
- **Tools:** Read, Grep, Glob, Bash
- **Best for:** Security-sensitive projects, anything handling user data or auth
- **Profiles:** lean (only if auth/sensitive data), standard, full

### CLI and Tool Projects

**cli-designer**
- **Purpose:** Design command structure, flags, subcommands, help text, and output formatting
- **Model:** sonnet
- **Tools:** Read, Write, Edit, Grep, Glob
- **Best for:** Command-line tools, developer tooling
- **Profiles:** lean, standard, full

### Library and Package Projects

**api-surface-reviewer**
- **Purpose:** Review public API design, backwards compatibility, documentation coverage
- **Model:** sonnet
- **Tools:** Read, Grep, Glob
- **Best for:** Libraries, SDKs, packages published for external use
- **Profiles:** lean, standard, full

---

## Selection Guidelines

1. **Always include** the three workflow agents (test-runner, code-reviewer, doc-writer)
2. **Include security-reviewer** for any project handling user data, authentication, or external input
3. **Include data-modeller and migration-writer** for any project with a database
4. **Include api-designer** for any project exposing HTTP endpoints
5. **Include component-architect** for any frontend project
6. **Limit domain agents to 3-4** per project to avoid overwhelming the user
7. **Prefer sonnet** for most agents; reserve opus for security and complex architectural review
8. **Every agent should have a clear, non-overlapping purpose** with other agents in the same project
9. **Respect the scaffold profile.** Lean profiles get 1-2 domain agents (the most critical). Standard profiles get 2-3. Full profiles get 3-4. Workflow agents are always included regardless of profile. Check each agent's **Profiles** tag before selecting it.
