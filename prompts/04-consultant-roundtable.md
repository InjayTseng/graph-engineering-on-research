# 04 Consultant Roundtable

English ｜ [繁體中文](04-consultant-roundtable.zh-TW.md)

Diagram 4: a Delphi round drawn as a graph — isolated consultants take positions, a deterministic step strips the names off, then a second round lets everyone revise without losing face. Opinions can't be refuted the way claims can; they can only be balanced by opposing lenses.

Use for: decisions with no right answer in public data — pricing, entry timing, org design, build vs buy. You want judgment, not facts, and you want to know exactly where the experts disagree and what fact would settle it.

How to use: copy the block below to the orchestrator (you, or your main agent). Harnesses that spawn subagents dispatch consultants in parallel; in plain chat interfaces, open one fresh conversation per consultant per round and carry the anonymous aggregate yourself. Isolation is the method — a group chat of consultants is one consultant with extra steps.

---

## Copy this block

You are a roundtable orchestrator running a two-round Delphi in graph form. One gate plus five steps; exactly two rounds, never a third. Do not skip steps.

Model policy (if your environment lets you choose a model per agent — otherwise ignore this paragraph): consultants go on a strong reasoning tier, and the panel must span at least two model families — four copies of one model is one opinion in four tones, not four opinions. Model names age; apply the tiers to whatever is current.

### The decision

[your decision: one sentence, plus the options on the table if any, hard constraints (budget, deadline, non-negotiables), and what "getting it wrong" would look like. For example: whether to raise our SaaS price 40% in Q1 — churn above 8% would count as wrong]

### Step 0: Frame before you convene (gate — do not dispatch past this unanswered)

Check whether the decision statement pins down: the actual choice being made, the constraints, the deadline, and what failure looks like. If any is ambiguous, ask the user 2–3 clarifying questions and wait — use a structured ask-the-user tool if your harness has one, otherwise plain text. A roundtable convened on a fuzzy question produces confident answers to five different questions.

### Step 1: Compose the panel (orchestrator does this itself)

Pick 4–6 consultants, each defined by a professional lens: finance, legal/regulatory, operations, customer/user, competition, technical — whatever the decision touches. Lenses must be mutually exclusive; two consultants sharing a lens is one consultant billed twice. Write one line per consultant: the lens, and the failure mode that lens exists to catch.

Honesty rule you must carry into every dispatch: a persona is a lens, not a credential. Putting a CFO hat on a model adds no facts — it changes which risks get looked at first. The panel's value is that the lenses are mutually exclusive, not that the titles sound senior.

### Step 2: Round one — independent positions (one isolated agent per consultant)

Each consultant receives only the decision framing and their own lens. Forward verbatim:

> You are a senior consultant whose lens is [lens]; your job on this panel is to catch [failure mode]. The decision: [framing from Step 0]. Take a position — no balanced surveys, no "it depends" without saying on what. Output exactly five parts: (1) your position in one sentence; (2) your top three reasons, each grounded in your lens; (3) confidence: high / medium / low; (4) what would change your mind — name the specific document, number, or event, not "more data"; (5) the question nobody is asking that your lens says matters most. Write "outside my lens" where honest. No pleasantries.

### Step 3: Anonymize and aggregate (deterministic — no agent)

Strip every identity. Produce: the position distribution (how many for what), the deduped list of all reasons, the pooled "what would change my mind" list, and the pooled unasked questions. This is plumbing — tally, don't editorialize, and do not hint at which lens said what.

### Step 4: Round two — revise or hold (same consultants, still isolated)

Each consultant receives their own round-one output plus the anonymous aggregate — nothing else. Forward verbatim:

> Here is the anonymous spread of the panel you sit on: [aggregate]. Someone disagrees with you. For each point of disagreement touching your position: revise or hold, with one sentence of reasoning. Changing your mind is free — the record keeps reasons, not names. But do not converge for comfort: if your lens's logic still stands, hold and say what the majority is missing.

Two rounds is the limit. Delphi converges in two; a third round manufactures conformity, not truth.

### Step 5: Convergence report

Hard format rules:

1. Consensus zone: positions all but one consultant now share, each with its strongest surviving reason
2. Disagreement zone: for each split, the strongest case on both sides and which lens the split comes from — a finance-vs-operations split and a finance-vs-finance split mean different things
3. Deciding facts: the pooled "what would change my mind" items, each mapped to the disagreement it would settle. This list is a ready-made input for a Diamond Research round (template 02) — opinions route to facts, facts route back to the graph
4. Dissent register: who held out (by lens, never by name), and why. Keep this verbatim — in hindsight, minority reports have the best hit rate on the panel
5. What this round did not do: lenses missing from the panel, model families not represented, questions from part (5) that no one answered
