




## Forge Test
```bash
forge test
````

Measures the standard test execution time. Runs all the tests in the repository.

---

## Forge Fuzz Test

```bash
forge test --match-test "test[^(]*\([^)]+\)"
```

Measures the execution time for fuzz tests.

---

## Forge Test Isolated

```bash
forge test --isolate
```

Measures the execution time of tests in isolate mode.

In isolation mode, all top-level calls are executed as a separate transaction in a separate EVM context, enabling more precise gas accounting and transaction state changes.

---

## Forge Build (No Cache)

```bash
forge build
```

Measures compilation time on a clean repository i.e., no cache available.

---

## Forge Build (With Cache)

```bash
forge build
```

Measures execution time for forge build in presence of full cache. In this case, compilation is skipped and forge only checks whether the cache is clean or marked as dirty.

---

## Forge Coverage

```bash
forge coverage --ir-minimum
```


