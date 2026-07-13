# Report Stack Map

Use this reference to choose the correct stack and touch the minimum necessary files.

## 1. Pick The Stack

### `platform-source`

Use this when the report lives in the older standard report system.

Primary files:
- `platform-source/src/django/pam/reports/report_views.py`
- `platform-source/src/django/pam/reports/report_urls.py`
- `platform-source/src/django/pam/reports/reportconstants.py`
- `platform-source/src/django/pam/reports/utils/report_utils.py`
- `platform-source/src/django/pam/webclient/config/tableconfig.py`
- `platform-source/src/django/pam/webclient/config/queryconfig.py`
- `platform-source/src/django/pam/security/constraints.py`
- `platform-source/src/ember/app/components/reports/`
- `platform-source/src/ember/app/templates/components/reports/`
- `platform-source/src/ember/app/templates/report/`

Typical signs:
- endpoint under `/reports/...`
- uses `return_data_for_report(...)`
- export metadata in `reportconstants.py`
- jqGrid config in `webclient/config/*`

### `pam-source`

Use this when the report lives in the concise-report system.

Primary files:
- `pam-source/src/django/pam/reportsapp/reportsapp_views.py`
- `pam-source/src/django/pam/reportsapp/reportsapp_urls.py`
- `pam-source/src/django/pam/reportsapp/report_constants.py`
- `pam-source/src/django/pam/reportsapp/reportsapp_utils.py`
- `pam-source/src/django/pam/securden_framework/client/constant_files/table_config.py`
- `pam-source/src/django/pam/securden_framework/client/constant_files/query_config.py`
- `pam-source/src/django/pam/securden_framework/server/constant_files/constraints.py`
- `pam-source/src/ember/app/components/reports/`
- `pam-source/src/ember/app/templates/components/reports/`
- `pam-source/src/ember/app/routes/report/`

Typical signs:
- report listed under concise or quick reports
- utilities live under `reportsapp_*`
- jqGrid config in `securden_framework/client/constant_files/*`

## 2. Build The Data Plan

For every new report, write this down before editing:

- base model
- joins
- selected output fields
- row identifier
- filters
- sortable columns
- export-only columns
- detail/inner-table source if any

If the report needs:
- latest activity per row
- effective policy per row
- aggregated values
- unioned data
- values not naturally represented by query config

use a custom backend method instead of trying to force everything through query config.

## 3. Backend Checklist

### `platform-source`

1. Add/update data method in `reports/utils/report_utils.py`
2. Add/update `tableconfig.py`
3. Add/update `queryconfig.py` if applicable
4. Add/update view in `reports/report_views.py`
5. Add/update URL in `reports/report_urls.py`
6. Add/update export/report metadata in `reports/reportconstants.py`
7. Add/update `security/constraints.py` if endpoint or table name needs whitelisting

### `pam-source`

1. Add/update data method in `reportsapp/reportsapp_utils.py`
2. Add/update `table_config.py`
3. Add/update `query_config.py` if applicable
4. Add/update view in `reportsapp/reportsapp_views.py`
5. Add/update URL in `reportsapp/reportsapp_urls.py`
6. Add/update report/export/schedule metadata in `reportsapp/report_constants.py`
7. Add/update `constraints.py` table whitelist if needed

## 4. Frontend Checklist

- add/update report component
- add/update wrapper template
- add/update route template or route file if the stack uses them
- set the correct:
  - `table_model` or `tableUrl`
  - `table_config`
  - `report_name_id`
- if there is a details modal:
  - add modal component
  - add detail table component
  - add detail endpoint

## 5. Export Rules

- If export should look exactly like the screen table, reuse the same data method only if action columns and hidden UI-only columns are not a problem.
- Create a separate export table config when:
  - screen table contains action columns
  - export needs extra hidden identifiers
  - export includes inner tables
  - export needs different labels/order

## 6. Validation

- run `python -m py_compile` on changed Django files
- confirm exact string matches for:
  - `report_name_id`
  - `table_config`
  - `query_config`
  - endpoint function
  - URL path
- confirm export path works from metadata to data method
- if there is a detail table or inner export table:
  - confirm function arg names match the row fields exactly
