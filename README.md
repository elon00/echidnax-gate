# Echidnax Gate — QMOOSA-0755

**Status: engineering scaffold; NOT security-approved or deployment-ready.**

This repository is the **tests** component of QMOOSA-1000 Batch 151 (Automated Smart Contract Invariant Fuzzer). The current Rust CLI performs a deterministic integer-state demonstration. It does **not** implement contract fuzzing, PQC signatures or encapsulation, TLS, QuickNode connectivity, x402 settlement, escrow verification, or a production security gate.

## Verified scope and limitations

- `cargo check --all-targets` checks whether Rust source builds; it is not a protocol security audit.
- `python -m compileall -q tests` and `python -m pytest -q tests -o 'python_files=*.py'` fail closed on invalid Python and failing assertions.
- The existing `tests/reality_battery.py` contains invalid control characters (including U+0004). The existing `tests/reality_check_harness.py` uses constant assertions, not implementation-bound security tests. They must be replaced with genuine regression/adversarial tests against the approved fuzzer implementation.
- CodeQL analyzes **Python only** in the current workflow; this does not validate Rust, Solidity, PQC or contract safety.
- `contracts/LaunchpadToken.sol` is present but is not demonstrated here to be compiled, audited, or deployed.

## Local commands

```bash
cargo check --all-targets
python -m pip install 'pytest>=8,<10'
python -m compileall -q tests
python -m pytest -q tests -o 'python_files=*.py'
```

A failing gate is an actionable blocker, not a reason to delete tests, skip failures, invent pass scores, or mark this component production-ready.

## Release-blocking acceptance work

1. Implement or link the approved Batch 151 fuzzer engine and its contract-under-test fixtures.
2. Replace corrupted/fake tests with actual invariant, adversarial and regression tests, with deterministic seeds and counterexamples.
3. Add contract build and static-analysis checks; audit supply/authorization/economic logic separately.
4. Re-run CI on the exact reviewed commit; obtain independent security approval before deployment.

See `SECURITY.md` and the 2026-10-25 audit checkpoint. No testnet/mainnet deployment, secret usage or paid execution is authorized by this document.
