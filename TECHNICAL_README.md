# Chronos Vault - Technical README

## Trinity Protocol Architecture

Chronos Vault implements mathematical 2-of-3 consensus across three independent blockchains:

### Blockchain Roles
1. **Arbitrum L2 (PRIMARY)** - Main security layer on Arbitrum Sepolia
2. **Solana (MONITOR)** - High-frequency validation (2000+ TPS)
3. **TON (BACKUP)** - Quantum-resistant emergency recovery
4. **Bitcoin (FEATURE)** - Read-only for Halving Vault tracking

### Security Model
- **2-of-3 Consensus**: Funds safe even if 1 chain compromised
- **Attack Probability**: 10^-18 (requires 2+ chains simultaneously)
- **No Human Validators**: Pure mathematical verification

## V3 Security Features (Deployed Oct 8, 2025)

### Circuit Breakers
**CrossChainBridgeV3** (`0x39601883CD9A115Aba0228fe0620f468Dc710d54`):
- Auto-pause on 500% volume spike
- Auto-pause on 20% proof failure rate
- 4-hour auto-recovery

**CVTBridgeV3** (`0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0`):
- Auto-pause on 500% volume spike
- Auto-pause on 20% signature failure
- 2-hour auto-recovery

### Emergency Controls
**EmergencyMultiSig** (`0xFafCA23a7c085A842E827f53A853141C8243F924`):
- 2-of-3 multi-signature requirement
- 48-hour time-lock on actions
- IMMUTABLE controller (cannot be changed)

## Core Technologies

### Quantum-Resistant Cryptography
- **CRYSTALS-Kyber**: Post-quantum key encapsulation
- **CRYSTALS-Dilithium**: Post-quantum signatures
- **Performance**: 900% improvement with key pooling

### Zero-Knowledge Proofs
- **zk-SNARKs**: Privacy-preserving vault verification
- **Proof Types**: Ownership, multi-sig, metadata
- **Generation Time**: 1.2 seconds (192% faster)

### MEV Protection
- **Commit-Reveal**: Prevents front-running
- **Flashbots**: Private mempool submission
- **Nonce Persistence**: Transaction integrity

## Development Stack

### Frontend
- React.js + TypeScript
- TailwindCSS + shadcn/ui
- React Three Fiber (3D visualizations)
- Wouter (routing)

### Backend
- Express.js + TypeScript
- PostgreSQL + Drizzle ORM
- WebSocket for real-time updates

### Blockchain
- **Arbitrum**: Hardhat + Ethers.js
- **Solana**: Anchor + web3.js
- **TON**: Blueprint + TON SDK

## API Endpoints

See [API_REFERENCE.md](./API_REFERENCE.md) for complete documentation.

**Key Endpoints**:
- `/api/bridge/*` - Cross-chain operations
- `/api/vault-creation/*` - Vault creation with Trinity Protocol
- `/api/token/*` - CVT token operations
- `/api/security/*` - Security & monitoring

## Smart Contracts (Arbitrum Sepolia)

| Contract | Address |
|----------|---------|
| CVT Token | `0xFb419D8E32c14F774279a4dEEf330dc893257147` |
| CrossChainBridgeV3 | `0x39601883CD9A115Aba0228fe0620f468Dc710d54` |
| CVTBridgeV3 | `0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0` |
| EmergencyMultiSig | `0xFafCA23a7c085A842E827f53A853141C8243F924` |

## Getting Started

1. Clone: `git clone https://github.com/Chronos-Vault/chronos-vault-platform-.git`
2. Configure: `export ETHEREUM_NETWORK=arbitrum`
3. Deploy: See [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)

## Resources
- Discord: https://discord.gg/WHuexYSV
- X: https://x.com/chronosvaultx

*Last Updated: October 8, 2025*
