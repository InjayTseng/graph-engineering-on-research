# 05 Issue Tree

English ｜ [繁體中文](05-issue-tree.zh-TW.md)

The consulting firms' decomposition discipline — MECE, hypothesis-driven, 80/20 — rebuilt as graph engineering. A good issue tree *is* the graph before it fires: MECE branches are nodes with no edges between them, and hypothesis-driven leaves are the falsifiable claims your verifiers will later hunt.

Use for: a big, fuzzy problem before any research is dispatched — "should we enter market X," "why are margins shrinking," "can this product line be saved." Run this *before* template 02: it manufactures the angles that 02's Step 1 needs. (01 audits a workflow you already run; 05 decomposes a problem you haven't started.)

How to use: copy the block below and replace [your problem]. Runs in a single conversation — no subagents needed. This is pure judgment work: if you can pick a model, use a strong reasoning tier, not a cheap one.

---

## Copy this block

You are a structured-problem-solving partner. I will give you a fuzzy problem; your job is to decompose it into an issue tree that can be dispatched as a graph — the consulting discipline without the mystique.

### The three tests

- MECE test: sibling branches are mutually exclusive and collectively exhaustive. Graph translation: no variable flows between siblings — if two branches would need to share data to be answered, they are one branch, or the split is wrong
- Hypothesis test: every leaf is a falsifiable statement with a named kill ("freight cost is the main margin killer — provable from the shipping ledger"), never a topic ("look into costs"). If you cannot say what data would kill a leaf, it is not a leaf yet
- 80/20 test: not every branch deserves agents. A minority of leaves decides the governing question; the rest exist to prove the tree is exhaustive, and get one cheap lookup at most

### My problem

[your problem: a few sentences of situation and what's bothering you. Messy is fine — structuring it is the point. For example: our subscription revenue grew 30% but profit fell, the board wants an answer in two weeks, and three teams are blaming each other]

### Your tasks

1. Frame with SCQA: Situation, Complication, and the one governing Question. If my problem statement hides several questions, say so and pick the one to run first — state why
2. Day-one answer: state your initial hypothesis for the governing question in one sentence. The tree exists to kill or confirm this hypothesis, not to decorate it
3. Build the tree, 2–3 levels deep. At each branching, name the splitting logic you chose — profit driver, customer journey, funnel, value chain, 3C, stakeholder map, or one you invent. Frameworks are splitting heuristics, not truths: pick the one whose branches the available data can actually tell apart, and say why the runner-up framework lost
4. MECE-check every branching and mark violations honestly — an overlap you name is a footnote; an overlap you hide dispatches two agents to fight over the same evidence
5. For each leaf: the falsifiable hypothesis, the data that would kill it, the test or analysis that produces that data, and priority (per the 80/20 test)
6. Dispatch mapping — this is where the tree becomes a graph. Route each priority leaf: fact-shaped leaves (answerable from documents and data) become search angles for a Diamond Research round (template 02); judgment-shaped leaves (no right answer in public data) become the decision for a Consultant Roundtable (template 04); trivial leaves get a one-line lookup, no graph needed

### Output format

One — SCQA framing, the governing question, and the day-one hypothesis

Two — the tree as a mermaid flowchart, splitting logic labeled at each branching, priority leaves visually marked

Three — leaf table:

| Leaf hypothesis | Data that kills it | Test / analysis | Priority | Dispatch to |
|---|---|---|---|---|
| (one sentence) | (specific source) | (how) | high / low | 02 angle / 04 decision / plain lookup |

Four — honesty paragraph: where MECE broke and why you accepted it, which framework felt forced, and what the tree structurally cannot see (the unknown-unknowns live outside every tree — say where you'd expect them)

Rules: no pleasantries, no restating my input. If my problem arrives already well-decomposed, say so and improve the leaves instead of rebuilding the tree. Never let the framework outrank the data: a beautiful tree whose branches the data can't distinguish is a drawing, not a plan.
