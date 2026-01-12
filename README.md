# Ralph Loop Experiments

Exploring autonomous AI coding loops using the Ralph technique.

## What is Ralph Loop?

Ralph Loop is an autonomous coding technique where an AI agent iterates on a task until completion. The agent sees its own previous work in files, learns from failures, and keeps trying until success criteria are met.

Original concept by [Geoffrey Huntley](https://ghuntley.com/ralph/).

## Setup

This repo uses [flow-next](https://github.com/gmickel/gmickel-claude-marketplace) for structured task management and autonomous execution.

```bash
# Install flow-next plugin
claude plugin marketplace add https://github.com/gmickel/gmickel-claude-marketplace
claude plugin install flow-next

# Initialize Ralph
/flow-next:setup
/flow-next:ralph-init
```

## Usage

### Manual Mode
```bash
.flow/bin/flowctl list                    # See all tasks
.flow/bin/flowctl ready                   # See ready tasks
# In Claude: /flow-next:work fn-1.1       # Work on specific task
```

### Autonomous Mode
```bash
./scripts/ralph/ralph_once.sh             # Test one iteration
./scripts/ralph/ralph.sh                  # Full autonomous loop
```

## Key Concepts

**Fresh context per iteration.** Each loop iteration starts a new Claude session. State lives in files, not in the transcript.

**Re-anchoring.** Before every task, the agent re-reads the epic spec, task spec, and git state. No drift.

**Multi-model reviews.** Optional second model (Codex) reviews changes before marking tasks complete.

**Receipt-based gating.** Reviews must produce proof-of-work receipts. No receipt = no progress.

## Structure

```
.flow/                      # Task management
  epics/                    # Epic definitions
  tasks/                    # Task definitions
  specs/                    # Detailed specs
scripts/ralph/              # Autonomous loop
  ralph.sh                  # Main loop script
  config.env                # Settings
  runs/                     # Artifacts and logs
```

## Configuration

Edit `scripts/ralph/config.env`:

```bash
PLAN_REVIEW=codex           # Review backend: codex, rp, none
WORK_REVIEW=codex
MAX_ITERATIONS=25
MAX_ATTEMPTS_PER_TASK=5
```

## Resources

- [flow-next plugin](https://github.com/gmickel/gmickel-claude-marketplace)
- [Original Ralph technique](https://ghuntley.com/ralph/)
- [Anthropic Claude Code](https://claude.ai/code)

## License

MIT
