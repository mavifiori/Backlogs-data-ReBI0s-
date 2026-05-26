# Stage VII — Risk & Impact Analysis (RIA)
## Residual Risk Calculation & Treatment Strategy — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifacts:** `fase_4__5_6_7.xlsx` (sheets: `7.2 Identify the Countermeasur`, `7.3 Calculate the Residual Ri`, `7.4Recommend Strategy to Manage`)  
> **PASTA stage:** Stage VII — Risk & Impact Analysis (RIA)

---

## 1. Risk Formulas

### Risk Score (Stage IV)

```
RS = (PV × PA × I) / C

  PV = Probability of Vulnerability occurrence   [0.1 – 1.0]
  PA = Probability of Threat Agent exploitation  [0.1 – 1.0]
  I  = Impact on CIA triad                       [0.1 = Low | 0.5 = Medium | 0.8 = High | 1.0 = Critical]
  C  = Control/Countermeasure effectiveness      [0.1 = Low | 0.5 = Partial | 0.9 = Effective]
```

### Residual Risk (Stage VII)

```
RR = [(tp × vp) / c] × i

  tp = Threat probability (same as PA)
  vp = Vulnerability probability (same as PV)
  c  = Control effectiveness after countermeasure
  i  = Impact value

Risk Scale:
  Low:      [0.00 – 0.29]  🟢
  Medium:   [0.30 – 0.59]  🟡
  High:     [0.60 – 0.89]  🔴
  Critical: [0.90 – 1.00+] 🔴🔴
```

---

## 2. Residual Risk Calculations — Activity 7.3

| ID | Threat | vp | tp | Impact (i) | Control (c) | Formula | Residual Risk | Status |
|----|--------|----|----|-----------|-------------|---------|---------------|--------|
| T01 | Access Control Failures | 0.7 | 0.9 | 1.0 (Critical) | 0.5 | [(0.7 × 0.9) / 0.5] × 1.0 | **1.26** | 🔴🔴 Critical Priority |
| T02 | Weak Cryptography | 0.6 | 0.9 | 0.5 (Medium) | 0.5 | [(0.9 × 0.6) / 0.5] × 0.5 | **0.54** | 🟡 Medium |
| T03 | Injection | 0.5 | 1.0 | 1.0 (Critical) | 0.5 | [(1.0 × 0.5) / 0.5] × 1.0 | **1.00** | 🔴 High |
| T04 | Software/Data Integrity | 1.0 | 1.0 | 0.8 (High) | 0.9 | [(1.0 × 1.0) / 0.9] × 0.8 | **0.89** | 🔴 High |
| T07 | Authentication Failures | 0.3 | 0.4 | 0.8 (High) | 0.9 | [(0.4 × 0.3) / 0.9] × 0.8 | **0.11** | 🟢 Low |
| T08 | Insufficient Logging | 0.9 | 0.8 | 0.1 (Low) | 0.1 | [(0.8 × 0.9) / 0.1] × 0.1 | **0.72** | 🔴 High |

### Impact and Risk Value Reference

| Impact Level | Value | Risk Level | Value |
|-------------|-------|-----------|-------|
| Critical — sensitive data exposure and unauthorized access | 1.0 | Critical | 1.0 |
| High — temporary system unavailability | 0.8 | High | 0.8 |
| Medium — risk of privacy violation | 0.5 | Medium | 0.4 |
| Low — limited impact if no privileged access | 0.1 | Low | 0.1 |

---

## 3. Countermeasures — Activity 7.2

| ID | Risk Description | OWASP/CAPEC Action | Priority |
|----|-----------------|-------------------|----------|
| **T1** | Hardcoded passwords in `scripts.yaml`, `cm.yaml`, and `conn.py` | **Cryptography and Secret Management:** Migrate credentials to a Secret Manager (HashiCorp Vault, AWS Secrets Manager, or Kubernetes Secrets) | 🔴 Immediate |
| **T2** | RBAC failure in Service Account for HUE/Airflow | **Principle of Least Privilege:** Review roles and bind them strictly to namespaces; disable unnecessary token automounting | 🔴 Immediate |
| **T3** | Missing CPU/Memory limits in Kubernetes manifests | **Availability (Denial of Service):** Implement ResourceQuotas and LimitRanges in the cluster to prevent resource exhaustion by malicious containers | 🟠 Short-term |
| **T4** | No injection vulnerability detected | **Defense in Depth:** Maintain query parameterization and output encoding to prevent regressions | 🟢 Accept |
| **T5** | Insufficient audit visibility | **Logging and Monitoring:** Implement alerts for failed login attempts and unauthorized access to sensitive assets | 🟠 Short-term |
| **T6–T15** | Additional threats from Threat Library | Apply Mitigate strategy following CAPEC mapping from Stage VI | 🟡 Medium-term |

---

## 4. Risk Treatment Strategy — Activity 7.4

| Strategy | Count | Threats | Description |
|----------|-------|---------|-------------|
| 🟠 **Mitigate** | 13 | T01, T02, T03, T05, T06, T07, T08, T09, T10, T11, T12, T13, T14 | Implement technical and organizational countermeasures to reduce probability and impact |
| 🟢 **Accept** | 2 | T04, T15 | Residual risk is tolerable after defense-in-depth controls are maintained |

### Priority Action Plan

#### Immediate Actions (Critical — address before next deployment)

1. **Remove all hardcoded credentials** — rotate and migrate to Kubernetes Secrets + HashiCorp Vault  
   *Affects: `hue/cm.yaml` (L964, L1702), `hue/conn.py` (L18), `superset/scripts.yaml` (L32)*

2. **Enforce RBAC with strict namespace binding** — disable automounting of unnecessary tokens on all Service Accounts  
   *Affects: `hue/hue.yaml`, `superset/bash.yaml`, `superset/redis.yaml`*

3. **Implement MFA** on all consumption interfaces: Superset, Jupyter, HUE, Airflow, DataHub

#### Short-term Actions (within 30 days)

4. **Add ResourceQuotas and LimitRanges** to all Kubernetes namespaces  
   *Prevents DoS via resource exhaustion from Jupyter/Spark*

5. **Configure centralized, immutable log storage** with real-time alerting on failed authentication and unauthorized data access

6. **Integrate Trivy/Snyk** into the CI/CD pipeline for automated vulnerability analysis of Docker images

#### Medium-term Actions (within 90 days)

7. **Deploy Apache Ranger** for fine-grained data access control (Iceberg/S3 RBAC)

8. **Enforce TLS 1.3** across all internal component communications (Airflow ↔ Spark, Spark ↔ Storage)

9. **Implement quarterly access review** for all researcher and collaborator accounts

10. **ClamAV integration** on HDFS for malware scanning of uploaded files via HUE

---

## 5. Final Risk Summary

```
Residual Risk Overview — reBi0s (Stage VII):

T01 — Access Control Failures:     RR = 1.26  🔴🔴 CRITICAL — Immediate action required
T03 — Injection:                    RR = 1.00  🔴   High
T04 — Software/Data Integrity:      RR = 0.89  🔴   High
T08 — Insufficient Logging:         RR = 0.72  🔴   High
T02 — Weak Cryptography:            RR = 0.54  🟡   Medium
T07 — Authentication Failures:      RR = 0.11  🟢   Low (controls effective)

Root Cause of Critical Issues:
  → Hardcoded credentials in Kubernetes YAML manifests (SonarQube BLOCKER × 4)
  → Incomplete RBAC on Service Accounts (SonarQube MAJOR)
  → Missing container resource limits (SonarQube MAJOR × multiple)
```

---

## 6. Countermeasure-to-Framework Mapping

| Countermeasure | OWASP Mapping | CAPEC Pattern |
|----------------|--------------|---------------|
| Kubernetes Secrets + HashiCorp Vault | A05:2021 — Security Misconfiguration | CAPEC-212 |
| RBAC + Least Privilege | A01:2021 — Broken Access Control | CAPEC-1 |
| MFA + Session Management | A07:2021 — Auth Failures | CAPEC-115 |
| ResourceQuotas + LimitRanges | A09:2021 — Logging Failures | CAPEC-81 (DoS vector) |
| TLS 1.3 + AES-256 | A02:2021 — Cryptographic Failures | CAPEC-97 |
| Prepared Statements + JSON Schema | A03:2021 — Injection | CAPEC-66 |
| Trivy/Snyk in CI/CD | A06:2021 — Outdated Components | CAPEC-549 |
| ClamAV on HDFS | A08:2021 — Integrity Failures | CAPEC-437 |
| Immutable Logs + SIEM | A09:2021 — Logging Failures | CAPEC-81 |
| Kubernetes Network Policies | A10:2021 — SSRF | CAPEC-664 |

---

*Source: `fase_4__5_6_7.xlsx` (sheets: `7.2`, `7.3`, `7.4`) · PASTA Stage VII — Risk & Impact Analysis (RIA)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
