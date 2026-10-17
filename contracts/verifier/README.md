# Verifier Contract

This contract exposes a Soroban verifier entry point called `verify`, which accepts three proof components (`proofA`, `proofB, `proofC`) and a set of public inputs, and returns a boolean indicating whether the proof is valid.

## Building

```sh
cargo build -p verifier
```

## Running tests

The contract has handwritten unit tests and a randomised fuzz test that exercises the `verify` entry point with random inputs.

```sh
cargo test -p verifier
```

## Fuzz tests

The fuzz test lives in `src/tests.rs` and is driven by the Soroban test framework. It generates random `BytesN<64>`, `BytesN<128>`, and `BytesN<32>` values, calls `verify`, and asserts that the contract never panics or traps and only returns a valid return variant (`true`, `false`, or an explicit error variant).

Each test run exercises at least 1000 random input combinations. To run only the fuzz test:

```sh
cargo test -p verifier fuzz_```

To run the fuzz test with more iterations, set the `FUZZ_ITERATIONS` environment variable:

```sh
FUZZ_ITERATIONS=10000 cargo test -p verifier fuzz_```

The default is 1000 iterations, which is the minimum required by the fuzz coverage acceptance criteria.
