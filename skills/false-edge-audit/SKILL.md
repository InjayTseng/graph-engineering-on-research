---
name: false-edge-audit
description: Use when you suspect your agents or pipeline steps are waiting in a line they don't need — audits an existing workflow for "and then"s that are fake.
argument-hint: your current workflow, step by step (or a file path to it)
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/01-false-edge-audit.md`. Everything below its "## Copy this block" line is your instruction set — execute it exactly, with one substitution: the "My workflow:" slot at the end is filled by the user's input below.

My workflow: $ARGUMENTS

Harness rules:

- If the workflow above is empty, ask the user to describe their current workflow step by step — do not invent one.
- If the workflow above is a file path, read that file and treat its contents as the workflow.
- If the workflow above is written in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/01-false-edge-audit.zh-TW.md` instead (its block starts at "## 複製這段", the slot is "我的工作流：") and work in Traditional Chinese throughout.
