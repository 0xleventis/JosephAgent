# Joseph

A financial discipline agent that automatically sweeps a percentage of your trading profits to a designated savings wallet and manages community tasks with $JOSEPH rewards.

## Overview

Joseph helps you build wealth by enforcing disciplined profit-taking and community engagement. After each trade, Joseph calculates your realized profit and automatically sweeps a configurable percentage into a separate savings wallet. Additionally, Joseph allows you to broadcast tasks to the community and reward contributors in $JOSEPH.

## Features

- **Profit Syncing**: Tracks realized profits from your trading activity on Base.
- **Automatic Sweeping**: Moves a percentage of profits to your savings wallet.
- **Task Management**: Broadcast tasks, collect submissions, and release $JOSEPH rewards.
- **Configurable Rate**: Set your savings rate (default: 50%).
- **Custom Savings Address**: Designate any wallet as your savings destination.
- **USDC-Native**: Operates in USDC on Base for stability and low fees.

## Configuration

| Setting | Description | Default |
|---------|-------------|---------|
| Savings Rate | Percentage of profits to sweep | 50% |
| Savings Address | Destination wallet for swept profits | None (must be set) |
| Reward Token | Token used for task rewards | $JOSEPH |

## Commands

### Savings
- `set savings address <wallet>` — Configure where profits are sent.
- `set savings rate <percentage>` — Adjust what portion of profits to save.
- `check savings` — View current savings balance and stats.
- `sync` — Pull latest profit data from your trading activity.
- `sweep` — Manually trigger a profit sweep to savings.

### Tasks
- `broadcast task: <description> for <amount> $JOSEPH` — Create a new community task.
- `submit task <taskId> with proof <x link>` — Submit completion proof for a task.
- `list submissions for task <taskId>` — View all pending submissions for your task.
- `confirm task <taskId> for <userAddress>` — Approve a submission and release rewards.

## Supported Assets

- **Chain**: Base
- **Savings Token**: USDC (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`)
- **Reward Token**: $JOSEPH (`0x178bed16e462209176d11592b7d1f81e4f803ba3`)

## Installation

Joseph is available as a Bankr skill. Install via:

`install skill joseph`

## Requirements

- Active Bankr Club membership.
- Base chain enabled.
- $JOSEPH balance for task rewards.

## License

MIT
