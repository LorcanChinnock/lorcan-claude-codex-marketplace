Run the contract-first hub-and-spoke simplification pipeline for the requested scope. Prefer runtime delegation when available, because isolated reviewers reduce context contamination. If delegation is unavailable, run the same stages sequentially in the current context and disclose that fallback before starting.

## Scope resolution

Interpret the user's argument as the scope:

- no arg or `branch` -> `branch` (diff vs merge-base with default branch)
- `staged` -> staged changes
- `working` -> all uncommitted changes
- anything else -> treat as one or more paths

Do not expand the scope beyond what the user specified.

## Delegated route

When the runtime supports a custom simplification subagent or equivalent delegated worker, invoke it with the scope verbatim:

> Run the simplification pipeline with scope: `<arg>`. Follow the full process in [../agents/simplify-code.md](../agents/simplify-code.md): preflight -> gate -> locate -> contract -> spokes -> validate -> score -> consolidate -> report -> apply. Print the report before any file edit capability.

Relay the worker's report and final status exactly as returned. Do not summarise away the file-scoped revert hint.

## Sequential fallback

When no delegation/subagent capability is available, run the same process from [../agents/simplify-code.md](../agents/simplify-code.md) in the current context. Keep the original pipeline semantics:

1. Resolve scope, cap non-test code at 5000 LOC, and detect stack/test commands.
2. Run the eligibility gate before proposing changes.
3. Locate project instruction files, adjacent tests, and stack configuration.
4. Extract the contract using [CONTRACT.md](CONTRACT.md); never skip this step.
5. Run the selected spoke analyses from [SPOKES.md](SPOKES.md). Use parallel reviewer workers where possible; otherwise run the spokes sequentially and say delegation was unavailable.
6. Validate every proposal against the contract, then score and drop proposals below 75.
7. Print the report before any file edits.
8. Apply only surviving proposals and run the detected test command once when available.
9. End with the file-scoped revert and review commands from [SPOKES.md](SPOKES.md).

## What the orchestrator must not do

- Do not skip contract extraction, validation, scoring, or the pre-edit report.
- Do not apply edits before the report.
- Do not broaden the scope beyond the user's argument.
- Do not suggest repository-wide destructive commands; revert hints are file-scoped only.
