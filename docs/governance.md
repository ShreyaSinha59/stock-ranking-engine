# Data Governance & Time Semantics (Phase 1.1.3)

This document defines the rules governing time semantics, data immutability, versioning, and reproducibility for the system.

These rules are mandatory and apply to all components.

---

## Time Semantics

All computations are performed as-of market close on the rebalance date.
No component may access data with timestamps later than the rebalance date.

---

## Immutability

All data tables are append-only.
Historical rows must never be updated or deleted.
Corrections are recorded as new rows.

---

## Versioning

All decision-producing logic (signals, weights, rankings) must be versioned.
The version used must be stored alongside outputs.

---

## Reproducibility

The system must be deterministic.
Given the same data, configuration, and code version, outputs must be identical.

---

## Auditability

All rankings must be explainable using stored signal values and configuration.
No explanation is generated dynamically.

---

## Fact vs Decision Separation

Raw data and computed signals are treated as facts.
Rankings and portfolio selections are treated as decisions.
Facts must never be modified by decision logic.
