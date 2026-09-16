# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: Larkspur disruption-care agent on claude-opus-5 — 9 local tools + 2 via MCP (next_available_day written, fare_rules inherited from MCP server), with prompt caching and tone safety active

Does: Handles flight cancellations and delays: looks up bookings, checks disruption policy, searches rebooking options, issues vouchers, finds the next available travel date, and quotes Handbook fare rules on demand; escalates abuse and legal threats to a human

Number: 2,855 tokens schema tax on every turn across 11 tools; turns 2+ read tools and system prompt from cache at ~10% of input cost — the timestamp moved to the user message so the cache prefix is stable

Guardrail: Rebooking requires a customer-minted confirmation token; groups, minors, partner flights, and refunds always escalate to a human — the agent cannot override that path

Next: Run bench.py --label after to measure cache hit rate and cost per contact against the baseline; answer Priya's four questions in this file

Still broken: Abusive-tone handling now in TONE_ADDENDUM; remaining gap is that TONE_ADDENDUM fires on keywords alone — a calm but firm complaint is not an abuse case and could be mis-routed

Lever: intelligence

## Priya asked

Costs: $0.026 per resolved contact with prompt caching vs $6.90 human — but that figure excludes infra, escalation handling, and ops overhead to keep policy rows current
Wrong: First untrue thing: a stale policy row gives the wrong entitlement; recovery is the policy_row_id on every check_policy call, which a human can audit, and irreversible actions require either a customer-minted token or auto-escalate above threshold
Runs it: The digital channel ops team — they update policy data when the Handbook changes and monitor MCP server uptime; no ML expertise required, every tool call and result is logged to the trace
Left out: Refunds, groups over 9 pax, unaccompanied minors, partner codeshare segments, SSR flags, and bookings in active dispute — all hard-escalate to human; these are the highest-risk cases and were explicitly out of scope
