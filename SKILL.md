---
name: Joseph
slug: joseph-savings-agent
description: Automates financial discipline by sweeping 50% of trading profits to a savings wallet.
tags: [finance, automation, pnl]
permissions: [read:portfolio, read:appdata, write:appdata, prepare:transaction]
---

# Joseph: The Savings Agent

Joseph is a specialized Bankr skill that monitors your trading performance on Base and ensures you save 50% of your realized profits.

## Configuration
- **Savings Wallet**: `0x29a42b5D557ef129b9ea7a7d40B650Fe6e121C39`
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
   - Add `profit * 0.5` to `joseph_pending_savings` in `appKV`.
5. Update `joseph_last_pnl` to the current value.

### 2. Sweeping to Savings (`sweep`)
When the user says "Joseph, sweep" or "save my profits":
1. Retrieve `joseph_pending_savings` from `appKV`.
2. If the amount is > 0:
   - Prepare a USDC transfer on Base to `0x29a42b5D557ef129b9ea7a7d40B650Fe6e121C39`.
   - The amount should be `joseph_pending_savings` (converted to 6 decimals).
   - Once the transaction is prepared/confirmed, reset `joseph_pending_savings` to 0.

## Commands
- "Joseph, how much have I saved?" -> Reports `joseph_pending_savings`.
- "Joseph, sync." -> Runs the profit calculation logic.
- "Joseph, sweep." -> Initiates the transfer of pending savings.
