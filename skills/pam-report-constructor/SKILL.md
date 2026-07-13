---
name: pam-report-constructor
description: Construct new PAM reports from provided model relationships, output columns, filters, and UI requirements. Use when Codex needs to add or modify a report in this workspace by deriving the data path from Django model relations, then wiring backend report methods, table/query config, routes, export behavior, and Ember report components. Trigger especially for requests like "build a report from these models", "add columns to this report", "create export for this table", or "implement a report using these joins".
---

# PAM Report Constructor

Build reports in this repo from the data model outward instead of copying report code blindly.

## Workflow

1. Identify the target report stack.
- Use `platform-source` for the older standard report stack:
  `reports/report_views.py`, `reports/report_urls.py`, `reports/reportconstants.py`, `reports/utils/report_utils.py`, `webclient/config/tableconfig.py`, `webclient/config/queryconfig.py`, Ember report components/templates.
- Use `pam-source` for the concise-report stack:
  `reportsapp/reportsapp_views.py`, `reportsapp/reportsapp_urls.py`, `reportsapp/report_constants.py`, `reportsapp/reportsapp_utils.py`, `securden_framework/client/constant_files/table_config.py`, `securden_framework/client/constant_files/query_config.py`, Ember concise report components/templates.
- Infer the stack by locating the nearest existing report the user referenced. If unclear, search for the current report id or endpoint first.

2. Convert model relationships into a data plan.
- List the base model.
- List every required join and which fields come from each model.
- Separate:
  - row data fields
  - filter/search fields
  - sort fields
  - export-only fields
  - inner-table/detail fields
- Decide whether query-config alone is enough or whether a custom query/annotation/subquery path is required.

3. Choose the retrieval pattern.
- Use query-config plus `return_data_for_report(...)` when the report is mostly direct model joins and ordinary filtering/sorting.
- Use a custom method when the report needs:
  - per-row annotations or subqueries
  - effective-policy selection or prioritization
  - post-processing of rows
  - combining multiple sources
  - export data that differs from screen data

4. Define the canonical identifiers before editing files.
- Keep one canonical id across backend and frontend.
- Preferred shapes:
  - `report_name_id`: snake_case
  - `table_config`: snake_case
  - `query_config`: snake_case
  - endpoint: `/reports/<name>` or stack-local equivalent
  - Ember route: kebab-case equivalent

5. Wire the backend in this order.
- Add or update the report utility/data method.
- Add table config.
- Add query config if used.
- Add view function and URL.
- Register export/report metadata and any schedule metadata.
- Update constraints or table-name whitelists when the stack requires it.

6. Wire the frontend.
- Add or update the report component with:
  - `table_model` or `baseTableUrl`/`tableUrl`
  - `table_config`
  - `report_name_id`
- Add/update the template wrapper and any modal/detail table.
- Reuse existing jqGrid formatter patterns from neighboring reports.

7. Validate.
- Run `python -m py_compile` on changed Django files.
- Verify all referenced table/query/report ids exist and match exactly.
- Verify export table config exists when export is enabled.
- If the report has a detail modal or export inner table, verify the supporting endpoint and arg names match.
- State whether Ember build/tests were run.

## Construction Rules

- Start from an existing nearby report and copy only the pattern, not the identifiers.
- Prefer extending an existing report method over creating a parallel duplicate if the data source is the same.
- Keep screen and export behavior explicit. If the export table differs from the screen table, create a separate export table config.
- Put display-only formatting in table/component formatters, not in query config.
- Put expensive or policy-derived computations in the backend, but make the table formatter resilient to raw booleans/ints/nulls when practical.
- If a report has a "Details" column, define whether it is:
  - an on-screen action only
  - an export inner table
  - both

## Decision Guide

- Use `query_config` only:
  direct joins, plain sorting, plain filters.
- Use custom `report_utils` logic:
  derived fields, batching, subqueries, effective policy resolution, row enrichment.
- Use a second endpoint or modal table:
  when detail rows are logically one-to-many from a main row.
- Use a separate export config:
  when export must include hidden fields, omit action columns, or include inner tables.

## Required Reference

Read [references/report-stack-map.md](references/report-stack-map.md) before building a new report. It contains the file map, stack selection rules, and a concrete implementation checklist.
