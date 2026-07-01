---
name: agentic-code-review
description: Risk-tiered code review workflow for AI-generated or agent-authored changes. Use when reviewing pull requests, diffs, or batches of incoming PRs produced or coauthored by coding agents; triaging review queues by blast radius; deciding what evidence is required before review; auditing test, CI, security, prompt-injection, or maintainability risks; or deciding where human judgment must stay in an AI-assisted review loop.
---

# Agentic Code Review

Use review effort where being wrong is expensive. Treat AI reviewers as sensors, not verdicts. A human owns the merge.

## Workflow

1. Require evidence before review:
   - What changed and why.
   - The intended behavior.
   - Tests, linters, type checks, builds, or manual checks run.
   - Known risks, skipped checks, and follow-up work.
   - If intent or proof is missing, ask for it before spending deep review time.

2. Classify blast radius:
   - Low: docs, formatting, small config, isolated UI copy, generated snapshots.
   - Medium: normal feature work, shared helpers, API behavior, migrations with rollback.
   - High: auth, payments, permissions, PII, security boundaries, data loss paths, LLM calls with user-controlled input, CI policy, deploy/release logic.

3. Pick the review depth:
   - Low: deterministic checks plus a quick human glance.
   - Medium: tests and diff review, with one AI review if available.
   - High: full CI, focused tests, two different AI reviewers if available, domain-owner human review, and security/privacy review when relevant.

4. Read in the failure-prone order:
   - Test changes first, especially rewritten assertions.
   - CI/build/lint/coverage changes.
   - Public contracts, migrations, and rollback behavior.
   - Core implementation.
   - Duplicated helpers, dead code, broad rewrites, and unnecessary abstraction.

5. Watch agent-specific failure modes:
   - Tests changed to accept broken behavior.
   - Lint, coverage, or failing checks weakened to get green CI.
   - Large unfocused diffs that hide unrelated behavior changes.
   - New helper code that already exists elsewhere.
   - User-controlled text flowing into prompts or tools without injection defenses.
   - Confident summaries with no runnable evidence.

6. Triage PR queues by risk, not arrival order:
   - Fast-track small, well-scoped changes with proof.
   - Send vague or oversized agent PRs back for smaller diffs and evidence.
   - Spend human attention on high-blast-radius paths and surprising behavior.

## Review Output

Use this shape unless the user asks for another format:

```markdown
Risk tier: low | medium | high
Decision: approve | needs work | block
Evidence checked: ...
Human attention needed: ...

Findings:
- [severity] file:line - issue, impact, fix

AI reviewer notes:
- Useful signals:
- False positives or gaps:
```

## Source

Distilled from Addy Osmani's "Agentic Code Review" (June 15, 2026): https://addyosmani.com/blog/agentic-code-review/
