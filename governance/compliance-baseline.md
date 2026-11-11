# Compliance Baseline (revision 3.2)

This baseline applies to every manifest under `catalog/services/`. The audit
date is the date on which the review is performed.

## Rules

| Code | Severity | Condition |
| --- | --- | --- |
| `encryption_gap` | critical | `data_classification` is `restricted` or `confidential` and `encryption_at_rest` is `false` |
| `waf_missing` | critical | `tier` is `1` and `exposure` is `public` and `waf_enabled` is `false` |
| `eol_runtime` | high | `runtime` is one of `python3.9`, `java11`, `node16`, `php7.4` |
| `review_overdue` | medium | `last_reviewed` is more than 365 days before the audit date |

A service may violate multiple rules. The service severity is the highest
severity among its violations (`critical` > `high` > `medium`).

## Catalog recording

Rewrite each manifest's `compliance` object with:

- `status`: `remediation_required` when the highest severity is `critical` or
  `high`; `review_required` when it is `medium`; `compliant` when there are no
  violations.
- `severity`: the highest severity code, or `none`.
- `violations`: the alphabetically sorted list of applicable rule codes.

## Remediation tickets

For every service whose highest severity is `critical` or `high`, open a
tracking ticket in this repository titled `[COMPLIANCE] <service_id>` with the
labels `compliance` and `severity/<level>` (`severity/critical` or
`severity/high`). The ticket body must state the `service_id`, `owner_team`,
`severity` and the `violations` (comma separated). Services whose highest
severity is `medium`, and compliant services, must not receive a ticket.
