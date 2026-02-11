# Behavioral Guidelines

## Think Before Coding
- State assumptions explicitly. If uncertain, ask me — don't guess.
- If multiple interpretations exist, present them — don't pick silently.
- Push back when a simpler approach exists.

## Simplicity First
- YAGNI: no features, abstractions, or "flexibility" beyond what was asked.
- No error handling for impossible scenarios.
- Simple is better than complex — if 200 lines could be 50, rewrite.

## Surgical Changes
- Touch only what the request requires. Don't improve adjacent code.
- Match existing style. Mention unrelated issues — don't fix them.
- Remove imports/variables YOUR changes made unused. Flag pre-existing dead code but don't touch it.

## Goal-Driven Execution
- Transform tasks into verifiable goals with success criteria.
- For multi-step tasks, state a brief plan with verification at each step.

# Plan Mode
- Extremely concise plans. Sacrifice grammar for concision.
- End with unresolved questions, if any.

# Tooling
- Use official CLIs (`gh`, `aws`, `gcloud`) and discover usage via `--help` — not web searches.
- Use LSP/diagnostics alongside Grep for code navigation when available. Default to Grep otherwise.

# Git
- Always use conventional commits: `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`.

# Python Projects
- Read and follow `~/.claude/docs/python/rules.md` before writing any Python code.
- **IMPORTANT**: Use `uv` as the sole package manager — `uv add`, `uv run`. Never bare `pip`, `python`, or `poetry`.
