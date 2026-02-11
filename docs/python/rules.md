# Python Projects

**Core principles** — every decision filters through these:
1. **Modern**: Python 3.12+. Use current syntax and stdlib features.
2. **Functional**: Pure functions, immutability, composition. No unnecessary classes.
3. **Test-driven**: Write tests first, then implement.
4. **Efficient**: Optimize for speed and memory. No wasteful allocations.

## Type Hints
- All code must pass `mypy --strict` (enforced at CI).
- **Never use `from __future__ import annotations`** — breaks Pydantic, FastAPI, and runtime annotation inspection.

## Docstrings
- Google-style docstrings on classes and functions only. No module-level docstrings. No types in docstrings — types belong in signatures.

## Testing
- Features are not complete without passing tests.
- Run with `uv run pytest`. Test files in `tests/`, mirroring source structure (e.g., `src/auth.py` → `tests/test_auth.py`).
- When tests fail, fix the implementation — not the test — unless the test is wrong.

## Code Style
- **IMPORTANT**: Functional style takes precedence over all other style guidance.
- Use `dataclass(frozen=True)`, `NamedTuple`, or `TypedDict` for data — behavior belongs in standalone functions, not methods.
- Prefer list comprehensions and functional patterns over imperative loops.
- Prefer context managers (`with`) over manual setup/teardown.
- Use `pathlib` over `os.path` for filesystem operations.
- Use generators over lists when iterating once. Lazy evaluation for logging (`logger.info("msg %s", var)` not f-strings).
- Prefer `async` implementations over synchronous when I/O is involved.
- Prefer EAFP over LBYL unless failures are frequent and performance-critical.

## Code Smell Review
- Review your changes for functional style and code smells before committing. See `~/.claude/docs/python/code-smells.md` for functional reinterpretations.
- **IMPORTANT**: Never use a code smell finding to introduce OOP patterns. Fix with functional tools: extract functions, create data types, improve module boundaries.
