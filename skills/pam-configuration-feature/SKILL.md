---
name: pam-configuration-feature
description: Implement or modify PAM admin configuration features in this codebase. Use when Codex needs to add or update a configuration item across Ember UI, Django endpoints, validation constraints, search metadata, persisted config values, scheduler wiring, or background execution logic.
---

# PAM Configuration Feature

Implement configuration features using the existing PAM patterns instead of inventing a parallel flow.

## Workflow

1. Read [references/config-patterns.md](references/config-patterns.md) first.
2. Identify which parts the change affects:
   - Ember configuration list UI
   - modal or edit component
   - Django get/change endpoints
   - validation constraints
   - config constants and defaults
   - search metadata
   - scheduler wiring
   - background function
3. Extend the existing pattern rather than creating a new one-off configuration system.
4. When a schedule is involved, verify the same schedule key is registered in both:
   - `initial_entries/constants.py`
   - `utils/schedule/schedule_constants.py` inside `SCHEDULE_INFO`
5. When the feature appears in configuration search, keep `display_index` unique and add useful synonyms.
6. If local code already uses `Serializer` and `Criteria` for similar access paths, follow that convention when requested.

## Implementation Rules

- Keep configuration state in the existing `pam.Configurations` model unless there is a clear reason not to.
- Prefer dedicated get/change endpoints in `configuration_views.py` and `configuration_urls.py`.
- Add request validation in `security/constraints.py`.
- Reuse existing UI patterns in the Ember configuration area before creating new patterns.
- Preserve current audit and mail helper flows instead of duplicating them.
- Add targeted logs when debugging scheduled or background configuration behavior.

## Common Checks

- If the UI card does not appear in search, check `configuration_constants.py` metadata, search text, and `display_index`.
- If a scheduled feature crashes during initialization, check `SCHEDULE_INFO` for the missing schedule key.
- If the feature saves but does not execute, verify schedule creation/removal logic and background function registration.

## Validation

- Run `py_compile` on modified Django files.
- If Ember files change, state whether the frontend build was not run.
- If schedules change, verify both constant registrations before finishing.
