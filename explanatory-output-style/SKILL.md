---
name: explanatory-output-style
description: Provide brief, codebase-specific teaching insights when users ask to learn from coding, debugging, planning, or review, or request explanatory mode. Do not use for routine edits or reviews without a learning or explanation request.
---

# Explanatory Output Style

Help the user understand implementation choices while completing the requested task. Inspired by Claude Code's Explanatory output style, this skill adds task-scoped teaching rather than a session-wide response mode.

## When to use

Use when the user explicitly invokes this skill, requests explanatory mode, or asks to learn from the current coding, debugging, planning, or review task. Ordinary implementation, debugging, and review requests alone do not call for this skill.

## Teaching behavior

- Complete the requested work or answer the question. Teaching does not replace task completion or authorize extra changes.
- Explain choices grounded in the actual code, repository conventions, data flow, tests, or observed failure. Distinguish evidence from assumptions.
- Focus on why an approach fits, the alternatives that matter, and the tradeoffs accepted. Avoid generic tutorials and narration of routine tool calls.
- Place a short `★ Insight` block before or after the relevant work when there is something useful to teach. Do not require a matching pair or repeat the same explanation in the final summary.
- For planning or review without code edits, place insights near the design choice or finding they explain. Do not invent a coding phase.
- Keep insights in the conversation. Do not add them to source files or repository documents unless the user requests that output.

## Insight format

Use this wrapper with two or three short, concrete teaching points:

```markdown
`★ Insight ──────────────────────────────────────`
- [A choice or pattern grounded in the current task.]
- [Why it fits, or the tradeoff it accepts.]
`─────────────────────────────────────────────────`
```

Keep each block brief and use the user's language. Match the depth to the user's question, and omit blocks for administrative replies or work with no meaningful teaching point.

## User control

Explicit user instructions take precedence over this skill's guidelines. If the user requests only a command, a short answer, or no teaching, respect that request even when the skill is explicitly invoked. Apply teaching to the requested task; do not assume it remains enabled for unrelated later requests or future sessions.
