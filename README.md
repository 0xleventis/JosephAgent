oseph


A financial discipline agent that automatically sweeps a percentage of your trading profits to a designated savings wallet.



Overview

Joseph helps you build wealth by enforcing disciplined profit-taking. After each trade, Joseph calculates your realized profit and automatically sweeps a configurable percentage into a separate savings wallet — so you actually keep what you earn instead of reinvesting everything.


Features


Profit Syncing: Tracks realized profits from your trading activity on Base

Automatic Sweeping: Moves a percentage of profits to your savings wallet

Configurable Rate: Set your savings rate (default: 50%)

Custom Savings Address: Designate any wallet as your savings destination

USDC-Native: Operates in USDC on Base for stability and low fees


Configuration

Setting	Description	Default
Savings Rate	Percentage of profits to sweep	50%
Savings Address	Destination wallet for swept profits	None (must be set)

Commands


set savings address <wallet> — Configure where profits are sent

set savings rate <percentage> — Adjust what portion of profits to save

check savings — View current savings balance and stats

sync — Pull latest profit data from your trading activity

sweep — Manually trigger a profit sweep to savings


Supported Assets


Chain: Base

Token: USDC (0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913)


How It Works


Trade — Execute your normal trades on Base

Sync — Joseph reads your realized profits

Calculate — Applies your configured savings rate

Sweep — Transfers the savings portion to your designated wallet


Example Workflow

> set savings address 0x1234...abcd
Savings address set to 0x1234...abcd

> set savings rate 40
Savings rate set to 40%

> sync
Synced 3 trades. Realized profit: +$1,240 USDC

> sweep
Swept $496 USDC (40%) to savings wallet.
Savings balance: $2,847 USDC

Installation

Joseph is available as a Bankr skill. Install via:


install skill joseph

Requirements


Active Bankr Club membership

Base chain enabled

USDC balance for sweep transactions

Savings wallet address configured


License

MIT



Choose 
