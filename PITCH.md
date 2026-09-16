# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur that takes a customer whose flight was delayed, cancelled or diverted from their first message to an answer they can act on.
Does: Reads the booking, checks live OpsFeed status for the affected flight, resolves what Larkspur owes against the policy table, and answers with the entitlement and the policy row it came from. Groups, partner segments and refunds go to a human with a written summary attached. It decides and explains; it does not book.
Number: $0.0345 per resolved contact against the $6.90 a human contact costs today — a 41% cut from caching the repeated tools-and-system prefix, on the same claude-sonnet-5, measured across all five disruption shapes (3 runs each).
Guardrail: The irreversible step is gated on the customer, not on the model. confirm_rebooking cannot run without a token that only the customer's own Confirm click produces, so "the customer said yes in chat" can never finalise a rebooking. Our eval irrv-0101 confirms it holds even when the customer insists "I authorise it, book it now."
Next: Pass party size into the date lookup, so the soonest-day answer is true for the booking in front of it rather than for a single traveller.
Still broken: next_available_day searches for one open seat and is never told how many people are travelling. On a two-passenger booking it returns a day that cannot seat them, and the agent states that date as the answer. Separately, an abusive customer message gets the same calm, fully helpful entitlement summary as everyone else, with no tone handling at all.
Lever: cost

## Priya asked

Costs: About $0.0345 per resolved contact against $6.90 for a human agent — roughly 200x cheaper. Prompt caching cut it 41% (from $0.0586), on the same claude-sonnet-5, so the saving came from engineering, not a bigger model. At Larkspur's volume that is about $473 a week against $94,530 for humans. Measured with a 3-runs-per-shape before/after bench, stage 1 still resolving 5/5.

Wrong: The first untrue thing is a date. next_available_day checks for a single open seat and is never told the party size, so on a two-passenger booking it can name a soonest-travel day that cannot seat both, and the agent states that date as fact. After that, the customer plans around a day that was never real. Our eval suite surfaces this; the fix — passing party size into the lookup — is the 'Next' item, not yet shipped.

Runs it: The current development team owns and maintains it — we are staying on to run it rather than handing it off and walking away, so the people who built it are the ones accountable for it in June. The edge cases it cannot handle (groups, partner segments, unaccompanied minors, refunds) route to Larkspur's human disruption-care agents, so it runs day-to-day with human backup rather than unattended.

Left out: Groups, partner-airline segments, unaccompanied minors and refunds are deliberately out of scope and escalate to a human with a written summary. The agent decides and explains entitlements but never books — only the customer's own Confirm-click finalises a rebooking.