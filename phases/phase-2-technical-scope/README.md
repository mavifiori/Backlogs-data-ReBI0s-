# Stage II — Definition of the Technical Scope (DTS)

## Overview

**Stage II** enumerates all technical components that constitute the reBi0s platform,
defining the attack surface upon which subsequent threat analysis will operate.

> "Stage II is essentially a large enumeration project. It focuses on itemizing
> software and hardware assets in order to later define a lean and relevant attack surface."
> — UcedaVélez & Morana (2015), Ch. 7

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│           reBi0s — Big Data Platform                │
│                                                     │
│  ┌─────────┐    ┌──────────┐    ┌────────────────┐  │
│  │ DataHub │───►│ Airflow  │───►│ Storage Layer  │  │
│  │(Govern.)│    │(Orchestr)│    │MinIO·Iceberg·  │  │
│  └─────────┘    └──────────┘    │PostgreSQL      │  │
│       │                │        └───────┬────────┘  │
│       │                │                │           │
│       ▼                ▼                ▼           │
│  ┌─────────────────────────────────────────────┐   │
│  │               Apache Spark                  │   │
│  │    Master + Workers (Streaming · SQL)        │   │
│  └─────────┬───────────────────────────────────┘   │
│            │                                        │
│     ┌──────┴──────┐                                 │
│     ▼      ▼      ▼                                 │
│  Jupyter Superset HUE                               │
│  (Consumption Layer)                                │
│                                                     │
│  ┌────────────────────────────────────────────┐    │
│  │  Kubernetes + Ansible (Infrastructure)     │    │
│  └────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────┘
```

## Technology Stack

| Layer | Component | Version | Security Relevance |
|-------|-----------|---------|-------------------|
| Governance | DataHub | 0.10+ | Metadata, access policies, lineage |
| Orchestration | Apache Airflow | 2.x | DAG execution, secret management |
| Processing | Apache Spark | 3.x | Data processing, RCE attack surface |
| Storage | Apache Iceberg | 1.x | ACL at table/partition level |
| Storage | PostgreSQL | 14+ | SQL injection, credential exposure |
| Storage | MinIO | 2023+ | S3-compatible; bucket policy surface |
| Consumption | Jupyter | 7.x | Code execution, session exposure |
| Consumption | Apache Superset | 3.x | SQL injection via chart queries |
| Consumption | HUE | 4.x | SQL interface; credential in YAML |
| Infrastructure | Kubernetes | 1.27+ | RBAC, pod security, secrets |
| Automation | Ansible | 2.15+ | Playbook credential exposure |

## Data Flow (Stage II Activity 2.2)

```
[Researcher] ──► [DataHub (Governance)] ──► [Airflow (Orchestration)]
                                                      │
                    ┌─────────────────────────────────┘
                    ▼
             [MinIO / Iceberg / PostgreSQL (Storage)]
                    │
                    ▼
             [Apache Spark (Processing)]
                    │
         ┌──────────┼──────────┐
         ▼          ▼          ▼
      [Jupyter] [Superset]  [HUE]    ◄── Consumption
```

## Methodological Note

> **Adaptation:** UML Data Flow Diagrams (DFDs) were replaced by **C4 Model diagrams**
> (System Context, Container, Component levels), which provide better representation
> of distributed data architectures and facilitate communication between technical
> and business stakeholders. See `../../docs/images/` for all C4 diagrams.

## Key Artifacts

| File | Description |
|------|-------------|
| `technical-scope-document.md` | Full DTS document |
| `technology-stack.md` | Component inventory |
| `../../docs/images/diagrama1-system-context.png` | C4 System Context |
| `../../docs/images/diagrama2-container.png` | C4 Container diagram |
| `../../docs/images/diagrama3-component-spark.png` | C4 Component (Spark) |
| `../../docs/images/fluxo-arquitetura.png` | Data flow diagram |

## Feeds Into

→ Stage III (Application Decomposition) — component inventory drives scenario mapping  
→ Stage IV (Threat Analysis) — defines assets to be threatened  
→ Stage V (Vulnerability Analysis) — defines scope for SonarQube scanning  
