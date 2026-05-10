# AGENTS.md

[![English](https://img.shields.io/badge/README-English-blue)](README.md) [![中文](https://img.shields.io/badge/README-中文-red)](README.zh-CN.md)

A two-layer template for AI coding-agent work guidelines, adapted from
[agents-md](https://github.com/TheRealSeanDonahoe/agents-md) by Sean Donahoe.

This repository builds on the following sources, keeping the broadly useful principles—non-negotiables, think-first
workflow, simplicity, surgical changes, verification, and session hygiene—and reorganizing them for cross-project and
multi-tool use:

- [agents-md](https://github.com/TheRealSeanDonahoe/agents-md) by Sean Donahoe — original structure and
  practical coding-agent principles
- IJFW principles by Sean Donahoe
- Andrej Karpathy's observations on LLM coding pitfalls
- [How Boris uses Claude Code](https://howborisusesclaudecode.com/) by Boris Cherny
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices) by Anthropic
- Community anti-sycophancy patterns
- [AGENTS.md open standard](https://agents.md/)

## Naming

This repository is named **agents-md** to stay close to the AGENTS.md ecosystem and signal that it is still a
Markdown-based convention for coding-agent guidance.

Inside the templates, the content uses **Agent Work Guidelines** instead of repeatedly saying `AGENTS.md`. The reason
is portability: the same guideline content may be read as `AGENTS.md`, imported from `CLAUDE.md`, copied into
`GEMINI.md`, or reused through a symlink. If the body keeps saying "AGENTS.md", it becomes confusing when another tool
reads the same content under a different filename.

Use concrete filenames only when discussing installation, compatibility, or tool-specific behavior. Use "Agent Work
Guidelines", "global guidelines", and "project guidelines" for the reusable content itself.

## When to use this

Use this repository when you want:

- one global guideline shared across projects;
- one project guideline per repository;
- one source that can be reused by Codex, Claude Code, Gemini CLI, Cursor, and similar tools;
- a concise template that separates operating principles from project facts;
- a clear place to discuss how personal or machine-specific overrides should work per tool.

## Core idea

Keep shared principles stable and keep project facts local to each repository.

- **Global guidelines** answer: "How should the agent work everywhere?"
- **Project guidelines** answer: "What does the agent need to know in this repo?"
- **Personal or machine-specific notes** answer: "What is true only for this person, machine, or temporary workflow?"

Personal/local notes are tool-specific. Do not assume every coding agent loads the same `.local.md` convention.

## How this differs from agents-md

agents-md provides a strong single-file `AGENTS.md` boilerplate. This version changes the packaging and layering:

1. **From one mixed file to separate global and project layers.**
   - Global guideline: cross-project behavior and engineering principles.
   - Project guideline: stack, commands, layout, conventions, forbidden areas, and project learnings.
2. **Designed for cross-agent reuse.**
   - The templates use "Agent Work Guidelines" instead of hard-coding `AGENTS.md` throughout the body.
   - Specific filenames are discussed only in setup and compatibility sections.
   - Symlink reuse is supported.
   - Claude Code `@AGENTS.md` reference reuse is also supported.

## Files

```text
global/AGENTS.md        # Global template, English
global/AGENTS.zh-CN.md  # Global template, Chinese
project/AGENTS.md       # Project template, English
project/AGENTS.zh-CN.md # Project template, Chinese
```

## Installation

### 1. Pick the templates you need

Use the English or Chinese version, and decide whether you need global guidelines, project guidelines, or both.

Typical setup:

```text
~/.agents/AGENTS.md      # global work guidelines, if your tool supports this location
<repo>/AGENTS.md         # project work guidelines, commit this
```

### 2. Install global guidelines

Create a global guideline directory and copy the global template:

```bash
mkdir -p ~/.agents
cp global/AGENTS.md ~/.agents/AGENTS.md
```

Use the Chinese version if preferred:

```bash
mkdir -p ~/.agents
cp global/AGENTS.zh-CN.md ~/.agents/AGENTS.md
```

Important: not every tool automatically reads `~/.agents/AGENTS.md`. If your tool has its own global location, copy
or reference the content there instead.

Examples:

```bash
# Claude Code user-level instructions
mkdir -p ~/.claude
cp global/AGENTS.md ~/.claude/CLAUDE.md

# Gemini CLI user-level context
mkdir -p ~/.gemini
cp global/AGENTS.md ~/.gemini/GEMINI.md
```

### 3. Install project guidelines

From the target repository root:

```bash
cp /path/to/agents-md/project/AGENTS.md ./AGENTS.md
```

Use the Chinese version if preferred:

```bash
cp /path/to/agents-md/project/AGENTS.zh-CN.md ./AGENTS.md
```

Then fill in the project-specific sections: stack, commands, layout, conventions, verification, risk areas, and project
learnings.

### 4. Reuse from tool-specific files

If a tool expects another filename, either symlink to `AGENTS.md` or create a wrapper file.

Symlink example:

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
```

Claude Code wrapper example:

```md
# CLAUDE.md

@AGENTS.md

## Claude Code additions

- Add Claude-specific rules here when needed.
```

### 5. Verify what the agent loaded

After installing, ask your agent to summarize the active guidelines before doing real work. For tools with a
memory/context inspection command, use that command as the source of truth.

Examples:

- Claude Code: check the loaded `CLAUDE.md` / imported files according to Claude Code memory docs.
- Gemini CLI: use `/memory show` to inspect the concatenated context.
- Generic `AGENTS.md` tools: ask the agent to summarize current instructions and confirm the file path it read, if
  the tool exposes that information.

## Multi-tool reuse

Maintain one primary file, usually `AGENTS.md`, and let tools reuse it.

### Symlink reuse

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
```

Use this when the environment handles symlinks reliably.

### Claude Code reference reuse

Claude Code can reuse `AGENTS.md` from `CLAUDE.md`:

```md
@AGENTS.md
```

This is useful when you want `CLAUDE.md` to keep Claude-specific additions:

```md
# CLAUDE.md

@AGENTS.md

## Claude Code additions

- Add Claude-specific rules here when needed.
```

## Personal/local overrides: current research

There is no confirmed universal `.local.md` convention across coding agents.

| Tool | Confirmed behavior | Local/private override status |
| --- | --- | --- |
| Claude Code | Official docs say Claude Code reads `CLAUDE.md`, supports imports with `@path`, and loads `CLAUDE.local.md` alongside `CLAUDE.md`. | `CLAUDE.local.md` is officially supported for personal project-specific preferences and should be gitignored. |
| Gemini CLI | Official docs describe hierarchical `GEMINI.md` context files and `context.fileName` as a string or string array. | No official default `GEMINI.local.md` was found. You can likely configure an additional filename through `context.fileName`, but it is not the default convention. |
| AGENTS.md standard | The official agents.md site describes `AGENTS.md`, nested `AGENTS.md`, symlinks, and Gemini configuration to use `AGENTS.md`. | No official `AGENTS.local.md` convention was found on agents.md. |
| OpenAI Codex | Codex/AGENTS.md support is documented through AGENTS.md ecosystem docs; some search results mention `AGENTS.override.md`, but this still needs confirmation against current official Codex docs before treating it as a recommendation. | Do not assume `AGENTS.local.md` is loaded by Codex unless verified in the installed Codex version or official docs. |
| Cursor | Cursor is listed in the AGENTS.md ecosystem and supports project rules, but no official `AGENTS.local.md` behavior was confirmed here. | Do not assume `.local.md` loading. Use Cursor's documented rules/settings mechanism if personal overrides are needed. |

Practical recommendation for now:

- For Claude Code: use `CLAUDE.local.md` for private project-specific preferences.
- For Gemini CLI: use `~/.gemini/GEMINI.md`, project `GEMINI.md`, or configure `context.fileName` explicitly if
  you want additional filenames.
- For generic `AGENTS.md`: keep committed shared instructions in `AGENTS.md`; do not document `AGENTS.local.md` as
  automatically loaded unless the target tool confirms it.

## Layering plan

Use two layers:

1. **Global guidelines**: cross-project rules.
2. **Project guidelines**: repository-specific facts.

Do not split user-level and team-level guidelines as separate conceptual layers. Use file location and
tool-supported mechanisms instead:

- Committed project guideline files are shared team rules.
- User/global guideline files are personal cross-project preferences when the tool supports them.
- Local/private files should only be recommended when the target tool officially supports or is explicitly configured
  to load them.

## License and attribution

This project is based on ideas and structure from [agents-md](https://github.com/TheRealSeanDonahoe/agents-md). Please
review the upstream project for its own license and attribution requirements before publishing derivative work.
