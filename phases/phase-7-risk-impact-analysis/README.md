# Stage VII — Risk & Impact Analysis (RIA)

## Overview

**Stage VII** is the culmination of the PASTA process. It quantifies residual risk per threat,
identifies countermeasures, and defines risk treatment strategies for the reBi0s platform.

## Risk Formulas

```
Risk Score (Stage IV):
  RS = (PV × PA × I) / C

Residual Risk (Stage VII):
  RR = [(tp × vp) / c] × i

Risk Scale:
  Low:      [0.00 – 0.29]  🟢
  Medium:   [0.30 – 0.59]  🟡
  High:     [0.60 – 0.89]  🔴
  Critical: [0.90+]        🔴🔴
```

## Residual Risk Calculations (Activity 7.3)

| ID | vp | tp | Impact (i) | Control (c) | Formula | RR | Status |
|----|----|----|-----------|-------------|---------|-----|--------|
| T01 | 0.7 | 0.9 | 1.0 | 0.5 | [(0.7×0.9)/0.5]×1.0 | **1.26** | 🔴🔴 Critical Priority |
| T02 | 0.6 | 0.9 | 0.5 | 0.5 | [(0.9×0.6)/0.5]×0.5 | **0.54** | 🟡 Medium |
| T03 | 0.5 | 1.0 | 1.0 | 0.5 | [(1.0×0.5)/0.5]×1.0 | **1.00** | 🔴 High |
| T04 | 1.0 | 1.0 | 0.8 | 0.9 | [(1.0×1.0)/0.9]×0.8 | **0.89** | 🔴 High |
| T07 | 0.3 | 0.4 | 0.8 | 0.9 | [(0.4×0.3)/0.9]×0.8 | **0.11** | 🟢 Low |
| T08 | 0.9 | 0.8 | 0.1 | 0.1 | [(0.8×0.9)/0.1]×0.1 | **0.72** | 🔴 High |

## Countermeasures (Activity 7.2)

| ID | Risk Description | Treatment | Action |
|----|-----------------|-----------|--------|
| T1 | Hardcoded passwords in `.yaml` and `conn.py` | **Mitigate** | Migrate to HashiCorp Vault / Kubernetes Secrets |
| T2 | Incomplete RBAC on Service Accounts | **Mitigate** | Least Privilege; disable unnecessary token automounting |
| T3 | Missing CPU/Memory limits in manifests | **Mitigate** | ResourceQuotas and LimitRanges on Kubernetes cluster |
| T4 | No injection vulnerability detected | **Accept** | Maintain query parameterization; output encoding |
| T5 | Insufficient audit and visibility | **Mitigate** | Immutable logs; alerts for failed logins and unauthorized access |

## Risk Treatment Strategy (Activity 7.4)

| Strategy | Count | Threats |
|----------|-------|---------|
| 🟠 **Mitigate** | 13 | T01, T02, T03, T05, T06, T07, T08, T09, T10, T11, T12, T13, T14 |
| 🟢 **Accept** | 2 | T04 (defense-in-depth maintained), T15 (low residual risk) |

## Priority Action Plan

### Immediate (Critical)
1. Rotate and remove all hardcoded credentials → Kubernetes Secrets + Vault
2. Enforce RBAC with strict namespace binding on all Service Accounts
3. Implement MFA on all consumption interfaces

### Short-term (30 days)
4. Add ResourceQuotas/LimitRanges to all Kubernetes namespaces
5. Configure centralized, immutable log storage with real-time alerting
6. Integrate Trivy/Snyk into CI/CD pipeline

### Medium-term (90 days)
7. Deploy Apache Ranger for fine-grained data access control
8. Enforce TLS 1.3 across all internal communications
9. Implement quarterly access review process

## Artifacts

| File | Description |
|------|-------------|
| `residual-risk-calculation.md` | Full risk calculation worksheet |
| `risk-treatment-strategy.md` | Treatment strategy and action plan |
| `../../artifacts/risk-analysis/` | Risk matrices and final report |
| `../../artifacts/reports/` | Complete RIA report |
