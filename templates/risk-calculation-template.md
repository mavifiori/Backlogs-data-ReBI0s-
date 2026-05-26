# Risk Calculation Template — Stage VII

## Risk Score (Stage IV)

```
RS = (PV × PA × I) / C

PV = Probability of Vulnerability occurrence
PA = Probability of Threat Agent exploitation  
I  = Impact on CIA triad
C  = Control/Countermeasure effectiveness
```

| ID | PV | PA | I | C | RS |
|----|----|----|---|---|----|
| T[N] | [0.0–1.0] | [0.0–1.0] | [0.0–1.0] | [0.0–1.0] | **[result]** |

## Residual Risk (Stage VII)

```
RR = [(tp × vp) / c] × i

tp = Threat probability (same as PA)
vp = Vulnerability probability (same as PV)
c  = Control effectiveness (same as C)
i  = Impact value (same as I)
```

| ID | tp | vp | i | c | Formula | RR | Level |
|----|----|----|---|---|---------|-----|-------|
| T[N] | | | | | `[(tp×vp)/c]×i` | **[result]** | 🟢/🟡/🔴 |

## Risk Scale

| Level | Range | Color |
|-------|-------|-------|
| Low | [0.00 – 0.29] | 🟢 |
| Medium | [0.30 – 0.59] | 🟡 |
| High | [0.60 – 0.89] | 🔴 |
| Critical | [0.90+] | 🔴🔴 |
