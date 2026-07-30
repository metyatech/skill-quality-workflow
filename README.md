# skill-quality-workflow

An [Agent Skill](https://agentskills.io) for test coverage methodology and
defect handling workflows.

## What it does

Provides guidance for:

- **Test coverage methodology** — what to cover (boundaries, state
  transitions, integration points), how to structure tests (snapshot/golden
  tests, test matrices), and how to handle nondeterminism
- **External contract surface verification** — derive a changed
  user-visible surface matrix from docs/specs/help/UI/API and
  require direct evidence for each changed surface item
- **Feedback loops and root causes** — defect root-cause classification,
  upstream prevention, and feedback-loop improvement
- **Verification reporting discipline** — keep detailed AC/evidence tracking
  internally while defaulting normal user-facing completion reports to concise
  summaries unless detailed proof is requested

## When it triggers

- Writing tests or designing test coverage
- Handling defect fixes or performing root-cause analysis
- Creating test matrices or choosing between unit/integration/E2E tests
- Setting up CI verification or commit hooks

## Installation

```bash
npx skills add metyatech/skill-quality-workflow --yes --global
```

## Usage

Once installed, the skill is automatically triggered by the Gemini CLI when relevant tasks are detected. You can also explicitly reference it by asking about "quality workflow" or "test coverage".

## Development

This repository uses `compose-agentsmd` to manage agent rules.

### Prerequisites

- Node.js (Latest LTS recommended)
- `npm`

### Verification

Run the verification suite to check for linting and formatting issues:

```powershell
npm run verify
```

## Compatibility

- **Agent System**: Gemini CLI, standard Agent Skills runtime.
- **OS**: Cross-platform (Windows, macOS, Linux).

## License

MIT
