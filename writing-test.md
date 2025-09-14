
# Writing Tests

Tests are written in Solidity. If the test function reverts, the test fails; otherwise, it passes.  

The most common way of writing tests is using the **Forge Standard Library's `Test` contract**, which is the preferred way of writing tests with Forge.  

In this section, we'll go over the basics using the functions from Forge Std's `Test` contract (a superset of `DSTest`). You will learn more advanced features later.

---

## Importing Test Contract

`DSTest` provides basic logging and assertion functionality.  
To access these functions, import `forge-std/Test.sol` and inherit from `Test` in your test contract:

```solidity
import {Test} from "forge-std/Test.sol";
````

---

## Example: Basic Test

```solidity
pragma solidity 0.8.10;

import {Test} from "forge-std/Test.sol";

contract ContractBTest is Test {
    uint256 testNumber;

    function setUp() public {
        testNumber = 42;
    }

    function test_NumberIs42() public {
        assertEq(testNumber, 42);
    }

    /// forge-config: default.allow_internal_expect_revert = true
    function testRevert_Subtract43() public {
        vm.expectRevert();
        testNumber -= 43;
    }
}
```

---

## Forge Keywords in Tests

* **`setUp`**: Optional function invoked before each test case.

  ```solidity
  function setUp() public {
      testNumber = 42;
  }
  ```

* **`test`**: Functions prefixed with `test` are run as test cases.

  ```solidity
  function test_NumberIs42() public {
      assertEq(testNumber, 42);
  }
  ```

✅ Good practice: Use the pattern `test_Revert[If|When]_Condition` with the `expectRevert` cheatcode.
(You’ll learn more about cheatcodes later.)

---

## Using `stdError`

To use `stdError` constants (like `arithmeticError`), import:

```solidity
import {stdError} from "forge-std/StdError.sol";
```

Example:

```solidity
function test_CannotSubtract43() public {
    vm.expectRevert(stdError.arithmeticError);
    testNumber -= 43;
}
```

---

## Deployment Behavior in Tests

Tests are deployed to:

```
0xb4c79daB8f259C7Aee6E5b2Aa729821864227e84
```

If you deploy a contract within your test, then this address will be its deployer.
If the deployed contract gives special permissions (e.g., `onlyOwner` in `Ownable.sol`), then your test contract will have those permissions.

⚠️ **Note**:
Test functions must have either **external** or **public** visibility.
Functions declared as `internal` or `private` won’t be picked up by Forge, even if prefixed with `test`.

---

## Before Test Setups

Unit and fuzz tests are **stateless** and executed as single transactions.
The state modified by one test is not available to another (except via `setUp`).

You can simulate multiple transactions in a single test using `beforeTestSetup`.

```solidity
function beforeTestSetup(
    bytes4 testSelector
) public returns (bytes[] memory beforeTestCalldata)
```

* `testSelector`: Selector of the test for which transactions are applied.
* `beforeTestCalldata`: Array of calldata applied before test execution.

💡 Useful for **chaining tests** or when a test needs pre-committed transactions (e.g., with `selfdestruct`).
If any configured transaction reverts, the test fails.

### Example:

```solidity
contract ContractTest is Test {
    uint256 a;
    uint256 b;

    function beforeTestSetup(
        bytes4 testSelector
    ) public pure returns (bytes[] memory beforeTestCalldata) {
        if (testSelector == this.testC.selector) {
            beforeTestCalldata = new bytes ;
            beforeTestCalldata[0] = abi.encodePacked(this.testA.selector);
            beforeTestCalldata[1] = abi.encodeWithSignature("setB(uint256)", 1);
        }
    }

    function testA() public {
        require(a == 0);
        a += 1;
    }

    function setB(uint256 value) public {
        b = value;
    }

    function testC() public {
        assertEq(a, 1);
        assertEq(b, 1);
    }
}
```

---

## Shared Setups

You can create **shared helper contracts** for setups and inherit them in your test contracts.

```solidity
abstract contract HelperContract {
    address constant IMPORTANT_ADDRESS = 0x543d...;
    SomeContract someContract;

    constructor() { ... }
}

contract MyContractTest is Test, HelperContract {
    function setUp() public {
        someContract = new SomeContract(0, IMPORTANT_ADDRESS);
        ...
    }
}

contract MyOtherContractTest is Test, HelperContract {
    function setUp() public {
        someContract = new SomeContract(1000, IMPORTANT_ADDRESS);
        ...
    }
}
```


