# Audit Patterns

## Required touchpoints

- Constants: `src/django/pam/initial_entries/constants.py`
- Activity registry: `src/django/pam/initial_entries/activities.py`
- Runtime logging helpers: `src/django/pam/audit/audit_utils.py`

## Typical call placement

- Creation/edit endpoints: after successful persistence, before returning response.
- Enable/disable bulk operations: loop over resolved names and log each entity.
- Launch/execution paths: log only when actual script/profile selection succeeds.

## Naming

- Prefer verb-past naming used in PAM constants (for example `*_ADDED`, `*_MODIFIED`, `*_ENABLED`, `*_DISABLED`, `*_LAUNCHED`, `*_EXECUTED`).
- Keep constant name and `activities.py` key text identical.
