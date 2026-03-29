# Stack Profiles

Reference document for stack-specific conventions, tooling, and configuration patterns. The planner and claude-infra-writer agents consult this when generating project infrastructure.

---

## Node.js / TypeScript

### Project Config
- `package.json` with `"type": "module"` for ESM
- `tsconfig.json` with strict mode enabled
- `.nvmrc` or `.node-version` for runtime version pinning

### Linting and Formatting
- ESLint with `@typescript-eslint` plugin
- Prettier for formatting
- Hook: `eslint --fix` and `prettier --write` on PostToolUse for `.ts`, `.tsx` files

### Testing
- vitest (preferred) or jest
- Test files: `*.test.ts` or `*.spec.ts` colocated with source
- Coverage with `vitest --coverage` or `jest --coverage`

### Permissions
```json
"Bash(npm:*)", "Bash(npx:*)", "Bash(node:*)", "Bash(vitest:*)"
```

### Commit Style
- Conventional Commits (feat:, fix:, chore:, docs:, test:, refactor:)

### Folder Structure (typical)
```
src/
  index.ts
  routes/
  services/
  models/
  middleware/
  utils/
tests/
docs/
```

### Error Patterns
- Use custom error classes extending `Error`
- Express/Fastify: centralised error handler middleware
- Never `catch (e) {}` without logging

---

## Python

### Project Config
- `pyproject.toml` (PEP 621) as single source of truth
- No `setup.py` or `setup.cfg` for new projects
- Python version in `pyproject.toml` `requires-python` field

### Linting and Formatting
- ruff for both linting and formatting (replaces flake8, isort, black)
- Hook: `ruff check --fix` and `ruff format` on PostToolUse for `.py` files
- Type hints mandatory; mypy or pyright for type checking

### Testing
- pytest with `pytest-cov` for coverage
- Test files: `tests/test_*.py` or `tests/*_test.py`
- Fixtures in `tests/conftest.py`

### Permissions
```json
"Bash(python:*)", "Bash(pip:*)", "Bash(pytest:*)", "Bash(ruff:*)", "Bash(uv:*)"
```

### Commit Style
- Conventional Commits

### Folder Structure (src layout)
```
src/
  <package_name>/
    __init__.py
    main.py
    routes/
    services/
    models/
tests/
  conftest.py
  test_*.py
docs/
migrations/
```

### Error Patterns
- Custom exceptions inheriting from base `ApplicationError`
- FastAPI: exception handlers registered on the app
- Django: middleware-based error handling
- Never bare `except:` or `except Exception: pass`

---

## Go

### Project Config
- `go.mod` for module management
- `Makefile` for common commands
- `.golangci.yml` for linter configuration

### Linting and Formatting
- `gofmt` (mandatory, built-in)
- `golangci-lint` for comprehensive linting
- Hook: `gofmt -w` on PostToolUse for `.go` files

### Testing
- Built-in `go test`
- Table-driven tests as standard pattern
- Test files: `*_test.go` colocated with source
- Coverage: `go test -cover ./...`

### Permissions
```json
"Bash(go:*)", "Bash(make:*)", "Bash(golangci-lint:*)"
```

### Commit Style
- Conventional Commits for libraries and services; plain imperative for CLI tools

### Folder Structure
```
cmd/
  <binary-name>/
    main.go
internal/
  handlers/
  models/
  services/
  db/
pkg/           (only for truly reusable code)
migrations/
docs/
```

### Error Patterns
- Return errors, don't panic (except truly unrecoverable situations)
- Wrap errors with `fmt.Errorf("context: %w", err)`
- Custom error types implement the `error` interface
- Never `_ = someFunc()` when `someFunc` returns an error

---

## Rust

### Project Config
- `Cargo.toml` for package management
- Workspace layout for multi-crate projects
- Edition 2021 or later

### Linting and Formatting
- `rustfmt` for formatting (mandatory)
- `clippy` for linting
- Hook: `cargo fmt` on PostToolUse, `cargo clippy` as check

### Testing
- Built-in `cargo test`
- Unit tests in `#[cfg(test)]` modules within source files
- Integration tests in `tests/` directory
- `#[should_panic]` for expected failure tests

### Permissions
```json
"Bash(cargo:*)", "Bash(rustup:*)"
```

### Commit Style
- Conventional Commits

### Folder Structure
```
src/
  main.rs or lib.rs
  models/
  handlers/
  services/
tests/
benches/
docs/
```

### Error Patterns
- Use `thiserror` for library error types, `anyhow` for application error handling
- Return `Result<T, E>` from fallible functions
- Never `.unwrap()` in production code (use `.expect("reason")` only in truly impossible cases)

---

## Ruby

### Project Config
- `Gemfile` for dependencies
- `.ruby-version` for runtime pinning
- `Rakefile` for task automation

### Linting and Formatting
- RuboCop for linting and formatting
- Hook: `rubocop -a` on PostToolUse for `.rb` files

### Testing
- RSpec (preferred) or Minitest
- Test files: `spec/**/*_spec.rb`
- Factory Bot for test data

### Permissions
```json
"Bash(bundle:*)", "Bash(rake:*)", "Bash(rspec:*)", "Bash(ruby:*)"
```

---

## Java / Kotlin

### Project Config
- Gradle (preferred) or Maven
- `build.gradle.kts` for Kotlin DSL

### Linting and Formatting
- Spotless for formatting
- Checkstyle or ktlint for linting

### Testing
- JUnit 5
- Mockito for mocking
- Test files in `src/test/`

### Permissions
```json
"Bash(gradle:*)", "Bash(./gradlew:*)", "Bash(mvn:*)", "Bash(java:*)"
```

---

## Cross-Stack Patterns

### Docker
- Multi-stage builds for production images
- `.dockerignore` mirroring `.gitignore` plus build artifacts
- `docker-compose.yml` for local development with dependencies

### CI/CD
- GitHub Actions as default CI
- Separate jobs for lint, test, build
- Cache dependencies between runs

### Database Projects
- Migrations directory with numbered or timestamped files
- Never modify existing migration files; always create new ones
- Seed data separate from migrations

### API Projects
- OpenAPI/Swagger spec in `docs/api.yaml` or auto-generated
- Input validation at the boundary
- Consistent error response format across all endpoints
- Rate limiting and CORS configuration

### CLI Tools
- `--help` and `--version` flags
- Exit codes: 0 for success, 1 for general errors, 2 for usage errors
- Stderr for errors, stdout for output

### Libraries/Packages
- Semantic versioning
- CHANGELOG.md
- Minimal dependencies
- Public API surface clearly documented

---

## Environment-Specific Notes

These notes apply across all stacks and should be factored into generated project configuration when the user's hosting environment warrants it.

### Linux (Native)
- Baseline environment. All tools and paths work as documented.
- Package managers: apt (Debian/Ubuntu), dnf (Fedora), pacman (Arch).

### macOS
- Install tools via Homebrew (`brew install`).
- GNU vs BSD differences: `sed -i` requires `''` argument on macOS, `grep` lacks some GNU extensions. Prefer cross-platform flags or install GNU coreutils.
- File system is case-insensitive by default (HFS+/APFS). Avoid relying on case-sensitive file names.
- Docker Desktop required for container workflows (not native Docker).

### WSL (Windows Subsystem for Linux)
- Use Linux-style paths (`~/claude/`), not Windows paths.
- File watching: some frameworks (e.g. Vite, webpack) may need polling mode (`CHOKIDAR_USEPOLLING=true`) for cross-filesystem watching.
- VS Code: `code .` opens VS Code on the Windows side with WSL extension.
- Docker: requires Docker Desktop with WSL2 backend enabled.
- Accessing Windows files from WSL (`/mnt/c/`) is slow; keep projects in the WSL filesystem (`~/`).
- Git: configure `core.autocrlf=input` to prevent line ending issues.

### Windows (Non-WSL)
- Target directory: `%USERPROFILE%\claude\<name>\` instead of `~/claude/<name>/`.
- Default shell: PowerShell. Command syntax differs from bash (e.g. `$env:VAR` instead of `export VAR`).
- Path separators: backslash (`\`). Many Node.js tools handle this automatically, but shell scripts may need adjustment.
- Some Unix tools (make, grep, sed) may not be available. Use cross-platform alternatives or install via Scoop/Chocolatey.
- Line endings: configure Git with `core.autocrlf=true`.
- Claude Code CLI requires Node.js installed via the official Windows installer or nvm-windows.
