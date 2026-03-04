# Validation Checklist

## Before Finishing

- Confirm the config key names are consistent across frontend, constraints, views, and defaults.
- Confirm GET and change endpoints both exist and use the same response field names.
- Confirm grouped values are persisted together.
- Confirm any search item has a unique `display_index`.
- Confirm any schedule key exists in both constants files and `SCHEDULE_INFO`.
- Confirm any background function is wired from the configuration save path.

## Suggested Compile Checks

Run `py_compile` for each modified Django file. Typical examples:

```powershell
python -m py_compile "platform-source/src/django/pam/configuration/configuration_views.py"
python -m py_compile "platform-source/src/django/pam/configuration/configuration_urls.py"
python -m py_compile "platform-source/src/django/pam/security/constraints.py"
python -m py_compile "platform-source/src/django/pam/utils/schedule/schedule_constants.py"
python -m py_compile "platform-source/src/django/pam/utils/schedule/schedule_functions.py"
```

## Reporting Guidance

- If Ember files changed and the frontend build was not run, state that explicitly.
- If the implementation depends on an existing local pattern you inferred, mention that in the handoff.
- If a schedule-related change was made without executing the scheduler locally, state that the wiring was checked statically only.
