# Report File Map

Use this map to apply concise report changes consistently.

## Backend (Django)

- `pam-source/src/django/pam/reportsapp/report_constants.py`
  - Report IDs, labels, tabs, export config, schedule map.
- `pam-source/src/django/pam/reportsapp/reportsapp_utils.py`
  - Data fetch functions, wrappers, export/schedule wrappers.
- `pam-source/src/django/pam/reportsapp/reportsapp_views.py`
  - HTTP view handlers.
- `pam-source/src/django/pam/reportsapp/reportsapp_urls.py`
  - Route registration.
- `pam-source/src/django/pam/securden_framework/client/constant_files/table_config.py`
  - Grid/table definitions.
- `pam-source/src/django/pam/securden_framework/client/constant_files/query_config.py`
  - Query model/joins/criteria.
- `pam-source/src/django/pam/securden_framework/server/constant_files/constraints.py`
  - Allowed table names and endpoint request validation.

## Frontend (Ember)

- `pam-source/src/ember/app/routes/report/`
  - Route modules for reports.
- `pam-source/src/ember/app/components/reports/`
  - Report components (`report_name_id`, URLs, headers).
- `pam-source/src/ember/app/templates/report/`
  - Route templates.
- `pam-source/src/ember/app/templates/components/reports/`
  - Report table template wrappers.

## Existing Third-Party Share Data

- `pam-source/src/django/pam/accountapp/accountapp_views.py`
  - `get_outsiders_table` endpoint.
- `table_config` / `query_config` key:
  - `externalpasswordshare`
