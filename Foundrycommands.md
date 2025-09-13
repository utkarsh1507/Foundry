# Build, Test, and Deploy Contracts

```bash
# 1. Compile Contracts
forge build

# 2. Run Test Suite
forge test

# 3. Test Against Live Chain State (Forking)
forge test --fork-url https://reth-ethereum.ithaca.xyz/rpc
```

# 4. Deploy Contracts
```bash

# Use forge scripts to deploy contracts
# Set your private key
export PRIVATE_KEY="0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"

# Deploy to local anvil instance
forge script script/Counter.s.sol --rpc-url http://127.0.0.1:8545 --broadcast --private-key $PRIVATE_KEY
