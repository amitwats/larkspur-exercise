# Client review: Larkspur disruption-care agent

**To:** Room 3  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-19 05:35

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md claims 18 tools, but the static scan of agent.py finds 16 schemas, 7 of them declared in EXTRA_TOOLS.**

The scan counts 16 schemas against a pack of 9. fare_rules is defined as a function in agent.py but its entry in EXTRA_TOOLS is still commented out, and the scan lists no schema for it. The PITCH.md Built line says "using 18 tools" and nothing in the material shows where 18 comes from.

Run python3 run.py --show-tools and paste the full tool list so the 18 can be checked against it.

**2. PITCH.md quotes $0.07 from 31,001 input tokens, but the one committed run in readout-trace.json shows 31,156 in and 983 out.**

The $0.07 comes from "5 ticket types" at one run each, and the Number line gives 31,001 in and 966 out as averages. The committed trace is a single booking: 4 API turns, 5 tool calls, 9.1s wall clock. No bench-after.json or bench compare output is in the material, so the 5-booking average has no artifact behind it.

Run python3 bench.py --label before --stage 1 --runs 3, then python3 bench.py --compare before after once after exists, and paste the compare output.

**3. PITCH.md proposes caching the 6,400 tool-definition tokens, and the scan finds no cache_control anywhere in agent.py.**

readout-trace.json shows 0 read, 0 written, hit ratio None, over 4 turns at about 31,156 input tokens. The 6,400 figure appears in the Next line with no source in the material. The scan reports 16 schemas with descriptions up to 1072 characters (care_entitlements), and 7 of them are yours. Nothing shows how much of the 31,156 the tool block is, or that the prefix is stable between turns.

Run python3 run.py --tool-tax and paste its output next to the 6,400 figure.

**4. tone-0101 is a hard gate in evals/cases.json that PITCH.md says the agent fails, and TONE_ADDENDUM is still 0 characters.**

PITCH.md Still broken says R8KD3F "gets a calm, ordinary resolution with no tone handling". tone-0101 uses that PNR, has hard_gate true, and requires escalate_to_human. Its author field is still "<your name>", as in grnd-0101, irrv-0101, ref-0101 and scope-0101; only team-0101 carries a name, Amit. The Safety check line promises every ticket type resolves correctly before shipping, while the committed trace has 5 gates banked and none for 3.1 or 4.1.

Run python3 eval_harness.py and paste the per-case results, including the tone-0101 line.

**5. The committed trace shows get_flight_status feeding care_entitlements and cause_in_plain_words, which no model upgrade changes.**

Both tool descriptions tell the model to take cause_code from get_flight_status and never from the customer. In readout-trace.json the order is lookup_booking, get_flight_status, check_policy, care_entitlements, cause_in_plain_words, and that matches. Your diff also adds departures_in_window and ground_stop_status with a hard-coded "now" of 2025-05-08 14:10 in their descriptions and an error outside 2025-05-07 to 2025-05-09. Whether those tools get picked correctly depends on the descriptions, and no run in the material exercises them.

Run python3 run.py --all --trace and paste the totals footer, then add one eval case in evals/cases.json that asks which flights leave DEN this afternoon.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | About $0.07 per customer, measured over 5 ticket types with one run each and no caching yet, against $6.90 per customer for a person. | **Thin.** "About $0.07" with "one run each" is honest about its sample, but it has no spread and no bench-after.json compare behind it. The only committed run, readout-trace.json, is one booking at 31,156 in and 983 out. |
| **When it is wrong** | The agent can state something untrue with confidence, so a sample of real conversations must be read each week by a named Larkspur reviewer. Any customer who disputes an answer is passed to a person with the conversation summary. | **Thin.** A weekly read by a named reviewer is a process, but the name is missing and so is the sample size. Nothing in evals/cases.json or PITCH.md says how many conversations per week or what the reviewer checks. |
| **Who runs it** | A named Larkspur support-operations owner, not our team, watches the weekly sample, edits the tool descriptions when answers drift, and updates policy data when the Handbook changes. | **Thin.** "A named Larkspur support-operations owner" has no name in it. TEAM.md and PITCH.md are the places a name would go, and neither shows one for Larkspur. |
| **What you left out** | Refunds, fare-difference collection, hotel approval and other-airline rebooking. We left them out because the Handbook gives them to people, and a wrong answer there costs the customer real money. | **Answered.** The four items match what_automation_will_not_do in agent.py, which returns Handbook section 7, and scope-0101 in evals/cases.json tests the refund case. Hotel approval stays human in care_entitlements, which agrees with your answer. |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (774 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `evals/cases.json`
- `PITCH.md (4 of 4 answers to Priya)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `pod_sync.py`, `run.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
