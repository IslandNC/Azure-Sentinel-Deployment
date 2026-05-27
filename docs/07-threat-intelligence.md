# 07 — Threat Intelligence

## Overview

Microsoft Defender Threat Intelligence capabilities were explored as part of this deployment, including threat actor profiles, technique profiles, and integration with Sentinel detection and hunting workflows.

## Features Explored

- **Threat Analytics** — Curated intelligence reports from Microsoft research teams covering active threat actors, TTPs, and environment-specific exposure
- **Intel Management** — Manage IoCs: IP addresses, domains, file hashes, STIX/TAXII feeds
- **Intel Profiles** — Detailed actor and malware write-ups with TTPs and defensive recommendations
- **Intel Explorer** — Graph-based view of relationships between actors, techniques, and indicators
- **Intel Projects** — Structured investigation project management

## Observed Threat Actor Score Drops

| Actor / Technique | Score Drop | Notes |
|---|---|---|
| Storm-2885 | -64.29% | Lab lacks controls relevant to this actor's TTPs |
| Granite Typhoon | -77.42% | Nation-state actor; missing defensive controls |
| BadSuccessor (technique) | -36.36% | AD delegation abuse; missing AD hardening |
| Cross-tenant helpdesk impersonation | -72.89% | Lab lacks PIM and conditional access policies |
| Microsoft Intune | -31.25% | Intune not configured in lab |

These drops reflect intentional VM exposure and absence of enterprise hardening. In production each drop triggers a remediation workstream.

## TI Integration with Sentinel

The Threat Intelligence connector ingests IoCs into ThreatIntelligenceIndicator, enabling TI-based detection rules, incident enrichment with threat actor context, and cross-referencing hunting queries against the TI feed.

## MITRE ATT&CK Coverage Map

| Phase | Technique | Detection Rule |
|---|---|---|
| Initial Access | T1078 — Valid Accounts | Entra ID Sign-in from Different Location |
| Credential Access | T1110.001 — Brute Force | RDP Brute Force Attack |
| Credential Access | T1110.001 — Brute Force | SSH Brute Force on Linux |

The MITRE ATT&CK workbook was used to visualise coverage and identify gaps.

## Key Takeaways

- IoC-based detection has limitations as IoCs age quickly; TTP-based behavioural rules are more durable
- UEBA (BehaviorAnalyticsInsights) complements TI with entity-level risk scoring based on behaviour patterns
- TI is most valuable when integrated with detection and hunting workflows
