# Threat Entry Template

## Threat ID: T[NN]

| Field | Value |
|-------|-------|
| **ID** | T[NN] |
| **Type** | Internal / External / Internal-External |
| **Threat Name** | [Name] |
| **Affected Asset** | [Asset] |
| **Threat Agent** | [Who/What] |
| **Attack Vector** | [How] |
| **Existing Controls** | [Current controls] |
| **STRIDE Category** | [Category] |
| **OWASP Web (2021)** | [ID] |
| **OWASP Data (2023)** | [ID] |

## Probabilistic Values

| Parameter | Value | Justification |
|-----------|-------|---------------|
| PV (Vulnerability Probability) | [0.0–1.0] | [Reason] |
| PA (Threat Agent Probability) | [0.0–1.0] | [Reason] |
| I (Impact) | [0.0–1.0] | [Reason] |
| C (Control Effectiveness) | [0.0–1.0] | [Reason] |
| **Risk Score** | **(PV × PA × I) / C = [value]** | |

## CAPEC Mapping

- **CAPEC ID:** [CAPEC-NNN]
- **Pattern Name:** [Name]
- **Attack Vector:** [Specific vector for reBi0s]
- **Countermeasure:** [Technical countermeasure]

## Residual Risk

`RR = [(tp × vp) / c] × i = [value]`

| Status | Value |
|--------|-------|
| Risk Level | 🟢 Low / 🟡 Medium / 🔴 High / 🔴🔴 Critical |
| Treatment Strategy | Mitigate / Accept / Transfer / Avoid |
