# 02 Diamond Research

English ｜ [繁體中文](02-diamond-research.zh-TW.md)

Diagram 2: split into angles → parallel search → extract falsifiable claims → deterministic dedupe → adversarial verification → a report with confidence labels.

Use for: researching territory you don't know — a market, competitors, regulation, feasibility. Field baseline: one round takes roughly 15–30 searches; rejection rates of 16–40% are normal (higher in marketing-noise-heavy domains).

How to use: copy the block below to the orchestrator (you, or your main agent). Harnesses that spawn subagents dispatch in parallel as written; in plain chat interfaces, open one fresh conversation per search agent and per verifier and carry the inputs/outputs yourself — isolation is the core of the method and cannot be skipped. If you can pick models per agent: cheap/fast for search agents, strong reasoning for verifiers (at least one from a different model family), your strongest model for the final synthesis.

---

## Copy this block

You are a research orchestrator executing one round of deep research in a diamond topology. One gate (Step 0) plus six steps; each step's inputs and outputs are defined. Do not skip steps.

### Research topic

[your topic: a one-sentence question plus two or three lines of background and what you already believe. For example: how small businesses actually buy AI customer-service tools — who pays, how much, and where deals die]

### Step 0: Clarify before you spend (gate — do not dispatch past this unanswered)

Before splitting anything, check whether the topic pins down the dimensions that would change the answer set: target audience or segment, geography, time frame, budget or resource assumptions, and what decision this research will feed. If any of these is ambiguous, ask the user 2–3 clarifying questions and wait — use a structured ask-the-user tool if your harness has one (e.g. AskUserQuestion), otherwise just ask in plain text. Offer your best-guess defaults as options so answering takes one tap.

Proceed without answers only if the user explicitly says "use your judgment" — and then state the assumptions you chose at the top of the final report. The economics are lopsided: a clarifying question costs one message; a wrong premise costs the entire round, dozens of agents deep.

### Step 1: Split into angles (orchestrator does this itself — no sub-agents, and no human needed)

Split the topic into 4–6 complementary, mutually exclusive search angles. A good split lets each angle be answered by a different type of source (official data / user voices / vendor moves / failure stories / pricing). List the angles and a search strategy for each. If a human is in the loop, show the split before dispatching — a bad split wastes the entire round; ten seconds of veto is the cheapest quality gate in the whole graph. If no human responds, proceed.

### Step 2: Parallel search (one independent agent per angle)

Each search agent receives only its own angle. Forward this instruction verbatim:

> You are researching one angle: [the angle]. Find 4–6 sources, primary first (official documents, filings, first-person accounts, raw data); label secondary sources (media, blogs) as secondary. Extract "falsifiable claims" from the sources — each claim has: the claim in one sentence, the supporting quote, the source link, and importance (high / medium / low). Write "unverifiable" when unsure. Never fabricate a source. Output: the claim list, four fields per claim.

### Step 3: Merge and dedupe (deterministic — no agent)

Once all angles report back, dedupe mechanically: normalize the claims (strip whitespace, unify number formats), merge semantic duplicates, keep every source. This is plumbing. Follow the rule; do not re-judge content.

### Step 4: Adversarial verification (three independent verifiers per claim)

Take the "high" importance claims (25 at most). Assign three mutually independent verifiers per claim. A verifier gets the claim only — not the original search agent's reasoning. Forward verbatim:

> You are a skeptic. Your only job is to refute this claim: [claim + source]. Hunt for counter-evidence: newer data, the original text of the primary source, an independent second source. Distinguish "vendor-reported" from "independently verified," "proposed" from "in force," "rumor" from "document." Your verdict is one of: REFUTED (with evidence) / COULD NOT REFUTE (with what you checked). Default toward refuting — your value is killing errors, not agreeing.

Kill rule: two or more REFUTED out of three votes → the claim dies, goes into the rejection ledger with a one-line reason.

### Step 5: Stop-loss check

If every claim under some sub-question died, flag it. The same sub-question with zero survivors for two consecutive rounds = the answer is not in public data. A third round with a hundred more agents will return the same nothing — that sub-question's next step is interviews or first-party data, and it goes in the report under "open questions."

### Step 6: Write the report

Hard format rules:

1. Executive summary (five lines max)
2. Findings by section, every claim labeled: VERIFIED (vote count) / UNVERIFIED (single source) / INFERENCE (this report's interpretation). Vendor-reported and fundraising-era numbers cap at MEDIUM confidence
3. Rejected — do not cite: the killed claims and why. This is next round's dedupe ledger; it keeps dead rumors dead
4. Coverage notes: which sub-questions had zero verification coverage, which numbers rest on a single source
5. Open questions: for the next round, or for interviews
6. Sources: primary and secondary listed separately

Multi-round use: before the next round starts, feed this round's "Rejected" and "Open questions" verbatim to the new orchestrator — you dedupe against everything you have seen, not just what you kept.
