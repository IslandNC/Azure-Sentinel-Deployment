## 07 — Threat Intelligence
### Overview

Microsoft Defender Threat Intelligence capabilities were explored as part of this deployment, including threat actor profiles, technique profiles, and integration with Sentinel detection and hunting workflows.

---

### Features Explored

- **Threat Analytics** — Curated intelligence reports from Microsoft research teams covering active threat actors, TTPs, and environment-specific exposure
- **Intel Management** — Manage IoCs: IP addresses, domains, file hashes, STIX/TAXII feeds
- **Intel Profiles** — Detailed actor and malware write-ups with TTPs and defensive recommendations
- **Intel Explorer** — Graph-based view of relationships between actors, techniques, and indicators
- **Intel Projects** — Structured investigation project management

---

### Observed Threat Actor Score Drops

| Actor / Technique | Score Drop | Notes |
|---|---|---|
| Storm-2885 | -64.29% | Lab lacks controls relevant to this actor's TTPs |
| Granite Typhoon | -77.42% | Nation-state actor; missing defensive controls |
| BadSuccessor (technique) | -36.36% | AD delegation abuse; missing AD hardening |
| Cross-tenant helpdesk impersonation | -72.89% | Lab lacks PIM and conditional access policies |
| Microsoft Intune | -31.25% | Intune not configured in lab |

These drops reflect intentional VM exposure and absence of enterprise hardening. In production each drop triggers a remediation workstream.

---

### TI Integration with Sentinel

The Threat Intelligence connector ingests IoCs into `ThreatIntelligenceIndicator`, enabling:

- TI-based detection rules (e.g., alert on sign-in from a known-malicious IP)
- Incident enrichment with threat actor context
- Cross-referencing hunting queries against the TI feed

---

### MITRE ATT&CK Coverage Map

#### Deployed Detection Rules

| Phase | Technique | Detection Rule | Status |
|---|---|---|---|
| Initial Access | T1078 — Valid Accounts | Entra ID Sign-in from Different Location | Enabled |
| Credential Access | T1110.001 — Brute Force | RDP Brute Force Attack | Enabled |
| Credential Access | T1110.001 — Brute Force | SSH Brute Force on Linux | Enabled |
| Execution | T1059 — Command and Scripting Interpreter | Process Creation Anomaly (LOLBin) | Query only (no rule yet) |

#### Planned Detection Rules

| Phase | Technique | Detection Rule | Status |
|---|---|---|---|
| Persistence | T1078.004 — Cloud Account Abuse | New high-privilege role assignments in Entra ID | Planned |
| Persistence | T1136 — Create Account | New user accounts created outside business hours | Planned |
| Defence Evasion | T1562 — Impair Defences | MDE tamper protection disabled | Planned |
| Execution | T1059 — Command and Scripting Interpreter | PowerShell with encoded commands (analytic rule) | Planned |
| Credential Access | T1003 — OS Credential Dumping | LSASS access on windows-vm | Planned |
| Defence Evasion | T1218 — System Binary Proxy Execution | LOLBin execution (analytic rule) | Planned |

The MITRE ATT&CK workbook was used to visualise coverage and identify gaps across all phases.

---

### Key Takeaways

- IoC-based detection has limitations as IoCs age quickly; TTP-based behavioural rules are more durable
- UEBA (BehaviorAnalyticsInsights) complements TI with entity-level risk scoring based on behaviour patterns
- TI is most valuable when integrated with detection and hunting workflows
- The planned rules above target the most significant coverage gaps identified during this deployment
