---
title: "Doxa Developer Documentation"
description: "Complete guide for developers building on Doxa"
---

# Welcome to Doxa Developer Documentation

Doxa is a multi-stablecoin platform built on the Internet Computer (IC). This documentation will help you integrate with our platform and build amazing applications.

## Quick Links

- [Getting Started](/docs/getting-started)
- [Core Concepts](/docs/core-concepts)
- [API Reference](/docs/api-reference)
- [Guides](/docs/guides)
- [Troubleshooting](/docs/troubleshooting)

## Platform Stack

- **Blockchain**: Internet Computer (IC) Mainnet
- **Smart Contracts**: Motoko Canisters
- **Frontend**: SvelteKit + IC AgentJS
- **Authentication**: Internet Identity & NFID

## Key Components

```motoko
module Constants {
  public let DOXA_CANISTER_ID : Text = "xeka7-7iaaa-...";
  public let CYCLE_FEE : Nat = 10_000; // 0.00001 XDR
}
```

## Getting Started

1. Install IC SDK
2. Set up your development environment
3. Deploy your first canister
4. Start building!

[Start Building →](/docs/getting-started) 