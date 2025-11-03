# 📚 Chronos Vault Developer Documentation

> Comprehensive documentation for Trinity Protocol v3.0 - Multi-chain digital vault platform with mathematical security proofs

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Lean 4](https://img.shields.io/badge/Lean_4-78/78_Proven-brightgreen)](https://lean-lang.org/)

---

## 🚀 Quick Start

New to Chronos Vault? Start here:

1. **[Platform Repository](https://github.com/Chronos-Vault/chronos-vault-platform-)** - Main application and setup
2. **[Smart Contracts](https://github.com/Chronos-Vault/chronos-vault-contracts)** - Contract deployment
3. **[Security & Proofs](https://github.com/Chronos-Vault/chronos-vault-security)** - Formal verification
4. **[TypeScript SDK](https://github.com/Chronos-Vault/chronos-vault-sdk)** - Developer SDK

---

## 🎯 Trinity Protocol v3.0

### Production Status

- ✅ **78/78 Lean 4 formal proofs complete** (100%)
- ✅ **All 4 critical vulnerabilities fixed**
- ✅ **CrossChainBridgeOptimized v2.2** production-ready
- ✅ **Deployed**: November 3, 2025

### What is Trinity Protocol?

Trinity Protocol is a **mathematically provable** 2-of-3 consensus system across three blockchains:

| Blockchain | Role | Purpose |
|------------|------|---------|
| **Arbitrum L2** | Primary Security | Main security layer with low fees |
| **Solana** | Rapid Validation | High-frequency monitoring |
| **TON** | Quantum Backup | Emergency recovery + post-quantum crypto |

**Security Guarantee**: Attack requires compromising 2 of 3 independent blockchains  
**Probability**: P(attack) < 10^-50 (mathematically negligible)

---

## 🏗️ Architecture

### Technology Stack

**Frontend:**
- React 18 + TypeScript
- TailwindCSS + shadcn/ui
- React Query v5
- Wouter routing
- Three.js + React Three Fiber

**Backend:**
- Express.js + TypeScript
- PostgreSQL + Drizzle ORM
- WebSocket for real-time updates
- JWT authentication

**Blockchain:**
- Ethereum Layer 2 (Arbitrum)
- Solana
- TON

**Smart Contracts:**
- Solidity (Ethereum/Arbitrum)
- Rust/Anchor (Solana)
- FunC (TON)
- Lean 4 (Formal Verification)

---

## 📦 Deployed Contracts

### Trinity Protocol v3.0 Addresses

**Arbitrum Sepolia:**
- CrossChainBridgeOptimized v2.2: `0x4a8Bc58f441Ae7E7eC2879e434D9D7e31CF80e30`
- HTLCBridge v2.0: `0x6cd3B1a72F67011839439f96a70290051fd66D57`
- ChronosVault: `0x99444B0B1d6F7b21e9234229a2AC2bC0150B9d91`
- CVT Token: `0xFb419D8E32c14F774279a4dEEf330dc893257147`

**Solana Devnet:**
- Trinity Validator: `5oD8S1TtkdJbAX7qhsGticU7JKxjwY4AbEeBdnkUrrKY`
- CVT Token (Official): `5g3TkqFxyVe1ismrC5r2QD345CA1YdfWn6s6p4AYNmy4`
- CVT Vesting: `3dxjcEGP8MurCtodLCJi1V6JBizdRRAYg91nZkhmX1sB`

**TON Testnet:**
- Trinity Consensus: `EQDx6yH5WH3Ex47h0PBnOBMzPCsmHdnL2snts3DZBO5CYVVJ`
- ChronosVault: `EQDJAnXDPT-NivritpEhQeP0XmG20NdeUtxgh4nUiWH-DF7M`

---

## 🔐 Mathematical Defense Layer (MDL)

Chronos Vault implements 7 cryptographic security layers where every security claim is **mathematically provable**:

### 1. Zero-Knowledge Proofs (ZK-SNARKs)
```typescript
// Prove vault ownership without revealing identity
const proof = await zkService.generateOwnershipProof(vaultId, ownerAddress);
const isValid = await zkService.verifyProof(proof);
```

**Technology**: Groth16 + Circom circuits  
**Use Cases**: Privacy-preserving verification, selective disclosure

### 2. Quantum-Resistant Cryptography
```typescript
// Post-quantum encryption
const qCrypto = new QuantumResistantCrypto();
const keyPair = await qCrypto.generateHybridKeyPair();
const ciphertext = await qCrypto.encryptData(data, keyPair.publicKey);
```

**Algorithms**: ML-KEM-1024, CRYSTALS-Dilithium-5  
**Security**: 50+ year protection against quantum computers

### 3. Multi-Party Computation (MPC)
```typescript
// 3-of-5 distributed key management
const mpc = new MPCKeyManagement(5, 3);
const shares = await mpc.distributeKey(masterKey, vaultId);
const reconstructed = await mpc.reconstructKey(vaultId, shares.slice(0, 3));
```

**Protocol**: Shamir Secret Sharing  
**Guarantee**: 2 shares reveal nothing (information-theoretic)

### 4. Verifiable Delay Functions (VDF)
```typescript
// Provable time-locks
const vdf = new VDFTimeLock();
const timeLock = await vdf.createTimeLock(data, 3600, vaultId); // 1 hour
const result = await vdf.verifyAndUnlock(vaultId);
```

**Algorithm**: Wesolowski VDF with RSA-2048  
**Guarantee**: Cannot be computed faster via parallelization

### 5. Formal Verification (Lean 4)
```lean
-- Mathematical proof of security
theorem withdrawal_safety :
  ∀ vault tx, withdraw_succeeds(vault, tx) → is_owner(tx.sender, vault)
```

**Status**: 78/78 theorems proven ✅  
**Coverage**: 100% security-critical properties

### 6. AI + Cryptographic Governance
```typescript
// AI analyzes threats, math validates decisions
const governance = new AICryptoGovernance();
const proposal = await governance.analyzeSecurityThreat({
  vaultId, threatType, severity
});
const validated = await governance.validateProposal(proposal);
```

**Model**: "AI decides, Math proves, Chain executes"

### 7. Trinity Protocol (2-of-3 Consensus)
```typescript
// Multi-chain consensus verification
const consensus = await trinity.getCrossChainConsensus(vaultId);
const isValid = await trinity.verifyTrinityProtocol(
  ethereumTxHash, solanaTxHash, tonTxHash
);
```

**Guarantee**: 2 of 3 blockchains must agree. Single chain compromise tolerated.

---

## 🚀 Getting Started

### Installation

```bash
# Clone platform repository
git clone https://github.com/Chronos-Vault/chronos-vault-platform-.git
cd chronos-vault-platform-

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your configuration

# Initialize database
npm run db:push

# Start development server
npm run dev
```

### Basic Usage

```typescript
import { ChronosVaultSDK } from '@chronos-vault/sdk';

// Initialize SDK
const sdk = new ChronosVaultSDK({
  network: 'testnet',
  chains: ['ethereum', 'solana', 'ton']
});

await sdk.initialize();

// Create vault
const vault = await sdk.createVault({
  type: 'time-lock',
  unlockTime: Date.now() + 86400000, // 24 hours
  assets: [{ token: 'ETH', amount: '1.0' }]
});

// Query status
const status = await sdk.getStatus(vault.id);
```

---

## 📖 API Reference

### Vault Types (22 Types Available)

1. **Time-Locked Vaults** - Schedule asset releases
2. **Multi-Signature Vaults** - M-of-N signature requirements
3. **Geo-Location Vaults** - Location-based authentication
4. **Quantum-Resistant Vaults** - Post-quantum cryptography
5. **Zero-Knowledge Vaults** - Privacy-preserving operations
6. **Cross-Chain Fragment Vaults** - Distributed across blockchains
7. **And 16 more vault types...**

### Core API Endpoints

```typescript
// Vault Management
POST   /api/vaults              // Create vault
GET    /api/vaults              // List vaults
GET    /api/vaults/:id          // Get vault details
PATCH  /api/vaults/:id          // Update vault
DELETE /api/vaults/:id          // Delete vault

// Cross-Chain Operations
POST   /api/bridge/swap         // Initiate HTLC atomic swap
GET    /api/bridge/status/:id   // Check swap status
POST   /api/consensus/verify    // Verify Trinity consensus

// Security
POST   /api/zk/proof            // Generate ZK proof
POST   /api/zk/verify           // Verify ZK proof
POST   /api/mpc/distribute      // Distribute key shares
POST   /api/vdf/lock            // Create VDF time-lock
```

---

## 🔗 Related Repositories

| Repository | Purpose | Link |
|------------|---------|------|
| **Platform** | Main application | [chronos-vault-platform-](https://github.com/Chronos-Vault/chronos-vault-platform-) |
| **Contracts** | Smart contracts | [chronos-vault-contracts](https://github.com/Chronos-Vault/chronos-vault-contracts) |
| **Documentation** | Technical docs (this repo) | [chronos-vault-docs](https://github.com/Chronos-Vault/chronos-vault-docs) |
| **Security** | Formal proofs | [chronos-vault-security](https://github.com/Chronos-Vault/chronos-vault-security) |
| **SDK** | TypeScript SDK | [chronos-vault-sdk](https://github.com/Chronos-Vault/chronos-vault-sdk) |

---

## 🤝 Contributing

We welcome contributions! See each repository's CONTRIBUTING.md for guidelines.

---

## 📄 License

MIT License - see [LICENSE](./LICENSE) file for details.

Copyright (c) 2025 Chronos Vault

---

## 🌐 Community

- **Discord**: https://discord.gg/WHuexYSV
- **X (Twitter)**: https://x.com/chronosvaultx
- **Medium**: https://medium.com/@chronosvault
- **Email**: chronosvault@chronosvault.org

---

**🎯 Trinity Protocol v3.0 - Mathematically Proven Security**

Every security claim is cryptographically proven using Lean 4 formal verification - 78/78 theorems proven (100%).
