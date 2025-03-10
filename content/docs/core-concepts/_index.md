---
title: "Core Concepts"
description: "Understanding the fundamental concepts of Doxa"
---

# Core Concepts

This section covers the fundamental concepts you need to understand when building on Doxa.

## Internet Computer Basics

| Concept        | Ethereum Equivalent | IC Implementation        |
|----------------|---------------------|--------------------------|
| Gas            | ETH                 | Cycles (XDR-pegged)      |
| Smart Contract | Solidity Contract   | Motoko Canister          |
| Wallet         | MetaMask            | Plug Wallet + NFID       |

## Authentication Flow

```
Developer -> DApp: Initiate auth
DApp -> NFID: Redirect auth request
NFID -> User: Biometric auth
User -> NFID: Approve
NFID -> DApp: Return principal ID
DApp -> Canister: Call with auth header
```

## Canisters

Canisters are the smart contracts of the Internet Computer. They are:

- Autonomous
- Deterministic
- Upgradeable
- Interoperable

Example of a basic canister:

```motoko
actor {
    private var state = 0;
    
    public func increment() : async Nat {
        state += 1;
        state
    };
    
    public query func get() : async Nat {
        state
    };
}
```

## Cycles

Cycles are the fuel that powers your canisters on the IC. They:

- Are pegged to XDR (Special Drawing Rights)
- Can be transferred between canisters
- Need to be managed carefully

Check cycle balance:

```motoko
public query func get_cycle_balance() : async Nat {
    Cycles.balance()
};
```

## Principal IDs

Principal IDs are unique identifiers for:

- Canisters
- Users
- Subnets

They look like this: `xeka7-7iaaa-aaaab-qabra-cai`

## Candid

Candid is the interface description language for the Internet Computer. It:

- Defines canister interfaces
- Handles data serialization
- Provides type safety

Example Candid interface:

```candid
type Result = variant {
    Ok: nat64;
    Err: text;
};

service : {
    create_pool: (Principal, Principal) -> (Result);
}
``` 