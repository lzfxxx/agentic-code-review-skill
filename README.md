# Agentic Code Review Skill

[![skills.sh](https://skills.sh/b/lzfxxx/agentic-code-review-skill)](https://skills.sh/lzfxxx/agentic-code-review-skill)

`SKILL.md` agent skill distilled from Addy Osmani's [Agentic Code Review](https://addyosmani.com/blog/agentic-code-review/) essay, published June 15, 2026.

## What It Does

This skill turns the essay's review guidance into a practical workflow for AI-generated and agent-authored code:

- classify PRs by blast radius
- require intent and proof before deep review
- run multiple AI review perspectives for risky changes
- review tests and CI changes before implementation details
- watch common agent failure modes
- use AI reviewers as signals, not merge authority
- keep humans on high-risk decisions

## Skill

| Skill | Description | Import URL |
|---|---|---|
| Agentic Code Review | Risk-tiered multi-perspective review for agent-authored code. | https://github.com/lzfxxx/agentic-code-review-skill/tree/main/skills/agentic-code-review |

## Install With skills.sh

```bash
npx skills add https://github.com/lzfxxx/agentic-code-review-skill --skill agentic-code-review
```

`skills.sh` lists repositories automatically after installs through the `skills` CLI.

## Manual Install

For agents that support `SKILL.md` packages but do not use the `skills` CLI, install the canonical skill folder:

Codex:

```bash
cp -R skills/agentic-code-review ~/.codex/skills/
```

Claude Code-style skill directories:

```bash
cp -R skills/agentic-code-review ~/.claude/skills/
```

Other `SKILL.md`-aware agents:

```text
Install the skill folder at:
https://github.com/lzfxxx/agentic-code-review-skill/tree/main/skills/agentic-code-review
```

Then use:

```text
Use $agentic-code-review to review this PR.
```

## Contents

- `skills/agentic-code-review/SKILL.md` - skill entry point
- `skills/agentic-code-review/references/review-system.md` - detailed review system, checklists, and prompts
- `LIMITS.md` - what cannot be fully captured in a generic skill

## License

MIT
