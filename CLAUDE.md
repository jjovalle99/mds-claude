# Workflow

## Plan Mode
- Make the plan extremely concise. Sacrifice grammar for the sake of concision.
- At the end of each plan, give me a list of unresolved questions to answer, if any.

# Python Projects
Apply these rules when working in any Python project.

## Tooling
- **IMPORTANT**: Always use `uv` for everything — `uv add` for dependencies, `uv run` for scripts and CLI tools. Never use bare `pip`, `pip install`, `python`, `python3`, or `poetry`.
- For external services (GitHub, cloud providers, etc.), always prefer their official CLIs (e.g., `gh`, `aws`, `gcloud`). Discover usage and flags via `--help` instead of searching the web for docs.

## Project Structure
- Organize code under a `src/` directory with a package per natural domain (e.g., `src/auth/`, `src/billing/`).
- Split into submodules when a module grows beyond a single clear responsibility — but do not pre-split; start with one file and extract when complexity warrants it.
- Every package must have an `__init__.py` that explicitly exports its public API.

## Type Hints
- All code must pass `mypy --strict`. Every function signature, variable, and return type must be fully parameterized.
- Use **built-in generics** first (`list`, `dict`, `tuple`, `set`, `type`). Only import from `typing` when no built-in equivalent exists (e.g., `Sequence`, `Mapping`, `Optional`).
- **Never use bare generics** — `list` is wrong, `list[str]` is correct. Same for `dict`, `tuple`, `set`.
- `Any` is a **last resort** — only when the type is genuinely unknowable (untyped third-party API). Try `object`, `Protocol`, `TypeVar`, or `Union` first.
- **Never use `from __future__ import annotations`** — all projects target Python 3.12+ where built-in generics are natively supported. Postponed evaluation breaks Pydantic, FastAPI, and runtime annotation inspection.

## Docstrings
- Use **Google-style docstrings** on classes and functions only — **never module-level docstrings**.
- **Do not include types in docstrings** — types belong in signatures, not prose.

## Testing
- All new features must include `pytest` tests before the feature is considered complete.
- Test files go in `tests/`, mirroring source structure (e.g., `src/auth.py` → `tests/test_auth.py`).
- Run tests with `uv run pytest` from the project root.
- Centralize shared fixtures in `conftest.py` files at the appropriate directory level. Avoid duplicating fixture definitions across test files.

## Code Style
- **IMPORTANT**: Functional style takes precedence over all other style guidance. When a functional approach conflicts with another rule, functional wins.
- Prefer simple, straightforward implementations over clever or elaborate ones. Complexity is only justified when a simpler approach would violate correctness or established best practices.
- Prefer pure functions, immutability, and composition over classes and mutation.
- **Always prefer context managers** (`with` statements) over manual setup/teardown for resource management, tracing, transactions, and any acquire/release pattern.
- Use `dataclass(frozen=True)`, `NamedTuple`, or `TypedDict` for data — behavior belongs in standalone functions, not methods.
- **ALWAYS PREFER LIST COMPREHENSIONS AND FUNCTIONAL PATTERNS** over imperative loops when possible.
- Use classes only when genuinely needed (e.g., protocol implementations, stateful resources like DB connections).

## Code Smell Review
- Before committing, first verify functional style is followed, then review code against these smells. If one is detected, refactor before committing. When uncertain, add a comment explaining the trade-off.
- Reference: https://sammancoaching.org/reference/code_smells/
- **Universal smells** (apply as-is): Duplicated Code, Long Function, Long Parameter List, Deep Nesting, Bumpy Road, Primitive Obsession, Global Data, Mutable Data, Mysterious Name, Comments (as deodorant), Speculative Generality, Divergent Change, Shotgun Surgery, Repeated Switches, Lazy Element, Temporary Field, Variable with Long Scope, complex Loops replaceable by comprehensions/itertools.
- **Reinterpreted for functional style**:
  - *Anemic Data Class* — In functional Python, data-only classes (`dataclass`, `NamedTuple`) are correct. This smell only applies if a class has methods that should be standalone functions instead.
  - *Feature Envy* — Reinterpret as excessive coupling: a function that reaches deep into another module's internals instead of using its public API.
  - *Large Class* — Reinterpret as large module: a module with too many unrelated functions that should be split.
  - *Message Chains* — Chaining on immutable data or functional composition is fine. Only flag mutable chains with hidden side effects.
  - *Insider Trading* — Reinterpret as module boundary violations: modules sharing internal state instead of exposing clean interfaces.
  - *Data Clumps* — Still applies: group related parameters into a `dataclass` or `NamedTuple` instead of passing them individually.
- **IMPORTANT**: Never use a code smell finding as justification to introduce OOP patterns (class hierarchies, method-heavy classes). Fix smells with functional tools: extract functions, create data types, improve module boundaries.
