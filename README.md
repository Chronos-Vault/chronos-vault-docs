# Chronos Vault Documentation

> Complete documentation for Trinity Protocol multi-chain security vault platform

## V3 Deployment Status - October 8, 2025

### Circuit Breaker Contracts (Arbitrum Sepolia)
- CrossChainBridgeV3: 0x39601883CD9A115Aba0228fe0620f468Dc710d54
- CVTBridgeV3: 0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0  
- EmergencyMultiSig: 0xFafCA23a7c085A842E827f53A853141C8243F924

## Documentation Index

### Getting Started
- [API Reference](./API_REFERENCE.md)
- [Deployment Guide](./DEPLOYMENT_GUIDE.md)
- [Technical README](./TECHNICAL_README.md)
- [Integration Examples](./INTEGRATION_EXAMPLES.md)

### Security & Architecture
- [Security Architecture](./SECURITY_ARCHITECTURE.md)
- [Trinity Protocol Mathematical Foundation](./trinity-protocol-mathematical-foundation.md)

### Token & Economics
- [CVT Tokenomics](./CVT_TOKENOMICS_SPECIFICATION.md)
- [CVT Whitepaper](./CVT_WHITEPAPER.md)

## Trinity Protocol Architecture

**Arbitrum L2 (PRIMARY)** - Main security layer
**Solana (MONITOR)** - High-frequency validation
**TON (BACKUP)** - Quantum-resistant recovery

## Quick Start

```bash
git clone https://github.com/Chronos-Vault/chronos-vault-platform-.git
export ETHEREUM_NETWORK=arbitrum
npx hardhat run scripts/deploy-v3-with-multisig.cjs --network arbitrumSepolia
```

## Community
- Discord: https://discord.gg/WHuexYSV
- X: https://x.com/chronosvaultx

*Last Updated: October 8, 2025*
