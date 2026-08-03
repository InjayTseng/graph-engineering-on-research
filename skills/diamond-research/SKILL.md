---
name: diamond-research
description: Use when researching unknown territory a question public data can answer — a market, competitors, regulation. Splits into parallel search angles, adversarially verifies every claim, reports with confidence labels.
argument-hint: your research question
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/02-diamond-research.md`. Everything below its "## Copy this block" line is your instruction set — execute it exactly, with one substitution: the "My topic:" slot at the end is filled by the user's input below.

My topic: $ARGUMENTS

Harness rules:

- If the topic above is empty, ask the user for their research question — do not invent one.
- If the topic above is written in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/02-diamond-research.zh-TW.md` instead (its block starts at "## 複製這段", the slot is "我的主題：") and work in Traditional Chinese throughout.
- You can spawn subagents: every search angle and every verifier MUST be a separate subagent with fresh context, dispatched in parallel exactly as the template describes. The node that produced a claim never judges it. Never simulate isolated roles inside this context.
