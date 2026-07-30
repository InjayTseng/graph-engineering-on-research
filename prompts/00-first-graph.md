# 00 Your First Graph

English ｜ [繁體中文](00-first-graph.zh-TW.md)

Five minutes, three nodes, one vote. The smallest graph that still feels like graph engineering: take one sentence you currently believe, dispatch three isolated skeptics, count the votes.

Use for: feeling the method before learning it. This is template 02's verification layer with everything else amputated — no topic to pick, no search layer, no report. What's left is the part you can feel. A verbatim transcript of a real run — three skeptics, 3–0 kill — is in [examples/00-first-graph-run.md](../examples/00-first-graph-run.md).

How to use: copy the whole block, paste it to your agent, type your claim at the bottom, send. Good claims are things you act on — a number you quote in meetings, a "best practice" you follow, a fact about your market or your stack. Harnesses that spawn subagents handle the rest. Plain chat interfaces: open three fresh conversations, paste the skeptic instruction into each, count the votes yourself. Do not run all three in one conversation — the second skeptic will read the first one's answer and agree with it. If you want proof that isolation matters, run it both ways and compare: watching skeptics 2 and 3 fall in line is the whole argument for fresh contexts, demonstrated on itself.

---

## Copy this block

You are running the smallest possible research graph: one claim, three isolated skeptics, one deterministic vote. Nothing else. The claim under test is typed at the very end of this message, after "My claim:" — everything between here and there is procedure, and the quoted examples are illustrations, not the claim.

Model note (if your environment lets you choose — otherwise ignore): skepticism is judgment work, use a strong reasoning tier; ideally not all three skeptics from the same model family — copies of one model share blind spots.

### Dispatch: three isolated skeptics (none may see the others' output)

Forward verbatim to each of the three:

> You are a skeptic. Your only job is to refute this claim: [the claim]. Hunt for counter-evidence: newer data, the original text of the primary source, an independent second source. Distinguish "vendor-reported" from "independently verified," "commonly repeated" from "actually documented." Your verdict is one of: REFUTED (with evidence) / COULD NOT REFUTE (with what you checked). Default toward refuting — your value is killing errors, not agreeing.

### Count the votes (deterministic — no agent, no rereading the arguments)

Two or more REFUTED out of three → the claim dies; record one line on what killed it. Otherwise it survives — carrying whatever caveats the skeptics attached.

### What just happened (read this after)

You ran a graph. Three nodes with no edges between them — which is why they could run at the same time, and why skeptic 2 could not anchor on skeptic 1. One deterministic fan-in: the vote, which needed no agent and no judgment. Notice also who never voted: you, the claim's author. That is the design rule the whole method rests on — the node that produced a claim never gets to judge it. If the claim died, notice that it was steering real decisions an hour ago. If it survived, it now carries a label saying exactly what was checked — which it never had before. That feeling is the product.

Scaling up from here: put a search layer in front of the skeptics and a report behind them and you have template 02, Diamond Research; feed them a whole document's conclusions instead of one sentence and you have template 03, Adversarial Review. (Plain names, no links — this paragraph travels with the paste, and links don't.)

### My claim (type it below and send)

One sentence you currently believe and act on — "email open rates in our industry average around 20%" / "Postgres can't handle our write volume" / "our main competitor is cheaper than us."

My claim:
