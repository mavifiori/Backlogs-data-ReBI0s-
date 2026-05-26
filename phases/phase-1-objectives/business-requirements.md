# Stage I — Definition of Objectives (DO)
## Business Requirements, Security Requirements & Risk Profile — reBi0s

> **Research:** Assessing Architectural Security in Scientific Repositories: An Action-Research Approach  
> **Institution:** UNICAMP — Instituto de Computação  
> **Source artifact:** `Cópia_de_Backlog_-Fase_1.docx`  
> **PASTA stage:** Stage I — Definition of Objectives (DO)

---

## 1. Mission

Integrate and manage policies related to the storage, access, and security of scientific data and solutions obtained throughout the project, considering the diversity of data types and formats within the reBi0s context.

---

## 2. Business Requirements

| # | Requirement |
|---|-------------|
| BR-01 | Enable data science studies with structured and unstructured data stored in BIOS repositories |
| BR-02 | Facilitate scalable and systematic curation of BIOS datasets, allowing efficient data organization |
| BR-03 | Enable federated data governance with Academia and Industry partners, implementing clear access and quality policies, ensuring integrity and compliance |
| BR-04 | Foster open collaboration among researchers interested in datasets hosted in the repository, via multiple tools adapted to different computational expertise levels |
| BR-05 | Rationalize storage and data consumption through efficient structures for storage and processing, optimizing resource utilization and improving data manipulation performance |

---

## 3. Security Requirements (Non-Functional)

| ID | Original Requirement (Miro) | Formal Security Requirement |
|----|----------------------------|-----------------------------|
| SR-01 | Firewall policies to block external access to internal ports (Spark, PostgreSQL, Redis, etc.) | The system **must** implement firewall policies to block any external access attempts to internal service ports, ensuring those services are accessible only from authorized IP addresses within the private network. |
| SR-02 | Data anonymization for personally identifiable data | The system **must** guarantee anonymization of personally identifiable data, ensuring protection of sensitive information. |
| SR-03 | Clear policies for data deletion or anonymization | The system **must** implement automated policies for deletion or anonymization of sensitive personal data, using time-based criteria, purpose-based criteria, and secure processing methods. |
| SR-04 | Disaster Recovery Plan | The system **must** implement a Disaster Recovery Plan (DRP) with regular backups, redundant infrastructure, recovery procedures, periodic testing, and incident communication, targeting acceptable RTO and RPO values. |
| SR-05 | Prevent unauthorized data modification or deletion | The system **must** ensure that only duly authorized users can alter or delete data, implementing strict access controls and action auditing. |
| SR-06 | Backup policies | The system **must** implement backup policies that ensure the creation and secure storage of periodic data copies. |
| SR-07 | Detailed audit logs | The system **must** maintain auditable records of all repository operations, including modifications, deletions, and access events, to enable traceability and compliance with security policies. |
| SR-08 | Minimum password requirements | The system **must** enforce minimum password requirements: at least 8 characters, combining uppercase, lowercase, numbers, and special characters. |
| SR-09 | Integrated access control across consumption tools | The system **must** implement centralized authentication and authorization across all consumption tools (Jupyter, Superset, HUE, DataHub). |
| SR-10 | Password expiration policy | The system **must** enforce a password expiration policy, requiring periodic password changes every 60 days, with advance user notification. |
| SR-11 | Data source transparency | The system **must** ensure transparency regarding data sources, providing clear and accessible information about how data is collected, stored, and used. |

---

## 4. Business Impact Analysis (BIA) — CIA Triad

The impact criteria are defined as events or failures that can compromise the functioning of the repository. They are based on the **CIA triad**: Confidentiality, Integrity, and Availability.

- **Confidentiality:** The system's ability to protect privacy and sensitive data, preventing leaks and unauthorized access.
- **Integrity:** Ensuring that information is consistent and reliable, preventing unauthorized modifications.
- **Availability:** Maintaining resources accessible when needed, ensuring service continuity.

### 4.1 Data Classification

| Class | Description | Sensitivity |
|-------|-------------|-------------|
| **Public** | No access restriction; data accessed without limitations | 🟢 Low |
| **Internal** | Restricted to reBi0s collaborators; moderate operational and scientific impact if exposed | 🟡 Medium |
| **Sensitive** | Restricted access; exposure can cause serious consequences to privacy, economy, and science | 🔴 High |

### 4.2 Key Contextual Questions

**Are the data sensitive or regulated?**  
Yes. reBi0s stores and manages sensitive scientific data of critical nature, including information related to health, climate, and agriculture that is not in the public domain. A data leak or misuse can cause privacy violations for individuals and compromise scientific studies.

**Can the application affect the health of individuals?**  
Yes. Incorrectly manipulated health data can generate life risk (erroneous diagnoses). Leaked climate data can harm food production or extreme weather alert systems.

**How is access to the application obtained?**  
Access requires registration by administrators. Registered users are employees or scientists working on the project.

**Applicable regulatory frameworks:**
- **LGPD** (Lei 13.709/2018) — Brazilian General Data Protection Law
- **UNICAMP Ethics Committee** — scientific research data governance

| LGPD Principle | Application to reBi0s |
|----------------|-----------------------|
| Finality | Data use must have legitimate, explicit, and specific purposes |
| Adequacy | Processing must be compatible with purposes communicated to the data subject |
| Necessity | Only strictly necessary data should be collected |
| Transparency | Data subjects must have clarity on how their data is used |
| Security | Measures to protect data against unauthorized access or accidental situations |

---

## 5. Risk Mapping — CIA Impact Table

| Risk | Confidentiality | Integrity | Availability |
|------|----------------|-----------|--------------|
| Unauthorized access (access control) | Compromise of sensitive data | Unauthorized data modification | Service disruption |
| Missing/weak cryptography | Data vulnerability | Data modifications without monitoring | Compromise of security-dependent operations |
| Data integrity compromise | Exposure of modified data | Unauthorized modifications | N/A |
| DoS/DDoS attack | N/A | N/A | System unavailable to legitimate users |
| Backup failure | Exposure of sensitive data | Corrupted data cannot be recovered | Service disruption |
| Absent metadata management | Exposure of sensitive data | Data integrity failure | Difficulty accessing and recovering logs |
| LGPD non-compliance | User privacy violation | Failures in data security processes | Service disruption |
| Sensitive data leak | Affects third-party privacy | Fraudulent data manipulation | N/A |
| Outdated software | Unauthorized access | Data corruption | Service disruption |
| Lack of security training | Accidental sensitive data exposure | Data integrity issues | System interruption and information loss |

### 5.1 Impact Classification by CIA Dimension

**Confidentiality:**

| Potential Impact | Business Impact on reBi0s | Classification |
|-----------------|---------------------------|----------------|
| Compromise of sensitive data | Loss of credibility, legal penalties, privacy violation, UNICAMP ethics committee proceedings | 🔴 High |
| Sharing/exposure of modified data | Loss of credibility, legal penalties, harm to research and business | 🔴 High |
| User privacy violation | Loss of credibility, legal penalties, privacy violation | 🔴 High |

**Integrity:**

| Potential Impact | Business Impact on reBi0s | Classification |
|-----------------|---------------------------|----------------|
| Unauthorized data modification | Loss of credibility, legal penalties, harm to research | 🟡 Medium |
| Corrupted data cannot be recovered | Loss of credibility, reBi0s operational shutdown | 🔴 High |
| Security process failures | reBi0s operational shutdown, vulnerability to attacks | 🔴 High |
| Fraudulent data manipulation | Loss of credibility, harm to research, UNICAMP ethics committee proceedings | 🔴 High |
| Data corruption | Loss of credibility, harm to research, negative image | 🔴 High |

**Availability:**

| Potential Impact | Business Impact on reBi0s | Classification |
|-----------------|---------------------------|----------------|
| Service disruption | reBi0s operational shutdown, harm to research | 🔴 High |
| Security-dependent operations compromised | reBi0s operational shutdown, vulnerability to attacks | 🔴 High |
| System unavailable to legitimate users | Harm to research | 🟡 Medium |
| Difficulty accessing/recovering logs | Harm to reBi0s management | 🟡 Medium |
| Loss of important information | Harm to management and research | 🔴 High |

---

## 6. Risk Scenarios (Inherent Risk Profile)

### Scenario 1 — Unauthorized Access and Misconfigured Permissions
**Risk:** Users obtain unnecessary privileges.  
**Scenario:** A malicious agent uses an account with excessively granted privileges to execute attack commands.  
**Mitigation:** Implement access policies following the Principle of Least Privilege.

### Scenario 2 — Credential Exposure
**Risk:** Credentials (API keys or passwords) exported in scripts, files, or code repositories.  
**Scenario:** A data scientist left a credential in a public script on the reBi0s GitHub, enabling a malicious agent to gain access to repository data.  
**Mitigation:** Credential rotation policies.

### Scenario 3 — Missing or Weak Cryptography
**Risk:** Data transmission without encryption; data stored without encryption.  
**Scenario:** During unencrypted data transmission, a malicious agent intercepts and sells the information.  
**Mitigation:** Mandatory HTTPS/TLS for data transfers; database encryption (AES).

### Scenario 4 — Malicious Attacks
**Risk:** Malicious commands executed against the database (injection attacks); system overloaded by mass requests (denial of service).  
**Scenario:** Bots perform mass requests, taking reBi0s offline; an agent gains unauthorized database access via login field injection.  
**Mitigation:** Web Application Firewall (WAF); ORM or parameterized queries.

### Scenario 5 — Sensitive Data Sharing Without Anonymization
**Risk:** Lack of anonymization of personal data in shared datasets.  
**Scenario:** A health dataset without anonymization is cross-referenced, shared, and misused.  
**Mitigation:** Anonymization techniques in compliance with LGPD.

### Scenario 6 — Lack of Traceability and Audit
**Risk:** No tracking of unauthorized changes or access.  
**Scenario:** A user with elevated privileges copies sensitive data without traceability, preventing log audit and violation investigation.  
**Mitigation:** Implementation of audit and monitoring systems.

### Scenario 7 — Outdated Software or Libraries
**Risk:** Exploitation of known vulnerabilities in public libraries.  
**Scenario:** reBi0s uses an outdated version of Apache with known vulnerabilities, enabling malicious agents to gain repository access.  
**Mitigation:** Regular software updates and vulnerability analysis tools.

---

## 7. Feeds Into

| Stage | How Stage I outputs are consumed |
|-------|----------------------------------|
| Stage II (DTS) | Security requirements constrain technology selection and hardening baseline |
| Stage IV (TA) | Risk profile and impact classification shape threat prioritization |
| Stage VII (RIA) | BIA impact values feed the residual risk formula: `RR = [(tp × vp) / c] × i` |

---

*Source: `Cópia_de_Backlog_-Fase_1.docx` · PASTA Stage I — Definition of Objectives (DO)*  
*Research: Assessing Architectural Security in Scientific Repositories — UNICAMP, 2025*
