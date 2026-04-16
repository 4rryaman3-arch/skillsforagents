# Query Patterns

## Ambiguous Intent

- Prompt: "fix this quickly"
- Action: inspect local changes, infer likely failing area from errors/tests, execute minimal safe fix, report assumptions.

## Missing Critical Input

- Prompt: "deploy it now"
- Action: ask one blocker question for environment/target; do not guess production.

## Conflicting Constraints

- Prompt: "no code changes, but fix failing tests"
- Action: explain contradiction, offer two feasible paths, default to non-code validation only.

## Mixed Domain

- Prompt: "update backend auth and redesign homepage style"
- Action: split into backend and frontend tracks; execute independently; merge status in one result.

## Time-Sensitive Wording

- Prompt: "use latest API behavior today"
- Action: verify current docs/status first, then answer with absolute dates.
