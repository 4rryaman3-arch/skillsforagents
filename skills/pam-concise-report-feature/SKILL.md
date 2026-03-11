---
name: pam-concise-report-feature
description: Implement or modify PAM concise/consise reports end-to-end. Use when Codex needs to add a new report or update an existing one across report identifiers, backend data methods, report endpoints, table/query config, frontend route/component wiring, and export/schedule metadata for repetitive report work.
---

# Pam Concise Report Feature

Create concise reports using the existing PAM reporting pattern instead of ad-hoc one-off implementations.

## Workflow

1. Identify the report scope and target identifier.
2. Reuse an existing data source if possible (for third-party shares, prefer `externalpasswordshare`).
3. Wire backend report generation path.
4. Wire frontend route/component/template path.
5. Register export/schedule/report metadata.
6. Validate constraints, naming consistency, and compile safety.

## Identify Target Values

- Use lowercase snake_case for `report_name_id`.
- Keep a single canonical identifier across backend + frontend.
- In this codebase, consolidated naming often appears as `consise_*` (typo retained for compatibility). Follow nearby existing naming in the touched module.
- Recommended shape:
  - `report_name_id`: `<report>_report`
  - `table_config`: `<table_config_key>`
  - endpoint: `/reports/<endpoint_name>`
  - route: `report.<route-link>`

## Backend Implementation

1. Add or update report constants in `reportsapp/report_constants.py`:
- quick report tabs/labels (`REPORT_TABS`, `QUICK_REPORT_TABS`, `USER_QUICK_REPORTS` as applicable)
- report id constants
- `REPORT_EXPORT_CONFIG` entry
- schedule map entry (`SCHEDULE_REPORT_GENERATION`)

2. Implement data method wrappers in `reportsapp/reportsapp_utils.py`:
- table data method
- wrapper for export generation
- scheduled wrapper (if scheduled exports supported)
- ensure `report_name_id` is passed to report utilities

3. Add/adjust endpoint handler in `reportsapp/reportsapp_views.py`.

4. Register URL in `reportsapp/reportsapp_urls.py`.

5. If new table/query keys are needed:
- add `table_config` in `securden_framework/client/constant_files/table_config.py`
- add query config in `securden_framework/client/constant_files/query_config.py`
- whitelist in `securden_framework/server/constant_files/constraints.py` (`table_names` and endpoint constraints)

## Frontend Implementation

1. Add/adjust route file in `ember/app/routes/report/`.
2. Add/adjust report component in `ember/app/components/reports/` with:
- `report_name_id`
- `baseTableUrl`/`tableUrl`
3. Add/adjust template in `ember/app/templates/components/reports/`.
4. Add route template in `ember/app/templates/report/` (typically through `reports/quick-report-default`).
5. Ensure route link in backend constants matches Ember route naming.

## Existing Third-Party Share Source

- Existing source for "accounts shared with third parties":
  - model/query source: `externalpasswordshare`
  - API source: `/accountapp/get_outsiders_table`
- Prefer reusing this source for concise third-party share reports before creating new joins.

## Validation

1. Ensure the same `report_name_id` is used in all touched backend/frontend paths.
2. Verify schedule/export metadata exists when report generation is expected.
3. Run `python -m py_compile` on changed Django files.
4. If Ember changed, note whether frontend build/tests were run.
5. Confirm constraints allow the new table and endpoint.

## References

- For file-level checklist and mappings, read:
  - [references/report-file-map.md](references/report-file-map.md)
  - [references/validation-checklist.md](references/validation-checklist.md)
