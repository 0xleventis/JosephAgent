---
name: Joseph
slug: joseph-savings-agent
description: A financial discipline agent that sweeps a percentage of your trading profits to a designated savings wallet and manages community tasks.
tags: [finance, automation, pnl, savings, tasks, rewards]
permissions: [read:portfolio, read:appdata, write:appdata, prepare:transaction, read:wallet]
---

# Joseph: The Savings Agent

Joseph is a specialized Bankr skill that monitors your trading performance and ensures you save a portion of your realized profits, while also managing community tasks with $JOSEPH rewards.

## Setup
Before Joseph can start saving for you, you must configure your savings address:
- **Command**: "Joseph, set my savings address to [0x...]"
- **Logic**: Stores the address in `appKV` under `joseph_savings_address`.

## Configuration (User-Specific)
- **Savings Wallet**: Stored in `joseph_savings_address`.
- **Savings Rate**: Default is 50% (stored in `joseph_savings_rate`, default 0.5).
- **Network**: Base
- **Asset**: USDC (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`)
- **Reward Token**: $JOSEPH (`0x178bed16e462209176d11592b7d1f81e4f803ba3`)

## Operational Logic

### 1. Syncing Profits (`sync`)
When the user asks to "sync Joseph" or "check my savings":
1. Call `bankr.wallet.balances` for the `base` chain with `include: { pnl: true }`.
2. Retrieve `totalRealizedUsd` from the response.
3. Compare this value with `joseph_last_pnl` stored in `appKV`.
4. If `currentRealized > joseph_last_pnl`:
   - Calculate `profit = currentRealized - joseph_last_pnl`.
   - Get `rate` from `joseph_savings_rate` (default 0.5).
   - Add `profit * rate` to `joseph_pending_savings` in `appKV`.
5. Update `joseph_last_pnl` to the current value.

### 2. Sweeping to Savings (`sweep`)
When the user says "Joseph, sweep" or "save my profits":
1. Retrieve `joseph_pending_savings` and `joseph_savings_address` from `appKV`.
2. If no address is set, prompt the user to run the setup command.
3. If the amount is > 0:
   - Prepare a USDC transfer on Base to the stored `joseph_savings_address`.
   - The amount should be `joseph_pending_savings` (converted to 6 decimals).
   - Once the transaction is prepared/confirmed, reset `joseph_pending_savings` to 0.

### 3. Task Management
Joseph allows users to broadcast tasks and reward contributors in $JOSEPH.

#### Broadcasting Tasks
- **Command**: "Joseph, broadcast task: [description] for [amount] $JOSEPH"
- **Logic**: Creates a unique `taskId`, stores task details (creator, description, reward) in `appKV` under `joseph_tasks/${taskId}`.

#### Submitting Proof
- **Command**: "Joseph, submit task [taskId] with proof [x link]"
- **Logic**: Stores the submission (user address, proof link, status: pending) in `appKV` under `joseph_submissions/${taskId}/${userAddress}`.

#### Reviewing & Confirming
- **Command**: "Joseph, list submissions for task [taskId]"
- **Logic**: Lists all submissions for a specific task.
- **Command**: "Joseph, confirm task [taskId] for [userAddress]"
- **Logic**: 
  1. Verifies the caller is the task creator.
  2. Updates submission status to `confirmed`.
  3. Prepares a $JOSEPH transfer from the creator to the contributor.

## Commands
- "Joseph, set my savings address to [address]" -> Configures the destination.
- "Joseph, set my savings rate to [percentage]" -> Configures how much to save.
- "Joseph, how much have I saved?" -> Reports `joseph_pending_savings`.
- "Joseph, sync." -> Runs the profit calculation logic.
- "Joseph, sweep." -> Initiates the transfer of pending savings.
- "Joseph, broadcast task: [description] for [amount] $JOSEPH" -> Posts a new task.
- "Joseph, submit task [taskId] with proof [x link]" -> Submits completion proof.
- "Joseph, list submissions for task [taskId]" -> Shows pending submissions.
- "Joseph, confirm task [taskId] for [userAddress]" -> Approves and pays reward.
