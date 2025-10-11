# Trinity Protocol Deployment Guide

## Prerequisites

- Node.js 18+ with TypeScript
- Access to deployed contracts on Arbitrum Sepolia, Solana Devnet, TON Testnet
- Environment variables configured

## Environment Configuration

```bash
# Arbitrum Configuration (PRIMARY)
ETHEREUM_NETWORK=arbitrum
ARBITRUM_RPC_URL=https://arbitrum-sepolia-rpc.publicnode.com
ARBITRUM_VAULT_ADDRESS=0x99444B0B1d6F7b21e9234229a2AC2bC0150B9d91
ARBITRUM_CVT_BRIDGE_ADDRESS=0x21De95EbA01E31173Efe1b9c4D57E58bb840bA86
ARBITRUM_BRIDGE_ADDRESS=0x13dc7df46c2e87E8B2010A28F13404580158Ed9A

# Solana Configuration (MONITOR)
SOLANA_RPC_URL=https://api.devnet.solana.com
SOL_VAULT_PROGRAM_ID=CYaDJYRqm35udQ8vkxoajSER8oaniQUcV8Vvw5BqJyo2

# TON Configuration (BACKUP)
TON_CHRONOS_VAULT_ADDRESS=EQDJAnXDPT-NivritpEhQeP0XmG20NdeUtxgh4nUiWH-DF7M
TON_CVT_BRIDGE_ADDRESS=EQAOJxa1WDjGZ7f3n53JILojhZoDdTOKWl6h41_yOWX3v0tq
```

## Installation

### 1. Install Dependencies

```bash
npm install
```

### 2. Initialize Trinity Protocol

```typescript
import { trinityStateCoordinator } from './services/trinity-state-coordinator';
import { circuitBreakerService } from './services/circuit-breaker-service';
import { emergencyRecoveryProtocol } from './services/emergency-recovery-protocol';

// Initialize Trinity Protocol
await trinityStateCoordinator.initialize();

// Start cross-chain state synchronization
await trinityStateCoordinator.start();

// Start circuit breaker monitoring
await circuitBreakerService.startMonitoring();

// Start emergency recovery protocol
await emergencyRecoveryProtocol.start();
```

### 3. Listen to Trinity Protocol Events

```typescript
// Listen to consensus events
trinityStateCoordinator.on('consensus:reached', (data) => {
  console.log(`✅ Consensus reached for vault ${data.vaultId}`);
});

// Listen to chain failures
circuitBreakerService.on('chain:failed', (data) => {
  console.log(`🚨 Chain failed: ${data.chain}`);
});

// Listen to emergency events
emergencyRecoveryProtocol.on('emergency:activated', (data) => {
  console.log(`🚨 Emergency mode activated`);
});
```

## Component Deployment

### Event Listeners

Event listeners automatically start when `trinityStateCoordinator.start()` is called:

- **Arbitrum Event Listener:** Connects to Arbitrum Sepolia RPC, monitors ChronosVault contract
- **Solana Event Listener:** Connects to Solana Devnet RPC, subscribes to program accounts
- **TON Event Listener:** Polls TON Testnet for contract state changes

### State Coordinator

The Trinity State Coordinator orchestrates all components:

```typescript
// Get vault cross-chain state
const state = trinityStateCoordinator.getVaultState(vaultId);

// Check health of all chains
const health = await trinityStateCoordinator.healthCheck();
```

### Merkle Proof System

Use Merkle proofs for cryptographic state validation:

```typescript
import { merkleProofVerifier } from './services/merkle-proof-verifier';

// Generate proof for a vault
const proof = merkleProofVerifier.generateProof(vaultStates, vaultId);

// Verify proof
const isValid = merkleProofVerifier.verifyProof(proof);

// Generate cross-chain consensus proof
const crossChainProof = merkleProofVerifier.generateCrossChainProof(
  arbitrumStates,
  solanaStates,
  tonStates,
  vaultId
);
```

### Circuit Breaker

Circuit breaker automatically monitors chain health:

```typescript
// Get chain health status
const arbitrumHealth = circuitBreakerService.getChainHealth('arbitrum');
const solanaHealth = circuitBreakerService.getChainHealth('solana');
const tonHealth = circuitBreakerService.getChainHealth('ton');

// Get Trinity Protocol status
const trinityStatus = circuitBreakerService.getTrinityStatus();

// Manually reset circuit
circuitBreakerService.resetCircuit('arbitrum');
```

### Emergency Recovery

Emergency recovery protocol handles chain failures:

```typescript
// Get recovery status
const status = emergencyRecoveryProtocol.getRecoveryStatus();

// Execute emergency vault recovery (requires all 3 chains)
const success = await emergencyRecoveryProtocol.executeEmergencyVaultRecovery(
  vaultId,
  recoveryKey
);
```

## Production Deployment

### 1. Switch to Mainnet

Update environment variables for mainnet:

```bash
# Arbitrum Mainnet
ETHEREUM_NETWORK=arbitrum
ARBITRUM_RPC_URL=https://arb1.arbitrum.io/rpc

# Solana Mainnet
SOLANA_RPC_URL=https://api.mainnet-beta.solana.com

# TON Mainnet
TON_API_URL=https://toncenter.com/api/v2/jsonRPC
```

### 2. Deploy Monitoring

Set up monitoring for Trinity Protocol:

- **Grafana Dashboard:** Monitor chain health, consensus rate, failover events
- **Alert System:** Alert on circuit breaker activation, emergency mode
- **Logging:** Aggregate logs from all event listeners

### 3. Configure Auto-Recovery

Adjust circuit breaker thresholds for production:

```typescript
// In circuit-breaker-service.ts
private readonly FAILURE_THRESHOLD = 5; // Increase for production
private readonly TIMEOUT_THRESHOLD = 10000; // 10 seconds for production
private readonly RESET_TIMEOUT = 60000; // 1 minute for production
```

### 4. Enable Emergency Notifications

Set up notifications for emergency events:

```typescript
emergencyRecoveryProtocol.on('emergency:activated', async (data) => {
  // Send alerts to ops team
  await sendSlackNotification(`🚨 Emergency mode activated: ${data.failedChains.length} chains failed`);
  await sendPagerDutyAlert('CRITICAL', 'Trinity Protocol emergency mode');
});
```

## Testing

### Run Integration Tests

```bash
npm test tests/trinity-protocol-integration.test.ts
```

### Test 2-of-3 Consensus

```typescript
import { trinityProtocol } from './security/trinity-protocol';

const result = await trinityProtocol.verifyOperation({
  operationId: 'test-unlock-001',
  operationType: OperationType.VAULT_UNLOCK,
  vaultId: 'test-vault-001',
  requester: 'TEST_SYSTEM',
  data: { unlockTime: Date.now() },
  requiredChains: 2
});

console.log('Consensus reached:', result.consensusReached);
```

### Simulate Chain Failure

```typescript
// Manually trigger circuit breaker
circuitBreakerService.on('circuit:open', (data) => {
  console.log(`Circuit opened for ${data.chain}`);
});

// Test emergency recovery
emergencyRecoveryProtocol.on('emergency:activated', (data) => {
  console.log('Emergency mode activated:', data);
});
```

## Troubleshooting

### Event Listener Issues

**Problem:** Events not being detected

**Solution:**
1. Check RPC connection: `curl -X POST <RPC_URL>`
2. Verify contract addresses in environment
3. Check event listener logs for errors
4. Ensure WebSocket connection is open

### Consensus Failures

**Problem:** 2-of-3 consensus not reached

**Solution:**
1. Check chain health: `trinityStateCoordinator.healthCheck()`
2. Verify at least 2 chains are healthy
3. Check circuit breaker status
4. Review Merkle proof verification logs

### Circuit Breaker Stuck Open

**Problem:** Circuit breaker not resetting

**Solution:**
1. Manually reset: `circuitBreakerService.resetCircuit('arbitrum')`
2. Check chain RPC availability
3. Verify timeout thresholds
4. Review failure logs

### Emergency Recovery Not Activating

**Problem:** Emergency mode not triggering

**Solution:**
1. Check if 2+ chains failed
2. Verify emergency recovery protocol is started
3. Review circuit breaker events
4. Check recovery attempt logs

## Monitoring & Metrics

### Key Metrics to Track

- **Consensus Rate:** % of operations achieving 2-of-3 consensus
- **Chain Health:** Uptime and response time for each chain
- **Circuit Breaker State:** CLOSED/OPEN/HALF_OPEN status
- **Recovery Attempts:** Number of automated recovery attempts
- **Emergency Activations:** Count of emergency mode activations

### Dashboard Setup

```typescript
// Export metrics for Prometheus/Grafana
app.get('/metrics/trinity-protocol', (req, res) => {
  const trinityStatus = circuitBreakerService.getTrinityStatus();
  const recoveryStatus = emergencyRecoveryProtocol.getRecoveryStatus();
  
  res.json({
    healthy_chains: trinityStatus.healthyChains,
    failed_chains: trinityStatus.failedChains,
    recovery_mode: recoveryStatus.mode,
    can_operate: recoveryStatus.canOperate
  });
});
```

## Security Best Practices

1. **Never expose recovery keys in logs**
2. **Use environment variables for all secrets**
3. **Enable HTTPS for all RPC connections**
4. **Implement rate limiting on event listeners**
5. **Regular security audits of emergency recovery logic**
6. **Monitor for unusual consensus patterns**
7. **Test failover scenarios regularly**

## Support

For issues or questions:
- GitHub: https://github.com/Chronos-Vault
- Email: chronosvault@chronosvault.org
- Documentation: https://github.com/Chronos-Vault/docs

---

**Last Updated:** October 11, 2025
