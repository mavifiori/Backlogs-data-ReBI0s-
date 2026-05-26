# Stage IV — Threat Analysis (TA)

## Overview

**Stage IV** builds the Threat Library by identifying, classifying, and mapping all
relevant internal and external threats to reBi0s assets.

**Frameworks used:** OWASP Top 10 Web (2021) · OWASP Data Security Top 10 (2023) · STRIDE

## Activities

| Activity | Description |
|----------|-------------|
| 4.1 | Analyze overall threat scenario |
| 4.2 | Gather threat intelligence from internal sources (SIEM, WAF, NIDS, SIRT) |
| 4.3 | Gather threat intelligence from external sources (CVE, advisories, threat feeds) |
| 4.4 | Update the Threat Library |
| 4.5 | Threat Agent to Asset mapping |
| 4.6 | Assign probabilistic values to identified threats |

## Threat Library — T01 to T15

| ID | Type | Threat | OWASP Web | OWASP Data | STRIDE Category |
|----|------|--------|-----------|-----------|-----------------|
| T01 | Int/Ext | Access Control Failures | A01:2021 | DATA2:2023 | Information Disclosure, EoP |
| T02 | Int/Ext | Weak Cryptography | A02:2021 | DATA6:2023 | Information Disclosure, Tampering |
| T03 | External | Injection (SQL/API) | A03:2021 | DATA1:2023 | Tampering, Elevation of Privilege |
| T04 | Internal | Software/Data Integrity Failures | A08:2021 | — | Tampering, Repudiation |
| T05 | Int/Ext | Security Misconfiguration | A05:2021 | — | EoP, Information Disclosure |
| T06 | Internal | Vulnerable/Outdated Components | A06:2021 | — | Tampering, EoP |
| T07 | Internal | Authentication & Session Failures | A07:2021 | DATA2:2023 | Spoofing, EoP |
| T08 | Internal | Insufficient Logging & Monitoring | A09:2021 | — | Repudiation |
| T09 | External | Server-Side Request Forgery (SSRF) | A10:2021 | — | Information Disclosure |
| T10 | Int/Ext | Malware & Ransomware Attacks | — | DATA4:2023 | DoS, Tampering |
| T11 | Internal | Insider Threats | — | DATA5:2023 | Information Disclosure |
| T12 | Internal | Insecure Data Handling | — | DATA7:2023 | Information Disclosure |
| T13 | External | Inadequate Third-Party Security | — | DATA8:2023 | Spoofing, Tampering |
| T14 | Internal | Data Inventory & Management Gaps | — | DATA9:2023 | Information Disclosure |
| T15 | Legal/Int | Non-Compliance with Regulations | — | DATA10:2023 | Repudiation, Info. Disclosure |

## Probabilistic Values (Activity 4.6)

| ID | PV | PA | Impact (I) | Control (C) | Risk Score |
|----|----|----|-----------|-------------|------------|
| T01 | 0.7 | 0.9 | 1.0 | 0.9 | **0.70** |
| T02 | 0.6 | 0.9 | 0.5 | 0.9 | **0.30** |
| T03 | 0.5 | 1.0 | 1.0 | 0.9 | **0.56** |
| T04 | 1.0 | 1.0 | 0.8 | 0.9 | **0.89** |
| T07 | 0.3 | 0.4 | 0.8 | 0.5 | **0.19** |
| T08 | 0.9 | 0.8 | 0.1 | 0.5 | **0.14** |

`Risk Score = (PV × PA × I) / C`

## Artifacts

| File | Description |
|------|-------------|
| `threat-library-T01-T15.md` | Full threat library with all metadata |
| `../../artifacts/threat-modeling/` | Spreadsheet artifacts (xlsx) |
