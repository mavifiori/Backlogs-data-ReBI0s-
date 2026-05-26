```mermaid
---
title: "reBi0s — C4 Container Diagram (Stage II · Technical Scope)"
---
C4Container
    title reBi0s — Plataforma de Governança e Processamento de Dados

    Person(researcher, "Researcher / Data Scientist", "Queries, processes and visualizes scientific data (Health · Climate · Agriculture)")
    Person(admin, "Administrator", "Manages orchestration, permissions and system auditing")

    System_Boundary(rebios, "reBi0s Platform") {

        Container(datahub, "DataHub", "Python / React", "Manages metadata, access policies, data quality rules and governance compliance")

        Container(airflow, "Apache Airflow", "Python / Celery", "Orchestrates data ingestion and ETL transformation pipelines")

        Container(spark, "Apache Spark", "JVM / PySpark", "Centralized processing engine — Spark Streaming + Spark SQL for all analytical workloads")

        Container(storage, "Storage Layer", "MinIO · Iceberg · PostgreSQL", "Object storage (MinIO/S3), columnar lake format (Apache Iceberg), relational store (PostgreSQL)")

        Container(consumption, "Consumption Tools", "Jupyter · Superset · HUE", "Interactive science (Jupyter), BI dashboards (Superset), SQL interface (HUE)")

        Container(ansible, "Ansible", "YAML / Python", "Automates infrastructure provisioning and cluster configuration")

        Container(k8s, "Kubernetes", "Container Orchestration", "Manages all containerized workloads, secrets, RBAC and resource policies")
    }

    Rel(researcher, consumption, "Queries and visualizes data", "HTTPS")
    Rel(researcher, datahub, "Browses data catalog", "HTTPS")
    Rel(admin, airflow, "Monitors DAGs and orchestration", "HTTPS")
    Rel(admin, datahub, "Manages permissions and audits", "HTTPS")
    Rel(admin, k8s, "Manages infrastructure", "kubectl / HTTPS")

    Rel(datahub, airflow, "Applies governance policies", "gRPC / REST")
    Rel(airflow, storage, "Ingests and writes data", "JDBC / S3 API")
    Rel(airflow, spark, "Triggers processing jobs", "REST / Spark Submit")
    Rel(spark, storage, "Reads and writes processed data", "JDBC / HDFS / S3")
    Rel(consumption, spark, "Submits queries and analysis", "JDBC / REST")
    Rel(ansible, k8s, "Provisions cluster components", "SSH / kubectl")
    Rel(k8s, spark, "Schedules Spark workers", "Kubernetes API")
    Rel(k8s, airflow, "Runs Airflow executor pods", "Kubernetes API")
```

---

> **Stage II — Methodological Note**
>
> Standard PASTA methodology calls for UML Data Flow Diagrams (Level 0–3).
> For reBi0s, these were replaced by **C4 Model diagrams** at three levels:
>
> | C4 Level | File | Scope |
> |----------|------|-------|
> | System Context | `docs/images/diagrama1-system-context.png` | Researcher ↔ Platform |
> | Container | `docs/images/diagrama2-container.png` + this diagram | All containers & relations |
> | Component | `docs/images/diagrama3-component-spark.png` | Apache Spark internals |
>
> **Rationale:** C4 diagrams are better suited to distributed data architectures,
> provide natural Trust Boundary demarcation, and facilitate communication
> across technical and non-technical stakeholders.
