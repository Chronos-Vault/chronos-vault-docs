# Chronos Vault API Reference

> Complete API documentation for Chronos Vault - Trinity Protocol multi-chain security vaults

**Base URL**: `https://api.chronosvault.com/v1`

**Blockchain Architecture**:
- **Arbitrum L2 (Primary)**: Main security layer on Arbitrum Sepolia
- **Solana (Monitor)**: High-frequency validation  
- **TON (Backup)**: Quantum-resistant recovery
- **Bitcoin**: Read-only for Halving Vault feature

## Authentication
Bearer token required for authenticated endpoints:
```
Authorization: Bearer YOUR_API_TOKEN
```

---

## Bridge & Cross-Chain Operations

### GET /api/bridge/status
Get all cross-chain bridge statuses

### GET /api/bridge/status/:sourceChain/:targetChain
Get specific bridge status between two chains

### POST /api/bridge/initialize
Initialize a cross-chain bridge
**Auth Required**

### POST /api/bridge/transfer
Cross-chain asset transfer
**Auth Required**

### POST /api/bridge/atomic-swap
Create trustless atomic swap
**Auth Required**

### GET /api/bridge/circuit-breaker/status
Real-time V3 circuit breaker status
**Response includes**:
- CrossChainBridgeV3: 0x39601883CD9A115Aba0228fe0620f468Dc710d54
- CVTBridgeV3: 0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0
- EmergencyMultiSig: 0xFafCA23a7c085A842E827f53A853141C8243F924

---

## Vault Creation

### POST /api/vault-creation/time-lock
Create Time-Lock Vault with Trinity Protocol
**Auth Required**

### POST /api/vault-creation/multi-sig
Create Multi-Sig Vault (m-of-n)
**Auth Required**

### POST /api/vault-creation/fragment
Create Cross-Chain Fragment Vault
**Auth Required**

---

## CVT Token

### GET /api/token/data/:chain
Get CVT token data (arbitrum, solana, ton)

### GET /api/token/balance/:chain/:address
Get CVT balance for address

### POST /api/token/stake
Stake CVT tokens
**Auth Required**

---

## Security

### GET /api/security/status
Security dashboard data

### GET /api/security/chain-health
Blockchain health metrics

### POST /api/security/zk-proofs/ownership
Create zero-knowledge ownership proof
**Auth Required**

### POST /api/security/zk-proofs/verify
Verify zero-knowledge proof

### POST /api/security/quantum/encrypt
Quantum-resistant encryption (CRYSTALS-Kyber)

### POST /api/security/quantum/sign
Quantum-resistant signature (CRYSTALS-Dilithium)

---

## Wallet

### POST /api/wallet/connect/:chain
Connect wallet for blockchain

### POST /api/wallet/verify-signature
Verify wallet signature

---

## Authentication

### GET /api/auth/nonce
Request authentication nonce

### POST /api/auth/verify
Verify signature and create session

### GET /api/auth/session
Get current session
**Auth Required**

### POST /api/auth/signout
End session
**Auth Required**

---

## Smart Contract Addresses

### Arbitrum Sepolia Testnet
- CVT Token: `0xFb419D8E32c14F774279a4dEEf330dc893257147`
- CrossChainBridgeV3: `0x39601883CD9A115Aba0228fe0620f468Dc710d54`
- CVTBridgeV3: `0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0`
- EmergencyMultiSig: `0xFafCA23a7c085A842E827f53A853141C8243F924`

### TON Testnet
- Chronos Vault: `EQDJAnXDPT-NivritpEhQeP0XmG20NdeUtxgh4nUiWH-DF7M`
- CVT Bridge: `EQAOJxa1WDjGZ7f3n53JILojhZoDdTOKWl6h41_yOWX3v0tq`

---

## Resources
- GitHub: https://github.com/Chronos-Vault
- Discord: https://discord.gg/WHuexYSV
- X: https://x.com/chronosvaultx

*Last Updated: October 8, 2025*
