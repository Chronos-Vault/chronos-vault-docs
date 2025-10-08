# Chronos Vault Integration Examples

> Real code examples for integrating with Chronos Vault V3 Trinity Protocol

## Quick Start Integration

### 1. Connect to Arbitrum L2
```javascript
import { ethers } from "ethers";

// Arbitrum Sepolia configuration
const provider = new ethers.JsonRpcProvider(
  "https://sepolia-rollup.arbitrum.io/rpc"
);

// V3 Contract addresses
const CROSSCHAIN_BRIDGE_V3 = "0x39601883CD9A115Aba0228fe0620f468Dc710d54";
const CVT_BRIDGE_V3 = "0x00d02550f2a8Fd2CeCa0d6b7882f05Beead1E5d0";
const EMERGENCY_MULTISIG = "0xFafCA23a7c085A842E827f53A853141C8243F924";
```

### 2. Check Circuit Breaker Status
```javascript
// Real-time circuit breaker monitoring
const response = await fetch(
  "https://api.chronosvault.com/api/bridge/circuit-breaker/status"
);
const status = await response.json();

console.log("CrossChainBridgeV3:", status.crossChainBridge.paused);
console.log("CVTBridgeV3:", status.cvtBridge.paused);
```

### 3. Create Time-Lock Vault
```javascript
// Trinity Protocol vault creation
const vault = await fetch(
  "https://api.chronosvault.com/api/vault-creation/time-lock",
  {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Bearer YOUR_TOKEN"
    },
    body: JSON.stringify({
      unlockTime: Math.floor(Date.now() / 1000) + 86400, // 24 hours
      beneficiary: "0xYourAddress",
      securityLevel: "ENHANCED" // 2-of-3 Trinity Protocol
    })
  }
);
```

### 4. Cross-Chain Atomic Swap
```javascript
// Trustless swap between Arbitrum <-> Solana
const swap = await fetch(
  "https://api.chronosvault.com/api/bridge/atomic-swap",
  {
    method: "POST",
    headers: { "Authorization": "Bearer YOUR_TOKEN" },
    body: JSON.stringify({
      sourceChain: "arbitrum",
      targetChain: "solana",
      amount: "100",
      token: "CVT"
    })
  }
);
```

## Smart Contract Integration

### Interact with CrossChainBridgeV3
```solidity
// Solidity example
interface ICrossChainBridgeV3 {
    function isPaused() external view returns (bool);
    function getCurrentVolume() external view returns (uint256);
    function getFailedProofRate() external view returns (uint256);
}

contract MyVaultIntegration {
    ICrossChainBridgeV3 bridge = ICrossChainBridgeV3(
        0x39601883CD9A115Aba0228fe0620f468Dc710d54
    );
    
    function checkBridgeSafety() public view returns (bool) {
        return !bridge.isPaused();
    }
}
```

### Query CVT Token Data
```javascript
// Get CVT balance on Arbitrum
const balance = await fetch(
  `https://api.chronosvault.com/api/token/balance/arbitrum/${address}`
);
const data = await balance.json();
console.log("CVT Balance:", data.balance);
```

## Zero-Knowledge Proofs

### Generate Ownership Proof
```javascript
// Create ZK proof without revealing vault contents
const proof = await fetch(
  "https://api.chronosvault.com/api/security/zk-proofs/ownership",
  {
    method: "POST",
    headers: { "Authorization": "Bearer YOUR_TOKEN" },
    body: JSON.stringify({
      vaultId: "vault_123",
      statement: "I own this vault"
    })
  }
);
```

### Verify ZK Proof
```javascript
const verification = await fetch(
  "https://api.chronosvault.com/api/security/zk-proofs/verify",
  {
    method: "POST",
    body: JSON.stringify({
      proof: zkProofData,
      publicInputs: [...]
    })
  }
);
```

## Quantum-Resistant Encryption

### Encrypt with CRYSTALS-Kyber
```javascript
const encrypted = await fetch(
  "https://api.chronosvault.com/api/security/quantum/encrypt",
  {
    method: "POST",
    body: JSON.stringify({
      data: "sensitive_data",
      publicKey: recipientPublicKey
    })
  }
);
```

### Sign with CRYSTALS-Dilithium
```javascript
const signature = await fetch(
  "https://api.chronosvault.com/api/security/quantum/sign",
  {
    method: "POST",
    headers: { "Authorization": "Bearer YOUR_TOKEN" },
    body: JSON.stringify({
      message: "transaction_data"
    })
  }
);
```

## Multi-Chain Wallet Integration

### Connect Arbitrum Wallet (MetaMask)
```javascript
// Connect MetaMask to Arbitrum Sepolia
await window.ethereum.request({
  method: "wallet_addEthereumChain",
  params: [{
    chainId: "0x66eee", // 421614 in hex
    chainName: "Arbitrum Sepolia",
    rpcUrls: ["https://sepolia-rollup.arbitrum.io/rpc"],
    nativeCurrency: {
      name: "ETH",
      symbol: "ETH",
      decimals: 18
    }
  }]
});
```

### Connect Solana Wallet (Phantom)
```javascript
// Connect Phantom wallet
const resp = await window.solana.connect();
console.log("Connected:", resp.publicKey.toString());
```

### Connect TON Wallet
```javascript
import { TonConnectUI } from "@tonconnect/ui";

const tonConnectUI = new TonConnectUI({
  manifestUrl: "https://chronosvault.com/tonconnect-manifest.json"
});
await tonConnectUI.connectWallet();
```

## Error Handling

### Handle Circuit Breaker Pause
```javascript
try {
  const transfer = await bridge.transfer(params);
} catch (error) {
  if (error.message.includes("CIRCUIT_BREAKER_PAUSED")) {
    // Wait for auto-recovery (2-4 hours)
    const status = await getCircuitBreakerStatus();
    console.log("Resume time:", status.resumeTime);
  }
}
```

## Resources
- API Reference: [API_REFERENCE.md](./API_REFERENCE.md)
- Deployment Guide: [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)
- SDK: https://github.com/Chronos-Vault/chronos-vault-sdk

*Last Updated: October 8, 2025*
