---
name: pam-configuration-feature
description: Implement or modify PAM admin configuration features in this codebase. Use when Codex needs to add or update a configuration item across Ember UI, Django configuration endpoints, request validation constraints, persisted config keys and defaults, configuration search metadata, scheduler wiring, or background execution logic.
---

# PAM Configuration Feature

Implement configuration features using the existing PAM patterns instead of inventing a parallel flow.

## Workflow

1. Read [references/config-patterns.md](references/config-patterns.md) first.
2. Load only the references needed for the requested change:
   - [references/backend-endpoints.md](references/backend-endpoints.md) for get/change handlers, URL registration, and persistence flow
   - [references/constraints-and-defaults.md](references/constraints-and-defaults.md) for request validation, config keys, defaults, and migration-safe changes
   - [references/frontend-patterns.md](references/frontend-patterns.md) for Ember list cards, modals, and save/update behavior
   - [references/search-and-schedules.md](references/search-and-schedules.md) for searchable cards, schedule constants, background jobs, and failure points
   - [references/validation-checklist.md](references/validation-checklist.md) before finishing
3. Identify which parts the change affects:
   - Ember configuration list UI
   - modal or edit component
   - Django get/change endpoints
   - validation constraints
   - config constants and defaults
   - search metadata
   - scheduler wiring
   - background function
4. Extend the existing pattern rather than creating a new one-off configuration system.
5. Trace the full save path before editing:
   - request payload and constraint name
   - view handler and permission/audit behavior
   - config key/default storage
   - schedule add/remove side effects
   - frontend refresh path after save
6. When a schedule is involved, verify the same schedule key is registered in both:
   - `initial_entries/constants.py`
   - `utils/schedule/schedule_constants.py` inside `SCHEDULE_INFO`
7. When the feature appears in configuration search, keep `display_index` unique and add useful synonyms.
8. If local code already uses `Serializer`, `Criteria`, `JoinTable`, or `QueryConstants` for similar access paths, follow that convention instead of mixing in a new query style.

## Implementation Rules

- Keep configuration state in the existing `pam.Configurations` model unless there is a clear reason not to.
- Prefer dedicated get/change endpoints in `configuration_views.py` and `configuration_urls.py`.
- Add request validation in `security/constraints.py`.
- Reuse existing UI patterns in the Ember configuration area before creating new patterns.
- Preserve current audit and mail helper flows instead of duplicating them.
- Add targeted logs when debugging scheduled or background configuration behavior.
- Do not introduce a new config key name when an existing key already represents the same behavior.
- Keep enable/disable, interval, and recipient fields grouped consistently between frontend labels, payload keys, and backend keys.
- Update both read and write paths in the same change; avoid adding a save handler without a matching fetch/display path.
- Prefer localized changes in nearby existing configuration modules over creating new files unless the feature genuinely introduces a new surface.

## Common Checks

- If the UI card does not appear in search, check `configuration_constants.py` metadata, search text, and `display_index`.
- If a scheduled feature crashes during initialization, check `SCHEDULE_INFO` for the missing schedule key.
- If the feature saves but does not execute, verify schedule creation/removal logic and background function registration.
- If the value saves but reopens incorrectly, compare API response key names with Ember tracked state and template bindings.
- If validation fails unexpectedly, check whether the frontend payload shape still matches the corresponding constraint schema.
- If only part of a grouped configuration persists, inspect whether all keys are written in the same transaction or helper path.

## Validation

- Follow [references/validation-checklist.md](references/validation-checklist.md).
- Run `py_compile` on modified Django files.
- If Ember files change, state whether the frontend build was not run.
- If schedules change, verify both constant registrations before finishing.
