# 04 Consultant Roundtable — a real run

Five isolated lenses, two rounds, one deterministic tally in between. This is a transcript of [template 04](../prompts/04-consultant-roundtable.md) executed on 2026-08-03 in Claude Code: one canary agent plus five consultant subagents, dispatched in parallel, each with fresh context and none able to see another's output or the aggregate before round two. The decision: a four-person team launching an AI research assistant for boutique consultancies must pick per-seat, usage-based, or hybrid pricing to launch with in thirty days. Cost: ~124k subagent tokens measured across the canary and round one, plus an estimated ~125k for round two — estimated, not measured, because the round-two returns arrived by manual relay without usage metadata (see the plumbing note below). Wall-clock 33 minutes end to end, of which measured agent compute was about four minutes; the rest was orchestrator polling and relay. All consultant text below is verbatim.

## The decision

> We are a four-person team launching an AI research assistant sold to boutique consultancies (5-50 person firms). Should we launch with per-seat pricing, usage-based pricing, or a hybrid? Our costs are dominated by LLM tokens and vary 10x between light and heavy users; buyers at these firms budget per-headcount and hate variable bills; we have no pricing data of our own yet and need to pick something to launch with in the next month.

## Step 0: the gate, and what happened to it

The template's Step 0 is a gate — it says do not dispatch until the decision pins down the choice, the constraints, the deadline, and what failure looks like, and if any is ambiguous, stop and ask. The person requesting this run pre-empted that: *use your judgment on any unstated details and state the assumptions at the top.*

So the gate was not satisfied, it was waived. Rather than guess at what was missing, I ran Step 1b first and let the canary tell me.

## Step 1b: the canary (verbatim)

One agent, one instruction: before taking any position, list every fact this brief does not give you.

> **Tier 1 — leave these open and five consultants answer five different questions**
>
> 1. **The failure condition and horizon.** The brief names none. Give the metric and threshold ("wrong" = blended gross margin below X%, or fewer than N paying firms by day 90) plus current cash runway in months and the next fundraise milestone. Without it one seat optimizes margin, one logo count, one runway.
>
> 2. **Reversibility.** Is this the public list price for the next 18 months, or the price for the first ~10 design partners? State the intended repricing date and whether launch contracts will carry a price-change / grandfathering clause. This single fact separates "what is the right pricing model" from "what is the cheapest experiment."
>
> 3. **What a seat is.** Expected paying seats per firm (3 power users vs. all 30 heads), whether the product is used by every consultant or by one researcher running work for the firm, and whether named-seat enforcement exists at launch. "Buyers budget per headcount" is unusable until we know whose headcount.
>
> 4. **Absolute COGS, not the 10x ratio.** Dollar cost to serve per month for a light and a heavy user, the heavy-user share of the base, and the worst single-account month seen in internal use. 10x on $3 is noise; 10x on $150 is the whole company. Also: the gross-margin floor the founders will actually accept.
>
> **Tier 2 — change the answer, not the question**
>
> 5. **Willingness-to-pay anchor.** What these firms already pay per head for research — contract researcher day rate, existing research/data subscriptions — and which budget line this purchase comes from.
>
> 6. **Named competitors' published pricing models and price points** — the comparison the buyer will make in the room.
>
> 7. **Buyer and contract shape.** Who signs at a 5–50 person firm, the deal size above which it becomes a committee purchase, contract term, and annual prepay vs. monthly.
>
> 8. **Billing capability by launch date.** Whether the stack can meter usage, invoice overages, and enforce hard caps within one month. Hybrid dies on this constraint, not on strategy.
>
> **Tier 3**
>
> 9. **COGS trajectory:** expected cost per task 12 months out given planned model-mix, routing, and caching work — decides whether price should be locked.
>
> 10. **Signal they may not be counting as "data":** waitlist size, design-partner usage logs, and how many prospects have already named a number. "No pricing data" is not "no users."

Ten holes in a brief that read as complete. The canary cost one round trip and 27k tokens.

## The assumptions I patched in

There was no user available to answer, so I invented answers and labelled them as inventions in every dispatch. Every consultant saw this block and was told: treat as given, but flag any that your lens says is load-bearing.

- **A1.** "Getting it wrong" = blended gross margin below 60% at 20 paying firms, OR fewer than 15 paying firms by day 120. Runway 14 months, no revenue today; next raise targeted for month 12 and needs a credible ARR plus margin story.
- **A2.** This is the public list price, intended to hold ~12 months. Launch contracts month-to-month with a clause: price may change on 30 days' notice, existing terms honored for 6 months.
- **A3.** A seat = a named login. Expect 3-8 actual users per firm out of 5-50 heads; usage concentrated, typically 1-2 people run most research and share the output. No technical enforcement against credential sharing at launch.
- **A4.** Internal use suggests ~$12/month COGS for a light user, ~$120/month for a heavy user, worst observed single account ~$300 in one month. Founders will not accept blended gross margin below 55%.
- **A5.** These firms pay contract researchers ~$400-800/day and hold a few $50-200/seat/month data and research subscriptions.
- **A6.** Competitors: general assistants at ~$25-30/seat/month; vertical research tools at ~$100-500/seat/month; several agent products priced in credits. No direct competitor sells exactly this to exactly this segment.
- **A7.** A managing partner or ops lead signs. Below ~$2,000/month it is a single-signature decision. Monthly card billing at launch; annual prepay by invoice possible for larger firms.
- **A8.** In 30 days the team can ship Stripe subscriptions, a per-account usage counter, and a hard cap. It cannot ship a full metered rating engine, real-time usage dashboards, overage dispute tooling, and per-user attribution.
- **A9.** Cost per task expected to fall 3-5x over 12 months from model price declines, routing and caching — an expectation, not a plan with owners.
- **A10.** Signal on hand: 40-person waitlist, 6 design partners using it free, none has named a price.

Read the closing section before you take anything below at face value. A8 and A3 turn out to carry the verdict, and both are mine.

## Step 1: the panel

Five lenses, one line each — the lens, and the failure mode it exists to catch.

| Lens | Failure mode it exists to catch |
| --- | --- |
| Unit economics and gross margin | A price that inverts margin on heavy users and hides it inside a blended average until scale makes it fatal |
| Buyer and procurement mechanics | A price that is analytically correct and unsignable — the deal that stalls at the partner's desk, or the bill cancelled after one surprise month |
| End-user adoption behavior | A price that suppresses the very usage the product needs to prove its value; the moment a user stops reaching for the tool because the meter is running |
| Billing systems and operational load | A model this four-person team cannot meter, invoice, explain, cap, refund or defend in a dispute; the support hours per invoice nobody budgets |
| Competitive positioning and price legibility | A price anchored in the wrong comparison set — next to a $30 chat assistant when it should sit next to a contract researcher — and an anchor expensive to move later |

A persona is a lens, not a credential. Nothing below is an expert speaking. It is five instances of one model, each pointed at a different set of risks first, which changes what gets looked at — not what is known.

## Dispatch

The Step 2 consultant instruction was forwarded verbatim to all five, in parallel, each seeing only its own lens and the framing. Two orchestrator additions, disclosed here because they are not in the template: a 400-word cap, and a fixed output shape (POSITION / REASONS / CONFIDENCE / WHAT WOULD CHANGE MY MIND / UNASKED QUESTION) with POSITION restricted to one of PER-SEAT, USAGE-BASED, HYBRID so the tally could be mechanical. No consultant saw the panel roster, the other lenses, or that other consultants existed.

## Round one — five positions

### Unit economics — HYBRID

> POSITION: HYBRID — a firm-level base fee with an included usage allowance and a hard cap, not per-seat and not pure metered.
>
> REASONS:
> 1. Per-seat inverts margin by construction in this segment. A3 says 1-2 people run most research and there is no enforcement against sharing. So the rational buyer licenses exactly the heavy users and shares output — revenue scales with seats bought, COGS scales with the heavy seats, which are the only seats they buy. Worked: a firm buys 2 seats at $150 = $300 MRR; COGS is 2 heavy users at ~$120 = $240. That is 20% gross margin, and the $300 worst account (A4) puts it negative. The light users who would carry the blend are exactly the ones never licensed. A3 is the load-bearing assumption; per-seat only survives if it is false.
> 2. The blend hides it until the failure point. Early accounts resemble the six free design partners — light. You clear 60% at eight firms and breach 55% somewhere near twenty, which is precisely A1's tripwire, discovered from the model provider's invoice a month late with no per-account attribution. Hybrid is the only option whose billing mechanism forces the per-account counter A8 says they can ship.
> 3. In 30 days the cap, not the meter, is the margin instrument. A cap converts an unbounded tail into a bounded, known loss plus an upgrade conversation; overage billing needs the dispute tooling A8 rules out. Price the base to cover a heavy firm's ~$276/month COGS at 60%+ → roughly $700-900/month per firm. That sits under A7's $2,000 single-signature line and reads as one contract-researcher day (A5).
>
> CONFIDENCE: High that per-seat is wrong; medium on the specific allowance size.
>
> WHAT WOULD CHANGE MY MIND: The provider usage export by API key for the six design partners over 60 days — specifically p90-to-median monthly COGS per account. If that ratio is under 2, per-seat is survivable and I drop the base fee. Also: one signed contract at ≥$150/seat containing a seat-minimum tied to professional headcount that survives negotiation. A9 (3-5x cost decline) is load-bearing and unowned; it may not be priced in until named engineers hold dated routing and caching tickets.
>
> UNASKED QUESTION: Do we know COGS per research task, or only per account per month? Every allowance number in the hybrid is a guess until we can cost one task — and are we actually willing to let a customer hit the cap, or is the cap a bluff?

### Buyer and procurement — HYBRID

> POSITION: HYBRID — a per-seat list price with a 3-seat minimum, a firm-level included allowance denominated in research runs, and a hard cap; zero auto-billed overage in the first 12 months.
>
> REASONS:
> 1. Usage-based is unsignable because of A8, not because of buyer taste. With no per-user attribution and no dispute tooling, the first anomalous invoice has no artifact the buyer can defend internally. A managing partner with single-signature authority (A7) signs against a known number; he cancels rather than litigate a $900 line he cannot attribute to a person or a matter. Worse, every meter bills the buyer for the model's own bad runs and re-runs — the associate re-ran it three times because the first two were wrong, and the invoice reads as three deliverables. That is the cancelled-after-one-surprise-month mechanic.
> 2. Pure per-seat breaches A4's floor because of A3. One or two people do the work, so the firm buys three seats and one shared credential burns $120-300. At any per-seat number a partner signs — benchmarked against the $50-200/seat subscriptions they already hold (A5) — a heavy account eats the margin. An allowance plus a cap is the only margin control shippable in 30 days.
> 3. Denominate the allowance in research runs or briefs, not tokens or credits. These firms already approve contract researcher days at $400-800 (A5); "20 briefs/month" is comparable to a purchase they price routinely. Credits (A6) are what procurement discounts to zero because they cannot forecast them.
>
> Flag: A2 is load-bearing and points the wrong way. "Price may change on 30 days' notice" reads open-ended at the partner's desk. Put a stated price-lock date on the order form face, not the 6-month honor clause in the terms.
>
> CONFIDENCE: High on structure. Medium on price level — A10 supplies no willingness-to-pay signal. A9 (cost decline) is outside my lens.
>
> WHAT WOULD CHANGE MY MIND:
> - Ninety days of per-account COGS from the six design partners: if p90-account / median-account is under 3x, drop the allowance and ship flat per-seat. The 10x spread is asserted from internal use, never measured on buyers.
> - One order form returned unsigned where the written objection is seat count or price level, not bill predictability.
> - A design partner asking, unprompted, to be billed by volume.
>
> UNASKED QUESTION: Is this firm overhead or a rebillable client-engagement cost? If a boutique books it to the matter the way it books contract research, the budget owner changes, the ceiling rises several times, and per-project pricing beats both seats and usage. Nobody has asked the six partners which cost code they would use.

### End-user adoption — HYBRID

> POSITION: HYBRID — a firm-level flat price (~$1,400/month, 5 named seats, pooled monthly research allowance, hard cap at ~3x expected) where the meter is visible only to the buyer and never to the person doing the work.
>
> REASONS:
> 1. A visible meter kills the behavior that makes this product work. Research value comes from discarding four of five queries. The moment a consultant can see a counter, they pre-judge each query — "is this worth $3?" — and the answer for exploratory queries is always no. That is precisely the usage that produces the wow moment and the renewal story. Usage-based pricing at launch, with zero first-party data (A10: six design partners, none named a price), also contaminates the only clean usage data you will ever collect. Meter later, once you know what unmetered use looks like.
> 2. Pure per-seat fails a different adoption test. Per A3, 1-2 people already run everything and there is no sharing enforcement. Per-seat makes the buyer ration logins, so the tool stays one person's tool, never reaches the 3-8 users whose habit drives renewal, and dies when that person is on a client engagement. Pricing the firm, not the head, removes the buyer's incentive to gatekeep access.
> 3. A8 makes any user-facing usage component actively hostile. No dashboards, no dispute tooling, no per-user attribution means an overage line is a number the user cannot verify, cannot contest, and cannot attribute. That is the worst possible first billing experience for a four-person company with one salesperson. A hard cap the founder handles by phone is manageable; an unexplainable invoice is a churn event.
>
> Load-bearing assumptions my lens flags: A3 (concentrated usage) drives the firm-level unit; A8 (no metering UX) forbids user-visible overage; A9 is a hope with no owner — do not price against it.
>
> CONFIDENCE: medium-high.
>
> WHAT WOULD CHANGE MY MIND: The per-account token spend and task counts for the six design partners over the last 60 days, broken out by account and by user, plus abandoned/discarded session rate. If P90 firm COGS is under ~$150/month, drop the cap and sell flat per-firm. If one partner alone exceeds $400/month and the distribution is bimodal rather than long-tailed, the flat price is mispriced and I would move to a two-tier flat.
>
> UNASKED QUESTION: When the pooled allowance runs low, who inside the firm gets told "no" first — and is it the junior who would have become your daily habit user?

### Billing operations — PER-SEAT

> POSITION: PER-SEAT — a named-seat subscription with a 3-seat minimum, an account-level hard cap on usage, and upgrade-not-overage when the cap is hit.
>
> REASONS:
> 1. A8 decides this and is the most load-bearing assumption on the list. Metered billing is not a counter; it is a rating engine, an immutable usage ledger, a pre-bill review step, a customer-visible dashboard, credit notes, and mid-cycle proration. They can ship one of six. Without the other five, every variable invoice is adjudicated by the founder who sells, the week after billing. Expect 20-30% of accounts to query a variable invoice in the first two cycles at 30-60 minutes each — roughly a founder-day per month at 20 firms, taken straight out of selling. Card-billed disputes become Stripe chargebacks; a sustained rate above ~1% puts the payment account itself at risk.
> 2. The cap only works pre-spend. Under per-seat, hitting the cap is a sales conversation ("upgrade"). Under usage billing you have sold the customer the right to spend, so the cap is a liability you cannot enforce, and the failure mode is A4's $300 invoice refunded by hand with no per-user attribution to justify the credit.
> 3. Price against the heavy user, not the blend. A3 concentrates cost in 1-2 people while 3-8 hold logins; the idle seats are the margin. ~$150/seat with a 3-seat floor ($450/firm) sits inside A6's $100-500 vertical band and far under A5's $400-800/day researcher. An account-level cap near $150-180 COGS keeps even the worst account positive and the blend well above 60%. Credential sharing (A3) is not a leak if the cap is per account, not per seat. Ship the counter, bill nothing on it, and use A2's 30-day notice to reprice at day 90 with real data.
>
> CONFIDENCE: High.
>
> WHAT WOULD CHANGE MY MIND: 60 days of per-account monthly COGS from the six design partners (A10), reported as p50/p90/max in dollars. If p90 account COGS exceeds ~40% of expected per-firm revenue, or 3+ of 6 breach the cap in one month, per-seat cannot hold and the answer becomes a firm-level plan with prepaid credits. Also: any signed order form naming a credit price.
>
> UNASKED QUESTION: What is the written refund rule when a research run returns garbage? Usage-based billing charges the customer for your failures, retries included, and A8 says there is no per-run attribution to credit back. Per-seat turns that from a billing dispute into a support ticket. Nobody has priced the retry.

### Competitive positioning — HYBRID

> POSITION: HYBRID — one firm-level subscription, unlimited named logins, three capacity tiers (~$900 / $1,800 / $3,500 per firm/month) sold as included research-runs behind a hard cap, with tier upgrade rather than metered overage.
>
> REASONS:
> 1. Per-seat drops you into the wrong comparison set and traps you there. A named login is the exact unit a $25-30 general assistant sells, so buyers price you as a multiple of that. Worse, with 3-8 real users of whom 1-2 do the work (A3 — load-bearing), seat count is small and shrinks as the product improves. Better product to fewer seats to less revenue is a positioning defect, not a packaging detail.
> 2. The unit is the anchor; the number is just the price. A2's 30-day notice clause lets you move numbers cheaply — it does not let you move units. Repricing $150/seat into firm-capacity later reopens every account, invites "we're paying more for the same thing," and hands a competitor the simpler line. Pick the unit you want in month 24 and adjust the number quarterly.
> 3. Capacity tiers put the invoice next to the $400-800/day contract researcher (A5 — load-bearing), which is the alternative actually in the buyer's head. 25 briefs at $900 reads as roughly $36 per analyst-day-equivalent; "$150/seat" reads as expensive chat. Entry and mid tiers clear under the $2,000 single-signature line (A7). Two heavy users at ~$240 COGS (A4) against $900 is ~73% margin, and the hard cap — not a rating engine — is what defends A1 within what 30 days can ship (A8).
>
> CONFIDENCE: High on rejecting per-seat. Medium on the tier numbers; with no first-party data (A10) I would not defend $900 over $1,200.
>
> WHAT WOULD CHANGE MY MIND: The six design partners' login logs — if median distinct weekly-active users per firm is 5 or more and usage is not concentrated in 1-2 people, A3 collapses and per-seat becomes legible and expandable. Or three of the next ten sales calls where the ops lead states they cannot open a budget line that is not headcount-denominated. Or a funded direct competitor launching per-seat into this segment first; they set the comparison set, and fighting a frame costs more than joining it.
>
> UNASKED QUESTION: Is this the firm's overhead or its cost of goods? Boutiques rebill research. If output attaches to a client matter, the unit is per-engagement and the ceiling is what the firm bills its client, not what it budgets per head — a much higher price. No one has asked a design partner whether they would pass this through. Whether billing can support per-engagement is outside my lens.

## Step 3: the anonymized aggregate (deterministic — no agent)

This is the exact text each consultant received in round two, with nothing else. Note the second and third tallies: the headline HYBRID 4-1 hides the split that was actually live, which was the *unit*, 3-2. Nobody asked for that tally. Adding it was mine to do, and it is the reason round two had something to argue about.

> ANONYMOUS AGGREGATE — ROUND ONE, FIVE CONSULTANTS
>
> POSITION DISTRIBUTION: HYBRID 4 | PER-SEAT 1 | USAGE-BASED 0
> SECONDARY TALLY — the unit the price is denominated in: firm-level subscription, seats not priced: 3 | per named seat with a seat minimum: 2
> SECONDARY TALLY — user-visible metered overage billing: proposed by 0 of 5. All five propose a hard cap plus an upgrade conversation instead of auto-billed overage.
>
> AGAINST PURE USAGE-BASED
> U1. A8 decides it: metered billing is not a counter but a rating engine, an immutable usage ledger, a pre-bill review step, a customer-visible dashboard, credit notes and mid-cycle proration. The team can ship one of six.
> U2. Every variable invoice is adjudicated by the founder who sells, the week after billing. Estimate 20-30% of accounts query a variable invoice in the first two cycles at 30-60 minutes each — roughly a founder-day per month at 20 firms, taken out of selling.
> U3. Card-billed disputes become Stripe chargebacks; a sustained rate above ~1% puts the payment account itself at risk.
> U4. A meter bills the buyer for the model's own bad runs and re-runs. The associate re-ran it three times because the first two were wrong; the invoice reads as three deliverables. There is no per-run attribution (A8) to credit it back.
> U5. A visible meter changes what users do. Research value comes from discarding four of five queries; a counter makes people pre-judge each exploratory query, and those are the queries that produce the renewal story.
> U6. Launching metered contaminates the only clean usage data the company will ever collect.
> U7. Credits are what procurement discounts to zero, because they cannot be forecast.
> U8. Under usage billing you have sold the customer the right to spend, so the cap becomes a liability you cannot enforce. Caps only work pre-spend.
>
> AGAINST PURE PER-SEAT
> P1. A3 means the rational buyer licenses only the heavy users and shares the output. Revenue scales with seats bought; COGS scales with the heavy seats, which are the only seats bought. Worked: 2 seats at $150 = $300 MRR against 2 heavy users at ~$120 = $240 COGS, i.e. 20% gross margin; the $300 worst account (A4) is negative.
> P2. The light users who would carry the blend are exactly the ones never licensed.
> P3. The blend hides the breach until roughly twenty firms — precisely A1's tripwire — and it is discovered from the model provider's invoice a month late, with no per-account attribution.
> P4. Per-seat makes the buyer ration logins, so the tool stays one person's tool, never reaches the 3-8 users whose habit drives renewal, and dies when that person is on a client engagement.
> P5. A named login is the exact unit a $25-30 general assistant sells; per-seat drops the product into that comparison set and buyers price it as a multiple of that.
> P6. Seat count is small and shrinks as the product improves. Better product to fewer seats to less revenue is a positioning defect.
> P7. The unit is the anchor; the number is just the price. A2's 30-day notice moves numbers cheaply, not units. Repricing seats into firm capacity later reopens every account and invites "we're paying more for the same thing."
> P8. At any per-seat number benchmarked against the $50-200/seat subscriptions these firms already hold (A5), a heavy account eats the margin.
>
> IN FAVOUR OF PER-SEAT
> S1. Idle seats are the margin: 3-8 hold logins while 1-2 generate the cost. Price against the heavy user and the idle seats carry the blend.
> S2. Credential sharing is not a leak if the cap is per account rather than per seat.
> S3. Under per-seat, hitting the cap is a sales conversation ("upgrade"), not a hand-issued refund with no attribution to justify it.
> S4. ~$150/seat with a 3-seat floor ($450/firm) sits inside A6's $100-500 vertical band and far under A5's $400-800/day researcher.
> S5. Ship the counter, bill nothing on it, and use A2's 30-day notice to reprice at day 90 with real data.
>
> ON STRUCTURE AND DENOMINATION
> D1. Denominate any allowance in research runs or briefs, not tokens or credits.
> D2. Price the base to cover a heavy firm's ~$276/month COGS at 60%+, roughly $700-900/month per firm — under A7's $2,000 single-signature line, and it reads as one contract-researcher day.
> D3. Firm-level capacity tiers at roughly $900 / $1,800 / $3,500 put the invoice next to the $400-800/day contract researcher. Two heavy users at ~$240 COGS against $900 is ~73% margin.
> D4. Pricing the firm rather than the head removes the buyer's incentive to gatekeep access.
> D5. Keep the meter visible to the buyer and invisible to the person doing the work.
> D6. A2 as written points the wrong way: "price may change on 30 days' notice" reads open-ended at the partner's desk. Put a stated price-lock date on the face of the order form.
> D7. A9 is an expectation with no owner. Do not price against a 3-5x cost decline until named engineers hold dated routing and caching tickets.
> D8. A hard cap plus upgrade is the only margin instrument shippable in 30 days.
>
> POOLED "WHAT WOULD CHANGE MY MIND"
> W1. The model provider's usage export by API key for the six design partners over 60-90 days: per-account monthly COGS as p50 / p90 / max in dollars. Thresholds named by the panel: p90/median under 2 → per-seat survivable, drop the base fee; p90/median under 3x → drop the allowance, ship flat per-seat; p90 account COGS above ~40% of expected per-firm revenue, or 3+ of 6 breaching the cap in one month → per-seat cannot hold; p90 firm COGS under ~$150/month → drop the cap, sell flat per firm; one partner alone above $400/month with a bimodal distribution → two-tier flat.
> W2. The same export broken out per user, not just per account, plus the abandoned/discarded session rate.
> W3. The six design partners' login logs: median distinct weekly-active users per firm. If 5 or more and usage is not concentrated in 1-2 people, A3 collapses.
> W4. One signed contract at >= $150/seat carrying a seat minimum tied to professional headcount that survives negotiation.
> W5. One order form returned unsigned where the written objection is seat count or price level rather than bill predictability.
> W6. A design partner asking, unprompted, to be billed by volume; or any signed order form naming a credit price.
> W7. Three of the next ten sales calls where the ops lead states they cannot open a budget line that is not headcount-denominated.
> W8. A funded direct competitor launching per-seat into this segment first, setting the comparison set.
> W9. Named engineers holding dated tickets for routing and caching work.
>
> POOLED UNASKED QUESTIONS
> Q1. Do we know COGS per research task, or only per account per month?
> Q2. Are we actually willing to let a customer hit the cap, or is the cap a bluff?
> Q3. When the pooled allowance runs low, who inside the firm gets told "no" first — and is it the junior who would have become the daily habit user?
> Q4. Is this firm overhead or a rebillable client-engagement cost? If a boutique books it to the matter, the budget owner changes, the ceiling rises several times, and per-engagement pricing beats both seats and usage. (Raised independently by two of the five seats.)
> Q5. What is the written refund rule when a research run returns garbage? Nobody has priced the retry.

## Round two — revise or hold

Each consultant was resumed with its own round-one output still in context, given the aggregate above and the verbatim Step 4 instruction, and nothing else. None saw another's round-two reply.

**A plumbing failure a reader will hit too:** all five round-two returns failed to route back to the orchestrator. Each was delivered by manual relay from the outer conversation, verbatim. One of them arrived twice under two different labels with identical substance and is counted once. The isolation held — no consultant saw another's output — but the return path did not, and the round-two token counts were lost with it. If you run this in your own harness, budget for the second round's delivery, not just its compute.

### Unit economics — HYBRID, held on structure, revised on shape

> REVISED POSITION: HYBRID — HELD on structure, REVISED on shape: firm-level capacity tiers near D3's $900/$1,800/$3,500, allowance in research runs, hard cap, no metered overage, seats unpriced and unlimited.
>
> POINT BY POINT:
> S1 — Hold: idle seats are margin only if bought, and with no enforcement (A3) the buyer's cheapest path is fewer seats; you cannot underwrite margin on a line item the ops lead can see is unused at renewal.
> S2 — Concede, and it argues against its own side: if cap and counter sit on the account, the seat does no economic work and is just a multiplier the buyer minimizes.
> S3 — Agree, but it does not discriminate; all five propose cap-plus-upgrade, so it argues for the cap, not the seat.
> S4 — Hold, and this is the crux: $450/firm against a heavy firm's ~$276 COGS is 39% gross margin, under A4's 55% floor on day one, before heaviness grows.
> S5 — Revise toward it: ship the counter live and bill nothing on it. But at day 90 reprice the number, not the unit — P7 is right that A2 moves numbers cheaply and units expensively.
> D2 vs D3 — Revise to D3: a single $700-900 point price is a blend by construction against a 10x cost spread; three capacity tiers are price discrimination, the only clean instrument for that spread.
> Q4 — Outside my lens to judge, but load-bearing on it: if this books to the client matter, the ceiling moves several times and every margin number here goes slack.
>
> WHAT THE MAJORITY IS MISSING:
> W1's thresholds are computed on six free accounts that ran with no price friction and no cap — yet U5 argues a cap changes behavior. The free distribution is a biased estimate of the paid, capped one; pull the export, but do not let "p90/median under 2" settle the unit on the wrong sample.
>
> And nobody has costed the cap's revenue side. A cap defends gross margin by rationing the product: a capped firm stops building the habit that drives renewal (Q3), and at 20 firms that retention loss can exceed the COGS saved. Set the cap as a tail instrument, not a rationing one, and forecast how often it binds — an unbudgeted bind rate is how 60% margin arrives with 12 firms instead of 20.

### Buyer and procurement — HYBRID, revised the unit

> REVISED POSITION: HYBRID — REVISED. Hybrid holds; the denomination changes. Drop per named seat with a 3-seat floor; sell firm-level capacity tiers with unlimited named logins, an allowance in research runs, a hard per-account cap, no auto-billed overage.
>
> POINT BY POINT:
> - P7 flipped me. A2's notice reprices numbers cheaply and units expensively. My 3-seat floor was a firm price wearing a seat costume, and I would have paid to take the costume off at every account.
> - P1/P8: hold — this was my own round-one reason 2. The allowance was the wrong repair; it patched margin while leaving the anchor on the head.
> - P5: accept, and add the procurement half. A per-seat number is a negotiation surface. Every buyer tries to buy two. One founder selling against a day-120 count discounts the floor away by the tenth deal. A tier has no per-unit to haggle.
> - S1: reject. Idle seats carry the blend only if bought, and A3 says the rational buyer does not buy them.
> - S2/S3: accept; both survive the revision and work better at firm level.
> - S4: abandoned. S5: held — ship the counter, bill nothing on it, reprice at day 90.
> - U1-U8: no disagreement.
> - D3: partial hold. $900/$1,800 fine; $3,500 crosses A7's $2,000 single-signature line and turns every upgrade into a committee decision. Keep the published ladder under $2,000 and make the top tier "contact us."
> - D6/D7/Q2/Q4: held.
> - W: restating my round-one trigger under the new unit — p90/median under 3 now means sell one flat firm price, not flat per seat. W7 is the reading that flips me back: three of ten ops leads stating they cannot open a non-headcount budget line.
>
> WHAT THE MAJORITY IS MISSING: The panel chose a price unit and never named the artifact that gets signed. A8 ships Stripe subscriptions — that is a checkout page. At $150/seat somebody expenses it on a card. At $900-1,800 per firm it is past what a partner puts on a personal card, so it needs an invoice, a PO number, a W-9, sometimes a COI and a vendor form, and a named internal owner who renews it. None of that is in the 30-day build, and A7 calls invoiced billing merely "possible." Moving to firm-level pricing is right, and it moves the purchase out of self-serve. Scope the order form and invoicing this month or the correct price sits unsigned.

### End-user adoption — HYBRID, revised structure, held the unit

> REVISED POSITION: HYBRID — REVISED in structure, held in unit. Firm-level capacity subscription with unlimited named logins (was: 5 included seats), a ladder at roughly $900 / $1,800 / $3,500 (was: flat $1,400), allowance denominated in research briefs, hard cap, nothing billed on the counter at launch, meter invisible to the person doing the work.
>
> POINT BY POINT:
> - D4 + my own P4: revise. Including exactly 5 seats reintroduced the gatekeeping I was trying to remove; if the buyer counts logins at all, the fifth user never gets added. Unlimited logins, capped capacity.
> - D2/D3 price level: revise down. $1,400 as an entry point makes month one an ROI trial rather than a habit; a $900 door lowers the threshold to put a second and third person on it, and the ladder still reaches my number.
> - D1: adopt. A user can act on "9 briefs left this month"; nobody can act on tokens or credits.
> - S5: adopt. Ship the counter, bill nothing on it, reprice at day 90 with real data.
> - D6: adopt, and my lens sharpens it — an open-ended "price may change" clause makes the buyer stage the rollout ("let's not put everyone on it yet"), a direct adoption tax. A dated price lock is an adoption instrument, not a legal nicety.
> - U4 + Q5: adopt as reinforcement. The retry problem is why the meter cannot face the user — the associate re-ran it because it was wrong, and a visible counter charges them for the model's failure.
> - S1/S2 (idle seats carry the blend): hold against. Idle seats are not margin, they are failed adoption you have decided to bill for, and P1 says the rational buyer never buys them.
> - S3: hold, but it is not a per-seat property — cap-plus-upgrade works identically under a firm plan.
> - W3: concede as a genuine changer. If the design partners show 5+ distinct weekly-active users and usage is not concentrated, A3 collapses and per-seat revenue tracks real use.
> - Q4: concede the strongest counter on the board. If these firms book it to the matter rather than to overhead, the meter becomes a feature — you need line items to rebill — and my objection to usage-based largely dissolves. Answer this before the order form is written.
>
> WHAT THE MAJORITY IS MISSING: The panel treats the cap as a margin instrument (D8) and argues over whether it is a bluff (Q2), but nobody has specified what the end user sees at 80% and at 100%. That screen is the only pricing artifact most users will ever encounter, it ships inside the 30 days, and it is being handled as a billing detail rather than as the product's most consequential piece of copy. Related: both of A1's tripwires are lagging indicators discovered from someone else's invoice. The leading indicator is briefs per active user in weeks 3 and 4, after novelty. The realistic failure at 20 firms is not a margin breach — it is quiet disuse, where the last thing the tool ever said to the user was a warning banner.

### Billing operations — revised from PER-SEAT to HYBRID

This is the only consultant that changed its headline position, and it changed it by re-doing its own arithmetic in public.

> REVISED POSITION: HYBRID — REVISED from PER-SEAT. Firm-level capacity tiers (~$900/$1,800/$3,500), allowance denominated in research runs, hard account cap; hybrid in tier structure, never in invoice variance — the amount is identical every month.
>
> POINT BY POINT:
> - P1: Revise. My own arithmetic fails: a 3-seat floor at $150 is $450 against ~$240 COGS = 47%, under A4's 55% floor before a single $300 account arrives.
> - P2, P4, P5, P7: Revise, conceded. The unit is the anchor; A2 moves numbers, not units, and re-denominating later reopens every account.
> - Point I under-weighted, and it cuts against me: seats are themselves a metered quantity. Every add or remove is proration, a mid-cycle credit note, and a seat reconciliation — with no per-user attribution (A8) to defend one. Firm-level flat is the only structure with zero recurring invoice-change events.
> - S2, S3, S5: Hold. Cap at the account, not the seat, so sharing is irrelevant; upgrade never overage; run the counter silently for 90 days and bill nothing on it.
> - U1-U8: Hold unchanged. D6: accept — a dated price-lock on the order form.
> - D2 vs D3: hold with D3; $700-900 sits too close to a heavy firm's COGS to survive one bad account.
> - W1: revised to firm denomination — p90 firm COGS above 40% of tier price kills the tier, not the model.
> - Q2: Hold hard. The cap is a bluff unless a same-cycle one-click upgrade ships with it; an unenforced cap is an unpriced discount granted by whoever answers the email.
>
> WHAT THE MAJORITY IS MISSING: Q4 was raised; its billing consequence was not. If this is rebillable to a client matter, the buyer needs matter-level breakout as invoice line items — per-engagement attribution, a larger build than the meter the panel just refused — plus deferred revenue and allowance-expiry accounting on A7's annual prepay that four people without an accountant will get wrong in front of month-12 diligence. And nobody owns dunning: at 20 firms one or two cards fail every month, there is no retry ladder, and the first involuntary churn will be a paying customer with an expired card.

### Competitive positioning — HYBRID, held

The only seat that did not move.

> REVISED POSITION: HYBRID — HELD. Firm-level tiers, seats unpriced, allowance in briefs, hard cap plus upgrade. Two amendments adopted: D6's stated price-lock date on the order form, and S5's ship-the-counter-bill-nothing-on-it.
>
> POINT BY POINT:
> S1 (idle seats are the margin) — hold, and it is self-refuting: an idle login has no buyer, so P1/P4 say those seats are never purchased, and you cannot blend margin across revenue you did not collect.
> S2 (sharing is fine if the cap is per account) — concede the fact, reverse the conclusion: if the cap is per account, the account is already the priced unit and the seat line is decoration.
> S3 (cap-hit is an upgrade conversation, not a refund) — concede fully; it is equally true under firm tiers, so it does not discriminate between the models.
> S4 ($150/seat, 3-seat floor, $450/firm) — hold: that is a firm-level price wearing a seat costume, and the costume costs you twice — it books you into the per-seat comparison set (P5) while collecting $450 against an anchor (A5) that supports roughly double.
> S5 (reprice at day 90 with real data) — adopt for the number, reject for the unit; A2 moves numbers cheaply and units expensively, so day-90 flexibility is a reason to launch on the unit you intend to keep, not a reason to defer choosing it.
> W1 thresholds — hold with a flag: every threshold listed is a COGS ratio, and a COGS export can tell you what to charge but never what to charge for. Only W3 (breadth of weekly-active use) and Q4 (who owns the budget line) can overturn the unit.
> D7 (A9 has no owner) — adopt, unchanged.
> Tier numbers — soften to D2's floor: entry $800 rather than $900, price-locked in writing for six months.
>
> WHAT THE MAJORITY IS MISSING: Four of five converged on firm-level economics but three still print a seat on the order form. The seat is not a pricing mechanism there, it is a comfort object for the seller. It costs the thing that is most expensive to undo: it files you next to the $30 assistant instead of the $400-800/day researcher, and it makes Q4 unaskable, because a per-seat line cannot be booked to a client matter.

### The movement, tallied

| Lens | Round one | Round two | Moved? |
| --- | --- | --- | --- |
| Unit economics | HYBRID: single firm base ~$700-900 + allowance + cap | HYBRID: three firm tiers $900/$1,800/$3,500 | Held position, revised shape |
| Buyer / procurement | HYBRID: per-seat list + 3-seat minimum + allowance | HYBRID: firm tiers, seats unpriced and unlimited | **Revised the unit** |
| Adoption behavior | HYBRID: flat $1,400/firm, 5 seats included | HYBRID: unlimited logins, ladder $900/$1,800/$3,500 | Revised structure, held unit |
| Billing operations | **PER-SEAT**: $150 × 3-seat floor + account cap | **HYBRID**: firm tiers | **Revised position** |
| Competitive positioning | HYBRID: firm tiers $900/$1,800/$3,500 | Same, entry softened to $800, two amendments adopted | **Held** |

Round-two distribution: HYBRID 5, PER-SEAT 0, USAGE-BASED 0. Unit: firm-level 5, seat-denominated 0.

Two rounds only. No third was run.

## Step 5: convergence report

### 1. Consensus zone

Positions all five now share, each with its strongest surviving reason.

- **Do not launch pure usage-based.** Metered billing is a rating engine plus an immutable ledger plus a customer-visible dashboard plus credit notes plus proration; the team can ship one of six pieces in thirty days, and every variable invoice then lands on the one founder who sells.
- **Do not launch pure per-seat.** Three seats at $150 is $450/firm against roughly $240 COGS for a firm's two heavy users — 47% gross margin on day one, under the 55% floor, before the worst observed account arrives. This is the argument that flipped the one holdout, computed by the holdout.
- **Price the firm, not the head.** The number is repriceable on thirty days' notice; the unit is not. Re-denominating later reopens every account.
- **Ship the usage counter and bill nothing on it for ninety days.** It produces the per-account data every consultant asked for without exposing an invoice nobody can defend.
- **Cap, then upgrade — never auto-billed overage.**
- **Denominate the allowance in research briefs or runs, not tokens or credits.** A user can act on "9 briefs left"; procurement discounts credits to zero because it cannot forecast them.
- **Put a dated price lock on the face of the order form**, not a six-month honor clause in the terms.
- **Do not price against the expected 3-5x cost decline** until named engineers hold dated routing and caching tickets.

### 2. Disagreement zone

**(a) The top of the published ladder.** Three seats print $3,500. Procurement holds alone: $3,500 crosses the $2,000 single-signature line and turns every upgrade into a committee decision — publish nothing above $2,000 and make the top rung "contact us." Strongest case for publishing it: a 10x cost spread requires price discrimination, and an unpublished top tier is a tier that never sells itself. Strongest case against: the single-signature threshold is the difference between a deal that closes on a call and one that waits for a partner meeting. This is a **procurement-vs-finance/positioning** split — a real cross-lens disagreement, not a matter of taste.

**(b) The entry rung.** $800 (positioning) vs $900 (three seats) vs the now-rejected $700-900 single point price. Positioning wants the door low enough to make the first yes easy and locked in writing for six months; unit economics and billing operations both reject any single point price near $700-900 as too close to a heavy firm's COGS to survive one bad account. A **positioning-vs-finance** split, and narrow — $100 on one rung.

**(c) What the cap is actually for.** Three-way, unresolved. Billing operations holds hard that the cap is a bluff unless a same-cycle one-click upgrade ships beside it, because an unenforced cap is an unpriced discount granted by whoever answers the email. Unit economics holds that the cap must be a tail instrument and not a rationing one, because a capped firm stops building the habit that drives renewal and at twenty firms the retention loss can exceed the COGS saved. Adoption holds that nobody has written what the user sees at 80% and 100%, which is the only pricing artifact most users will ever encounter. **Operations vs finance vs adoption** — the sharpest surviving disagreement on the panel, and the one nobody resolved.

**(d) Whether the panel's own deciding fact decides anything.** Four seats named the design-partner COGS export as the thing that would move them. Positioning attacks it: every threshold on that list is a COGS ratio, and a COGS export tells you what to charge, never what to charge for — only breadth-of-use (W3) and budget ownership (Q4) can overturn the unit. Unit economics attacks it from the other side: the export comes from six free, uncapped accounts and is a biased estimate of the paid, capped distribution. Two lenses, independently, attacking the escape hatch the panel built for itself.

### 3. Deciding facts, mapped to the disagreement each would settle

Ready-made input for a Diamond Research round ([template 02](../prompts/02-diamond-research.md)).

| Fact to go get | Settles |
| --- | --- |
| Design-partner per-account monthly COGS, p50/p90/max, 60-90 days, from the model provider's usage export | (a) and (b) — whether any ladder rung sits where anyone put it. On record: two seats say this sample is free and uncapped, therefore biased |
| The same export broken out per user, plus abandoned/discarded session rate | (c) — what a cap would actually have bound on, and how often |
| Design-partner login logs: median distinct weekly-active users per firm. 5+ and not concentrated in 1-2 people | **The unit itself.** This is the only fact that reopens the consensus zone — it collapses A3, and per-seat becomes legible again |
| Ask the six design partners which cost code they would use: firm overhead, or rebillable to a client matter (Q4) | Everything. Budget owner, ceiling, and whether a meter becomes a feature rather than a liability. Raised independently by two seats in round one; conceded as "the strongest counter on the board" by a third in round two |
| One order form returned unsigned, with the written objection being seat count or price level rather than bill predictability | Whether the panel read the buyer right at all |
| Three of the next ten sales calls where the ops lead says they cannot open a non-headcount budget line | Flips the unit back to seats |
| Named engineers holding dated routing and caching tickets | Whether the 3-5x cost decline can be priced against |

### 4. Dissent register (by lens, verbatim, never by name)

- **Competitive positioning** — the only seat that held its position: *"Four of five converged on firm-level economics but three still print a seat on the order form. The seat is not a pricing mechanism there, it is a comfort object for the seller. It costs the thing that is most expensive to undo: it files you next to the $30 assistant instead of the $400-800/day researcher, and it makes Q4 unaskable, because a per-seat line cannot be booked to a client matter."*
- **Buyer and procurement**, dissenting alone on the ladder: *"$3,500 crosses A7's $2,000 single-signature line and turns every upgrade into a committee decision. Keep the published ladder under $2,000 and make the top tier 'contact us.'"*
- **Billing operations**, holding hard against the rest on enforcement: *"The cap is a bluff unless a same-cycle one-click upgrade ships with it; an unenforced cap is an unpriced discount granted by whoever answers the email."*
- **Unit economics**, dissenting on the panel's own evidence: *"W1's thresholds are computed on six free accounts that ran with no price friction and no cap — yet U5 argues a cap changes behavior. The free distribution is a biased estimate of the paid, capped one."*
- **Adoption**, dissenting on what failure will look like: *"The realistic failure at 20 firms is not a margin breach — it is quiet disuse, where the last thing the tool ever said to the user was a warning banner."*

### 5. What this round did not do

- **Lenses missing.** No legal or contract lens — these firms handle client-confidential material, and nobody touched what the pricing unit implies for a DPA, a per-matter data boundary, or the confidentiality representations in an order form. No investor lens on what pricing story the month-12 raise actually needs. No pricing-research lens: nobody proposed running a price ladder or a van Westendorp on the forty-person waitlist inside the thirty days, which is the one first-party pricing signal available for free.
- **Model families.** All six agents ran on one vendor's models. The template requires the panel to span at least two families; this run did not meet that, and the convergence below should be discounted accordingly.
- **Unasked questions nobody answered.** Q1 — COGS per research task rather than per account per month — was never answered, which means every allowance number in every proposal is still a guess. Q3 (who gets told "no" first) and Q5 (the written refund rule when a run returns garbage) were raised and dropped. Q2 got three partial answers that disagree with each other, and that disagreement is preserved above rather than resolved.

### 6. Where the consensus goes next

Route the consensus zone through an [Adversarial Review](../prompts/03-adversarial-review.md) before anyone acts on it. Specific target for the adversary: the consensus rests on A8 and A3, and both are orchestrator inventions, not facts. Route the deciding-facts table through [Diamond Research](../prompts/02-diamond-research.md) — with the two dissents on record that the table is the wrong table.

## What to notice

- **The canary was the cheapest step and it created the run's worst weakness.** One agent, 27k tokens, found ten missing facts in a brief that read as complete — including the one it flagged as decisive, "hybrid dies on this constraint, not on strategy." But there was no user to answer, so I invented the answers. Two of those inventions, A8 (what billing can ship in thirty days) and A3 (usage is concentrated in 1-2 people), carry the entire verdict. Consultants flagged both as load-bearing, which is the system working. Still: a panel that unanimously rejects usage-based because of a billing constraint the orchestrator made up has not told you about pricing. It has told you what follows from A8. If you run this on a real decision, get those facts from a human.
- **The tally that mattered was not the one the template asks for.** Round one was HYBRID 4, PER-SEAT 1 — which reads as near-consensus and a lone dissenter. The live disagreement was the *unit*: firm-level 3, seat-denominated 2, and two of the four "HYBRID" votes were per-seat prices wearing a hybrid label. Adding that second tally is what gave round two something to argue about. The template says the aggregation step is plumbing; it is not. Choosing what to count is the orchestrator's one real judgment call in this method.
- **The flip was earned, and I still cannot fully score it.** The per-seat seat re-did its own arithmetic in public — $450 against $240 is 47%, under its own stated floor — and changed position. That sum was available to it in round one and it did not do it; it did it only after reading a counter-argument that put the sum in front of it. That is exactly what a Delphi round is for. It is also indistinguishable, from this transcript, from a lone dissenter yielding to a 4-1 majority. I cannot separate those two and am not going to pretend otherwise.
- **The direction converged genuinely; the numbers did not.** Four of five said hybrid and three of five said firm-level *before any aggregate existed* — isolated agents landing in the same place is the signal the method exists to produce. But $900/$1,800/$3,500 came from exactly one seat in round one, and in round two three other seats adopted those exact three figures. That is anchoring wearing the costume of corroboration. If you want price levels out of this method, strip specific numbers from the aggregate, or present them as an unattributed range. A tier ladder that four consultants "agree on" because three of them read it in a summary is one consultant's guess with three signatures.
- **Round two produced things round one could not.** Three items appeared only when a consultant was defending a position: the order-form gap (at $900/firm the purchase leaves self-serve and needs an invoice, a PO, a W-9 and a vendor form — none of which is in the thirty-day build); dunning and involuntary churn at twenty card-billed firms; and the observation that the 80%-and-100% cap screen is the only pricing artifact most users will ever see and is being handled as a billing detail. None of these is a pricing-model argument. All of them are things that would sink the launch. A single strong model asked "per-seat, usage, or hybrid?" returns the model. It does not return the vendor form.
- **Lens redundancy, checked honestly.** Unit economics and billing operations arrived at the same decisive argument — margin arithmetic — from different starting points; that is convergence, not redundancy, and billing operations separately produced chargebacks, seat proration, deferred revenue and dunning that no other lens touched. Buyer/procurement and competitive positioning rhymed in round one (both argue "how the price reads") and separated cleanly in round two, into signing mechanics and comparison-set anchoring respectively. Adoption was the least replaceable seat on the panel: the gatekeeping objection to included seats, the cap screen, and "the realistic failure is quiet disuse, not a margin breach" came from nowhere else. If you were cutting this panel to four, cut by asking which seat produced something no other seat could have — not by which lens sounds most senior.
- **Anonymization is thinner than it looks.** Stripping names from five mutually exclusive lenses does not strip identity. Reason U5 could only have come from the adoption seat; U3 could only have come from billing. The names were removed and the voices were not. In a five-seat panel with disjoint lenses, "anonymous" mostly buys face-saving on the revision, not true blindness — which is still worth having, but it is a smaller thing than the template implies.
- **The plumbing.** Every round-two return failed to route back and had to be relayed by hand, and one arrived twice under two labels. The isolation held; the return path did not. Two rounds of resumed isolated agents is a harness stress test, not just a token cost.

The panel spent 250k tokens to conclude "hybrid, priced at the firm." A single model would have said that in ten seconds. What it would not have said is that the answer hangs on two facts nobody has checked, that the price ladder everyone agreed on came from one guess, and that the thing most likely to kill the launch is a vendor form.
