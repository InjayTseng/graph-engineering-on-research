---
name: adversarial-review
description: Use on a document that matters, that has been revised many times, that its author believes is complete — isolated attackers try to kill its conclusions before reality does.
argument-hint: file path to the document under review (or paste its conclusions)
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/prompts/03-adversarial-review.md` in full — this template has no single copy block; the whole file is the workflow: Prep, then attacker dispatch, then integration rules, then outputs. The document under review is the user's input below.

Document under review: $ARGUMENTS

Harness rules:

- If the input above is empty, ask the user which document or set of frozen conclusions to attack — do not pick one yourself.
- If the input above is a file path, read that file; it is the document under review.
- If the input above (or the document itself) is in Chinese, use `${CLAUDE_PLUGIN_ROOT}/prompts/03-adversarial-review.zh-TW.md` instead and work in Traditional Chinese throughout.
- You do the Prep and Integration layers yourself in this context — the template marks them as the layers that cannot be outsourced. Each attacker MUST be a separate subagent with fresh context that sees only its own de-identified slice, dispatched in parallel. Never simulate attackers sequentially in this context.
