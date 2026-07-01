# Agentic Code Review Skill

Codex skill distilled from Addy Osmani's [Agentic Code Review](https://addyosmani.com/blog/agentic-code-review/) essay, published June 15, 2026.

## What It Does

This skill turns the essay's review guidance into a practical workflow for AI-generated and agent-authored code:

- classify PRs by blast radius
- require intent and proof before deep review
- run multiple AI review perspectives for risky changes
- review tests and CI changes before implementation details
- watch common agent failure modes
- use AI reviewers as signals, not merge authority
- keep humans on high-risk decisions

## Install

Place the `agentic-code-review` directory in your Codex skills directory:

```bash
cp -R agentic-code-review ~/.codex/skills/
```

Then invoke it with:

```text
Use $agentic-code-review to review this PR.
```

## License

MIT
