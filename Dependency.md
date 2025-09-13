
# Dependencies

Forge manages dependencies using **git submodules** by default, which means that it works with any GitHub repository that contains smart contracts.

---

## Adding a Dependency
To add a dependency, run:

```bash
forge install vectorized/solady
````

Example output:

```
Installing solady in /tmp/tmp.qMjuCvaOuK/hello_foundry/lib/solady (url: Some("https://github.com/vectorized/solady"), tag: None)
    Installed solady tag=v0.1.24@a5bb996e91aae5b0c068087af7594d92068b12f1
```

This pulls the `solady` library, stages the `.gitmodules` file in git, and makes a commit with the message **Installed solady**.

Check the `lib` folder:

```bash
tree . -d -L 1
```

Output:

```
.
├── lib
├── script
├── src
└── test

5 directories
```

Forge has installed **solady**!

By default, `forge install` installs the **latest master branch** version. To install a specific tag or commit:

```bash
forge install vectorized/solady@v0.1.3
```

---

## Remapping Dependencies

Forge can remap dependencies to make them easier to import. Forge will automatically deduce some remappings:

```bash
forge remappings
```

Example output:

```
forge-std/=lib/forge-std/src/
solady/=lib/solady/src/
```

These remappings mean:

* Import from forge-std:

  ```solidity
  import "forge-std/Contract.sol";
  ```
* Import from solady:

  ```solidity
  import "solady/Contract.sol";
  ```

You can customize these remappings by creating a `remappings.txt` file in the root of your project.

Example remapping for `solady-utils`:

```
@solady-utils/=lib/solady/src/utils/
```

Or set in `foundry.toml`:

```toml
remappings = [
    "@solady-utils/=lib/solady/src/utils/",
]
```

Now we can import contracts from `src/utils` of solady like so:

```solidity
import {LibString} from "@solady-utils/LibString.sol";
```

---

## Remapping Conflicts

Sometimes two or more dependencies use the same namespace. Example: installing `org/lib_1` and `org/lib_2`, both depending on `@openzeppelin`.

Running `forge remappings` may result in only one being mapped:

```
@openzeppelin/=lib/lib_1/node_modules/@openzeppelin/
```

This can cause **import issues** or unexpected behavior.

To fix, add **remapping contexts** to `remappings.txt`:

```
lib/lib_1/:@openzeppelin/=lib/lib_1/node_modules/@openzeppelin/
lib/lib_2/:@openzeppelin/=lib/lib_2/node_modules/@openzeppelin/
```

This ensures each dependency is mapped correctly.
For more info, see the [Solidity Lang Docs](https://docs.soliditylang.org/).

---

## Updating Dependencies

Update a specific dependency:

```bash
forge update lib/solady
```

Update all dependencies at once:

```bash
forge update
```

---

## Removing Dependencies

Remove a dependency with:

```bash
forge remove solady
```

Equivalent command:

```bash
forge remove lib/solady
```

---

## Hardhat Compatibility

Forge supports **Hardhat-style projects** where:

* Dependencies are installed as **npm packages** (in `node_modules`).
* Contracts are stored in `contracts/` instead of `src/`.

Enable compatibility mode with:

```bash
forge install --hh
```


