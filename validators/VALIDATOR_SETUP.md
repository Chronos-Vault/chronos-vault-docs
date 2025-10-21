# Trinity Protocol Multi-Validator Setup Guide

**Version:** 1.0.0  
**Network:** Testnet (Arbitrum Sepolia, Solana Devnet, TON Testnet)  
**Date:** October 21, 2025

---

## Overview

This guide explains how to set up the Trinity Protocol's multi-validator infrastructure for 2-of-3 cross-chain consensus across Ethereum Layer 2 (Arbitrum), Solana, and TON blockchains.

## Validator Architecture

### 2-of-3 Consensus Model

The Trinity Protocol requires validators on **three independent blockchains**:

1. **Ethereum Layer 2 (Arbitrum)** - Primary security layer
2. **Solana** - High-frequency monitoring layer
3. **TON** - Emergency recovery and quantum-safe layer

For any cross-chain operation to be valid, it must receive **2-of-3 confirmations** from validators across different chains.

### Validator Requirements

- **3 validators per chain** (9 total)
- Each validator has a unique signing key
- Validators must be distributed across different entities for true decentralization
- In production: Separate servers/cloud infrastructure for each validator

---

## Generated Validators

### Ethereum/Arbitrum Validators

```
Validator 1: 0x0be8788807DA1E4f95057F564562594D65a0C4f9
Validator 2: 0x0A19B76c3C8FE9C88f910C3212e2B44b5b263E26
Validator 3: 0xCf2847d3c872998F5FbFFD7eCb23e8932E890c2d
```

### Solana Validators

```
Validator 1: Epi28nV2op8hFLN8NVapiUiyW3f8LUtE8A5qDVyY3xET
Validator 2: AXDkesdHyAp7egzYdULGJU9A9Ar2VX1JBogLEqaSiWj8
Validator 3: 5oa3idk9PixR1PuYiiQjkfTuDpZXf4Svi2WipkvPX7Nr
```

### TON Validators

```
Validator 1: 0x1520c281cd057eead87e4671d5affd8df4090a07...
Validator 2: 0x228a35ee2682d359d56661c18765aef68d18015b...
Validator 3: 0xe8c759772e0eb2eb5aba1b9233bccd2c8156531e...
```

---

## Setup Instructions

### 1. Generate Validator Keys

```bash
# Generate fresh validator keys
node scripts/generate-validators.cjs
```

This creates:
- `config/validators.json` - Full validator configuration
- `config/validators.config.cjs` - JavaScript config for deployment
- `config/.env.validators` - Environment variables with private keys

⚠️ **SECURITY WARNING**: Never commit `config/.env.validators` to version control!

### 2. Fund Validator Addresses

Each validator address needs testnet tokens:

#### Ethereum/Arbitrum Sepolia
```bash
# Get testnet ETH from faucets:
# https://sepolia.arbiscan.io/
# https://sepoliafaucet.com/
```

#### Solana Devnet
```bash
# Airdrop SOL to each validator
solana airdrop 2 Epi28nV2op8hFLN8NVapiUiyW3f8LUtE8A5qDVyY3xET --url devnet
solana airdrop 2 AXDkesdHyAp7egzYdULGJU9A9Ar2VX1JBogLEqaSiWj8 --url devnet
solana airdrop 2 5oa3idk9PixR1PuYiiQjkfTuDpZXf4Svi2WipkvPX7Nr --url devnet
```

#### TON Testnet
```bash
# Use TON testnet faucet:
# https://testnet.tonscan.org/
```

### 3. Deploy Multi-Validator Contract

```bash
# Deploy CrossChainBridge with all validators
npx hardhat run scripts/deploy-multi-validator.cjs --network arbitrumSepolia
```

This deploys the contract with:
- 3 Ethereum validators
- 3 Solana validators (mapped to Ethereum addresses for contract)
- 3 TON validators (mapped to Ethereum addresses for contract)
- Emergency controller configured

### 4. Verify Deployment

```bash
# Run integration tests
node scripts/testnet-integration.cjs
```

Expected output:
- ✅ All validator counts correct (3 per chain)
- ✅ Circuit breaker inactive (healthy state)
- ✅ Anomaly detection metrics initialized

---

## Validator Key Management

### Storage Options

#### Development/Testnet
- Local `.env` file (add to `.gitignore`)
- Environment variables in deployment pipeline

#### Production
- **AWS KMS** - Key Management Service
- **HashiCorp Vault** - Secret management
- **Google Cloud KMS** - Key management
- **Hardware Security Modules (HSM)** - Physical key storage

### Key Rotation Schedule

| Environment | Rotation Frequency | Method |
|-------------|-------------------|--------|
| Development | 30 days | Manual |
| Testnet | 60 days | Manual |
| Production | 90 days | Automated |

### Key Rotation Process

1. Generate new validator keys
2. Deploy updated contract or call `updateValidators()` function
3. Wait for grace period (24 hours)
4. Deactivate old keys
5. Securely delete old private keys
6. Update monitoring systems

---

## Cross-Chain Proof Submission

### How It Works

1. **User creates operation** on Arbitrum (CrossChainBridge)
2. **Validators monitor** the operation on their respective chains
3. **Each validator submits proof** using `submitChainProof()`
4. **2-of-3 consensus required** for operation validity
5. **Operation executes** once threshold met

### Example: Cross-Chain Transfer

```javascript
// 1. Create operation on Arbitrum
const tx = await bridge.createOperation(
  0, // OperationType.TRANSFER
  "solana",
  ETH_ADDRESS,
  amount,
  false, // prioritizeSpeed
  true,  // prioritizeSecurity
  100    // 1% slippage
);

// 2. Get operation ID
const receipt = await tx.wait();
const operationId = receipt.logs[0].args.operationId;

// 3. Validators submit proofs (automated in production)
// Ethereum validator signs
await bridge.connect(ethereumValidator).submitChainProof(
  operationId,
  1, // ETHEREUM_CHAIN_ID
  proof
);

// Solana validator signs
await bridge.connect(solanaValidator).submitChainProof(
  operationId,
  2, // SOLANA_CHAIN_ID
  proof
);

// 4. Operation now has 2/3 consensus ✅
```

---

## Monitoring & Alerts

### Key Metrics to Monitor

1. **Validator Health**
   - Uptime percentage
   - Response time to operations
   - Gas balance on each chain

2. **Circuit Breaker Status**
   - Active/inactive state
   - Trigger frequency
   - Auto-recovery time

3. **Anomaly Detection**
   - Same-block operation count
   - Volume spikes (24h window)
   - Proof failure rate

4. **Cross-Chain Consensus**
   - Average time to 2-of-3 consensus
   - Failed consensus attempts
   - Validator disagreements

### Alerting Thresholds

| Metric | Warning | Critical |
|--------|---------|----------|
| Validator downtime | > 5% | > 10% |
| Circuit breaker triggers | > 3/day | > 10/day |
| Failed proofs | > 1% | > 5% |
| Gas balance | < 0.1 ETH | < 0.01 ETH |

---

## Troubleshooting

### Validator Not Responding

**Problem**: Validator isn't submitting proofs

**Solutions**:
1. Check validator key is funded (gas for transactions)
2. Verify validator is registered on contract
3. Check RPC endpoint connectivity
4. Review validator logs for errors

### Circuit Breaker Triggered

**Problem**: Circuit breaker activated, blocking operations

**Causes**:
- Volume spike detected (> 10x normal in 1 hour)
- High proof failure rate (> 10% in 1 hour)
- Same-block attack detected

**Resolution**:
```javascript
// Emergency controller can manually resume
await bridge.emergencyResume();

// Or wait for auto-recovery (1 hour)
```

### 2-of-3 Consensus Not Reached

**Problem**: Operation stuck with only 1/3 confirmations

**Solutions**:
1. Check all 3 validator nodes are running
2. Verify cross-chain proof format is correct
3. Ensure validators have latest block data
4. Review operation parameters (valid chains, amounts)

---

## Security Best Practices

### Validator Isolation

- Run each validator on separate infrastructure
- Use different cloud providers for true decentralization
- Implement network isolation between validators
- Never store multiple validator keys on same server

### Key Security

✅ **DO**:
- Use hardware wallets for production validators
- Implement multi-signature emergency controls
- Rotate keys every 90 days
- Store backups in encrypted cold storage
- Use environment variables, never hardcode keys

❌ **DON'T**:
- Commit private keys to version control
- Share validator keys between team members
- Store keys in plaintext
- Reuse keys across environments
- Leave test keys in production config

### Network Security

- Use VPN for validator communication
- Implement DDoS protection
- Whitelist validator IP addresses
- Enable rate limiting on RPC endpoints
- Monitor for suspicious activity

---

## Production Deployment Checklist

### Pre-Deployment

- [ ] Generate production validator keys (not test keys!)
- [ ] Store keys in secure vault (AWS KMS/HashiCorp Vault)
- [ ] Fund validator addresses with production tokens
- [ ] Set up monitoring and alerting
- [ ] Configure backup validators (failover)
- [ ] Document emergency procedures

### Deployment

- [ ] Deploy contract with production validators
- [ ] Verify all validators registered correctly
- [ ] Test cross-chain proof submission
- [ ] Validate 2-of-3 consensus execution
- [ ] Run security audit on deployed contract
- [ ] Enable circuit breaker monitoring

### Post-Deployment

- [ ] Monitor for 24 hours continuously
- [ ] Test emergency pause/resume functionality
- [ ] Verify anomaly detection triggers
- [ ] Document all validator addresses publicly
- [ ] Implement key rotation schedule
- [ ] Train incident response team

---

## Additional Resources

- **GitHub Repositories**:
  - Contracts: https://github.com/Chronos-Vault/chronos-vault-contracts
  - Docs: https://github.com/Chronos-Vault/chronos-vault-docs
  - Security: https://github.com/Chronos-Vault/chronos-vault-security

- **Network Explorers**:
  - Arbitrum Sepolia: https://sepolia.arbiscan.io/
  - Solana Devnet: https://explorer.solana.com/?cluster=devnet
  - TON Testnet: https://testnet.tonscan.org/

- **Faucets**:
  - Arbitrum Sepolia: https://sepoliafaucet.com/
  - Solana Devnet: Use `solana airdrop` command
  - TON Testnet: https://testnet.tonscan.org/

---

## Support

For questions or issues:
- Open an issue on GitHub
- Join our Discord community
- Review documentation at chronos-vault-docs

---

**Generated:** October 21, 2025  
**Chronos Vault - Trust Math, Not Humans**
