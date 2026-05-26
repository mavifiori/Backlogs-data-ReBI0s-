# Stage III — Application Decomposition and Analysis (ADA)
## Interaction Scenarios & Trust Boundaries — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifact:** `fase_3_-_analise_e.docx`  
> **PASTA stage:** Stage III — Application Decomposition and Analysis (ADA)

---

## 1. Purpose

The Application Decomposition document for reBi0s is an artifact produced through the activities of **Stage III — Application Decomposition and Analysis (ADA)**. The document provides an understanding of how the components of the threat model under construction are related, decomposing the platform into interaction scenarios, data flows, and Trust Boundaries.

---

## 2. Methodological Note — Use Cases & Abuse Cases

> **Adaptation — Stage III:**  
> Standard PASTA methodology calls for formal UML Use Case and Abuse Case diagrams.  
> In this research, these were replaced by **general interaction and threat scenarios** (C1–C6).  
>
> **Rationale:**
> - More aligned with the data pipeline architectural context of reBi0s
> - Easier to maintain and update as the system evolves
> - Directly traceable to threats in Stage IV (Threat Library T01–T15)
> - Usable as test case references in Stage VI (Attack Modeling)
>
> Each scenario describes actor interactions, the components involved, the trust boundary crossed, and the threats most likely to materialize in that context.

---

## 3. General Context

Data management in reBi0s is applied across multiple scenarios, promoting efficiency and collaboration in areas such as health, agriculture, and climate. Researchers use tools such as DataHub, Apache Airflow, and HUE for data curation, processing, and analysis, while tools such as Apache Superset enable visualization and informed decision-making. Governance policies ensure secure and organized access, while storage and consumption optimization ensures high operational performance.

---

## 4. Interaction Scenarios (C1–C6)

### Scenario C1 — Data Ingestion

**Actors:** Data collaborators, system processes  
**Components:** Apache Airflow, Apache Spark (Master + Workers), Apache Iceberg, DataHub  
**Trust Boundary:** Curadoria (Airflow/Spark boundary)

**Description:**  
The Spark Master (Apache Spark) adds tasks to read data from sources (e.g., SINASC). Spark Workers (via Apache Airflow) process the data and store it in the Data Lake using Apache Iceberg format. DataHub is updated with metadata about the ingested and processed data.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T03 | Injection (SQL/API) | Malicious inputs via data sources or ETL pipelines |
| T02 | Weak Cryptography | Unencrypted data in transit between Airflow and storage |
| T12 | Insecure Data Handling | Logs containing sensitive data; unprotected backups |
| T14 | Data Inventory Gaps | Incomplete metadata ingestion in DataHub |
| T05 | Security Misconfiguration | YAML credentials exposed; open Airflow ports |
| T06 | Vulnerable/Outdated Components | Outdated Airflow/Spark images with known CVEs |

---

### Scenario C2 — Data Consumption via SQL

**Actors:** Researchers, visitors, community  
**Components:** DataHub, HUE, Apache Superset, PostgreSQL, Apache Spark  
**Trust Boundary:** Colaboração Aberta (HUE/Superset ↔ Spark)

**Description:**  
Researchers/visitors/community query DataHub (the graphical interface) to find data. HUE works in the backend to run SQL queries directly against Apache Iceberg (Data Lake) and PostgreSQL. Apache Superset handles visualization and data exploration through dashboards and charts. HUE and Apache Superset return data to the Spark Master.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T14 | Data Inventory Gaps | Inability to find or catalog consumed data |
| T03 | Injection (SQL/API) | SQL injection via HUE or Superset query interfaces |
| T10 | Malware & Ransomware | Malicious files uploaded via consumption interfaces |

---

### Scenario C3 — Data Consumption via Jupyter

**Actors:** Data scientists  
**Components:** Jupyter, Apache Superset, Apache Spark  
**Trust Boundary:** Colaboração Aberta (Jupyter ↔ Spark)

**Description:**  
Data scientists query data through Jupyter using Python and R languages. Apache Superset handles visualization and exploration through dashboards and charts. Jupyter and Apache Superset return data to the Spark Master.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T09 | SSRF | Malicious requests via Jupyter API endpoints accessing internal services |
| T01 | Access Control Failures | Insufficient session isolation; missing MFA on Jupyter |
| T12 | Insecure Data Handling | Notebooks storing sensitive data or credentials in plain text |
| T11 | Insider Threats | Legitimate user misusing notebook access to exfiltrate data |
| T04 | Software/Data Integrity | Malicious scripts injected into Jupyter notebooks |

---

### Scenario C4 — Catalog Management

**Actors:** Collaborators, system processes  
**Components:** DataHub, Apache Airflow, Apache Iceberg  
**Trust Boundary:** Governança (DataHub boundary)

**Description:**  
Collaborators submit data. DataHub automatically ingests metadata for the new dataset (table name, format, schema — column names and types — and storage location). The Spark Master adds tasks and Spark Workers (via Apache Airflow) process the data and store it in the Data Lake using Apache Iceberg.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T12 | Insecure Data Handling | Incorrect or incomplete metadata submitted by collaborators |
| T14 | Data Inventory Gaps | Metadata not properly cataloged or validated |
| T15 | LGPD Non-Compliance | Sensitive data submitted without proper anonymization or consent |
| T11 | Insider Threats | Collaborator submitting malicious or falsified data |

---

### Scenario C5 — Data Quality

**Actors:** Researchers, data engineers  
**Components:** DataHub  
**Trust Boundary:** Governança (DataHub boundary)

**Description:**  
Researchers and data engineers verify and may complement metadata ingested by collaborators to ensure the quality of the ingested data.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T12 | Insecure Data Handling | Inadvertent exposure or corruption of metadata during quality checks |
| T11 | Insider Threats | Legitimate user deliberately modifying quality metadata to obscure data issues |

---

### Scenario C6 — Administration

**Actors:** Administrator  
**Components:** Apache Airflow, Apache Spark, DataHub  
**Trust Boundary:** All boundaries (cross-cutting)

**Description:**  
The administrator monitors tools to ensure orchestrations are running without failures (Apache Airflow) and that resources are optimized and scaled for large-scale data processing (Apache Spark). Additionally, using DataHub, the administrator manages permissions, ensures compliance with access policies, and audits data, guaranteeing system security.

**Threats mapped (Stage IV):**

| Threat ID | Threat | Rationale |
|-----------|--------|-----------|
| T01 | Access Control Failures | Admin account with excessive privileges; missing MFA |
| T12 | Insecure Data Handling | Admin actions not fully audited |
| T08 | Insufficient Logging | Missing or incomplete audit logs for admin operations |
| T15 | LGPD Non-Compliance | Admin actions affecting personal data without compliance logging |

---

## 5. Trust Boundaries

| Boundary | Components | Primary Threats | Security Controls |
|----------|-----------|-----------------|-------------------|
| **Data Governance** | DataHub | Unauthorized access; integrity violation | Authentication, authorization, audit, monitoring |
| **Data Curation** | Airflow, Spark, Iceberg, PostgreSQL | Data corruption; access policy violations | Quality validation, data encryption |
| **Open Collaboration** | HUE, Jupyter, Superset | DoS attacks; quality policy violations | Anti-DoS protection, data encryption |
| **Storage & Consumption** | Spark, Airflow, Iceberg, MinIO, PostgreSQL, Superset | Sensitive data manipulation and exposure | Encryption, authentication, authorization |

---

## 6. Component–Actor Mapping

| Actor | Primary Interactions | Trust Boundary |
|-------|---------------------|----------------|
| **Researcher / Data Scientist** | DataHub (catalog), HUE (SQL), Jupyter (analysis), Superset (viz) | Open Collaboration |
| **Collaborator** | DataHub (metadata upload), Airflow-triggered ingestion | Data Governance |
| **Data Engineer** | DataHub (quality), Airflow (pipeline config), Spark (job tuning) | Data Curation |
| **Administrator** | Airflow (DAG monitoring), Spark (resource management), DataHub (permissions) | All boundaries |
| **System Processes** | Airflow → Spark → Storage (automated pipelines) | Data Curation |

---

## 7. Feeds Into

| Stage | How Stage III outputs are consumed |
|-------|----------------------------------|
| Stage IV (TA) | Scenarios C1–C6 define the threat surface per component and actor |
| Stage V (WVA) | Trust Boundaries map to vulnerability scan scopes in SonarQube |
| Stage VI (AMS) | Scenarios become attack simulation test cases |

---

*Source: `fase_3_-_analise_e.docx` · PASTA Stage III — Application Decomposition and Analysis (ADA)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
