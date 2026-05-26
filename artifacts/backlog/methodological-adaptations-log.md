# Methodological Adaptations Log
## What Was Used, Adapted, or Excluded — and Why

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Purpose:** This log documents every decision taken when adapting the standard PASTA methodology  
> to the reBi0s context — fulfilling the backlog item: *"criar um log de todas as etapas: use original, adaptei/exclui e pq"*

---

## Format

Each entry follows the structure:  
`[Stage] | [Original PASTA element] | [Decision: Used / Adapted / Excluded] | [Rationale]`

---

## Stage I — Definition of Objectives (DO)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Business requirements definition | ✅ **Used as-is** | Directly applicable; reBi0s has clear business objectives around scientific data governance |
| Security requirements elicitation | ✅ **Used as-is** | Produced 11 formal security requirements from Miro board items |
| Business Impact Analysis (BIA) | ✅ **Used as-is** | CIA triad mapping applied; data classified into Public/Internal/Sensitive |
| Inherent risk profile | ✅ **Used as-is** | 7 risk scenarios documented |
| Financial impact assessment | 🔄 **Adapted** | reBi0s does not have direct financial transactions; focus shifted to research credibility, LGPD compliance, and UNICAMP ethics committee implications |
| Regulatory compliance scope | 🔄 **Adapted** | Instead of generic compliance, scoped to LGPD (Lei 13.709/2018) and UNICAMP Ethics Committee requirements specific to scientific research |

---

## Stage II — Definition of the Technical Scope (DTS)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Software component enumeration | ✅ **Used as-is** | Full inventory of 8 components: DataHub, Airflow, Spark, Iceberg, PostgreSQL, MinIO, Kubernetes, Ansible |
| Actors and data sinks/sources identification | ✅ **Used as-is** | Researchers, Collaborators, Data Engineers, Administrators identified |
| System-level services enumeration | ✅ **Used as-is** | Internal ports and services mapped |
| Third-party infrastructure enumeration | ✅ **Used as-is** | Kubernetes, Ansible, MinIO (S3-compatible) |
| **UML Data Flow Diagrams (DFDs)** | 🔄 **Adapted → C4 Model** | UML DFDs were replaced by C4 Model diagrams (System Context, Container, Component). Rationale: C4 is better suited to distributed microservice/container architectures, provides natural Trust Boundary demarcation, and communicates more clearly to non-UML audiences. Three C4 diagrams produced: System Context, Container, Component (Spark). |
| Secure technical design assertion | ✅ **Used as-is** | Verified that all components have defined security hardening baseline |

---

## Stage III — Application Decomposition and Analysis (ADA)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Data flow exercise | 🔄 **Adapted** | DFD exercise replaced by C4 Container diagram (Stage II) + data flow narrative in interaction scenarios |
| **UML Use Case Diagrams** | 🔄 **Adapted → Interaction Scenarios** | Replaced by 6 descriptive interaction scenarios (C1–C6). Rationale: formal UML use cases would add notation overhead without improving threat identification for a data pipeline architecture. Scenarios preserve the same semantic coverage (actor, system, interaction, outcome) with less formalism. |
| **UML Abuse Case Diagrams** | 🔄 **Adapted → Threat Scenarios per Scenario** | Replaced by threat mapping embedded within each interaction scenario. Each scenario (C1–C6) includes a table of applicable threats (T01–T15). Rationale: abuse cases in UML are rarely used in data engineering contexts; threat-per-scenario tables provide more direct traceability to Stage IV. |
| Trust Boundaries definition | ✅ **Used as-is** | 4 Trust Boundaries mapped: Governança, Curadoria, Colaboração Aberta, Armazenamento/Consumo |
| Security functional analysis | ✅ **Used as-is** | Security controls documented per Trust Boundary |

---

## Stage IV — Threat Analysis (TA)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Overall threat scenario analysis | ✅ **Used as-is** | Combined OWASP Top 10 Web (2021) and OWASP Data Security Top 10 (2023) as dual reference frameworks |
| Internal threat intelligence gathering | ✅ **Used as-is** | SonarQube results (Stage V) used as internal threat evidence |
| External threat intelligence gathering | ✅ **Used as-is** | CVE advisories and OWASP references used |
| Threat library update | ✅ **Used as-is** | Built iteratively across two spreadsheet versions; final library: T01–T15 |
| Threat agent to asset mapping | ✅ **Used as-is** | Each threat entry includes affected assets and threat agents |
| Probabilistic values assignment | ✅ **Used as-is** | PV, PA, I, C values assigned; Risk Score calculated per `RS = (PV × PA × I) / C` |
| **STRIDE classification** | ✅ **Used as-is** (supplementary) | Added as a complementary classification layer alongside OWASP categories, not as the primary framework — OWASP provides more data-engineering-specific categories for reBi0s |

---

## Stage V — Weakness & Vulnerability Analysis (WVA)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Existing vulnerability data review | ✅ **Used as-is** | SonarQube SAST analysis conducted on reBi0s codebase; 40 unique issues found |
| Weak design pattern identification | ✅ **Used as-is** | Missing resource limits, RBAC gaps, hardcoded credentials identified |
| Threat to vulnerability mapping | ✅ **Used as-is** | Each SonarQube finding mapped to one or more threats from T01–T15 |
| Contextual risk analysis | ✅ **Used as-is** | Risk analysis performed per interaction scenario (C1–C6) |
| Targeted vulnerability testing | ✅ **Used as-is** | Directed tests per scenario (Stage VI activity 6.6 conducts these) |
| **Graphical Attack Tree** | 🔄 **Adapted → Structured Spreadsheet** | The standard PASTA Attack Tree (graphical, hierarchical format) was replaced by a structured tabular spreadsheet (`fase_4__5_6_7.xlsx`). Rationale: (1) the spreadsheet enables row-by-row traceability between threats, vulnerabilities, attack cases, and risk scores; (2) it is easier to update incrementally across research phases; (3) it serves as a direct handoff artifact between Stages V, VI, and VII; (4) it can be version-controlled in Git. |
| **Additional SAST tooling** | ➕ **Added** (backlog item) | SonarQube was selected as the primary SAST tool. Backlog item identified need to research additional tools (Trivy for container CVEs, Snyk for dependency analysis). These are recommended as countermeasures in Stage VII. |

---

## Stage VI — Attack Modeling & Simulation (AMS)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Attack library update | ✅ **Used as-is** | CAPEC patterns mapped to T01–T15 threats |
| Attack surface enumeration | ✅ **Used as-is** | Surface derived from Stage II (technical scope) + Stage III (Trust Boundaries) |
| Probability and impact assessment | ✅ **Used as-is** | Per-scenario probability and impact evaluated (Activity 6.4) |
| Countermeasure test case derivation | ✅ **Used as-is** | Test cases derived per CAPEC pattern (Activity 6.5) |
| Security tests and simulations | ✅ **Used as-is** | 6 targeted simulations conducted: Airflow (API), Superset (DAST), DataHub (DAST + integration), HUE HDFS (upload), HUE UI (XSS fuzzing), Jupyter/Spark (stress) |
| **Formal penetration testing** | 🔄 **Adapted** | Full red-team penetration testing was replaced by targeted functional security tests against the staging environment. Rationale: reBi0s is a research environment without a dedicated security operations team; targeted tests are proportionate to the research scope and produce actionable findings. |

---

## Stage VII — Risk & Impact Analysis (RIA)

| Element | Decision | Rationale |
|---------|----------|-----------|
| Risk score calculation | ✅ **Used as-is** | `RS = (PV × PA × I) / C` applied to all 15 threats |
| Countermeasure identification | ✅ **Used as-is** | 5 primary countermeasures identified; mapped to OWASP and CAPEC |
| Residual risk calculation | ✅ **Used as-is** | `RR = [(tp × vp) / c] × i` applied to priority threats |
| Risk treatment strategy | ✅ **Used as-is** | 13 Mitigate, 2 Accept |
| **Quantitative financial risk modeling** | 🔄 **Excluded** | reBi0s does not process financial transactions; financial risk quantification would be inappropriate. Impact is expressed in research credibility, LGPD penalties, and UNICAMP ethics committee consequences instead. |
| Risk communication to business | ✅ **Used as-is** | Priority action plan produced with Immediate / Short-term / Medium-term actions |

---

## Summary Table

| Stage | Elements Used | Elements Adapted | Elements Excluded |
|-------|--------------|-----------------|-------------------|
| Stage I | 4 | 2 | 0 |
| Stage II | 4 | 1 (UML DFD → C4) | 0 |
| Stage III | 2 | 3 (Use Cases, Abuse Cases, DFD) | 0 |
| Stage IV | 6 | 1 (STRIDE as supplement) | 0 |
| Stage V | 5 | 1 (Attack Tree → Spreadsheet) + 1 added (SonarQube) | 0 |
| Stage VI | 5 | 1 (full pen test → targeted tests) | 0 |
| Stage VII | 4 | 1 (financial risk excluded) | 0 |
| **Total** | **30** | **10** | **1** |

---

*Maintained by: Maria Victoria Soares Fiori*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
