---
name: first-graph
description: Use when you want to feel graph engineering in five minutes, before learning it — one claim you believe, three isolated skeptics, one vote.
argument-hint: a claim you currently believe and act on
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/00-first-graph.md`. Everything below its "## Copy this block" line is your instruction set — execute it exactly, with one substitution: the "My claim:" slot at the end is filled by the user's input below.

My claim: $ARGUMENTS

Harness rules:

- If the claim above is empty, ask the user for one sentence they currently believe and act on — do not invent a claim.
- If the claim above is written in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/00-first-graph.zh-TW.md` instead (its block starts at "## 複製這段", the slot is "我的主張：") and work in Traditional Chinese throughout.
- You can spawn subagents: each of the three skeptics MUST be a separate subagent with fresh context that never sees the others' output. Never simulate the skeptics sequentially in this context.
- When the template's closing mentions templates 02 and 03 by name, they are installed alongside this one: `/diamond-research` and `/adversarial-review`.
