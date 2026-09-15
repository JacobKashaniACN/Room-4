# Overnight review: Larkspur disruption-care agent

**To:** JacobKashaniACN__Room-4  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-15 12:56

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

**1. agent.py's search_alternatives description grew from 6 characters to 780 in this pod's diff.**

The template shipped with description "search" and a bare pnr property. This team's diff replaces it with 780 characters covering when to call it, what it returns (option_id, flight numbers, seats, operating carrier), and how option_id feeds hold_seat and check_policy. That is a description change, not a model change, and it is the kind of thing that moves tool-selection accuracy regardless of which model sits behind it.

Run python3 run.py --tool-tax and paste the token cost of the new description against the old one.

**2. The wire run in readout-trace.json shows only 3 of the 9 tools ever get called.**

readout-trace.json records lookup_booking, get_flight_status, check_policy called in that order, across 4 API turns and 13826 input tokens against 641 output tokens. search_alternatives, the tool this pod rewrote, does not appear in this trace. Nobody has a run yet showing the rewritten description actually changes what gets picked.

Run python3 run.py K7PQ2M --trace on a case that forces a rebooking path and paste the tools-called line.

**3. tool_list() in agent.py now merges mcp_client.tools() but EXTRA_TOOLS and LOCAL_TOOLS are still empty.**

The diff changes the return line to build_tools() + EXTRA_TOOLS + mcp_client.tools(), wiring in a remote source of tools. The static scan still reports EXTRA_TOOLS at 0 declared and no LOCAL_TOOLS executors, so whatever next_available_day now returns comes entirely from the MCP server, unverified from this file. Nothing in the trace confirms an MCP tool was ever called or returned.

Run python3 run.py --show-tools and paste the full tool list including anything sourced from mcp_client.

**4. The date format for get_flight_status flipped from MM/DD/YYYY to YYYY-MM-DD in this pod's diff with no test attached.**

The schema's date field description changed from "MM/DD/YYYY" to "YYYY-MM-DD". That is a real fix if the upstream OpsFeed tool expects ISO dates, but there is no eval case in this repository and no evals/cases.json file to show a call was ever made with the new format and returned a correct status.

Run python3 verify.py 1.3 and paste the result for the get_flight_status gate.

**5. TONE_ADDENDUM is 0 characters and PITCH.md is still the unedited template.**

The static scan confirms TONE_ADDENDUM at zero characters, so nothing constrains how the model talks to a disrupted passenger beyond SYSTEM_PROMPT. PITCH.md has not been touched, so there is no stated case yet for what this build is optimizing or what tradeoff a model choice would need to serve.

Run python3 readout.py after filling PITCH.md and paste the resulting evidence block.

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

- `agent.py (251 lines)`
- `PITCH.md (unchanged template)`
- `TEAM.md (unchanged template)`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
