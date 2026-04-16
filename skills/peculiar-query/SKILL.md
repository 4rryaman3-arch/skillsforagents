---
name: peculiar-query
description: Resolve ambiguous or unusual user requests into executable tasks with explicit assumptions, risk checks, and minimal clarification loops. Use when prompts are under-specified, contradictory, mixed-domain, or contain unusual constraints ("peculiar queries") and Codex must still produce a concrete, testable outcome.
---

# Peculiar Query

Turn odd or unclear requests into an answer that can be executed and validated without long back-and-forth.

## Workflow

1. Parse the request into `goal`, `inputs`, `constraints`, `success criteria`.
2. Detect ambiguity class:
- missing data
- conflicting constraints
- mixed domains in one prompt
- impossible/unsafe requirement
3. Choose action mode:
- proceed with explicit assumptions when risk is low
- ask one concise blocker question when execution is risky
4. Produce an implementation-ready plan:
- interpreted request
- assumptions
- concrete steps
- validation checks
5. Execute and report what was assumed vs verified.

## Clarification Rule

- Ask a question only when a wrong assumption can cause material rework, data loss, security impact, or cost.
- Otherwise proceed immediately with best-fit assumptions and state them clearly in one short block.
- Keep questions singular and specific.

## Output Shape

Use this structure for peculiar queries:

1. `Interpreted Intent`: one sentence.
2. `Assumptions`: short bullet list; mark each as `assumed` or `confirmed`.
3. `Execution`: commands/edits/steps.
4. `Validation`: how correctness was checked.
5. `Result`: final output and known limitations.

## Edge-Case Handling

- For contradictory constraints, prioritize safety, correctness, and reversibility.
- For mixed-domain prompts, split into independent tracks and resolve each track separately.
- For impossible asks, return the closest feasible alternative with tradeoffs.
- For vague time words (`today`, `latest`, `recent`), pin exact dates before finalizing.

## References

- Use [references/query-patterns.md](references/query-patterns.md) for quick mappings from ambiguous prompts to action plans.
