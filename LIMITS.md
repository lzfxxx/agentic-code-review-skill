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

## Needs Runtime Or Installer Support

`skills.sh` covers cross-agent distribution for supported coding agents through the `skills` CLI. That means this repo does not need a separate adapter for each supported agent.

Known install path:

- `npx skills add lzfxxx/agentic-code-review-skill`

Manual fallback paths:

- Codex: `~/.codex/skills/agentic-code-review`
- Claude Code-style skill directories: `~/.claude/skills/agentic-code-review`

What still cannot be captured in the skill:

- agents not supported by `skills.sh`
- private or locked-down environments where `npx skills add` cannot run
- agent runtimes that install the file but do not auto-invoke skills reliably
- organization-specific allowlists, mirrors, or marketplace approval rules

How to handle it: use `skills.sh` as the default installer, and add a small adapter only for unsupported or locked-down environments.
