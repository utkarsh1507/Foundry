
# Creating a New Project

## Initialize the Project
To start a new project with Foundry, use:

```bash
forge init hello_foundry
````

This creates a new directory `hello_foundry` from the default template.
It also initializes a new git repository.

To create a new project using a different template, pass the `--template` flag:

```bash
forge init --template https://github.com/foundry-rs/forge-template hello_template
```

---

## Navigate to the Project Directory

Move into your newly created project:

```bash
cd hello_foundry
```

---

## Explore the Project Structure

Check what the default template looks like:

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

The default template comes with one dependency installed: **Forge Standard Library**.
This is the preferred testing library for Foundry projects.
It also includes an empty starter contract and a simple test.

---

## Build the Project

Compile your contracts:

```bash
forge build
```

Example output:

```
Compiling 23 files with Solc 0.8.19
Solc 0.8.19 finished in 575.60ms
Compiler run successful!
```

---

## Run the Tests

Execute the test suite:

```bash
forge test
```

Example output:

```
No files changed, compilation skipped

Ran 2 tests for test/Counter.t.sol:CounterTest
[PASS] testFuzz_SetNumber(uint256) (runs: 256, μ: 29236, ~: 29314)
[PASS] test_Increment() (gas: 28800)
Suite result: ok. 2 passed; 0 failed; 0 skipped; finished in 6.56ms (6.37ms CPU time)

Ran 1 test suite in 8.59ms (6.56ms CPU time): 2 tests passed, 0 failed, 0 skipped (2 total tests)
```




