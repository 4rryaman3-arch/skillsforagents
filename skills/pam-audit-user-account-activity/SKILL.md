---
name: pam-audit-user-account-activity
description: Add or modify PAM audit events for features that must log both user activity and account activity. Use when implementing create/edit/enable/disable/execute flows that require constants, ActivityTypes registration, backend audit calls, and end-to-end validation for both categories.
---

# PAM Audit User + Account Activity

Implement audit changes using PAM's existing audit registry and logging utilities. Do not add one-off audit paths.

## Workflow

1. Read [references/audit-patterns.md](references/audit-patterns.md).
2. Identify event points in the feature flow:
   - create
   - edit
   - enable
   - disable
   - launch/execution
3. Add event constants in `src/django/pam/initial_entries/constants.py`.
4. Register matching events in both activity maps in `src/django/pam/initial_entries/activities.py`:
   - `USER_ACTIVITY`
   - `ACCOUNT_ACTIVITY`
5. Add backend calls at action points:
   - `register_user_activity(...)`
   - `register_account_activity(...)`
6. Keep event names aligned across constants, activity maps, and caller usage.
7. Avoid silent fallbacks that hide missing registrations; fix registration at source.
8. Validate event payload fields:
   - user-side: `affected_user_name`, `group_name`, reason/comments where needed
   - account-side: `account_title`, `account_id`, device/asset context where needed

## Implementation Rules

- Reuse existing nearby activity naming patterns.
- Keep both user and account events in the same change when the feature impacts both.
- If activity display name/localization flow is used for adjacent events, follow that same path.
- Do not introduce duplicate constants for the same semantic event.
- Keep audit writes after successful state change, not before persistence.

## Common Failure Points

- `KeyError` in audit registration due to missing entry in `activities.py`.
- Constant exists but category map mismatch (`USER_ACTIVITY` vs `ACCOUNT_ACTIVITY`).
- Logging only one side (user or account) causing incomplete compliance trail.
- Event names diverge between constant and caller.

## Validation

- Follow [references/validation-checklist.md](references/validation-checklist.md).
- Run `python -m py_compile` on modified Django files.
- Manually verify the action creates both user and account audit rows.
- Confirm no `KeyError` in `audit_utils.get_activty_type_value`.
