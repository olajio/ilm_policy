Here's the ILM policy table in markdown:

```markdown
| Group | Active Policy | Active Index Patterns | Hot | Warm | Frozen | Delete | Total Retention |
|---|---|---|---|---|---|---|---|
| Metrics | `metrics` v16 · 2026-07-14 | `metrics-system.*` `metrics-elasticsearch.*` `metrics-logstash.*` `metrics-elastic_agent.*` `metrics-fleet_server.*` `metrics-kubernetes.*` `metrics-windows.*` `metrics-elk-{nprd,prod}` `metrics-class-nprd` `metrics-openshift-*` | 5d · rollover at 5d / 1.8B docs / 30GB | 0d · forcemerge → 1 seg, shrink → 1 shard | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Metrics (APM) | `metrics-apm.app_metrics-default_policy` v10 · 2026-07-14 | `metrics-apm.app.*-default` `metrics-apm.service_*` `metrics-apm.transaction.*` `metrics-apm.internal-default` | 5d · rollover at 5d / 1.8B docs / 30GB | 0d · forcemerge, shrink → 1 shard | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Logs | `logs` v12 · 2026-07-14 | `logs-elastic_agent.*` `logs-system.*` `logs-elasticsearch.*` `logs-logstash.*` `logs-elk-{nprd,prod}` `logs-class-{nprd,prod}` `logs-crowdstrike.*` `logs-sec-*` `logs-openshift-*` `logs-default-{nprd,prod}` | 5d · rollover at 5d / 1.8B docs / 30GB | 0d · forcemerge, shrink → 1 shard | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Logs (APM app) | `logs-apm.app_logs-default_policy` v4 · 2026-07-14 | `logs-apm.app.*-default` `logs-apm.error-default` | 5d · rollover at 5d / 1.8B docs / 30GB | 0d · forcemerge, shrink → 1 shard | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Traces | `custom-traces-apm-45d-policy` v3 · 2026-07-14 | `traces-apm-default` `traces-apm.rum-default` | 5d · rollover at 5d / 1.8B docs / 50GB | 0d · no actions defined | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Traces (sampled) | `traces-apm.sampled_traces-default_policy` v5 · 2024-07-08 | `traces-apm.sampled-default` | 1h · rollover at 1h | — | — | 1h | **~1 hour** |
| Synthetics (custom) | `synthetics` v8 · 2026-07-14 | `synthetics-default-prod` `synthetics-mulesoft-prod` `synthetics-openshift-nprd` `synthetics-openshift-muletest-nprd` | 5d · rollover at 5d / 1.8B docs / 30GB | 0d · forcemerge, readonly, shrink → 1 shard | 2d · searchable snapshot → `pure` | 10d | **10 days** |
| Synthetics (fleet-managed) | `custom-synthetics-pure-14d-policy` v1 · 2026-05-19 | `synthetics-browser.*` `synthetics-http` `synthetics-tcp` `synthetics-icmp` `synthetics-browser.network` `synthetics-browser.screenshot` | 7d · rollover at 7d / 50GB | 7d · no actions | — | 14d | **14 days** |
| Uptime / Alerts | `.alerts-ilm-policy` v500 · 2026-09-21 | `.internal.alerts-observability.uptime.alerts-default-*` `.internal.alerts-observability.uptime.alerts-enterprise-services-*` `.internal.alerts-observability.uptime.alerts-application-systems-*` | 30d · rollover at 30d / 50GB | — | — | — | **Indefinite** (no delete phase) |
```
