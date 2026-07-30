# 03 Adversarial Review

English ｜ [繁體中文](03-adversarial-review.zh-TW.md)

Diagram 3: flip the graph around — the input is not a question but the conclusions you have already frozen, and the middle nodes don't discover, they kill.

Use for: a document that matters, that you revised many times yourself, that you believe is complete — a business plan, an architecture decision, an investment memo, a pricing evaluation. Field result: a plan hand-revised three times lost roughly one fifth of its conclusions in one overnight round, including its single biggest number (overstated ~5×).

How to use: do the prep below, then dispatch the attacker instruction one copy per domain (parallel subagents if your harness supports them; otherwise one fresh conversation per attacker — attackers must not see each other, that is the core). If you can pick models: attackers on strong reasoning models, ideally mixing model families — copies of one model share blind spots. Then run the integration rules yourself.

---

## Prep (before dispatching — you do this)

1. Extract the document's core conclusions into a list, one per line. Each line is an assertion that can be overturned ("we chose option A because it costs 30% less"), not a topic ("regarding option selection")
2. Group conclusions by professional domain, 3–8 per group, one group per attacker. Domains must be mutually exclusive (finance to finance, legal to legal, technical to technical)
3. De-identify sensitive documents first: names → codenames, exact amounts → ranges, institutions → types. Attackers verify against general questions; they do not need your private facts
4. Write 2–3 "mandatory attack questions" per group: the points you are least sure of, the points that depend on external conditions. Always end with the open question below — in field use, the most valuable findings all came from the open question
5. Pick models: attackers go on a strong reasoning tier, and mix at least two model families if you can — copies of one model share blind spots. If everything ends up on one family, record it under "what this round did not do" in the integration step
6. Canary the brief: send the de-identified facts plus one group's conclusion list to a single attacker with one instruction — *before attacking, list every fact you would need that this brief does not give you.* Patch the holes, then dispatch the rest. The alternative is N attackers reasoning around the same missing fact at the same time, which comes back looking like agreement and is not

## Attacker instruction (one per domain, forwarded verbatim, each in a fresh conversation)

> You are a senior expert in [domain], hired to run an adversarial review of a finished plan. Your job is not to recite common knowledge — it is to find where our conclusions are wrong or too optimistic.
>
> Discipline: primary sources first (statutes, official documents, raw data); label secondary sources. Verify against the current state of the world — rules get amended, precedents get reversed. Write "unverifiable" when unsure. Never fabricate a citation.
>
> Background facts (de-identified):
> [the facts this domain needs]
>
> Our frozen conclusions — attack each one:
> [the conclusion list for this group]
>
> Mandatory attack questions:
> [your 2–3 weakest points]
> Final question: what is the risk or opportunity we never thought of that would materially change these conclusions? (Assume this plan failed a year from now — what was the most likely cause?)
>
> Output format:
> One — per-conclusion verdict: HOLDS / PARTIALLY HOLDS / OVERTURNED / UNVERIFIABLE, each with reasoning, citations, and confidence (high / medium / low)
> Two — answers to the attack questions
> Three — what we missed
> Four — the facts that decide the outcome: which document to pull or which person to ask to settle each open point
> Five — primary and secondary sources, listed separately
> Dense. No pleasantries.

## Integration rules (when collating — you do this yourself; this layer cannot be outsourced)

1. False-positive filter: attackers can't see the whole document, so their "what we missed" lists will contain things the document already covers. Check each item against the original text and split into "truly missed" vs "the attacker couldn't know." Never copy the list wholesale
2. Arbitrate disagreements: when two attackers reach opposite verdicts on the same point, the tiebreaker is who cites the more specific primary source — and write down your ruling. Attackers are independent; disagreements will not surface themselves. You must diff the overlap zones
3. Recompute the arithmetic by hand: totals, both-sides sums, threshold math. In field use, the single worst error (a number computed on one side of a two-sided ledger) survived every attacker and three rounds of human revision — one manual recomputation caught it
4. Same-round cross-citation is not independent verification: a new rule found by attacker A and cited by attacker B is still one source. List the single-source high-impact claims and check each against the original before citing externally
5. Record honestly what this round did not do: no second-round cross-verification, no counter-examiner from a different model family (multiple copies of one model share blind spots — Knight & Leveson, 1986), no probability weighting of scenarios. That list is next round's starting point

## Outputs

1. Review report: a verdict table (which conclusion, what verdict, how big the impact) plus a "Rejected — do not cite" ledger
2. Decision dependency graph: gates (which unknowns block which decisions), clocks (irreversible deadlines), no-regret moves (actions valid under every scenario, doable now), and false dependencies (things that look sequential but can run in parallel)
3. If the output grows past three or four cross-referencing documents, add a three-minute human entry page: where things stand, what decisions are pending, what to do this week, which file to open for depth
