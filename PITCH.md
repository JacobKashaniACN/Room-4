# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur that takes a customer whose flight was delayed, cancelled or diverted from their first message to an answer they can act on.
Does: Reads the booking, checks live OpsFeed status for the affected flight, resolves what Larkspur owes against the policy table, and answers with the entitlement and the policy row it came from. Groups, partner segments and refunds go to a human with a written summary attached. It decides and explains; it does not book.
Number: $0.049 per resolved contact against the $6.90 a human contact costs today, measured across all five disruption shapes.
Guardrail: The irreversible step is gated on the customer, not on the model. confirm_rebooking cannot run without a token that only the customer's own Confirm click produces, so "the customer said yes in chat" can never finalise a rebooking.
Next: Pass party size into the date lookup, so the soonest-day answer is true for the booking in front of it rather than for a single traveller.
Still broken: next_available_day searches for one open seat and is never told how many people are travelling. On a two-passenger booking it returns a day that cannot seat them, and the agent states that date as the answer. Separately, an abusive customer message gets the same calm, fully helpful entitlement summary as everyone else, with no tone handling at all.
Lever: cost

## Priya asked

Costs: 
Wrong:
Runs it: 
Left out: 