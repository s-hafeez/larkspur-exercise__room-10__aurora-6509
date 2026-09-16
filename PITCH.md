# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: Larkspur disruption-care agent — 9 local tools + 2 via MCP (next_available_day written, fare_rules inherited from MCP server)

Does: Handles flight cancellations and delays: looks up bookings, checks disruption policy, searches rebooking options, issues vouchers, finds the next available travel date, and quotes Handbook fare rules on demand

Number: 2,855 tokens schema tax on every turn across 11 tools; next_available_day alone costs 495 tokens per turn whether it fires or not

Guardrail: Rebooking requires a customer-minted confirmation token; groups, minors, partner flights, and refunds always escalate to a human — the agent cannot override that path

Next: Build 3 — write an eval case with a real PNR and a customer message the agent exists to answer

Still broken: Abusive-tone responses get a calm, normal resolution — TONE_ADDENDUM is empty (Build 4 not yet done)

Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
