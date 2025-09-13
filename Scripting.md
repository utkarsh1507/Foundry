


# Scripting with Solidity

Solidity scripting is a way to declaratively deploy contracts using Solidity, instead of using the more limiting and less user friendly `forge create`.

Solidity scripts are like the scripts you write when working with tools like Hardhat; what makes Solidity scripting different is that they are written in Solidity instead of JavaScript, and they are run on the fast Foundry EVM backend, which provides advanced simulation with dry-run capabilities.

---

## How `forge script` works?

`forge script` does not work in an asynchronous manner. First, it collects all transactions from the script, and only then does it broadcast them all. It can essentially be split into **4 phases**:

1. **Local Simulation**  
   - The contract script is run in a local evm.  
   - If a rpc/fork url has been provided, it will execute the script in that context.  
   - Any external call (not static, not internal) from a `vm.broadcast` and/or `vm.startBroadcast` will be appended to a list.

2. **Onchain Simulation (Optional)**  
   - If a rpc/fork url has been provided, then it will sequentially execute all the collected transactions from the previous phase here.

3. **Broadcasting (Optional)**  
   - If the `--broadcast` flag is provided and the previous phases have succeeded, it will broadcast the transactions collected at step 1 and simulated at step 2.

4. **Verification (Optional)**  
   - If the `--verify` flag is provided, there's an API key, and the previous phases have succeeded, it will attempt to verify the contract. (e.g. Etherscan).

Transactions that previously failed or timed-out can be submitted again by providing `--resume` flag.

⚠️ **Note:** Transactions whose behaviour can be influenced by external state/actors might have a different result than what was simulated on step 2, e.g. front running.

---

## Setup Counter Project

Let's try to deploy the basic Counter contract Foundry provides:

```bash
forge init Counter
````

Next compile our contracts to make sure everything is in order:

```bash
forge build
```

---

## Deploying the Counter contract

### Get RPC and Etherscan API Keys

We are going to deploy the Counter contract to the **Sepolia testnet**, but in order to do so we will need a few prerequisites:

1. **Get a Sepolia RPC URL**

   * Grab one from [Chainlist](https://chainlist.org) or use an RPC provider like **Alchemy** or **Infura**.

2. **Get a one-time use private key for deploying**

   ```bash
   cast wallet new
   ```

   Output:

   ```
   Successfully created new keypair.
   Address:     <PUBLIC KEY>
   Private key: <PRIVATE_KEY>
   ```

3. **Fund the private key**

   * Grab some Sepolia testnet ETH from faucets:

     * Proof of Work faucet
     * Alchemy
     * Quicknode
     * Chainstack

   Some faucets may require a mainnet ETH balance.

4. **Get a Sepolia Etherscan API key** [here](https://etherscan.io/).

---

### Configuring `.env` and `foundry.toml`

Create a `.env` file:

```bash
SEPOLIA_RPC_URL=
ETHERSCAN_API_KEY=
```

Update **foundry.toml**:

```toml
[rpc_endpoints]
sepolia = "${SEPOLIA_RPC_URL}"

[etherscan]
sepolia = { key = "${ETHERSCAN_API_KEY}" }
```

This creates a RPC alias for Sepolia and loads the Etherscan API key.

---

## Writing the script

Navigate to the `script` folder and locate `Counter.s.sol`. Update it:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import {Script, console} from "forge-std/Script.sol";
import {Counter} from "../src/Counter.sol";

contract CounterScript is Script {
    Counter public counter;

    function setUp() public {}

    function run() public {
        vm.startBroadcast();

        counter = new Counter();

        vm.stopBroadcast();
    }
}
```

### Explanation

* `pragma solidity ^0.8.13;`
  Even if it's a script it still works like a contract. You must specify pragma version.

* `import {Script, console} from "forge-std/Script.sol";`
  Import Forge Std scripting utilities.

* `import {Counter} from "../src/Counter.sol";`
  Import the Counter contract.

* `contract CounterScript is Script`
  CounterScript inherits Forge’s `Script`.

* `function run()`
  Entry point of the script.

* `vm.startBroadcast()`
  Records transactions and contract creations, signing them with your private key.

* `counter = new Counter();`
  Creates a new Counter contract, recorded for broadcasting.

* `vm.stopBroadcast()`
  Stops recording transactions.

---

## Deploying to Sepolia Testnet

Run at the project root:

```bash
# Load .env variables
source .env

# Deploy and verify
forge script --chain sepolia script/Counter.s.sol:CounterScript \
  --rpc-url $SEPOLIA_RPC_URL --broadcast --verify -vvvv --interactives 1
```

Interactive prompt:

```
Enter private key: <PRIVATE_KEY>
```

Forge compiles, simulates, deploys, and verifies. Example output:

```
✅  [Success] Hash: <HASH>
Contract Address: <ADDRESS>
Block: <BLOCK>
Paid: <GAS>
```

Verification:

```
Contract successfully verified
All (1) contracts were verified!
```

Broadcast + cache files are saved to `/broadcast` and `/cache`.

---

## Deploying to Local Anvil

Start Anvil in one terminal:

```bash
anvil
```

It shows default accounts and private keys.

Example:

```
(0) 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 (10000 ETH)
Private Key: 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

Deploy in another terminal:

```bash
forge script script/Counter.s.sol:CounterScript \
  --fork-url http://localhost:8545 --broadcast --interactives 1
```

Enter private key, script executes, and deployment succeeds locally.

Output example:

```
✅  [Success] Hash: 0x6795deaad7fd483eda4b16af7d8b871c7f6e49beb50709ce1cf0ca81c29247d1
Contract Address: 0x5FbDB2315678afecb367f032d93F642f64180aa3
```

---

# ✅ Summary

* **forge script** allows Solidity-based scripting.
* Supports simulation, broadcasting, and verification.
* Deployment works on both **Sepolia testnet** and **local Anvil instance**.
* All done with just one command.

