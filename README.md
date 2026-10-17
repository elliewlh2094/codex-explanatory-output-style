# Codex Explanatory Output Style Skill

English | **[中文](README.zh-TW.md)**

**Let Codex explain its design choices while completing your task.**

`explanatory-output-style` is a Codex skill that adds short `★ Insight` blocks when you ask to learn from coding, debugging, planning, or review. Each block contains two or three concrete points about the code and the choices behind it.

## Example

After installation, mention the skill in your request. This hypothetical example adds support for a missing label:

```text
Use $explanatory-output-style to make normalize_label(None) return an empty string. Teach me how the change preserves existing string behavior.
```

An additional teaching block could look like this:

```markdown
`★ Insight ──────────────────────────────────────`
- An explicit value is None check handles the missing label without also accepting 0 or False.
- Strings still go through the existing strip().lower() call, preserving whitespace removal and lowercase conversion.
`─────────────────────────────────────────────────`
```

Codex still completes the requested work and reports the changes and verification results. The Insight block explains the relevant implementation choices.

## When to Use

| Your request | Intended behavior |
|---|---|
| Explicitly mention `$explanatory-output-style` | Use the skill for the requested task. |
| Ask to learn, such as “fix this bug and teach me why” | Codex may select the skill automatically. |
| Ask to understand a plan or learn from a review | Explain the relevant tradeoffs or findings, without requiring code edits. |
| Only ask to implement, debug, or review | The request alone should not activate the teaching skill. |
| Ask for commands only, a short answer, or no teaching | Respect that instruction, even if the skill is explicitly mentioned. |

Automatic selection is enabled through `policy.allow_implicit_invocation: true`, but matching is not guaranteed. Mention the skill explicitly if Codex misses it.

## Installation

This repository distributes the complete `explanatory-output-style/` folder. No build, server, or additional runtime dependencies are required beyond Codex.

Copy the folder into one of the [documented local skill locations](https://learn.chatgpt.com/docs/build-skills):

| Scope | Destination |
|---|---|
| One project | `<your-project>/.agents/skills/explanatory-output-style/` |
| Your projects | `~/.agents/skills/explanatory-output-style/` |

Inspect the destination first. Preserve any existing installation and avoid installing the same skill in multiple locations. The installed folder should contain:

```text
explanatory-output-style/
  SKILL.md
  agents/
    openai.yaml
```

Codex detects skill changes automatically. If the skill does not appear, restart Codex.

## Behavior and Limits

- Explain choices grounded in the actual code, repository conventions, tests, or observed failures. Keep the explanations brief and use the user's language.
- Place Insight blocks before or after relevant work when there is something useful to teach. Matching pairs are not required.
- Keep insights in the conversation unless the user explicitly requests them in a file. Avoid generic tutorials, routine tool narration, and repeated summaries.
- Apply teaching to the requested task. This package does not set a session-wide style or modify global instructions. Do not assume teaching carries over to unrelated later requests.

## Design and Sources

The skill is inspired by [Claude Code's current Explanatory output style](https://code.claude.com/docs/en/output-styles). Claude's style applies across a session; this skill provides teaching for the requested Codex task, including planning and review.

The historical starting point is Anthropic's [explanatory-output-style plugin](https://github.com/anthropics/claude-code/tree/main/plugins/explanatory-output-style), which injects instructions through a `SessionStart` hook. Its README still calls Explanatory deprecated; use the current Claude documentation for current behavior.

The implementation follows OpenAI's [skill documentation](https://learn.chatgpt.com/docs/build-skills) and [skill design guidance](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra). Sources reviewed on 2026-10-17.

## Contributing and Verification

See [Repository Guidelines](AGENTS.md) for the file structure, editing conventions, and verification cases. Keep both READMEs consistent with `SKILL.md` and `agents/openai.yaml`.

For documentation changes, check Markdown links, examples, and whitespace with `git diff --check`. For behavior changes, follow the comparison procedure in the contributor guide and OpenAI's [skill evaluation guide](https://developers.openai.com/blog/eval-skills). Record skill-loading evidence separately from the output format and task result.

This repository has no automated test suite or coverage threshold. A few trial runs provide preliminary evidence, not reliability across models or versions.
