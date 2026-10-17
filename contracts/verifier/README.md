# Verifier Contract

This contract exposes a Soroban verifier entry point that checks a zero-knowledge proof against public inputs. The `verify` function takes a `BytesN<64>` proof a component, a `BytsN<128>` proof b component, a `BytesN<64>` proof c component, and a vector of `BytesN<32>` public inputs, and returns a boolean or an explicit error variant.

## Fuzz Tests

The contract includes a randomised fuzz test that feeds random byte inputs into the `verify` entry point to ensure it never panics or traps. The test generates random `BytsN<64>`, `BytesN<128>`, and `BytesN<32>` values, calls `verify`, and asserts that the call only returns a valid return variant (`true`, `false`, or an explicit error). At least 1000 random input combinations are tried per test run.

### Running the Fuzz Tests

From the repository root:

```bash
cargo test -p verifier fuzz
```

To run the entire verifier test suite (including the fuzz test):

```bash
cargo test -p verifier
```

The fuzz test is deterministic per run but uses a randomised seed, so repeated runs exercise different input combinations. To reproduce a specific failure, set the `RNG_SEED` environment variable before running the test:

```bash
RNG_SEED=12345 cargo test -p verifier fuzz
```

## Test Layout

The fuzz test lives in `contracts/verifier/src/tests.rs` alongside the handwritten unit tests. It uses the Soropan test framework to invoke the contract and asserts that no panic or contract trap occurs.
