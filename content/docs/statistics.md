# Statistics and Data

This guide explains how to access and analyze statistical data from the Doxa Protocol.

## Overview

The Doxa Protocol provides comprehensive statistics and analytics to monitor protocol health, user activity, and market conditions.

## Protocol Statistics

### TVL (Total Value Locked)
```motoko
public query func getTVL() : async {
    total: Nat;
    byAsset: [(Text, Nat)];
    byPool: [(Principal, Nat)];
}
```

### Volume Statistics
```motoko
public query func getVolumeStats() : async {
    total24h: Nat;
    totalWeek: Nat;
    totalMonth: Nat;
    byPool: [(Principal, Nat)];
}
```

## Market Data

### Price Feeds
```motoko
public query func getPrices() : async [(Text, Float)] {
    // Returns current prices for all supported assets
}
```

### Historical Data
```motoko
public query func getPriceHistory(
    asset: Text,
    startTime: Time,
    endTime: Time
) : async [PricePoint]
```

## Pool Analytics

### Pool Statistics
```motoko
type PoolStats = {
    tvl: Nat;
    volume24h: Nat;
    fees24h: Nat;
    apy: Float;
    totalTrades: Nat;
};
```

### Liquidity Analysis
```motoko
public query func getLiquidityDepth(
    poolId: Principal
) : async [(Float, Nat)]
```

## User Analytics

### Account Statistics
```motoko
type AccountStats = {
    totalValue: Nat;
    positions: [Position];
    rewards: Nat;
    volume24h: Nat;
};
```

### Position Tracking
```motoko
public query func getPositionStats(
    positionId: PositionId
) : async PositionStats
```

## System Metrics

### Performance Stats
```motoko
public query func getSystemMetrics() : async {
    tps: Float;
    latency: Nat;
    successRate: Float;
    uptime: Float;
}
```

### Health Indicators
```motoko
public query func getHealthMetrics() : async {
    collateralization: Float;
    liquidityUtilization: Float;
    riskMetrics: RiskStats;
}
```

## Data Export

### CSV Export
```motoko
public func exportData(
    dataType: ExportType,
    timeRange: TimeRange
) : async Blob
```

### API Integration
```motoko
public query func getDataFeed(
    feed: FeedType
) : async Stream<DataPoint>
```

## Analytics Tools

### 1. Dashboard Components
- TVL Charts
- Volume Graphs
- Price Charts
- Position Tables

### 2. Reporting Tools
- Daily Reports
- Weekly Summaries
- Monthly Analytics
- Custom Reports

## Integration Examples

### 1. Basic Stats Display
```motoko
// Get basic protocol stats
let stats = await analytics.getBasicStats();
```

### 2. Custom Analytics
```motoko
// Create custom analytics query
let customStats = await analytics.queryBuilder()
    .addMetric("tvl")
    .addDimension("time")
    .setTimeRange(start, end)
    .execute();
```

## Best Practices

### 1. Data Handling
- Cache frequently accessed data
- Implement rate limiting
- Handle large datasets efficiently

### 2. Performance Optimization
- Use pagination
- Implement data aggregation
- Optimize query patterns

### 3. Data Accuracy
- Verify data sources
- Implement validation
- Handle edge cases

### 4. Security
- Protect sensitive data
- Implement access controls
- Monitor usage patterns

## Common Use Cases

### 1. Portfolio Tracking
- Position monitoring
- Performance analysis
- Risk assessment

### 2. Market Analysis
- Trend identification
- Volume analysis
- Price discovery

### 3. Risk Management
- Collateral monitoring
- Liquidation risk
- Market exposure 