A minimal, production-ready Rust reference demonstrating atomic composable flash loans on **Save Finance** (formerly Solend) using `solana-sdk` 2.x and Versioned Transactions (`v0`).

## Overview

Unlike legacy CPI callback patterns, composable flash loans on Save Finance utilize instructions introspection via `sysvar::instructions`:
1. **`FlashBorrowReserveLiquidity` (#19)** — Borrows reserve liquidity.
2. **Arbitrage / Swaps** — Arbitrary business logic executed between borrow and repay.
3. **`FlashRepayReserveLiquidity` (#20)** — Repays the borrowed amount + protocol fees, introspecting the earlier borrow instruction.

### Key Considerations Solved:
- **Dynamic Introspection Indexing:** The repay instruction requires the exact 0-based index of the matching borrow instruction (`borrow_instruction_index`). This implementation dynamically computes the index regardless of prepended Compute Budget or setup instructions.
- **Fee Receiver Layout:** Handles the protocol's requirement of using the Associated Token Account (ATA) of the reserve fee receiver rather than the wallet pubkey (`0xd / InvalidAccountInput`).

## Quickstart

### Prerequirements
- [Rust](https://rust-lang.org/tools/install/) 
- A funded Solana keypair located at `~/.config/solana/id.json`  

### 1. Clone repository
```bash
git clone https://github.com/TrinityWeb3/save-flash-loan-rust.git
cd save-flash-loan-rust
```

### Simulation transaction

```bash
cargo run
```

### TxID
Solscan: [4ZTVMEjBnjDRHtjF14nd8jQ44aksN9Ln6nReP8TnhYfLYwrpeXeXgpB5An1zqrFBapTPL7EXSurfU1VxxuCLE1Rz](https://solscan.io/tx/4ZTVMEjBnjDRHtjF14nD8jQ44aksN9Ln6nReP8TnhYfLYwrpeXeXgpB5An1zqrFBapTPL7EXSurfU1VxxuCLe1Rz)
