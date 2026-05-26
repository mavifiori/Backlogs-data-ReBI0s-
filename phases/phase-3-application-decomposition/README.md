# Stage III — Application Decomposition and Analysis (ADA)

## Overview

**Stage III** decomposes the reBi0s platform into interaction scenarios, data flows,
and Trust Boundaries, producing the security context map for Stages IV–VI.

## Interaction Scenarios

| Scenario | Components | Primary Threats |
|----------|-----------|-----------------|
| C1 — Data Ingestion | Airflow → Spark → Iceberg/PostgreSQL | T03, T02, T12, T05, T06 |
| C2 — SQL Consumption | DataHub, HUE, Superset, PostgreSQL | T14, T03, T10 |
| C3 — Data Science (Jupyter) | Jupyter, Superset, Spark | T09, T01, T12, T11, T04 |
| C4 — Catalog Management | DataHub, Airflow, Iceberg | T12, T14, T15, T11 |
| C5 — Data Quality | DataHub | T12, T11 |
| C6 — Administration | Airflow, Spark, DataHub | T01, T12, T08, T15 |

## Trust Boundaries

| Boundary | Components | Key Threats | Controls |
|----------|-----------|-------------|---------|
| Governance | DataHub | Unauthorized access, integrity violation | Auth, authz, audit |
| Curadoria | Airflow, Spark, Iceberg, PostgreSQL | Data corruption, policy violations | Validation, encryption |
| Colaboração | HUE, Jupyter, Superset | DoS, policy violations | Anti-DoS, encryption |
| Armazenamento | Spark, Airflow, Iceberg, MinIO, PostgreSQL | Data manipulation, exposure | Encryption, auth |

## Methodological Note

> **Adaptation — Use Cases & Abuse Cases:**
> Standard PASTA methodology calls for formal UML Use Case and Abuse Case diagrams.
> In this research, these were replaced by **general interaction and threat scenarios**
> (C1–C6), which are:
> - More aligned to the data pipeline architectural context
> - Easier to update as the system evolves
> - Directly traceable to threats in Stage IV
> - Usable as test case references in Stage VI

## Artifacts

| File | Description |
|------|-------------|
| `interaction-scenarios.md` | Full C1–C6 scenario descriptions |
| `trust-boundaries.md` | Trust Boundary definitions and security controls |
