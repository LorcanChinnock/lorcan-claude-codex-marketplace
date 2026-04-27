# Alert pre-commit checklist

Run through this before presenting the draft to the user. Failures are not blockers — they are things the user should see called out in the Checklist section of the draft.

## Signal quality

- [ ] Filter validated against real data in the last 24h? (ran `get_logs` or `metrics__range_query`, got non-zero and non-flood result)
- [ ] Threshold is sensible — not always-on under normal load, not never-firing even under the worst recent day.
- [ ] Window is long enough to de-noise (typically ≥5 min for log thresholds, ≥10 min for metric thresholds).
- [ ] Query survives label cardinality — no `groupby` on trace_id, request_id, or user_id in the firing expression.

## Definition hygiene

- [ ] Name follows `<severity>: <service> — <what>` convention.
- [ ] Description is 1–3 sentences and answers "what fires this?" and "what do I do about it?".
- [ ] Runbook URL is set OR explicitly `TODO: add runbook`. Never silently omitted.
- [ ] Labels include at least `team`, `service`, `severity`.

## Noise control

- [ ] Dedup / group-by configured — an alert that fires once per matching log line is almost never what the user wants.
- [ ] Notification channel matches severity (critical → pager, warning → Slack; confirm with user if ambiguous).
- [ ] Silence during deploys considered? (note in description if the alert is known to be noisy during rollouts)

## Drift resilience

- [ ] If the underlying log format changed, would the filter still work? (filters on top-level fields are more resilient than those on deeply nested parsed fields)
- [ ] "No data" companion considered for high-stakes log alerts — if the filter matches zero because the parser broke, fire a separate low-urgency alert.

## Common failure modes to surface

If any of these are likely, call them out in the draft Summary:

- **Too sensitive**: threshold set using a quiet period; will flood during normal peak.
- **Too loose**: matches during every deploy; user will mute it within a week.
- **Orphan runbook**: runbook URL set but page doesn't exist / hasn't been updated for the new alert.
- **Duplicate**: an alert already exists with an overlapping condition — look it up via `get_alert_event_details` and note it.
