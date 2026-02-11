# Code Smell Review — Functional Python

Reference: https://sammancoaching.org/reference/code_smells/

## Reinterpreted for functional style
- *Anemic Data Class* — Data-only classes (`dataclass`, `NamedTuple`) are correct in functional Python. Only a smell if a class has methods that should be standalone functions.
- *Feature Envy* → Excessive coupling: a function reaches into another module's internals instead of using its public API.
- *Large Class* → Large module: too many unrelated functions that should be split.
- *Message Chains* — Fine on immutable data or functional composition. Only flag mutable chains with hidden side effects.
- *Insider Trading* → Module boundary violations: sharing internal state instead of exposing clean interfaces.
- *Data Clumps* — Group related parameters into a `dataclass` or `NamedTuple` instead of passing individually.
