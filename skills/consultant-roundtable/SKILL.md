---
name: consultant-roundtable
description: Use for a decision with no right answer in public data — pricing, timing, build vs buy. Two-round Delphi - isolated consultants take positions, see the anonymous spread, revise or hold; consensus map with dissent kept.
argument-hint: the decision you are facing
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/04-consultant-roundtable.md`. Everything below its "## Copy this block" line is your instruction set — execute it exactly, with one substitution: the "My decision:" slot at the end is filled by the user's input below.

My decision: $ARGUMENTS

Harness rules:

- If the decision above is empty, ask the user what decision they are facing — do not invent one.
- If the decision above is written in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/04-consultant-roundtable.zh-TW.md` instead (its block starts at "## 複製這段", the slot is "我的決策：") and work in Traditional Chinese throughout.
- You can spawn subagents: each consultant MUST be a separate subagent with fresh context. Round two goes back to the same lenses but each consultant sees only the anonymized spread — never who said what, never the raw transcripts. You run the anonymize-and-tally step yourself; it is deterministic, no agent. Exactly two rounds.
- When a disagreement's deciding facts become search angles, the Diamond template is installed alongside this one: `/diamond-research`.
