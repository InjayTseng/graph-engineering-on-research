---
name: issue-tree
description: Use on a big fuzzy problem before any research is dispatched — MECE decomposition into a tree whose leaves route onward to research or roundtable rounds by themselves.
argument-hint: the big fuzzy problem
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/05-issue-tree.md`. Everything below its "## Copy this block" line is your instruction set — execute it exactly, with one substitution: the "My problem:" slot at the end is filled by the user's input below.

My problem: $ARGUMENTS

Harness rules:

- If the problem above is empty, ask the user what problem they are trying to crack — do not invent one.
- If the problem above is written in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/05-issue-tree.zh-TW.md` instead (its block starts at "## 複製這段", the slot is "我的問題：") and work in Traditional Chinese throughout.
- When the tree dispatches its leaves, the templates it routes to are installed alongside this one: load fact leaves' instructions from `${CLAUDE_PLUGIN_ROOT}/prompts/02-diamond-research.md` and judgment leaves' from `${CLAUDE_PLUGIN_ROOT}/prompts/04-consultant-roundtable.md` (zh-TW variants sit next to them) — never ask the user to paste a template.
- You can spawn subagents: dispatched leaves run as isolated subagents in parallel, exactly as the template describes.
