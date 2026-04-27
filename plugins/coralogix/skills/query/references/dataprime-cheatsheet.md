# DataPrime cheatsheet

Common idioms for the `get_logs` filter expression. Authoritative syntax lives in `read_dataprime_intro_docs` — call that when in doubt rather than guessing operators.

## Basic shape

DataPrime queries are pipeline-style: `source logs | filter <expr> | groupby <field> | top <n>`. The `get_logs` MCP tool typically takes the filter and groupby parts; the source is implicit.

## Filtering

| Goal | Idiom |
|---|---|
| Severity at or above warning | `$m.severity >= warning` |
| Application name | `$l.applicationname == 'checkout'` |
| Subsystem name | `$l.subsystemname == 'api'` |
| Substring in message | `$d.message.contains('timeout')` |
| Regex on message | `$d.message ~ '/timeout|deadline/'` |
| Field exists | `$d.user_id.isPresent()` |
| Field equals | `$d.http.status_code == 500` |
| Numeric range | `$d.duration_ms > 1000 && $d.duration_ms < 5000` |
| Combine | `&&`, `\|\|`, `!`; wrap in `()` |

`$m` = metadata (severity, timestamp). `$l` = label (applicationname, subsystemname). `$d` = data (the parsed JSON log body).

## Aggregation

| Goal | Idiom |
|---|---|
| Count by field | `... | groupby $d.error_code agg count() as n | orderby n desc` |
| Top N | `... | top 10 by $d.user_id` |
| Distinct | `... | distinct $d.trace_id` |

## Time

`get_logs` takes time bounds as parameters; do not bake them into the filter. Use ISO-8601 with timezone.

## Common mistakes

- **Wrong namespace**: `applicationname` is on `$l`, not `$d`. If a filter on `$d.applicationname` returns nothing, that is why.
- **Severity strings**: lowercase (`error`, `warning`, `info`, `debug`), not uppercase.
- **Quote style**: single quotes for strings. Double quotes are not always accepted.
- **Substring vs equals**: `==` is exact; use `.contains()` or `~ '/regex/'` for partial matches.
- **Field doesn't exist**: filtering on a field the parser never extracts returns zero results silently. Run `get_schemas` first.

## Worked examples

Errors from a service in the last hour:

```
$l.applicationname == 'checkout' && $m.severity >= error
```

Latency timeouts grouped by endpoint:

```
$l.applicationname == 'api' && $d.duration_ms > 5000
| groupby $d.http.route agg count() as n
| orderby n desc
```

Failed payments for a specific user:

```
$l.applicationname == 'payments' && $d.user_id == 'u_12345' && $d.outcome == 'failed'
```
