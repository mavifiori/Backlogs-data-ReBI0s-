# Stage VI — Attack Modeling & Simulation (AMS)

## Overview

**Stage VI** models and simulates attack scenarios per component, assesses exploitability
via targeted tests, and derives attack cases mapped to CAPEC patterns.

## Activities

| Activity | Description |
|----------|-------------|
| 6.1 | Update attack/vector library and control framework |
| 6.2 | Identify attack surface and enumerate attack vectors |
| 6.3 | Assess probability and impact of each attack scenario |
| 6.4 | Derive a set of cases to test existing countermeasures |
| 6.5 | Conduct security tests and simulations |

## Attack Simulation Results (Activity 6.4)

| Scenario | Stack Tested | Positive Result | Risk / Vulnerability Found |
|----------|-------------|----------------|---------------------------|
| C1 — Ingestion | Spark, Airflow, Iceberg | API 200 OK; tasks accepted | BLOCKER: Duplicated SQL literals in `loading.py` |
| C2 — SQL Consumption | DataHub, HUE, Superset, PostgreSQL | Security Active (HTTP 405) | BLOCKER: Plaintext passwords in YAML manifests |
| C3 — Data Science | Jupyter, Superset, Spark | XSS: False (protected) | MAJOR: No CPU/RAM limits on containers |
| C4 — Catalog | DataHub, Airflow, Iceberg | GMS processing OK | Risk: Metadata injection via captured token |
| C5 — Data Quality | DataHub | Special chars not reflected | None critical — app layer resistant |
| C6 — Administration | Airflow, Spark, DataHub | Risk identified | CRITICAL: Compromised password in `hue/conn.py` |

**Test Artifacts Referenced:**
`resultado_postjsonair.txt` · `resultado_postsuper.txt` · `resultado_getssuper.txt`
`datahub_upload_test.py` · `resultado_getair.txt` · `resultado_getdata.txt`

## CAPEC Attack Case Mapping (Activity 6.5)

| Threat | CAPEC | Pattern | Priority Attack Vector | Countermeasure |
|--------|-------|---------|----------------------|----------------|
| T01 | CAPEC-1 | Accessing/Intercepting/Modifying HTTP Cookies | ACL bypass in Iceberg/S3 | RBAC via Apache Ranger; SIEM audit logging |
| T02 | CAPEC-97 | Cryptanalysis of Cellular Phone Communication | Spark/Airflow traffic interception | TLS 1.3 everywhere; AES-256 at rest |
| T03 | CAPEC-66 | SQL Injection | Inputs via APIs or Superset | Prepared Statements; JSON Schema validation |
| T04 | CAPEC-437 | Poisoning Web Caches / Upload Exploit | Zip Bomb / Malware upload via HUE | ClamAV on HDFS; backend recursion limit |
| T05 | CAPEC-212 | Functionality Misuse | Plaintext credentials in Kubernetes YAMLs | Kubernetes Secrets + HashiCorp Vault |
| T06 | CAPEC-549 | Local Execution of Code | CVEs in Docker base images | Trivy/Snyk in CI/CD; monthly image updates |
| T07 | CAPEC-115 | Authentication Abuse | Exposed tokens; no MFA on interfaces | MFA mandatory; 90-day key rotation |
| T08 | CAPEC-81 | Web Server Logs Tampering | Deletion / manipulation of audit logs | Immutable log storage; real-time alerts |
| T09 | CAPEC-664 | Server Side Request Forgery | SSRF via Airflow API endpoints | Kubernetes Network Policies; egress filtering |
| T11 | CAPEC-121 | Exploit Non-Production Interfaces | Privilege abuse by legitimate users | Least Privilege principle; quarterly access review |

## Artifacts

| File | Description |
|------|-------------|
| `attack-cases-capec.md` | Full CAPEC-mapped attack cases |
| `../../artifacts/scenarios/` | Simulation scenarios and test references |
