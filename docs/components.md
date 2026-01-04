# System Components & Responsibilities

This document defines the responsibilities and boundaries of each major system component.  
Each component must operate strictly within its defined scope.

These boundaries are enforced to ensure:

- reproducibility
- auditability
- correctness
- ease of debugging
- future scalability

---

## 1. Ingestion Layer

### Responsibility

Fetch raw external data and persist it in raw form without transformation.

### Allowed

- Call external APIs (prices, filings, news, macro)
- Write raw payloads to `data/raw/`
- Log ingestion metadata (timestamps, source, success/failure)

### Forbidden

- Computing signals
- Filtering universe membership
- Writing to PostgreSQL
- Modifying historical data

---

## 2. Normalization & Canonicalization Layer

### Responsibility

Transform raw data into canonical, clean, schema-compliant records.

### Allowed

- Read from `data/raw/`
- Normalize identifiers (ticker, CUSIP)
- Adjust prices for splits/dividends
- Write clean data to PostgreSQL tables

### Forbidden

- Ranking securities
- Applying weights
- Making inclusion/exclusion decisions

---

## 3. Universe Engine

### Responsibility

Determine stock eligibility for each rebalance date.

### Allowed

- Read prices from PostgreSQL
- Compute liquidity metrics (ADDV)
- Write universe snapshots to `universe_membership`

### Forbidden

- Computing alpha signals
- Ranking or scoring securities

---

## 4. Signal Computation Layer

### Responsibility

Compute raw and normalized signals for eligible securities.

### Allowed

- Read canonical data from PostgreSQL
- Compute technical, fundamental, news, institutional, and macro signals
- Write signal values to `signal_scores`

### Forbidden

- Making Top-N decisions
- Applying portfolio constraints

---

## 5. Scoring & Ranking Engine

### Responsibility

Combine signals into a composite score and generate final rankings.

### Allowed

- Read from `signal_scores`
- Apply weighting and normalization
- Write results to `final_scores` and `rankings`

### Forbidden

- Fetching raw data
- Modifying universe membership

---

## 6. API Layer

### Responsibility

Expose read-only access to results and explanations.

### Allowed

- Read from PostgreSQL
- Serve rankings, signal breakdowns, and history

### Forbidden

- Writing to the database
- Triggering ingestion or scoring jobs

---

## 7. Scheduler

### Responsibility

Trigger execution of system components at predefined times.

### Allowed

- Invoke scripts or workflows

### Forbidden

- Containing business logic
