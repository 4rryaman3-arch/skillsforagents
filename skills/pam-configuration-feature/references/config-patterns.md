## Typical File Map

For most PAM configuration features, check these files first:

- Frontend list and render logic:
  - `platform-source/src/ember/app/components/configuration/general-configuration/general-configuration-list.js`
  - `platform-source/src/ember/app/templates/components/configuration/general-configuration/general-configuration-list.hbs`
- Frontend modal or edit component:
  - `platform-source/src/ember/app/components/configuration/`
  - `platform-source/src/ember/app/templates/components/configuration/`
- Backend endpoints:
  - `platform-source/src/django/pam/configuration/configuration_views.py`
  - `platform-source/src/django/pam/configuration/configuration_urls.py`
- Request validation:
  - `platform-source/src/django/pam/security/constraints.py`
- Search metadata and UI indexing:
  - `platform-source/src/django/pam/configuration/configuration_constants.py`
- Config defaults and system constants:
  - `platform-source/src/django/pam/initial_entries/constants.py`
- Scheduler registration:
  - `platform-source/src/django/pam/utils/schedule/schedule_constants.py`
  - `platform-source/src/django/pam/utils/schedule/schedule_functions.py`

## Backend Pattern

Most configuration features follow this shape:

1. Add a config key and default in `initial_entries/constants.py`.
2. Add dedicated GET and POST handlers in `configuration_views.py`.
3. Add URL routes in `configuration_urls.py`.
4. Add validation rules in `security/constraints.py`.
5. If the feature needs search visibility, update `configuration_constants.py`.

## Frontend Pattern

For features in the general configuration page:

1. Load current state in `general-configuration-list.js`.
2. Render the card in `general-configuration-list.hbs`.
3. Open a focused configuration component for edit/save behavior.
4. Update local display state after save.

If the request explicitly says to use the general configuration files, prefer those files over inventing a different frontend entry point.

## Search Pattern

When adding a searchable configuration item:

- add exact title text
- add description text
- add short synonyms users may search for
- keep `display_index` unique

If search does not resolve the new item, check for `display_index` collisions first.

## Scheduler Pattern

If the configuration enables a background job:

1. Add the schedule type in `initial_entries/constants.py`.
2. Add the schedule constant in `utils/schedule/schedule_constants.py`.
3. Add the schedule entry to `SCHEDULE_INFO`.
4. Implement add/remove schedule management in `configuration_views.py`.
5. Implement the actual scheduled function in `schedule_functions.py`.

Missing step 3 causes scheduler startup `KeyError`.

## Query Pattern

When the codebase expects local query helpers:

- use `Serializer`
- use `Criteria`
- use `JoinTable`
- use `QueryConstants`

Use raw ORM only when that is already the surrounding local pattern or the request does not ask for serializer-style access.

## Logging Pattern

For scheduled or background configuration tasks, add logs for:

- skip reason
- start of execution
- key counts or selection results
- per-item action when needed
- success or no-op
- completion

Use f-strings if the local request or nearby code expects them.

## Validation

Typical checks:

```powershell
python -m py_compile "platform-source/src/django/pam/configuration/configuration_views.py"
python -m py_compile "platform-source/src/django/pam/utils/schedule/schedule_constants.py"
python -m py_compile "platform-source/src/django/pam/utils/schedule/schedule_functions.py"
```

If Ember files were changed, note whether the frontend build was not run.
