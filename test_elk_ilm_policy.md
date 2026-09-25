Looking at the document, let me analyze the active indices and data streams for each group and build a comprehensive table.A few things worth calling out from the table:

**Consistent pattern across most groups** — logs, metrics, APM metrics, APM logs, and synthetics (custom) all follow the same hot→warm→frozen→delete arc with an effective 10-day total retention. The warm phase is immediate (0d), meaning data is forcemerged and shrunk as soon as it rolls out of hot, then frozen to Pure Storage at day 2, and purged at day 10.

**Traces warm phase is empty** — the `custom-traces-apm-45d-policy` passes through warm at 0d with no actions before freezing. This means traces skip the forcemerge/shrink optimisation that everything else gets. Worth confirming whether that's intentional or an oversight before promoting to prod.

**Traces sampled is essentially a buffer** — the `traces-apm.sampled-default` stream rolls every hour and deletes after an hour. This is by design for tail-based sampling coordination, not a concern.

**Uptime alert indices never delete** — the `.alerts-ilm-policy` is hot-only with no delete phase. Those nine `.internal.alerts-observability.uptime.*` indices will grow indefinitely unless you add a delete phase or set an ILM action explicitly.

**Fleet-managed synthetics diverges** — the browser/http/tcp/icmp templates use `custom-synthetics-pure-14d-policy` which goes to 14d but skips frozen entirely. That means screenshot and network data doesn't land on Pure Storage the way the custom synthetics streams do — it just sits on hot/warm nodes before deletion.
