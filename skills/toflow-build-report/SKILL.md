---
name: toflow-build-report
description: Query CRM data into a saved report and add it to a dashboard. Use this when the user wants a custom view of their data — pipeline value, outreach performance, conversion funnels — beyond what list views and sequence analytics already show.
license: MIT
---
# toflow — Build a Report or Dashboard

## Steps

1. Call `list_datasets` first to see what data is available and the exact field names to use in the report configuration — don't guess at dataset/field names.
2. Call `list_dashboards` to check whether a suitable dashboard already exists for this report, or `create_dashboard` if the user wants a dedicated new view.
3. Always call `validate_and_preview_report` before saving — it validates the configuration and returns a 100-row data preview. Catch configuration errors and confirm the data looks right with the user before persisting anything.
4. Call `create_report` only after the preview looks correct. This saves the report permanently to a dashboard.
5. Use `run_report` to re-fetch the latest data for an existing saved report.

## Guardrails

- Never call `create_report` without first calling `validate_and_preview_report` and showing the preview to the user.
- If the preview looks wrong (empty, unexpected values), fix the filter/field configuration before saving — don't save a broken report and iterate afterward.
