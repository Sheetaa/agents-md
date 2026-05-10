# Project Agent Work Guidelines

## Naming

This file is a **Project Agent Work Guidelines** template. The term means repository-specific working agreements for
AI coding agents: stack, commands, layout, conventions, verification requirements, risk areas, and project learnings.

The filename may vary by tool (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or a wrapper/import file). The body uses
"Project Agent Work Guidelines" instead of hard-coding `AGENTS.md` so the same content remains clear when reused by
different tools.

Repository-specific work guidelines for AI coding agents. Keep this file specific to the current project. Cross-project
principles belong in the global guideline; personal or machine-specific details should use tool-supported private/local
files and should not be committed.

---

## 1. Project context

**Fill this in per project. Keep it specific. Delete sections that do not apply.**

### Stack

- Language and version:
- Framework(s):
- Package manager:
- Runtime / deployment target:

### Commands

- Install: `TODO`
- Build: `TODO`
- Test all: `TODO`
- Test single file: `TODO`
- Lint: `TODO`
- Typecheck: `TODO`
- Run locally: `TODO`

Prefer single-file or single-test runs during iteration. Full suites are for the final verification pass.

### Layout

- Source lives in: `TODO`
- Tests live in: `TODO`
- Documentation lives in: `TODO`
- Do not modify: `TODO` (generated code, vendored dependencies, legacy areas, or other high-risk paths)

### Conventions specific to this repo

- Naming: `TODO`
- Import style: `TODO`
- Error handling pattern: `TODO`
- Testing pattern and framework: `TODO`
- Formatting: `TODO`
- API compatibility rules: `TODO`

### Forbidden

- `TODO`: things that look reasonable but will break this project.

---

## 2. Project workflow

- Required planning or design review before implementation:
- Required issue, ticket, or spec references:
- Required branch naming convention:
- Required pull request checks:
- Required release or migration process:

---

## 3. Verification requirements

Define the smallest meaningful verification for common changes:

- Documentation-only change:
- Unit-level code change:
- Integration change:
- UI change:
- API or schema change:
- Dependency change:
- Performance-sensitive change:

State which checks are required before claiming completion.

---

## 4. Risk areas

List areas where agents must be especially conservative:

- Authentication / authorization:
- Billing / payments:
- Data migrations:
- Security-sensitive code:
- Generated code:
- Public API contracts:
- Backward compatibility:

---

## 5. Project learnings

**Accumulated corrections. This section is for the agent to maintain, not just the human.**

When the user corrects your project-specific approach, append a one-line rule here before ending the session. Write it
concretely: "Always use X for Y", never abstractly: "be careful with Y". If an existing line already covers the
correction, tighten it instead of adding a duplicate. Remove lines when the underlying issue goes away.

- (empty)
