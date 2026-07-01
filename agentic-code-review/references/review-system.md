# Agentic Code Review System

Detailed reference for high-risk reviews, review-policy design, queue triage, and multi-agent review setup.

## Contents

- Operating model
- Risk model
- Intake gate
- Multiple AI perspectives
- Review order
- Agent-specific failure modes
- Queue triage
- Human role
- Team policy
- Templates
- Source

## Operating Model

Agentic coding changes the bottleneck. Writing code gets cheaper; understanding and verification do not. Review is no longer mainly a style pass over human-written code. It is the system that decides whether machine-speed output can be trusted.

Do not review every diff to the same depth. Review depth should follow the cost of being wrong.

Good review depends on three variables:

- Blast radius: what breaks if this is wrong.
- Code lifetime: throwaway prototype or long-lived system.
- Shared ownership: one person understands it or a team must maintain it.

Agent-authored PRs often lack preserved intent. Push intent reconstruction back to the author or agent before deep review.

## Risk Model

Low risk:

- docs, comments, formatting
- isolated copy changes
- generated snapshots
- small config changes with obvious rollback

Medium risk:

- normal product features
- shared helpers
- API behavior
- migrations with rollback
- cross-module refactors

High risk:

- auth, permissions, payments
- PII, privacy, security boundaries
- data deletion or irreversible migrations
- LLM calls that include user-controlled input
- CI, coverage, release, deploy, or rollback policy
- code paths with production incident history

Escalate one tier when the diff is large, poorly scoped, missing evidence, or changes many tests.

## Intake Gate

Before deep review, require:

- intent: what problem is being solved and why
- scope: what changed and what deliberately did not change
- evidence: commands run, CI links, screenshots, traces, or logs
- risk notes: known hazards and skipped checks
- reversibility: rollback or mitigation for risky changes

Defer review when:

- there is no stated intent
- checks were not run and no reason is given
- a large diff mixes unrelated work
- tests were rewritten without explaining the behavioral change
- CI, lint, coverage, or security gates were weakened

## Multiple AI Perspectives

Use heterogeneous review for risky changes. Different AI reviewers often find different issues, so overlap is not required for a finding to matter.

Prefer:

- different tools when available
- different model families when available
- different prompts when only one model is available

Useful perspectives:

- Correctness: logic, edge cases, invariants, broken assumptions.
- Security/privacy: auth, injection, secrets, PII, unsafe defaults.
- Production impact: failure modes, observability, rollback, performance.
- Tests/CI integrity: weakened assertions, skipped checks, coverage drops.
- Maintainability: duplication, local conventions, unnecessary abstraction.

When reviewers disagree:

- inspect single-reviewer findings instead of discarding them
- treat consensus as stronger signal, not proof
- watch for shared blind spots when reviewers use similar models or prompts
- let deterministic checks and human ownership settle high-risk decisions

## Review Order

1. Test changes:
   - rewritten assertions
   - deleted tests
   - new tests that only mirror implementation
   - broad snapshot updates

2. CI and gates:
   - skipped lint/type checks
   - lowered coverage thresholds
   - relaxed security scans
   - modified release/deploy checks

3. Contracts and reversibility:
   - API behavior
   - database migrations
   - backward compatibility
   - rollback path

4. Implementation:
   - correctness
   - data flow
   - failure handling
   - concurrency and race conditions

5. Existing codebase fit:
   - duplicated helpers
   - ignored local patterns
   - speculative abstraction
   - dead code

6. LLM-specific paths:
   - user-controlled input entering prompts or tools
   - prompt-injection exposure
   - unsafe tool permissions
   - missing output validation

## Agent-Specific Failure Modes

- The agent changes behavior, then updates tests to bless the new behavior.
- The agent weakens CI because green checks are rewarded.
- The diff is large and plausible but mixes unrelated work.
- The implementation duplicates helpers that already exist.
- The PR summary is confident but has no runnable proof.
- The agent handles the specified path but misses sibling callers.
- LLM features pass trusted examples but expose untrusted-input paths.

Mitigation:

- read tests first
- grep callers before accepting shared-function changes
- reject unfocused diffs
- require command output or CI links
- keep deterministic gates strict
- run a security/privacy perspective for user-input and LLM paths

## Queue Triage

Triage by risk, not arrival order.

Fast-track:

- small diffs
- clear intent
- passing evidence
- low blast radius

Send back:

- oversized diffs
- missing intent
- missing proof
- mixed concerns
- subjective feedback that the agent is unlikely to resolve without a smaller task

Spend human time on:

- high-risk code paths
- surprising behavior changes
- test rewrites
- security/privacy boundaries
- production rollback and observability

## Human Role

The human does not need to read every line of every low-risk diff. The human must own:

- whether the change should exist
- which risks matter
- when the loop is allowed to stop
- high-blast-radius gates
- final merge accountability

AI review output is telemetry. It is not authority.

## Team Policy

Teams should measure review capacity as a scarce resource:

- review queue age
- time to first review
- PRs merged with no substantive review
- defect rate after AI-authored changes
- review time spent by senior maintainers
- rate of PRs sent back for missing evidence

Do not assume faster code generation means less QA or fewer senior reviewers. It can increase the verification burden.

## Templates

PR intake request:

```markdown
Please add:
- intent and expected behavior
- commands/checks run with output or CI links
- risk notes and skipped checks
- rollback plan if this touches production data, deploy, auth, payments, privacy, or CI policy
```

Batch triage:

```markdown
Fast-track:
- PR: reason

Needs work before review:
- PR: missing evidence or scope issue

High-risk human review:
- PR: risk path and required owner/security review
```

Perspective prompts:

```text
Review this diff only for correctness and edge cases. Ignore style unless it affects behavior.
```

```text
Review this diff only for security, privacy, permissions, secrets, untrusted input, and prompt-injection risk.
```

```text
Review this diff only for tests and CI integrity. Focus on deleted tests, rewritten assertions, skipped checks, and weakened gates.
```

```text
Review this diff only for production impact: rollback, observability, migration safety, performance, and incident risk.
```

```text
Review this diff only for maintainability: duplicated helpers, local patterns, unnecessary abstraction, and ownership clarity.
```

## Source

Distilled from Addy Osmani's "Agentic Code Review" (June 15, 2026): https://addyosmani.com/blog/agentic-code-review/
