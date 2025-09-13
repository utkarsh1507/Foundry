# Chisel
Chisel is a fast, utilitarian, and verbose Solidity REPL for rapid prototyping and debugging. It's perfect for testing Solidity snippets and exploring contract behavior interactively.Chisel is a fast, utilitarian, and verbose Solidity REPL.
The chisel binary can be used both within and outside of a Foundry project. If the binary is executed in a Foundry project root, Chisel will inherit the project's configuration options.

SET THE REPL

```bash
# Launch chisel REPL
chisel
```

# Interactive Solidity Development

```bash
// Create and query variables
➜ uint256 a = 123;
➜ a
Type: uint256
├ Hex: 0x7b
├ Hex (full word): 0x000000000000000000000000000000000000000000000000000000000000007b
└ Decimal: 123
 
// Test contract functions
➜ function add(uint256 x, uint256 y) public pure returns (uint256) { return x + y; }
➜ add(5, 10)
Type: uint256
└ Decimal: 15
```
