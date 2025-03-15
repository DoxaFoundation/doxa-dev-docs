---
title: "Staking Integration"
linkTitle: "Staking Integration"
weight: 3
description: "Guide for integrating with Doxa Protocol's staking system"
---

# Staking Integration

This guide covers the staking mechanisms in the Doxa Protocol.

## Overview

Doxa Protocol offers flexible staking options with different durations and reward structures. Users can stake tokens to earn rewards and participate in protocol governance.

## Staking Configuration

### Duration Parameters
```motoko
MIN_LOCK_DURATION = 30 days
MAX_LOCK_DURATION = 365 days
MIN_STAKE_AMOUNT = 100 tokens
```

### Reward Tiers
| Duration | Weight | APY Range |
|----------|--------|-----------|
| 30 days  | 1x     | 3-5%      |
| 90 days  | 2x     | 6-8%      |
| 180 days | 3x     | 9-12%     |
| 365 days | 4x     | 15-18%    |

## Staking Functions

### Initialize Stake
```motoko
public shared ({ caller }) func stake(
    amount: Nat,
    duration: Nat
) : async Result<StakeId, Text>
```

#### Parameters
- `amount`: Number of tokens to stake
- `duration`: Staking period in days

#### Example Usage
```motoko
let stakeResult = await Staking.stake({
    amount = 1_000_000; // 1000 tokens
    duration = 90; // 90 days
});
```

### Harvest Rewards
```motoko
public shared ({ caller }) func harvestRewards(
    stakeId: StakeId
) : async Result<Nat, Text>
```

### Check Stake Status
```motoko
public query func getStakeInfo(
    stakeId: StakeId
) : async ?StakeInfo
```

## Reward Calculation

### Weekly Rewards Formula
```motoko
weeklyReward = (stakedAmount * weight * rewardRate) / totalStakedValue
```

### APY Calculation
```motoko
effectiveAPY = baseRate * weightMultiplier * (1 + bonusMultiplier)
```

## Governance Integration

### Voting Power
```motoko
votingPower = stakedAmount * stakeDuration / maxDuration
```

### Proposal Creation
```motoko
public shared ({ caller }) func createProposal(
    proposal: ProposalData
) : async Result<ProposalId, Text>
```

## Error Handling

Common staking errors:
```motoko
type StakingError = variant {
    InsufficientBalance;
    InvalidDuration;
    StakeNotFound;
    RewardNotReady;
    UnauthorizedCaller;
};
```

## Best Practices

1. **Stake Management**
   - Monitor reward accrual
   - Plan stake duration carefully
   - Consider governance participation

2. **Risk Management**
   - Understand lock periods
   - Diversify stake durations
   - Keep reserve for emergencies

3. **Optimization**
   - Compound rewards regularly
   - Time stakes with reward cycles
   - Consider gas costs

4. **Security**
   - Verify transactions
   - Use secure wallets
   - Keep private keys safe 