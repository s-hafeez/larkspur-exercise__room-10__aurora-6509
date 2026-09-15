# Overnight review: Larkspur disruption-care agent

**To:** s-hafeez__larkspur-exercise__room-10__aurora-6509  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-15 13:00

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

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py's diff fixes the message history bug by appending response.content directly instead of text_of(response).**

The diff changes messages.append({"role": "assistant", "content": text_of(response)}) to messages.append({"role": "assistant", "content": response.content}), and moves answer = text_of(response) to after the loop instead of inside it. This means the full content blocks, including tool_use blocks, now go into message history rather than just the trailing text. The last committed trace shows a clean 4-turn, 3-tool run (lookup_booking, get_flight_status, check_policy), consistent with this fix working on at least one path.

Run python3 verify.py 1.2 and paste the pass/fail output to confirm the fix holds against the gate it targets.

**2. search_alternatives description grew from the placeholder string "search" to 214 characters in this pod's diff.**

The template shipped with description: "search" for this tool. This pod's diff replaces it with a 214-character description naming when to call it ("after get_flight_status confirms a cancellation or significant delay") and what it returns ("a list of alternatives, each with an option_id that hold_seat needs"). The static scan confirms the description is now 214 characters. This tool was not exercised in the last committed trace, so nothing shows whether the new description changes call behavior.

Run python3 run.py <PNR> --trace on a booking that requires rebooking and confirm search_alternatives fires with the new description in place.

**3. TONE_ADDENDUM sits at 0 characters and EXTRA_TOOLS at 0 entries, both untouched by this pod's diff.**

The static scan confirms TONE_ADDENDUM is still empty and EXTRA_TOOLS is an empty list, and the six-line diff does not touch either. LOCAL_TOOLS has no executors registered. Nothing in the nine tool schemas or the one committed trace speaks to tone or to any tool beyond the shipped nine.

Run python3 run.py --show-tools and confirm the tool count stays at nine until EXTRA_TOOLS is populated.

**4. The one committed trace shows 0 cache tokens read or written against 12,933 input tokens per turn.**

readout-trace.json records tokens: 12933 in, 713 out and prompt caching: 0 read, 0 written, hit ratio None across 4 API turns. The system prompt, tool list, and growing message history are being resent uncached on every turn of this single run. No cache_control call exists anywhere in agent.py, confirmed by the static scan.

Run python3 bench.py --label baseline --stage 1 --runs 3 and paste the token totals to see whether repeat runs still show zero cache hits.

**5. PITCH.md is untouched and no evals/cases.json exists in this repository.**

PITCH.md remains byte-identical to the shipped template, and the file list confirms no eval cases directory exists yet. The only evidence of behavior is the single trace in readout-trace.json: 4 turns, 3 tool calls, 12.9s wall clock. There is no case set or before/after comparison to say whether the diff's history fix or the new search_alternatives description generalizes past that one run.

Run python3 eval_harness.py once eval cases exist and paste the pass count against the denominator.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (226 lines)`
- `PITCH.md (unchanged template)`
- `TEAM.md (unchanged template)`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
