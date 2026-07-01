# Agentic Code Review

Portable coding-agent review skill distilled from Addy Osmani's [Agentic Code Review](https://addyosmani.com/blog/agentic-code-review/) essay, published June 15, 2026.

## What It Does

This package turns the essay's review guidance into a practical workflow for AI-generated and agent-authored code:

- classify PRs by blast radius
- require intent and proof before deep review
- run multiple AI review perspectives for risky changes
- review tests and CI changes before implementation details
- watch common agent failure modes
- use AI reviewers as signals, not merge authority
- keep humans on high-risk decisions

## Use With Any Coding Agent

If your agent can read Markdown, point it at `AGENTIC_CODE_REVIEW.md`:

```text
Read AGENTIC_CODE_REVIEW.md and use it to review this PR.
```

That path works for Codex, Claude Code, Hermes, OpenClaw, and other agents even when they do not support a native skill registry.

## Native Skill Install

For agents that support `SKILL.md` packages, install the `agentic-code-review` directory.

Codex:

```bash
cp -R agentic-code-review ~/.codex/skills/
```

Claude Code-style skill directories:

```bash
cp -R agentic-code-review ~/.claude/skills/
```

Then invoke it with:

```text
Use $agentic-code-review to review this PR.
```

## Contents

- `AGENTIC_CODE_REVIEW.md` - portable entry point for any coding agent
- `agentic-code-review/SKILL.md` - native skill entry point
- `agentic-code-review/references/review-system.md` - detailed review system, checklists, and prompts
- `LIMITS.md` - what cannot be fully captured in a generic skill

## License

MIT
