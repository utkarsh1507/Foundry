# Cast
Cast is your Swiss army knife for interacting with Ethereum applications from the command line. You can make smart contract calls, send transactions, or retrieve any type of chain data.

Read Contract Data
```bash
# Check ETH balance
cast balance vitalik.eth --ether --rpc-url https://reth-ethereum.ithaca.xyz/rpc
 
# Call a contract function to read data
cast call 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 \
"balanceOf(address)" 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 \
--rpc-url https://reth-ethereum.ithaca.xyz/rpc
```
# Send Transactions

```bash
# Set your private key
export PRIVATE_KEY="0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"
 
# Send ETH to an address
cast send 0x70997970C51812dc3A010C7d01b50e0d17dc79C8 --value 10000000 --private-key $PRIVATE_KEY
```

# Interact with JSON-RPC

```bash
# Call JSON-RPC methods directly
cast rpc eth_getHeaderByNumber $(cast 2h 22539851) --rpc-url https://reth-ethereum.ithaca.xyz/rpc
 
# Get latest block number
cast block-number --rpc-url https://reth-ethereum.ithaca.xyz/rpc
```
