---
name: Joseph
slug: joseph-savings-agent
description: A financial discipline agent that sweeps a percentage of your trading profits to a designated savings wallet.
tags: [finance, automation, pnl, savings]
permissions: [read:portfolio, read:appdata, write:appdata, prepare:transaction]
---

# Joseph: The Savings Agent

Joseph is a specialized Bankr skill that monitors your trading performance and ensures you save a portion of your realized profits.

## Setup
Before Joseph can start saving for you, you must configure your savings address:
- **Command**: "Joseph, set my savings address to [0x...]"
- **Logic**: Stores the address in `appKV` under `joseph_savings_address`.

## Configuration (User-Specific)
- **Savings Wallet**: Stored in `joseph_savings_address`.
- **Savings Rate**: Default is 50% (stored in `joseph_savings_rate`, default 0.5).
- **Network**: Base
- **Asset**: USDC (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`)

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

## Commands
- "Joseph, set my savings address to [address]" -> Configures the destination.
- "Joseph, set my savings rate to [percentage]" -> Configures how much to save (e.g., 20%).
- "Joseph, how much have I saved?" -> Reports `joseph_pending_savings`.
- "Joseph, sync." -> Runs the profit calculation logic.
- "Joseph, sweep." -> Initiates the transfer of pending savings.
