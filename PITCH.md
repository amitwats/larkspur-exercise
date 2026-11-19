# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur that looks up a customer's booking and flight, checks policy, and handles the rebooking conversation using 18 tools.
Does: When a flight is cancelled or delayed, it explains what happened in plain words, finds alternatives, holds a seat for 15 minutes, and rebooks only after the customer presses confirm. Anything the Handbook reserves for people (refunds, hotels, other airlines, minors, group bookings) it hands to a person with a summary.
Number: About $0.07 per customer helped (31,001 input and 966 output tokens on average across 5 ticket types, at Sonnet 5.5 prices of $2 and $10 per million), against $6.90 for a person, so roughly 1% of today's cost.
Safety check: Every ticket type in our eval cases must resolve or hand over to a human correctly before we ship, and the agent never rebooks without the customer's confirm.
Next: Cut the cost further by caching the 6,400 tokens of tool definitions that we resend on every turn, then re-run bench.py to prove it.
Still broken: The abusive-message ticket (R8KD3F) gets a calm, ordinary resolution with no tone handling at all, so the agent does not yet recognise or respond to abuse.
Lever: cost

## Priya asked

Costs: About $0.07 per customer, measured over 5 ticket types with one run each and no caching yet, against $6.90 per customer for a person.
Wrong: The agent can state something untrue with confidence, so a sample of real conversations must be read each week by a named Larkspur reviewer. Any customer who disputes an answer is passed to a person with the conversation summary.
Runs it: A named Larkspur support-operations owner, not our team, watches the weekly sample, edits the tool descriptions when answers drift, and updates policy data when the Handbook changes.
Left out: Refunds, fare-difference collection, hotel approval and other-airline rebooking. We left them out because the Handbook gives them to people, and a wrong answer there costs the customer real money.

Lever: Speed