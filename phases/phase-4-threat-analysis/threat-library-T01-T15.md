# Stage IV — Threat Analysis (TA)
## Threat Library T01–T15 — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifacts:** `fase_4__5_6_7.xlsx` (sheets: `4.44.5– Update the threat libra`, `4.6- probabilistic values for i`, `OWASP Catalog`, `Categoria STRIDE`)  
> **PASTA stage:** Stage IV — Threat Analysis (TA)

---

## 1. Overview

Stage IV identifies, classifies, and maps all relevant internal and external threats to reBi0s assets. The Threat Library was built using:

- **OWASP Top 10 Web (2021)** — web vulnerability classification
- **OWASP Data Security Top 10 (2023)** — data-specific threat reference
- **STRIDE** — threat categorization model (Microsoft SDL)

**Total threats identified: 15 (T01–T15)**

---

## 2. Risk Score Formula

```
Risk Score (RS) = (PV × PA × I) / C

  PV = Probability of Vulnerability occurrence
  PA = Probability of Threat Agent exploitation  
  I  = Impact on CIA triad (0.1 = Low, 0.5 = Medium, 0.8 = High, 1.0 = Critical)
  C  = Control/Countermeasure effectiveness (0.1 = Low, 0.5 = Partial, 0.9 = Effective)
```

**Risk Scale:**

| Score Range | Level |
|-------------|-------|
| 0.00 – 0.29 | 🟢 Low |
| 0.30 – 0.59 | 🟡 Medium |
| 0.60 – 0.89 | 🔴 High |
| 0.90 – 1.00 | 🔴🔴 Critical |

---

## 3. Threat Library — T01 to T15

### T01 — Access Control Failures

| Field | Value |
|-------|-------|
| **Type** | Internal / External |
| **Affected Assets** | PostgreSQL, Apache Iceberg, Cloud Storage, front-ends |
| **Threat Agents** | Hackers, collaborators, researchers, IT staff |
| **Attack Vector** | Direct URLs, login bypass, ACL failures |
| **Existing Controls** | Strong authentication, RBAC, access logs |
| **STRIDE** | Information Disclosure, Elevation of Privilege |
| **OWASP Web** | A01:2021 — Broken Access Control |
| **OWASP Data** | DATA2:2023 — Broken Authentication and Access Control |
| **PV** | 0.7 | **PA** | 0.9 | **I** | 1.0 | **C** | 0.9 |
| **Risk Score** | **0.70** 🔴 High |

---

### T02 — Weak Cryptography

| Field | Value |
|-------|-------|
| **Type** | Internal / External |
| **Affected Assets** | Data in transit (Airflow, Spark) and at rest (Cloud Storage, PostgreSQL) |
| **Threat Agents** | Hackers, collaborators, researchers, IT staff |
| **Attack Vector** | HTTP communication, unencrypted disks |
| **Existing Controls** | HTTPS, TLS 1.3, AES encryption, encryption at rest |
| **STRIDE** | Information Disclosure, Tampering |
| **OWASP Web** | A02:2021 — Cryptographic Failures |
| **OWASP Data** | DATA6:2023 — Weak Cryptography |
| **PV** | 0.6 | **PA** | 0.9 | **I** | 0.5 | **C** | 0.9 |
| **Risk Score** | **0.30** 🟡 Medium |

---

### T03 — Injection

| Field | Value |
|-------|-------|
| **Type** | External |
| **Affected Assets** | PostgreSQL, DataHub metadata |
| **Threat Agents** | External hackers |
| **Attack Vector** | Malicious inputs via APIs or consumption tools |
| **Existing Controls** | Input validation, secure ORM, prepared statements |
| **STRIDE** | Tampering, Elevation of Privilege |
| **OWASP Web** | A03:2021 — Injection |
| **OWASP Data** | DATA1:2023 — Injection Attacks |
| **PV** | 0.5 | **PA** | 1.0 | **I** | 1.0 | **C** | 0.9 |
| **Risk Score** | **0.56** 🟡 Medium |

---

### T04 — Software and Data Integrity Failures

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | File servers, pipelines, object storage |
| **Threat Agents** | Collaborators, internal processing |
| **Attack Vector** | Malicious file uploads, misconfigured pipelines |
| **Existing Controls** | File validation, antivirus, manual curation, versioning |
| **STRIDE** | Tampering, Repudiation |
| **OWASP Web** | A08:2021 — Software and Data Integrity Failures |
| **PV** | 1.0 | **PA** | 1.0 | **I** | 0.8 | **C** | 0.9 |
| **Risk Score** | **0.89** 🔴 High |

---

### T05 — Security Misconfiguration

| Field | Value |
|-------|-------|
| **Type** | Internal / External |
| **Affected Assets** | Airflow, Spark Workers, PostgreSQL, Cloud Storage |
| **Threat Agents** | Internal admin or external attacker |
| **Attack Vector** | Default credentials, open ports, public buckets |
| **Existing Controls** | Hardening, strong authentication, firewall, config as code |
| **STRIDE** | Elevation of Privilege, Information Disclosure |
| **OWASP Web** | A05:2021 — Security Misconfiguration |
| **⚠️ SonarQube** | BLOCKER: hardcoded passwords in `hue/cm.yaml` (L964, L1702), `hue/conn.py` (L18), `superset/scripts.yaml` (L32) |
| **PV** | 0.3 | **PA** | 0.4 | **I** | 0.7 | **C** | 0.2 |
| **Risk Score** | **0.42** 🟡 Medium |

---

### T06 — Vulnerable and Outdated Components

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | Airflow, Spark, PostgreSQL, Iceberg, DataHub |
| **Threat Agents** | DevOps, SysAdmin |
| **Attack Vector** | Outdated dependencies, vulnerable Docker images |
| **Existing Controls** | CI/CD with dependency analysis, continuous updates |
| **STRIDE** | Tampering, Elevation of Privilege |
| **OWASP Web** | A06:2021 — Vulnerable and Outdated Components |
| **PV** | 0.3 | **PA** | 0.2 | **I** | 0.5 | **C** | 0.1 |
| **Risk Score** | **0.30** 🟡 Medium |

---

### T07 — Authentication and Session Control Failures

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | Superset, Jupyter, HUE, Airflow, DataHub |
| **Threat Agents** | Hackers, collaborators, researchers, IT staff |
| **Attack Vector** | Login without MFA, persistent sessions, exposed tokens |
| **Existing Controls** | MFA, session control, automatic logout |
| **STRIDE** | Spoofing, Elevation of Privilege |
| **OWASP Web** | A07:2021 — Identification and Authentication Failures |
| **OWASP Data** | DATA2:2023 — Broken Authentication and Access Control |
| **⚠️ SonarQube** | MAJOR: Service Account RBAC incomplete in `hue/hue.yaml` and `superset/bash.yaml` |
| **PV** | 0.3 | **PA** | 0.4 | **I** | 0.8 | **C** | 0.5 |
| **Risk Score** | **0.19** 🟢 Low |

---

### T08 — Insufficient Logging and Monitoring

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | Airflow, Spark, PostgreSQL, Superset |
| **Threat Agents** | All actors |
| **Attack Vector** | Critical actions not audited; traceability failures |
| **Existing Controls** | Structured logging, alerts, SIEM |
| **STRIDE** | Repudiation |
| **OWASP Web** | A09:2021 — Security Logging and Monitoring Failures |
| **⚠️ SonarQube** | MAJOR: missing CPU/memory/storage limits (DoS vector) in multiple Kubernetes manifests |
| **PV** | 0.9 | **PA** | 0.8 | **I** | 0.1 | **C** | 0.5 |
| **Risk Score** | **0.14** 🟢 Low |

---

### T09 — Server-Side Request Forgery (SSRF)

| Field | Value |
|-------|-------|
| **Type** | External |
| **Affected Assets** | Cloud Object Storage, internal APIs |
| **Threat Agents** | External hackers |
| **Attack Vector** | Malicious requests via exposed endpoints |
| **Existing Controls** | URL whitelisting, input validation |
| **STRIDE** | Information Disclosure |
| **OWASP Web** | A10:2021 — Server-Side Request Forgery (SSRF) |
| **PV** | 0.2 | **PA** | 1.0 | **I** | 1.0 | **C** | 0.5 |
| **Risk Score** | **0.40** 🟡 Medium |

---

### T10 — Malware and Ransomware Attacks

| Field | Value |
|-------|-------|
| **Type** | Internal / External |
| **Affected Assets** | Servers, files |
| **Threat Agents** | Hackers or malicious collaborators |
| **Attack Vector** | Phishing, file payloads, remote access |
| **Existing Controls** | Antivirus, backup, strong authentication, updates |
| **STRIDE** | Denial of Service, Tampering |
| **OWASP Data** | DATA4:2023 — Malware and Ransomware Attacks |
| **PV** | 0.2 | **PA** | 0.5 | **I** | 1.0 | **C** | 0.1 |
| **Risk Score** | **1.00** 🔴🔴 Critical |

---

### T11 — Insider Threats

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | Sensitive data, pipelines, dashboards |
| **Threat Agents** | Legitimate users (collaborators, researchers, IT) |
| **Attack Vector** | Privilege misuse, unauthorized data extraction |
| **Existing Controls** | Monitoring, training, RBAC, segregation of duties |
| **STRIDE** | Information Disclosure |
| **OWASP Data** | DATA5:2023 — Insider Threats |
| **PV** | 0.7 | **PA** | 1.0 | **I** | 0.5 | **C** | 0.9 |
| **Risk Score** | **0.39** 🟡 Medium |

---

### T12 — Insecure Data Handling

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | Pipelines (Airflow, Spark), logs, backups |
| **Threat Agents** | Developers or operators |
| **Attack Vector** | Missing anonymization, logs containing sensitive data, open backups |
| **Existing Controls** | Privacy policies, anonymization, access control |
| **STRIDE** | Information Disclosure |
| **OWASP Data** | DATA7:2023 — Insecure Data Handling |
| **PV** | 0.4 | **PA** | 0.6 | **I** | 1.0 | **C** | 0.9 |
| **Risk Score** | **0.27** 🟢 Low |

---

### T13 — Inadequate Third-Party Security

| Field | Value |
|-------|-------|
| **Type** | External |
| **Affected Assets** | APIs, DataHub, Superset, Cloud Storage |
| **Threat Agents** | Third parties |
| **Attack Vector** | Insecure integrations and SDKs; misconfigured APIs |
| **Existing Controls** | Vendor assessments, audits, SLA contracts |
| **STRIDE** | Spoofing, Tampering |
| **OWASP Data** | DATA8:2023 — Inadequate Third-Party Security |
| **PV** | 0.5 | **PA** | 0.5 | **I** | 0.5 | **C** | 0.5 |
| **Risk Score** | **0.25** 🟢 Low |

---

### T14 — Data Inventory and Management Gaps

| Field | Value |
|-------|-------|
| **Type** | Internal |
| **Affected Assets** | DataHub, Cloud Storage, Iceberg, PostgreSQL |
| **Threat Agents** | Internal users |
| **Attack Vector** | Uncatalogued data, sensitive data in wrong locations |
| **Existing Controls** | DataHub, CMDB, data governance |
| **STRIDE** | Information Disclosure |
| **OWASP Data** | DATA9:2023 — Data Inventory and Data Management |
| **PV** | 0.6 | **PA** | 0.6 | **I** | 1.0 | **C** | 0.9 |
| **Risk Score** | **0.40** 🟡 Medium |

---

### T15 — Non-Compliance with Regulations (LGPD/GDPR)

| Field | Value |
|-------|-------|
| **Type** | Legal / Internal |
| **Affected Assets** | Personal data in the ecosystem |
| **Threat Agents** | Collaborators, researchers, IT staff |
| **Attack Vector** | Processes without privacy-by-design or privacy-by-default |
| **Existing Controls** | DPO, audits, privacy policies, training |
| **STRIDE** | Repudiation, Information Disclosure |
| **OWASP Data** | DATA10:2023 — Non-Compliance with Data Protection Regulations |
| **PV** | 0.2 | **PA** | 0.3 | **I** | 0.1 | **C** | 0.1 |
| **Risk Score** | **0.06** 🟢 Low |

---

## 4. Threat Summary Table

| ID | Threat | Type | OWASP Web | OWASP Data | STRIDE | RS | Level |
|----|--------|------|-----------|-----------|--------|-----|-------|
| T01 | Access Control Failures | Int/Ext | A01:2021 | DATA2:2023 | Info. Disclosure, EoP | 0.70 | 🔴 High |
| T02 | Weak Cryptography | Int/Ext | A02:2021 | DATA6:2023 | Info. Disclosure, Tampering | 0.30 | 🟡 Medium |
| T03 | Injection | External | A03:2021 | DATA1:2023 | Tampering, EoP | 0.56 | 🟡 Medium |
| T04 | Software/Data Integrity Failures | Internal | A08:2021 | — | Tampering, Repudiation | 0.89 | 🔴 High |
| T05 | Security Misconfiguration | Int/Ext | A05:2021 | — | EoP, Info. Disclosure | 0.42 | 🟡 Medium |
| T06 | Vulnerable/Outdated Components | Internal | A06:2021 | — | Tampering, EoP | 0.30 | 🟡 Medium |
| T07 | Authentication & Session Failures | Internal | A07:2021 | DATA2:2023 | Spoofing, EoP | 0.19 | 🟢 Low |
| T08 | Insufficient Logging & Monitoring | Internal | A09:2021 | — | Repudiation | 0.14 | 🟢 Low |
| T09 | SSRF | External | A10:2021 | — | Info. Disclosure | 0.40 | 🟡 Medium |
| T10 | Malware & Ransomware | Int/Ext | — | DATA4:2023 | DoS, Tampering | 1.00 | 🔴🔴 Critical |
| T11 | Insider Threats | Internal | — | DATA5:2023 | Info. Disclosure | 0.39 | 🟡 Medium |
| T12 | Insecure Data Handling | Internal | — | DATA7:2023 | Info. Disclosure | 0.27 | 🟢 Low |
| T13 | Inadequate Third-Party Security | External | — | DATA8:2023 | Spoofing, Tampering | 0.25 | 🟢 Low |
| T14 | Data Inventory & Management Gaps | Internal | — | DATA9:2023 | Info. Disclosure | 0.40 | 🟡 Medium |
| T15 | LGPD Non-Compliance | Legal/Int | — | DATA10:2023 | Repudiation, Info. Disclosure | 0.06 | 🟢 Low |

---

## 5. OWASP Reference Catalog

### OWASP Top 10 Web (2021) — Applied to reBi0s

| Code | Risk | reBi0s Application |
|------|------|--------------------|
| A01:2021 | Broken Access Control | Users accessing or downloading datasets without adequate permission, including sensitive or unpublished data |
| A02:2021 | Cryptographic Failures | Data at rest or in transit not properly encrypted; use of obsolete protocols (e.g., HTTP, TLS 1.0) |
| A03:2021 | Injection | SQL/XML injection in search forms |
| A05:2021 | Security Misconfiguration | Repository exposed with default credentials, open directories, publicly accessible admin interfaces |
| A06:2021 | Vulnerable and Outdated Components | Use of plugins, libraries, or platforms (e.g., Java, PostgreSQL) with known CVEs without patches |
| A07:2021 | Identification and Authentication Failures | Login without MFA, reusable and predictable API tokens, sessions that do not expire properly |
| A08:2021 | Software and Data Integrity Failures | Datasets altered without registration or verification; no use of hashes or checksums for validation |
| A09:2021 | Security Logging and Monitoring Failures | Critical actions (upload, edit, download of sensitive data) not audited or alerted |
| A10:2021 | Server-Side Request Forgery (SSRF) | Submission of URLs used internally, potentially enabling access to internal services |

### OWASP Data Security Top 10 (2023) — Applied to reBi0s

| Code | Risk | reBi0s Application |
|------|------|--------------------|
| DATA1:2023 | Injection Attacks | Unauthorized individuals exploiting vulnerabilities to inject malicious code |
| DATA2:2023 | Broken Authentication and Access Control | Weak authentication mechanisms and misconfigured access controls |
| DATA3:2023 | Data Breaches | Leakage or theft of sensitive data |
| DATA4:2023 | Malware and Ransomware Attacks | Malicious software compromising data availability and integrity |
| DATA5:2023 | Insider Threats | Malicious or accidental actions by authorized users |
| DATA6:2023 | Weak Cryptography | Inadequate encryption, use of weak algorithms or poor key management |
| DATA7:2023 | Insecure Data Handling | Inadequate storage, transmission, or disposal of sensitive data |
| DATA8:2023 | Inadequate Third-Party Security | Lack of effective security measures by third-party integrations |
| DATA9:2023 | Data Inventory and Data Management | Incomplete inventory or poor management of data assets |
| DATA10:2023 | Non-Compliance with Data Regulations | Non-compliance with data protection laws |

---

## 6. Feeds Into

| Stage | How Stage IV outputs are consumed |
|-------|----------------------------------|
| Stage V (WVA) | Threat IDs (T01–T15) used to correlate SonarQube vulnerabilities |
| Stage VI (AMS) | Threat IDs drive CAPEC attack case selection |
| Stage VII (RIA) | Probabilistic values (PV, PA, I, C) feed residual risk formula |

---

*Source: `fase_4__5_6_7.xlsx` (sheets: `4.44.5`, `4.6`, `OWASP Catalog`, `Categoria STRIDE`) · PASTA Stage IV — Threat Analysis (TA)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
