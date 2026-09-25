The issue is that too much content is crammed into each cell. Here's a restructured version that splits the index patterns and tier details into more digestible rows:

```markdown
## ILM Policy — Data Tier Transitions & Retention

| Group | Active Policy | Hot | Warm | Frozen | Delete | Total Retention |
|---|---|---|---|---|---|---|
| **Metrics** | `metrics` v16 · 2026-07-14 | 5d · 30GB / 1.8B docs | 0d · forcemerge, shrink | 2d · snapshot → `pure` | 10d | **10 days** |
| **Metrics (APM)** | `metrics-apm.app_metrics-default_policy` v10 · 2026-07-14 | 5d · 30GB / 1.8B docs | 0d · forcemerge, shrink | 2d · snapshot → `pure` | 10d | **10 days** |
| **Logs** | `logs` v12 · 2026-07-14 | 5d · 30GB / 1.8B docs | 0d · forcemerge, shrink | 2d · snapshot → `pure` | 10d | **10 days** |
| **Logs (APM)** | `logs-apm.app_logs-default_policy` v4 · 2026-07-14 | 5d · 30GB / 1.8B docs | 0d · forcemerge, shrink | 2d · snapshot → `pure` | 10d | **10 days** |
| **Traces** | `custom-traces-apm-45d-policy` v3 · 2026-07-14 | 5d · 50GB / 1.8B docs | 0d · ⚠️ no actions | 2d · snapshot → `pure` | 10d | **10 days** |
| **Traces (sampled)** | `traces-apm.sampled_traces-default_policy` v5 · 2024-07-08 | 1h | — | — | 1h | **~1 hour** |
| **Synthetics (custom)** | `synthetics` v8 · 2026-07-14 | 5d · 30GB / 1.8B docs | 0d · forcemerge, readonly, shrink | 2d · snapshot → `pure` | 10d | **10 days** |
| **Synthetics (fleet)** | `custom-synthetics-pure-14d-policy` v1 · 2026-05-19 | 7d · 50GB | 7d · no actions | — | 14d | **14 days** |
| **Uptime / Alerts** | `.alerts-ilm-policy` v500 · 2026-09-21 | 30d · 50GB | — | — | ❌ none | **Indefinite** |

### Active index patterns by group

| Group | Index patterns |
|---|---|
| **Metrics** | `metrics-system.*` · `metrics-elasticsearch.*` · `metrics-logstash.*` · `metrics-elastic_agent.*` · `metrics-fleet_server.*` · `metrics-kubernetes.*` · `metrics-windows.*` · `metrics-elk-{nprd,prod}` · `metrics-class-nprd` · `metrics-openshift-*` |
| **Metrics (APM)** | `metrics-apm.app.*-default` · `metrics-apm.service_*` · `metrics-apm.transaction.*` · `metrics-apm.internal-default` |
| **Logs** | `logs-elastic_agent.*` · `logs-system.*` · `logs-elasticsearch.*` · `logs-logstash.*` · `logs-elk-{nprd,prod}` · `logs-class-{nprd,prod}` · `logs-crowdstrike.*` · `logs-sec-*` · `logs-openshift-*` · `logs-default-{nprd,prod}` |
| **Logs (APM)** | `logs-apm.app.*-default` · `logs-apm.error-default` |
| **Traces** | `traces-apm-default` · `traces-apm.rum-default` |
| **Traces (sampled)** | `traces-apm.sampled-default` |
| **Synthetics (custom)** | `synthetics-default-prod` · `synthetics-mulesoft-prod` · `synthetics-openshift-nprd` · `synthetics-openshift-muletest-nprd` |
| **Synthetics (fleet)** | `synthetics-browser.*` · `synthetics-http` · `synthetics-tcp` · `synthetics-icmp` · `synthetics-browser.network` · `synthetics-browser.screenshot` |
| **Uptime / Alerts** | `.internal.alerts-observability.uptime.alerts-default-*` · `.internal.alerts-observability.uptime.alerts-enterprise-services-*` · `.internal.alerts-observability.uptime.alerts-application-systems-*` |

> **Notes:**
> - ⚠️ Traces warm phase has no actions defined — forcemerge/shrink optimisation is skipped before freezing.
> - ❌ Uptime/Alerts has no delete phase — indices grow indefinitely until manually managed.
> - All frozen snapshots target the `pure` repository.
> - Traces (sampled) is an intentional 1-hour buffer used for tail-based sampling coordination.
```

The key change is splitting it into two tables — one focused on the tier transition timeline (narrow, readable columns) and a second dedicated to index patterns (one row per group, patterns separated by `·` instead of stacked in a cell). The notes section captures the three actionable flags without burying them in a cell.
