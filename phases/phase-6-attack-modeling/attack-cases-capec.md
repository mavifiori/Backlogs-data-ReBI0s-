# Stage VI — Attack Modeling & Simulation (AMS)
## Attack Cases, Simulation Results & CAPEC Mapping — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifacts:** `fase_4__5_6_7.xlsx` (sheets: `6.4Assess the probability and i`, `6.5 Derive a Set of Attack Case`, `6.6 security tests and simulati`)  
> **PASTA stage:** Stage VI — Attack Modeling & Simulation (AMS)

---

## 1. Overview

Stage VI models and simulates attack scenarios per component, assesses exploitability via directed security tests, and derives attack cases mapped to CAPEC patterns. The simulation was conducted against the reBi0s staging environment.

---

## 2. Attack Simulation Results — Activity 6.4

Simulations were conducted across the 6 interaction scenarios defined in Stage III (ADA).

| Scenario | Applications | Test Objective | Expected Result | Positive Result | Risk / Vulnerability Found | Source Files |
|----------|-------------|----------------|-----------------|----------------|---------------------------|--------------|
| **1. Ingestion** | Spark, Airflow, Iceberg | Validate orchestration integrity and Data Lake flow | Processing without failures; secure storage | ✅ API operational (200 OK); tasks accepted | Code Smells: SQL duplication and table names in `loading.py` | `resultado_postjsonair.txt`, `Relatorio_Issues_Unicas_reBi0s.pdf` |
| **2. SQL Consumption** | DataHub, HUE, Superset, PostgreSQL | Ensure SQL access does not expose credentials | Restricted access with data sanitization | ✅ Security Active: Blocking of unauthorized methods (405) | 🔴 BLOCKER: Plaintext passwords in Kubernetes YAML manifests | `resultado_postsuper.txt`, `Relatorio_Issues_Unicas_reBi0s.pdf` |
| **3. Data Science** | Jupyter, Superset, Spark | Validate isolation and return to Master Spark | Data returned without execution of malicious scripts | ✅ Protected: No script reflection (XSS: False) | 🟠 MAJOR: Missing resource limits (CPU/RAM) on containers | `resultado_getssuper.txt`, `Relatorio_Issues_Unicas_reBi0s.pdf` |
| **4. Catalog** | DataHub, Airflow, Iceberg | Validate automatic metadata ingestion | Correct schema mapping without false metadata | ✅ Validated: DataHub processing via GMS successfully | Risk: Ingestion of unvalidated metadata via captured token | `datahub_upload_test.py`, `resultado_getdata.txt` |
| **5. Quality** | DataHub | Ensure manual metadata editing is secure | Blocking of scripts in description fields | ✅ Security Validated: System does not reflect special characters | None critical — application layer resisted tests | `resultado_postdata.txt`, `resultado_postjsondata.txt` |
| **6. Admin** | Airflow, Spark, DataHub | Monitor orchestration and manage permissions | RBAC policies applied; resources scaled | ⚠️ Risk Identified: Service Accounts mounted without strict binding | 🔴 CRITICAL: Compromised password in `hue/conn.py` | `Relatorio_Issues_Unicas_reBi0s.pdf`, `resultado_getair.txt` |

---

## 3. Security Tests by Application — Activity 6.6

### Apache Airflow

| Field | Value |
|-------|-------|
| **Objective** | Validate connectivity, authentication (Basic Auth), and REST API |
| **Test Type** | API (Black-box) — Integration and Security |
| **Expected Result** | Stable connectivity, credentials accepted, JSON/Cookie return |
| **Tools** | Python scripts (`requests`) for GET/POST calls and header inspection |
| **Result (Success)** | API operational; ingestion tasks accepted (Status 200 OK, cookies generated) |
| **Failure Scenario** | Write failure: Error 500 on API or Iceberg metadata corruption |

---

### Apache Superset

| Field | Value |
|-------|-------|
| **Objective** | Validate security against XSS/SQLi and acceptance of JSON/Form formats |
| **Test Type** | DAST (Dynamic): Attack simulation with malicious payloads |
| **Expected Result** | Identify if server reflects scripts or presents DB failures |
| **Tools** | `repeater_test.py` sending mass injection payloads |
| **Result (Success)** | Security Active: Blocking of unauthorized methods (405); no script reflection |
| **Failure Scenario** | Leakage/Injection: SQLi execution or unauthorized admin access |

---

### DataHub

| Field | Value |
|-------|-------|
| **Objective** | Validate metadata ingestion and resilience against search injection |
| **Test Type** | DAST and Integration: REST/GraphQL ingestion and search fuzzer |
| **Expected Result** | Correct ingestion of entities; blocking of payloads in search filters |
| **Tools** | `datahub_upload_test.py` and URL parameter fuzzer |
| **Result (Success)** | Security Validated: Search protected (404/200); ingestion processed correctly; XSS: False |
| **Failure Scenario** | Poisoning: Insertion of false metadata polluting the catalog |

---

### HUE (HDFS Backend)

| Field | Value |
|-------|-------|
| **Objective** | Validate backend against malicious uploads and filter evasion |
| **Test Type** | Security (Upload): Malware signature testing and ZIP bombs |
| **Expected Result** | Blocking of malicious files; prevention of Path Traversal |
| **Tools** | `hue_upload_tester.py` generating EICAR files and dangerous extensions |
| **Result (Success)** | Validated: Backend defenses resisted signature-based attacks (8 critical upload categories tested) |
| **Failure Scenario** | Ransomware: Successful upload of infected files to HDFS |

---

### HUE (Interface)

| Field | Value |
|-------|-------|
| **Objective** | Validate UI resilience against reflection attacks and XSS |
| **Test Type** | DAST (UI Fuzzing): Script injection into form fields |
| **Expected Result** | Prevention of script execution in user browser |
| **Tools** | Manual and automated simulation of injection in metadata fields |
| **Result (Success)** | Security Validated: Special characters properly escaped |
| **Failure Scenario** | Persistent XSS: Injected script in description executes in other users' browsers |

---

### Jupyter / Spark

| Field | Value |
|-------|-------|
| **Objective** | Validate resource isolation and Spark Master stability |
| **Test Type** | Stress and Availability Test |
| **Expected Result** | Computation execution without compromising global cluster resources |
| **Tools** | Memory notebook execution via Python/R |
| **Result (Warning)** | ⚠️ Risk Identified: Lack of thermal/logical container isolation; tasks executed without resource quota protection |
| **Failure Scenario** | DoS: Jupyter scripts causing Spark Master failure due to resource exhaustion |

---

## 4. CAPEC Attack Case Mapping — Activity 6.5

| Threat | CAPEC Pattern | Priority Attack Vector (reBi0s) | Technical Countermeasure | Audit Reference |
|--------|--------------|--------------------------------|--------------------------|-----------------|
| T01 | CAPEC-1 — Access Control | ACL bypass in Iceberg/S3 | RBAC via Apache Ranger or AWS Lake Formation; SIEM audit logging | `resultado_getair.txt` |
| T02 | CAPEC-97 — Cryptanalysis | Spark/Airflow traffic interception | Force TLS 1.3 on all internal communications; AES-256 disk encryption at rest | `resultado_postair.txt` |
| T03 | CAPEC-66 — SQL Injection | Inputs via APIs or Superset | Mandatory Prepared Statements; JSON Schema validation in DataHub | `resultado_postsuper.txt` |
| T04 | CAPEC-437 — Upload Exploit | Zip Bomb / Malware upload via HUE | ClamAV scanner on HDFS; limit decompression recursion in HUE backend | `hue_upload_tester.py` |
| T05 | CAPEC-212 — Functionality Misuse | Plaintext credentials in YAMLs | Migrate credentials from ConfigMaps to Kubernetes Secrets; use HashiCorp Vault | `Relatorio_Issues_Unicas_reBi0s.pdf` |
| T06 | CAPEC-549 — Component CVEs | CVEs in Docker images | Integrate Trivy/Snyk vulnerability analysis in CI/CD pipeline; monthly base image updates | `Relatorio_Issues_Unicas_reBi0s.pdf` |
| T07 | CAPEC-115 — Authentication Abuse | Exposed tokens; no MFA | Implement MFA; automatic service key rotation every 90 days | `Relatorio_Issues_Unicas_reBi0s.pdf` |
| T08 | CAPEC-81 — Log Tampering | Log deletion / manipulation | Centralize logs in immutable storage; real-time alerts for consecutive login failures | `resultado_getdata.txt` |
| T09 | CAPEC-664 — SSRF | Requests via Airflow/API endpoints | Implement Kubernetes Network Policies to restrict access to cloud instance metadata | `datahub_upload_test.py` |
| T11 | CAPEC-121 — Insider Exploit | Privilege abuse by legitimate users | Apply Principle of Least Privilege; quarterly access review for researchers and collaborators | `Relatorio_Issues_Unicas_reBi0s.pdf` |

---

## 5. Test Artifact File Index

| File | Application | Test Type | Stage |
|------|-------------|-----------|-------|
| `resultado_postjsonair.txt` | Apache Airflow | API POST — ingestion payload | VI |
| `resultado_postair.txt` | Apache Airflow | API POST — connectivity | VI |
| `resultado_getair.txt` | Apache Airflow | API GET — DAG status | VI |
| `resultado_postsuper.txt` | Apache Superset | DAST — SQLi/XSS payload | VI |
| `resultado_getssuper.txt` | Apache Superset | DAST — GET with injection | VI |
| `datahub_upload_test.py` | DataHub | Integration — metadata ingestion | VI |
| `resultado_getdata.txt` | DataHub | API GET — entity retrieval | VI |
| `resultado_postdata.txt` | DataHub | API POST — metadata edit | VI |
| `resultado_postjsondata.txt` | DataHub | API POST — JSON payload | VI |
| `hue_upload_tester.py` | HUE (HDFS) | Upload security — malware/ZIP bomb | VI |
| `Relatorio_Issues_Unicas_reBi0s.pdf` | All | SonarQube SAST — 40 issues | V / VI |

> **Note:** Test result files (`resultado_*.txt`) and test scripts should be placed in `../../artifacts/scenarios/` for reference. The SonarQube report goes in `../../artifacts/reports/`.

---

## 6. Feeds Into

| Stage | How Stage VI outputs are consumed |
|-------|----------------------------------|
| Stage VII (RIA) | Attack cases and simulation results feed residual risk quantification |
| Stage VII (RIA) | Countermeasure test cases establish the control effectiveness baseline (C) |

---

*Source: `fase_4__5_6_7.xlsx` (sheets: `6.4`, `6.5`, `6.6`) · PASTA Stage VI — Attack Modeling & Simulation (AMS)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
