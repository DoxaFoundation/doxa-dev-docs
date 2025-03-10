---
title: "Getting Started"
description: "Quick start guide for Doxa development"
---

# Getting Started with Doxa

This guide will help you set up your development environment and create your first Doxa application.

## Prerequisites

- Node.js 16 or higher
- Git
- Basic understanding of blockchain concepts

## Installation

1. Install the Internet Computer SDK:

```bash
sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
```

2. Clone the Doxa repository:

```bash
git clone https://github.com/doxa-fi/core
cd core
```

3. Install dependencies:

```bash
npm install
```

## Local Development

1. Start the local network:

```bash
dfx start --background
```

2. Deploy the canisters:

```bash
dfx deploy
```

3. Start the development server:

```bash
npm run dev
```

## Your First Transaction

Here's a simple example of creating a new USDC swap pool:

```motoko
public shared(msg) func create_pool(
  token0 : Principal, 
  token1 : Principal
) : async Result.Result<Text, Text> {
  let pool = await SwapCanister.create({
    token0 = token0;
    token1 = token1;
    fee = Constants.CYCLE_FEE;
  });
  Debug.print("Pool created: " # debug_show(pool));
};
```

## Next Steps

- Learn about [Core Concepts](/docs/core-concepts)
- Explore the [API Reference](/docs/api-reference)
- Check out our [Guides](/docs/guides) 