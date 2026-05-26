# Stage II — Definition of the Technical Scope (DTS)
## Technical Architecture Document — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifact:** `fase_2_-_escopo_tecnico.docx`  
> **PASTA stage:** Stage II — Definition of the Technical Scope (DTS)

---

## 1. Purpose

The Technical Scope document for reBi0s is an artifact produced as a result of the activities carried out in **Stage II — Definition of the Technical Scope (DTS)**. The document aims to define the initial architecture scope of the repository, describing the technical components, tools, and their interactions, thereby establishing the attack surface upon which subsequent threat analysis (Stage IV) will operate.

---

## 2. General Architecture Objective

The reBi0s project aims to develop a **Big Data architecture** to manage and consume scientific data, using open-source tooling and automated infrastructure.

### Specific Objectives

- Implement a secure repository for data governance
- Enable querying, analysis, and visualization of scientific data
- Ensure scalability and efficiency in data processing
- Automate the integration and maintenance of components

---

## 3. Technology Stack

The technologies used in the construction of the reBi0s architecture span the following technical domains:

| Domain | Component | Role | Security Relevance |
|--------|-----------|------|--------------------|
| **Data Governance** | DataHub | Manages metadata, access policies, and data quality rules | Metadata integrity; access control surface |
| **Data Orchestration** | Apache Airflow | Collects and performs initial data transformation; prepares data for storage | DAG execution; credential management in YAML configs |
| **Data Processing** | Apache Spark (Master + Workers) | Centralizes data processing; executes all queries, analyses, and workflows | RCE attack surface; resource limit exposure |
| **Data Storage** | Apache Iceberg | Columnar lake format; stores transformed data ready for Spark | ACL at table/partition level |
| **Data Storage** | PostgreSQL | Relational database; stores structured data | SQL injection; credential exposure |
| **Data Storage** | MinIO | S3-compatible object storage | Bucket policy surface; credential exposure |
| **Data Consumption** | Jupyter | Interactive data science (Python, R) | Code execution; session exposure |
| **Data Consumption** | Apache Superset | BI dashboards and data visualization | SQL injection via chart queries |
| **Data Consumption** | HUE | SQL interface for the data lake | Credential in YAML (BLOCKER — SonarQube Stage V) |
| **Infrastructure** | Kubernetes | Container orchestration | RBAC; pod security; secret management |
| **Automation** | Ansible | Infrastructure provisioning and configuration | Playbook credential exposure |

---

## 4. Data Flow Architecture

```
[Data Sources]
      │
      ▼
[Apache Airflow] ── orchestration ──► [MinIO / Apache Iceberg / PostgreSQL]
                                                      │
                                                      ▼
                                            [Apache Spark]
                                         (Master + Workers)
                                                      │
                              ┌───────────────────────┼───────────────────────┐
                              ▼                       ▼                       ▼
                          [Jupyter]            [Apache Superset]           [HUE]
                       (Python / R)            (Dashboards)          (SQL Interface)
                              │                       │                       │
                              └───────────────────────┴───────────────────────┘
                                                      │
                                                      ▼
                                              [DataHub]
                                         (Governance layer —
                                      metadata, policies, lineage)
```

### 4.1 Data Ingestion

**Apache Airflow** performs initial data collection and transformation, preparing data for storage in **MinIO, Apache Iceberg, and PostgreSQL** before processing.

### 4.2 Data Storage

**MinIO, Apache Iceberg, and PostgreSQL** store the transformed data, making it ready for processing by **Apache Spark**.

### 4.3 Data Processing

**Apache Spark** centralizes data processing, executing all queries, analyses, and workflows, ensuring data is ready for consumption.

### 4.4 Data Consumption

**Jupyter** and **Apache Superset** interact with **Apache Spark** to enable querying, analysis, and visualization of processed data. **HUE** provides a direct SQL interface to the data lake (Apache Iceberg) and PostgreSQL.

### 4.5 Data Governance

**DataHub** manages metadata, access policies, and data quality, integrating with all other tools to ensure governance and data compliance.

---

## 5. C4 Model Diagrams

In this research, UML Data Flow Diagrams (DFDs) were replaced by **C4 Model diagrams** at three levels of abstraction. This decision was made because C4 diagrams are better suited to distributed data architectures and provide clearer communication between technical and non-technical stakeholders.

> **Methodological Adaptation — Stage II:**  
> UML DFDs → C4 Model (System Context, Container, Component levels)  
> **Rationale:** C4 provides natural Trust Boundary demarcation and better represents the layered Big Data architecture of reBi0s.

| C4 Level | Diagram | Description |
|----------|---------|-------------|
| System Context | `../../docs/images/diagrama1-system-context.png` | Researcher ↔ Platform interaction |
| Container | `../../docs/images/diagrama2-container.png` | All containers and their relationships |
| Component | `../../docs/images/diagrama3-component-spark.png` | Apache Spark internal components (Spark Streaming + Spark SQL) |
| Data Flow | `../../docs/images/fluxo.png` | Full data flow across all layers |

---

## 6. Attack Surface — Initial Enumeration

Based on the technical scope, the following components define the **initial attack surface** for Stage IV (Threat Analysis):

| Component | Exposure Level | Key Attack Vectors |
|-----------|---------------|-------------------|
| Apache Airflow | High | DAG injection; YAML credential exposure; unauthorized API access |
| Apache Spark | High | Resource exhaustion (no CPU/RAM limits); RCE via worker pods |
| HUE | High | Hardcoded passwords in `cm.yaml` and `conn.py` (BLOCKER — SonarQube) |
| Apache Superset | Medium | SQL injection via dashboard queries; exposed admin endpoints |
| DataHub | Medium | Token capture; metadata poisoning |
| PostgreSQL | High | SQL injection; credential exposure in manifests |
| MinIO | Medium | Bucket policy misconfiguration; S3 API abuse |
| Kubernetes | High | RBAC misconfiguration; Service Account privilege escalation |

---

## 7. Feeds Into

| Stage | How Stage II outputs are consumed |
|-------|----------------------------------|
| Stage III (ADA) | Component inventory drives interaction scenario mapping and Trust Boundary definition |
| Stage IV (TA) | Technology stack defines assets to be threatened in the Threat Library (T01–T15) |
| Stage V (WVA) | Defines scope for SonarQube static analysis scanning |

---

*Source: `fase_2_-_escopo_tecnico.docx` · PASTA Stage II — Definition of the Technical Scope (DTS)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
