# Widget visualisation — when to use what

Pick the visualisation from the question it answers, not from what looks good.

## Stat

Single number, optionally coloured by threshold. Use for:
- Current value of a metric that has a sensible "right now" reading (error rate, p99, request count).
- Composite health indicators.

Pitfalls:
- A stat without a comparison ("vs previous hour" or "vs target") loses meaning fast. Always show the delta or the threshold colour.
- Stats averaged over a long window hide recent spikes. Keep the window ≤15 min for real-time dashboards.

## Time-series line / bar

Metric or log count plotted over time. Use for:
- Trend detection.
- Before/during/after comparisons.
- Percentile curves (p50/p95/p99 stacked).

Pitfalls:
- Too many lines (>8) makes the chart unreadable. Aggregate or use a heatmap.
- Auto-scaled y-axis hides flatlines at high values. Pin the axis if that matters.

## Table

Rows = entities, columns = attributes or aggregated metrics. Use for:
- Top N (endpoints, users, error codes).
- Cross-tabs (service × error type).
- Anything the user will want to copy/export.

Pitfalls:
- Tables are poor at showing trends. Pair with a sparkline column if trend matters.

## Heatmap

Density over two dimensions (usually time × bucket). Use for:
- Latency distribution over time — reveals bimodal behaviour a p99 hides.
- Request volume by hour × day-of-week (seasonality).

Pitfalls:
- Bucket choice matters; wrong buckets hide the story. Prefer histogram-metric-backed heatmaps over log-derived.

## Pie / donut

Almost never. Only for:
- Truly mutually-exclusive categories with ≤5 slices.

Pitfalls:
- Hard to compare slice sizes. A bar chart is almost always better. If tempted to use more than 5 slices, definitely use a bar chart.

## Gauge

Rarely. Use only for:
- Bounded 0–100% metrics where the viewer needs to read "am I near the limit" at a glance (disk %, CPU %).

Pitfalls:
- Adds little over a coloured stat. Prefer the stat.

## Log panel

Streaming / near-real-time log view filtered by a DataPrime expression. Use for:
- Incident triage dashboards where seeing the actual error text matters.

Pitfalls:
- Expensive. Keep the filter tight. Do not put a log panel on an executive dashboard.

## Trace table

List of recent slow or erroring traces. Use for:
- Drill-down rows on triage / service-overview dashboards.

Pitfalls:
- Requires correlation with a time range the user can actually act on; show only the last 15–60 min by default.
