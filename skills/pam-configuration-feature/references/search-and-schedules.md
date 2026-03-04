# Search And Schedules

## Scope

Use this reference when the configuration item must be discoverable in search or drives background execution.

## Search Metadata

Typical file:

- `platform-source/src/django/pam/configuration/configuration_constants.py`

Rules:

- Add a unique `display_index`.
- Use a clear title and description.
- Include practical synonyms users would search for.
- Keep search text aligned with the wording shown in the UI card.

Common failure points:

- duplicate `display_index`
- title present but no useful synonyms
- backend metadata updated but frontend card not wired

## Schedule Registration

Typical files:

- `platform-source/src/django/pam/initial_entries/constants.py`
- `platform-source/src/django/pam/utils/schedule/schedule_constants.py`
- `platform-source/src/django/pam/utils/schedule/schedule_functions.py`
- `platform-source/src/django/pam/configuration/configuration_views.py`

Required alignment:

1. Add the schedule type or related constant in `initial_entries/constants.py` when the feature persists schedule-linked state there.
2. Register the same schedule key in `schedule_constants.py`.
3. Add the entry inside `SCHEDULE_INFO`.
4. Implement the actual function in `schedule_functions.py`.
5. Ensure configuration save logic creates, updates, or removes the schedule as needed.

## Background Job Guidance

- Keep the scheduler key name identical across all files.
- Log skip reasons, execution start, selection counts, and completion.
- Handle disabled-state no-op behavior explicitly.
- Avoid leaving orphaned schedules behind when a feature is turned off.

## Common Failure Points

- `KeyError` during scheduler initialization because `SCHEDULE_INFO` is incomplete
- schedule registered but function missing
- config toggled off but schedule not removed
- function present but never referenced by the config save path
