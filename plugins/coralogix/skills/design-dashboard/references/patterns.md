# Dashboard patterns

Each pattern lists the widgets that belong on it. Use these as a starting skeleton; drop widgets the service cannot support (e.g. no metric exists for error rate).

## RED — Rate, Errors, Duration (service overview)

Goal: is the service healthy right now?

| Row | Widget | Viz | Source |
|---|---|---|---|
| 1 | Request rate | stat | metric: `rate(requests_total)` |
| 1 | Error rate | stat | metric: `rate(5xx)/rate(total)` |
| 1 | p99 latency | stat | metric: `histogram_quantile(0.99, ...)` |
| 1 | Saturation (CPU or queue depth) | stat | metric |
| 2 | Requests over time | time-series | metric |
| 2 | Error rate over time | time-series | metric |
| 2 | Latency percentiles (p50/p95/p99) | time-series | metric |
| 3 | Top error messages | table | logs: `$m.severity >= error \| groupby $d.message agg count() as n` |
| 3 | Slowest endpoints | table | metric: top N of `p99 by route` |

## USE — Utilisation, Saturation, Errors (capacity)

Goal: where is the system under pressure?

One row per resource (CPU, memory, disk, network, DB connections). Each row has 3 widgets: Utilisation (%), Saturation (queue / wait time), Errors (ENOMEM, OOMKill, conn refused).

## SLO burn-rate

Goal: are we burning the error budget too fast?

| Row | Widget | Viz |
|---|---|---|
| 1 | Current SLO attainment (last 30d) | stat |
| 1 | Remaining error budget | stat |
| 2 | Multi-window burn rate (5m, 1h, 6h, 24h) | time-series, stacked |
| 3 | Bad events driving burn | table by error type |

Burn rate formula per window W: `(actual_error_rate / target_error_rate)`. Alert when short-window and long-window both exceed 2x simultaneously.

## Incident triage

Goal: first 60 seconds of a page — where do I look?

Big stat widgets on top (green/amber/red), drill-down rows below. Link widgets to the `query` skill by embedding the DataPrime filter in the description.

| Row | Widget |
|---|---|
| 1 | Service health stat (composite) |
| 1 | Latency stat |
| 1 | Error-rate stat |
| 1 | Recent deploy marker (if available) |
| 2 | Error logs last 15m (table) |
| 2 | Slow traces last 15m (table) |
| 3 | Upstream dependency health (time-series) |
| 3 | Downstream dependency health (time-series) |

## RUM / front-end

Goal: what are real users experiencing?

| Row | Widget |
|---|---|
| 1 | Page load p75 | stat |
| 1 | Core web vitals (LCP, FID, CLS) | stats |
| 1 | JS error rate | stat |
| 2 | Page load by route | table |
| 2 | JS errors over time | time-series |
| 3 | Top JS errors (table) |
| 3 | Traffic by country / device | table |
