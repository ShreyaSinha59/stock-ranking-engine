# Testing & Quality Gates (Phase 1.1.7)

This document defines testing layers and quality gates required to maintain correctness and prevent regressions.

---

## Testing Layers

1. Unit tests: validate individual computations (signals, normalization, ADDV).
2. Data validation tests: sanity checks for ingestion outputs and schema integrity.
3. Integration tests: run a small end-to-end pipeline on a limited universe.
4. Backtest regression tests: confirm strategy metrics do not change unexpectedly.
5. API contract tests: validate endpoint response structure and key fields.
6. Production smoke tests: confirm deployment health and data freshness.

---

## Quality Gates

- Linting must pass.
- Unit and integration tests must pass.
- Data sanity checks must pass before rankings are generated.
- System must be reproducible given identical inputs and configuration.
