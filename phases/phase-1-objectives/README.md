# Stage I — Definition of Objectives (DO)

## Overview

**Stage I** is the foundation of the PASTA process. It defines *what* the system does,
*why* it exists, and *what* must be protected — from a business perspective first.

> "Security is meaningless without first understanding what the business is trying to achieve."
> — UcedaVélez & Morana (2015), Ch. 7

## Key Questions Answered

- What are the primary business requirements of the reBi0s repository?
- What security and compliance requirements apply?
- What is the Business Impact Analysis (BIA) for the platform?
- What inherent risk profile does the application carry?

## Artifacts Produced

| Artifact | File | Description |
|----------|------|-------------|
| Security Requirements Backlog | `security-requirements-backlog.md` | 11 functional security requirements |
| Business Requirements | `business-requirements.md` | Mission, business and scientific objectives |
| Business Impact Analysis | `../../artifacts/risk-analysis/bia-cia-matrix.md` | CIA-based impact mapping (10 risks) |
| Inherent Risk Profile | `../../artifacts/risk-analysis/risk-profile.md` | 7 risk scenarios |

## Activities (per PASTA methodology)

| Activity | Description | reBi0s Result |
|----------|-------------|---------------|
| 1.1 | Document business requirements | Federated data governance; multi-domain scientific support |
| 1.2 | Define security & compliance requirements | 11 requirements including RBAC, TLS 1.3, LGPD, DRP |
| 1.3 | Business Impact Analysis (BIA) | CIA triad mapping — majority classified as High impact |
| 1.4 | Define inherent risk profile | 10 risk categories; 7 scenario profiles documented |

## Data Classification — reBi0s

| Class | Definition | Example | Sensitivity |
|-------|-----------|---------|-------------|
| Public | No access restriction | Published datasets | 🟢 Low |
| Internal | Restricted to reBi0s collaborators | Operational metadata | 🟡 Medium |
| Sensitive | Restricted; exposure causes serious harm | Health records, identifiable data | 🔴 High |

## Regulatory Scope

- **LGPD** (Lei 13.709/2018) — Brazilian General Data Protection Law
- **UNICAMP Ethics Committee** — Scientific research data governance
- Principles applied: Finality, Adequacy, Necessity, Transparency, Security

## Feeds Into

→ Stage II (Technical Scope) — informs technology selection constraints  
→ Stage IV (Threat Analysis) — risk profile shapes threat prioritization  
→ Stage VII (Risk & Impact Analysis) — BIA informs impact scoring  
