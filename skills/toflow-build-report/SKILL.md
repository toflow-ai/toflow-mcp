---
name: toflow-build-report
description: Query CRM data into a saved report and add it to a dashboard. Use this when the user wants a custom view of their data — pipeline value, outreach performance, conversion funnels — beyond what list views and sequence analytics already show.
license: MIT
---
# toflow — Build a Report or Dashboard

## Steps

1. Call `list_datasets` first and use only field names it returns — never guess. If the user's request is ambiguous (unclear metric, grouping, or filter), ask before proceeding.
2. Call `list_dashboards` to check whether a suitable dashboard already exists for this report, or `create_dashboard` if the user wants a dedicated new view.
3. Call `get_report_guide` for the exact configuration shape. In short: `query` takes `group_by` (groupable fields), `metrics` (`{field, function, alias}` with `count`/`count_distinct`/`sum`/`avg`/`min`/`max`), `select_fields`, `order_by`, and `limit`; `chart` takes `chart_type` (`line`/`bar`/`area`/`pie`/`scatter`/`table`/`kpi`/`funnel`/`heatmap`), `x_axis`, `y_axis`, `color_field`; `filters` is a list of `{field, operator, value}` (`eq`, `contains`, `in`, `between`, `before`/`after`, `is_null`, etc.).
4. Always call `validate_and_preview_report` before saving. Show the preview rows to the user and ask if it looks right before saving — if `valid=false`, explain the errors in plain terms and don't just retry blindly.
5. Call `create_report` only after the user has seen the preview and confirmed. This saves the report permanently to a dashboard.
6. Use `run_report` to re-fetch the latest data for an existing saved report.

## Guardrails

- Never call `create_report` without first calling `validate_and_preview_report` and showing the preview to the user.
- If the preview looks wrong (empty, unexpected values), fix the filter/field configuration before saving — don't save a broken report and iterate afterward.
