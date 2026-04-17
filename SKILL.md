---
name: quality-workflow
description: Use when writing tests, designing test coverage, handling defect fixes, performing root-cause analysis, setting up CI verification, or configuring commit-time hooks. Also use when creating test matrices or choosing between unit/integration/E2E tests. Do not use for general development or linting setup.
---

# Quality workflow

## Test coverage methodology

- Tests MUST cover success, failure, boundary, invalid input, and
  key state transitions (including first-run/cold-start vs
  subsequent-run behavior when relevant). Tests MUST include
  representative concurrency, retry, and recovery cases when
  relevant.
- For non-trivial changes, the agent MUST enumerate the changed
  externally visible contract surface before choosing tests. Use
  the changed docs, specs, CLI help, API descriptions, UI
  controls, file formats, and user-facing messages as the source
  of truth for that surface.
- The agent MUST build a small surface matrix
  (`surface item × state/outcome`) and cover the highest-risk
  combinations directly. The agent MUST NOT treat one passing
  happy-path check as evidence that adjacent documented entry
  points also work.
- For deterministic output files, the agent MUST use full-content
  snapshot or golden tests.
- The agent MUST prefer making nondeterministic failures
  reproducible over adding sleeps or retries. The agent MUST NOT
  mask flakiness.
- For timing, order, or race issues, the agent MUST prefer
  deterministic synchronization (events, versioned state,
  acks/handshakes) over fixed sleeps.
- For integration boundaries (network, DB, external services, UI
  flows), the agent MUST add an integration, E2E, or contract test
  that exercises the boundary. The agent MUST NOT rely on
  unit-only coverage for integration bugs.
- For non-trivial changes, the agent MUST create a small test
  matrix (scenarios × inputs × states) and MUST cover the
  highest-risk combinations. Intentional gaps MUST be documented.

## Feedback loops and root causes

- The agent MUST treat time-to-detect and time-to-fix as quality
  attributes and MUST shorten the feedback loop with automation
  and observability rather than relying on manual QA.
- For any defect fix or incident remediation, the agent MUST
  perform a brief root-cause classification:
  1. Implementation mistake.
  2. Design deficit.
  3. Ambiguous or incorrect requirements.
- The agent MUST feed the root cause upstream in the same change
  set: add or tighten tests/checks/alerts, update specs and
  acceptance criteria, and update design docs or ADRs when
  applicable.
- If the failure should have been detected earlier, the agent MUST
  add a gate at the earliest reliable point (lint, typecheck,
  tests, CI required checks, runtime alerts, or health checks).
  Skipping this requires explicit user approval.
- The agent MUST record the prevention mechanism (what catches
  recurrence) in the PR description or issue comment. The agent
  MUST NOT report "fixed" without a concrete feedback-loop
  improvement.

## Verification evidence procedures

### AC->evidence mapping format

- The agent MUST maintain an explicit mapping of
  `AC -> evidence (tests/commands/manual steps)`.
- The agent SHOULD maintain a parallel mapping of
  `changed surface item -> direct evidence` whenever the change
  adds or alters a user-visible interface such as a command,
  flag, endpoint, view, or state.
- The mapping MAY be presented in a compact per-item form (one
  line per AC including evidence and outcome) to reduce verbosity.
- For non-code changes, the agent MUST run the relevant
  verification subset and MUST justify why the full suite is not
  needed.
- If required checks cannot be run, the agent MUST stop and
  request explicit approval to proceed with partial verification.
  The agent MUST provide an exact manual verification plan.

### Final delivery response format

- The agent MUST maintain an explicit `AC -> evidence` mapping
  internally for every state-changing task.
- In normal user-facing final responses, the agent MUST default to
  a concise summary of outcome, verification, and residual risk.
- The agent MUST expand to explicit `Goal/Verification`,
  `AC -> evidence`, or exact command listings only when the user
  requests them or when higher-priority rules require
  path-specific evidence to be shown separately.

## CI and commit-hook setup

For CI and commit-hook invariants, see the rule modules
`quality-and-verification` and `lint-format-static`. This section
covers setup procedures.

- The agent MUST configure required default-branch checks when
  permitted. Otherwise the agent MUST report the limitation.
- The agent MUST NOT rely on smoke-only or scheduled-only gates.
- If hooks are not installed, the agent MUST install them before
  the first commit in the session. If installation is impossible,
  the agent MUST run full verify manually before every commit.
- Verify scripts MUST enforce lock-file integrity (manifest/lock
  drift detection).

## Environment constraints

- If the environment limits execution (network, DB, sandbox), the
  agent MUST run the available subset, MUST document skipped
  coverage, and MUST ensure CI covers the remainder.
- For user-facing tools and GUIs, the agent MUST run end-to-end
  manual verification in addition to automated tests. When manual
  testing finds issues, the agent MUST add failing tests first,
  then fix.
- For CLI and API changes, direct contract checks MUST include
  discoverability surfaces such as help, usage, version, schema,
  or equivalent documented entry points when those surfaces are
  part of the delivered contract.
- If required tests are impractical, the agent MUST document the
  gap, MUST provide a manual verification plan, and MUST get
  explicit approval.

## Test practices

- Heuristic waits MUST require condition-based logic, hard
  deadlines, diagnostics, and explicit requester approval.
- Logs MUST be minimal with diagnostic context. Logs MUST NOT
  contain secrets or personal data. The agent MUST remove
  debugging instrumentation before the final patch.
