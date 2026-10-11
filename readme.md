[![Solana](https://img.shields.io/badge/Solana-2.x-14F195?style=flat-square&logo=solana)](https://solana.com)
[![Rust](https://img.shields.io/badge/Rust-1.75+-CE412B?style=flat-square&logo=rust)](https://www.rust-lang.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)

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

### Prerequisites
- [Rust](https://rustup.rs/) (edition 2021+)
- A funded Solana keypair located at `~/.config/solana/id.json`

### Simulation (Default & Safe)

By default, running the project performs an end-to-end atomic simulation against Mainnet without risking user funds:

```bash
cargo run
```

Expected output:

Signer: 
Simulating transaction...
Simulation succeeded


### Verification
Live execution signature: [4ZTVMEjBnjDRHtjF14nd8jQ44aksN9Ln6nReP8TnhYfLYwrpeXeXgpB5An1zqrFBapTPL7EXSurfU1VxxuCLE1Rz](https://solscan.io/tx/4ZTVMEjBnjDRHtjF14nD8jQ44aksN9Ln6nReP8TnhYfLYwrpeXeXgpB5An1zqrFBapTPL7EXSurfU1VxxuCLe1Rz)