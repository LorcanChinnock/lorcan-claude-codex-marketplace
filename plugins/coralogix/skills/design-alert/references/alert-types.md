# Alert types

Pick the type based on the question the alert should answer. When in doubt, favour metric-based over log-based — metrics are cheaper to evaluate and less prone to parser drift.

## Log threshold

"Fire when <filter> matches more than N events in W minutes."

Good for:
- Error count spikes without a pre-existing metric (`$m.severity >= error && applicationname == X`).
- Specific exception classes (`$d.exception.type == 'PaymentDeclined'`).
- Audit-style triggers (a specific admin action occurred).

Watch out for:
- Parser drift — if the log format changes, the filter silently stops matching. Add a companion "no data" check where possible.
- Cardinality — `groupby` on high-cardinality fields (user id, request id) blows up.

## Metric threshold

"Fire when <metric> <op> <value> for <duration>."

Good for:
- Latency SLOs (`p99 > 1s for 10m`).
- Error rate (`rate(http_5xx) / rate(http_total) > 0.01 for 5m`).
- Saturation (CPU > 85% for 15m).

Watch out for:
- Short windows causing flap; use a duration long enough to de-noise.
- Threshold set from a single day's data; check a week's range.

## Ratio (A/B)

"Fire when A is more than X% of B."

Good for:
- Error rate as a proportion (preferred over raw count when traffic is bursty).
- Slow-request proportion (`requests > 1s / total_requests`).

Watch out for:
- Low-traffic windows where the ratio is mathematically wild (1 error out of 2 requests = 50%). Add a minimum-volume floor.

## Unique count

"Fire when the number of distinct <field> matching <filter> exceeds N."

Good for:
- "More than 10 distinct users hit this error in 5m."
- Blast-radius detection.

## New value

"Fire when a previously-unseen value appears in <field>."

Good for:
- New error codes, new user agents, new IP ranges.
- Config drift detection.

Watch out for:
- Baseline period must be long enough to have seen normal variance.

## Flow

"Fire when alert A fires, then alert B fires within X min" (or: A fires without B firing).

Good for:
- Multi-stage incidents where the combined signal matters more than either alone.

## Time relative

"Fire when <metric> is X% different from the same time last week/day."

Good for:
- Traffic anomalies (Tuesday at 2pm should look like last Tuesday at 2pm).
- Seasonal metrics where static thresholds fail.
