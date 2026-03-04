# Constraints And Defaults

## Scope

Use this reference when the change introduces or modifies request payload fields, config keys, or default values.

## Typical Files

- `platform-source/src/django/pam/security/constraints.py`
- `platform-source/src/django/pam/initial_entries/constants.py`
- `platform-source/src/django/pam/configuration/configuration_views.py`

## Constraint Rules

- Add a named constraint entry for every new editable payload field.
- Match the expected frontend payload shape exactly.
- Reuse existing validators for booleans, integers, email lists, enums, and intervals when available.
- Keep optional fields explicitly optional; do not silently coerce missing values unless nearby code already does.
- If one field depends on another, validate that combination in the same constraint path.

## Default Value Rules

- Store defaults in `initial_entries/constants.py` using the established configuration constant pattern.
- Use one canonical key per persisted setting.
- Keep fallback defaults aligned across:
  - backend read path
  - backend write path
  - frontend initial render assumptions
- If the system already has live deployments, avoid changing the semantic meaning of an existing key unless the task explicitly requires migration behavior.

## Grouped Configurations

For a feature with multiple fields, define the full set up front:

- enable flag
- interval/frequency
- mode or scope
- recipients or exclusions
- schedule type if background work is involved

Persist and validate them as one configuration set so the UI does not display half-saved state.

## Common Mistakes

- Adding a new payload field without updating the constraint schema
- Using a frontend key name that does not match the backend validator name
- Introducing a second config key for an already existing setting
- Relying on implicit defaults in one code path but explicit defaults in another
- Accepting invalid combinations such as disabled state with mandatory schedule inputs
