# Agentic Code Review - Portable Agent Instructions

Use this file with any coding agent that can read Markdown: Codex, Claude Code, Hermes, OpenClaw, or a custom agent.

Task prompt:

```text
Read AGENTIC_CODE_REVIEW.md and use it to review this PR/diff.
If the review is high risk or policy-related, also read agentic-code-review/references/review-system.md.
```

## Core Rules

- Spend review effort where being wrong is expensive.
- Require intent and runnable evidence before deep review.
- Classify risk by blast radius, code lifetime, and shared ownership.
- Run multiple AI perspectives for risky changes; prefer different tools, models, or prompts.
- Read test and CI changes before implementation code.
- Treat AI review as signal, not decision.
- Keep a human owner for merge and accountability.

## Minimum Workflow

1. Ask for missing evidence:
   - change intent
   - expected behavior
   - tests/checks run
   - known risks
   - skipped checks

2. Assign risk:
   - Low: docs, formatting, small config, isolated UI copy.
   - Medium: normal product code, shared helpers, API behavior, migrations with rollback.
   - High: auth, payments, permissions, PII, security boundaries, LLM calls with user input, CI policy, release/deploy logic, data loss paths.

3. Pick review depth:
   - Low: deterministic checks and quick human glance.
   - Medium: tests, diff review, one AI review if available.
   - High: full CI, focused tests, at least two different AI review perspectives, owner review, and security/privacy review when relevant.

4. Review in this order:
   - test changes
   - CI/build/lint/coverage changes
   - public contracts, migrations, rollback behavior
   - implementation
   - duplication, dead code, unnecessary abstraction
   - prompt-injection and untrusted-input paths

5. Report with:

```markdown
Risk tier: low | medium | high
Decision: approve | needs work | block
Evidence checked: ...
AI perspectives run: correctness | security | production | tests/CI | maintainability
Human attention needed: ...

Findings:
- [severity] file:line - issue, impact, fix

AI reviewer notes:
- Signals by perspective:
- Disagreements, false positives, or gaps:
```

## Deep Reference

Read `agentic-code-review/references/review-system.md` when:

- designing a team's review policy
- triaging a PR queue
- reviewing high-risk code
- coordinating multiple AI reviewers
- deciding what cannot be automated
- auditing whether an agentic review loop is trustworthy
