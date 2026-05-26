```mermaid
---
title: "Threat Library T01–T15 — STRIDE × OWASP Mapping (Stage IV)"
---
mindmap
  root((Threat Library\nreBi0s\nT01–T15))
    SPOOFING
      T07[T07 · Authentication Failures\nA07:2021 · DATA2:2023\nRisk: Low · RR=0.11]
      T13[T13 · Third-Party Security\nDATA8:2023\nRisk: Medium]
    TAMPERING
      T03[T03 · Injection SQL/API\nA03:2021 · DATA1:2023\nRisk: High · RR=1.00]
      T04[T04 · Data Integrity Failures\nA08:2021\nRisk: High · RR=0.89]
      T06[T06 · Outdated Components\nA06:2021\nRisk: Medium]
      T10[T10 · Malware & Ransomware\nDATA4:2023\nRisk: High]
    REPUDIATION
      T08[T08 · Insufficient Logging\nA09:2021\nRisk: High · RR=0.72]
      T15[T15 · Non-Compliance LGPD\nDATA10:2023\nRisk: Low]
    INFORMATION DISCLOSURE
      T01[T01 · Access Control Failures\nA01:2021 · DATA2:2023\nRisk: Critical · RR=1.26]
      T02[T02 · Weak Cryptography\nA02:2021 · DATA6:2023\nRisk: Medium · RR=0.54]
      T09[T09 · SSRF\nA10:2021\nRisk: Medium]
      T11[T11 · Insider Threats\nDATA5:2023\nRisk: Medium]
      T12[T12 · Insecure Data Handling\nDATA7:2023\nRisk: Medium]
      T14[T14 · Data Inventory Gaps\nDATA9:2023\nRisk: Medium]
    DENIAL OF SERVICE
      T10b[T10 · Malware/Ransomware\nDoS vector · DATA4:2023]
    ELEVATION OF PRIVILEGE
      T01b[T01 · Access Control\nA01:2021 · EoP vector]
      T05[T05 · Security Misconfiguration\nA05:2021 · BLOCKER in SonarQube]
```

---

```mermaid
---
title: "Threat Distribution by Type and Risk Level (Stage IV · Activity 4.6)"
---
xychart-beta
    title "Risk Score per Threat (RS = PV × PA × I / C)"
    x-axis [T01, T02, T03, T04, T05, T06, T07, T08, T09, T10, T11, T12, T13, T14, T15]
    y-axis "Risk Score" 0 --> 1.0
    bar [0.70, 0.30, 0.56, 0.89, 0.42, 0.30, 0.19, 0.14, 0.40, 0.50, 0.35, 0.27, 0.25, 0.27, 0.06]
```

---

```mermaid
---
title: "Residual Risk after Countermeasures (Stage VII · RR = [(tp×vp)/c]×i)"
---
xychart-beta
    title "Residual Risk per Threat (RR)"
    x-axis [T01, T02, T03, T04, T07, T08]
    y-axis "Residual Risk" 0 --> 1.4
    bar [1.26, 0.54, 1.00, 0.89, 0.11, 0.72]
    line [0.90, 0.90, 0.90, 0.90, 0.90, 0.90]
```

> **Reading the chart:** The horizontal line at 0.90 represents the Critical threshold.
> T01 (1.26) and T03 (1.00) exceed even this level, confirming critical priority status.
> Both originate from hardcoded credentials identified by SonarQube (BLOCKER severity).
