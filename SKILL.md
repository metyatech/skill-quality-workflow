---
name: quality-workflow
description: Use when writing tests, designing test coverage, handling defect fixes, performing root-cause analysis, setting up CI verification, or configuring commit-time hooks. Also use when creating test matrices or choosing between unit/integration/E2E tests. Do not use for general development or linting setup.
---

# Quality workflow

## Test coverage methodology

- Cover success, failure, boundary, invalid input, and key state transitions (including first-run/cold-start vs subsequent-run behavior when relevant); include representative concurrency/retry/recovery when relevant.
- For deterministic output files, use full-content snapshot/golden tests.
- Prefer making nondeterministic failures reproducible over adding sleeps/retries; do not mask flakiness.
- For timing/order/race issues, prefer deterministic synchronization (events, versioned state, acks/handshakes) over fixed sleeps.
- For integration boundaries (network/DB/external services/UI flows), add an integration/E2E/contract test that exercises the boundary; avoid unit-only coverage for integration bugs.
- For non-trivial changes, create a small test matrix (scenarios x inputs x states) and cover the highest-risk combinations; document intentional gaps.

## Feedback loops and root causes

- Treat time-to-detect and time-to-fix as quality attributes; shorten the feedback loop with automation and observability rather than relying on manual QA.
- For any defect fix or incident remediation, perform a brief root-cause classification: implementation mistake, design deficit, and/or ambiguous/incorrect requirements.
- Feed the root cause upstream in the same change set: add or tighten tests/checks/alerts, update specs/acceptance criteria, and update design docs/ADRs when applicable.
- If the failure should have been detected earlier, add a gate at the earliest reliable point (lint/typecheck/tests/CI required checks or runtime alerts/health checks); skipping this requires explicit user approval.
- Record the prevention mechanism (what will catch it next time) in the PR description or issue comment; avoid "fixed" without a concrete feedback-loop improvement.

## Verification evidence procedures

### AC→evidence mapping format

- Maintain an explicit mapping: `AC -> evidence (tests/commands/manual steps)`.
- The mapping may be presented in a compact per-item form (one line per AC including evidence + outcome) to reduce verbosity.
- For non-code changes, run the relevant verification subset and justify why the full suite is not needed.
- If required checks cannot be run, stop and ask for explicit approval to proceed with partial verification, and provide an exact manual verification plan.

### Final delivery response format

Every final response for state-changing work MUST include:

- A compact goal+verification report. Labels may be `Goal`/`Verification` instead of `AC`.
- `AC -> evidence` mapping with outcomes (PASS/FAIL/NOT RUN/N/A), possibly in compact per-item form.
- The exact verification commands executed and their outcomes.

## CI and commit-hook setup

For CI and commit-hook invariants, see the quality-and-delivery rules. This section covers setup procedures.

- Configure required default-branch checks when permitted; otherwise report the limitation.
- Do not rely on smoke-only or scheduled-only gates.
- If hooks are not installed, install them before the first commit in the session. If impossible, run full verify manually before every commit.
- Verify scripts must enforce lock-file integrity (manifest/lock drift detection).

## Environment constraints

- If environment limits execution (network/db/sandbox), run the available subset, document skipped coverage, ensure CI covers the remainder.
- For user-facing tools/GUI, run end-to-end manual verification in addition to automated tests; when manual testing finds issues, add failing tests first, then fix.
- If required tests are impractical, document the gap, provide manual verification plan, and get explicit approval.

## Test practices

- Heuristic waits require condition-based logic, hard deadlines, diagnostics, and explicit requester approval.
- Log minimally with diagnostic context; never log secrets/personal data; remove debugging instrumentation before final patch.
