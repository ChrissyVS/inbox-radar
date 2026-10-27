# Classifying emails

The user's settings are the source of truth. These rules explain how to apply them.

## Must-see

The sender matches a `must_see` group: their address is in `addresses`, or their domain is in `domains`. Nothing else counts, however important the email looks. Important strangers go to a tracker or "Not sure", not to must-see.

A calendar invite or notification sent "on behalf of" a must-see person counts only if it contains a personal message.

**Needs you** when all of these hold:
- the user is in To, not only CC,
- the email asks a question, requests something, or names a date or deadline,
- the user hasn't replied to it, forwarded it or moved it out of the Inbox since (see "Already handled?" in SKILL.md).

## Trackers

Each tracker has:
- `counts`: what qualifies, in the user's words. The main guide.
- `not`: look-alikes that don't qualify. These override `counts`.
- `key`: what one item is (company, person, firm, subject). One email can produce several items (e.g. a demo-day list with five companies), but only when `counts` covers lists.

Match on what the email **is**, not on keywords. A newsletter that mentions "Series A" is not a deal. A forward from a must-see person with "thoughts?" about a company usually is one.

## Not sure

Don't log; list under "Not sure" when:
- you can't tell what the item is (no company or person clearly named),
- it fits a tracker's `counts` but also partly fits its `not`,
- it could belong to two trackers and they log to different places,
- it's a vague "let's catch up, building something new".

## Worked examples

**VC** (trackers: Deals)

| Email | Result |
|---|---|
| "I'm CEO of Robotix, raising a €3M seed for welding robots, deck attached." | Deals → Robotix |
| From partner@othervc.com: "Connecting you with Anna from QuantumCo, we're leading their A and have room." | Deals → QuantumCo, source = the introducer |
| From a colleague: "Fwd: SpaceBoost deck. Fits your thesis? Can you look before Thursday?" | Must-see (Needs you) + Deals → SpaceBoost |
| "Fund III is raising, LP deck attached" | Ignored (LP pitch is in `not`) |

**Founder** (trackers: Investor conversations, Customer & sales leads)

| Email | Result |
|---|---|
| From a seed investor on the board: "Can you send the Q3 numbers by Friday?" | Must-see (Investors & board, Needs you) |
| "Hi, I'm a partner at Northfund, saw your launch, would love to chat." | Investor conversations → Northfund |
| "We'd like a demo for our 200-person ops team." | Customer & sales leads → that company |
| "We help startups with SEO, 15 minutes this week?" | Ignored (vendor) |

**Sales / BD** (trackers: New leads, Partnership requests)

| Email | Result |
|---|---|
| "Got your name from Tom at Acme, we're looking for a tool like yours, budget approved for Q1." | New leads → that company, source = referral from Tom |
| From a key-account domain: "We're reviewing our contract before renewal." | Must-see (Key accounts, Needs you), plus Account risk if enabled |
| "Our platform could integrate with yours, interested in a co-marketing webinar?" | Partnership requests |
