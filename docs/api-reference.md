# API Reference

## Vault Operations

### Create Vault

```typescript
POST /api/vault/create
```

**Request Body:**
```json
{
  "vaultId": "string",
  "owner": "string",
  "vaultType": "TIME_LOCK" | "MULTI_SIG" | "QUANTUM_RESISTANT",
  "securityLevel": "STANDARD" | "ENHANCED" | "MAXIMUM",
  "requireMultiSig": boolean,
  "enableQuantumResistant": boolean,
  "chainPreference": "arbitrum" | "solana" | "ton"
}
```

**Response:**
```json
{
  "success": true,
  "vaultId": "string",
  "mdlStatus": {
    "trinityConsensus": "APPROVED",
    "aiGovernance": "VALIDATED",
    "mpcKeys": "GENERATED",
    "vdfTimeLock": "INITIALIZED",
    "zkProof": "VERIFIED",
    "quantumCrypto": "ENCRYPTED"
  }
}
```

## WebSocket Events

### Subscribe to MDL Updates

```typescript
ws://api.chronosvault.io/ws

// Message Types:
- "mdl-status"
- "trinity-consensus"
- "ai-governance"
- "vault-operation"
```

## SDK Usage

```typescript
import { ChronosVaultClient } from '@chronos-vault/sdk';

const client = new ChronosVaultClient({
  apiUrl: 'https://api.chronosvault.io',
  websocketUrl: 'wss://ws.chronosvault.io'
});

// Create vault
const vault = await client.createVault({
  vaultType: 'TIME_LOCK',
  securityLevel: 'MAXIMUM',
  enableQuantumResistant: true,
  chainPreference: 'arbitrum'
});
```
