# Infrastructure & Deployment Decisions (Phase 1.1.1)

This document records the final infrastructure and deployment decisions for the stock ranking engine.
These decisions are considered locked unless scaling requirements force a change.

---

## Compute

- Single low-cost cloud VM (~$5/month)
- Hosts API, scheduled jobs, and local PostgreSQL
- Chosen for simplicity, cost efficiency, and reliability

Rejected:

- Kubernetes (overkill)
- Serverless-only architecture (poor fit for scheduled batch jobs)

---

## Database

- PostgreSQL (local initially)
- Used for all canonical, time-aware data
- Chosen for relational integrity, auditability, and future scalability

Rejected:

- SQLite (poor concurrency and auditability)
- NoSQL stores (poor fit for time-aware relational queries)

---

## File Storage

- Local filesystem during development
- Amazon S3 planned for production durability
- Chosen for low cost and simple migration path

Rejected:

- S3-only from day one (slower iteration)
- Storing raw data inside the database

---

## Scheduling

- cron-based scheduling
- Daily ingestion and weekly rebalancing

Rejected:

- Managed workflow orchestration (Airflow, Step Functions) at MVP stage

---

## Data Sources

- Free, authoritative sources initially:
  - Market prices
  - SEC EDGAR filings (10-K, 10-Q, 13F)
  - FRED macro data
- Paid data deferred until system value is validated

---

## Frontend & API

- FastAPI backend on VM
- Static frontend hosted on free platform

---

## Cost Envelope

- Target monthly cost: $5–$15
- System designed to stay within this envelope at MVP stage

