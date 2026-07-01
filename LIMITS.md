# What This Skill Cannot Fully Capture

Some parts of agentic code review need live project context or tool integration. A generic skill can name them, but cannot decide them alone.

## Needs Project-Specific Policy

- high-risk paths: auth, payments, privacy, deploy, data deletion, regulated workflows
- required owners for those paths
- minimum CI, coverage, and security gates
- maximum acceptable PR size
- rollback requirements
- which AI reviewers or models are approved

How to handle it: add a repo-specific review policy file and make agents read it before this skill.

## Needs Real Evidence

The skill can require tests and CI output, but it cannot prove a human or agent actually ran them unless the evidence is attached.

How to handle it: require CI links, logs, screenshots, traces, or command output in the PR.

## Needs Tool Integration

The skill cannot by itself:

- fetch PR diffs
- run CI
- launch several AI reviewers
- merge duplicate findings
- post GitHub review comments
- enforce branch protection

How to handle it: add scripts or CI jobs when you want automation instead of instructions.

## Needs Codebase Knowledge

The skill can tell an agent to look for existing helpers and sibling callers. It cannot know the helper map for every repository.

How to handle it: combine it with repository search, language-aware indexing, or local architecture docs.

## Needs Human Judgment

The skill cannot decide:

- whether the requested product change should exist
- how much user harm is acceptable
- whether business risk justifies shipping
- who owns the merge after a high-risk review

How to handle it: keep a named human owner for high-risk merges.

## Needs Adapter Work For Native Skill Registries

Auto-discovery depends on each agent's registry format.

Known package path:

- Install `skills/agentic-code-review/`.
- Or use `npx skills add https://github.com/lzfxxx/agentic-code-review-skill --skill agentic-code-review`.

Known local install targets:

- Codex: `~/.codex/skills/agentic-code-review`
- Claude Code-style skill directories: `~/.claude/skills/agentic-code-review`

Anything beyond that should be added as a small adapter once the target agent's exact skill format is known.
