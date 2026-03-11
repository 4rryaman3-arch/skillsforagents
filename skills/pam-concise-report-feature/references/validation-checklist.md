# Validation Checklist

## Identifier Consistency

- Keep one canonical `report_name_id` across:
  - Django constants
  - Django report utils wrappers
  - Ember report component
  - Ember template binding

## Endpoint and Routing

- Add URL entry in `reportsapp_urls.py`.
- Ensure view function exists in `reportsapp_views.py`.
- Ensure Ember route/template path matches the backend constant route link.

## Data Config

- If using existing data source, reuse existing `table_config`/`query_config`.
- If adding new table key, update:
  - `table_config.py`
  - `query_config.py`
  - `constraints.py` table whitelist

## Export and Schedule

- Add `REPORT_EXPORT_CONFIG` entry for report generation.
- Add `SCHEDULE_REPORT_GENERATION` mapping if scheduled export is needed.
- Verify wrapper function path strings are correct.

## Quality Checks

- Run `python -m py_compile` on modified Django files.
- Verify no stale import/reference names.
- If Ember files changed, state whether frontend build/tests were run.
