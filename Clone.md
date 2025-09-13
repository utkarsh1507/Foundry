
# Clone a Verified Contract

## Clone the Contract
To clone an on-chain verified contract as a Forge project, use `forge clone`.  
Example: cloning **WETH9** on Ethereum mainnet:

```bash
forge clone 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 WETH9 --etherscan-api-key <YOUR_API_KEY>
````

This creates a new directory `WETH9`, configures it as a Foundry project, and clones all the source code of the contract into it.
It also initializes a new git repository.

---

## Review the Cloned Project

After cloning, you’ll see output like this:

```
Downloading the source code of 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 from Etherscan...
Initializing /home/zhan4987/WETH9...
Installing forge-std in /home/zhan4987/WETH9/lib/forge-std (url: Some("https://github.com/foundry-rs/forge-std"), tag: None)
Cloning into '/home/zhan4987/WETH9/lib/forge-std'...
remote: Enumerating objects: 2243, done.
remote: Counting objects: 100% (2238/2238), done.
remote: Compressing objects: 100% (778/778), done.
remote: Total 2243 (delta 1489), reused 2097 (delta 1391), pack-reused 5
Receiving objects: 100% (2243/2243), 649.07 KiB | 8.89 MiB/s, done.
Resolving deltas: 100% (1489/1489), done.
Installed forge-std v1.8.1
Initialized forge project
Collecting the creation information of 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 from Etherscan...
Waiting for 5 seconds to avoid rate limit...
[⠊] Compiling...
[⠒] Compiling 1 files with 0.4.19
[⠢] Solc 0.4.19 finished in 9.50ms
Compiler run successful!
```

The cloned Forge project comes with an additional `.clone.meta` metadata file besides the ordinary files of a normal Forge project.

---

## Examine the Metadata

The `.clone.meta` file looks like this:

```json
{
  "path": "src/Contract.sol",
  "targetContract": "WETH9",
  "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
  "chainId": 1,
  "creationTransaction": "0xb95343413e459a0f97461812111254163ae53467855c0d73e0f1e7c5b8442fa3",
  "deployer": "0x4f26ffbe5f04ed43630fdc30a87638d53d0b0876",
  "constructorArguments": "0x",
  "storageLayout": {
    "storage": [],
    "types": {}
  }
}
```

`.clone.meta` is a compact JSON data file that contains the information of the on-chain contract instance, such as:

* Contract address
* Constructor arguments
* Chain ID
* Deployer address
* Creation transaction hash
* Storage layout

More details of the metadata can be found in the official reference.


