# Azure-Sentinel-Deployment

> Hands-on deployment of Microsoft Azure Sentinel SIEM/SOAR — completed as part of the **CyberXcel** cybersecurity training program.

## Project Overview

This repository documents a comprehensive, hands-on deployment of **Microsoft Azure Sentinel** in a live Azure environment. It covers the full pipeline from infrastructure provisioning through to live threat detection, incident response, and security monitoring — reflecting real-world SOC workflows and practices.

The screenshots shown are examples only; significantly more work has been completed across all areas documented below.

---

## Live Environment

| Resource | Details |
|---|---|
| **Resource Group** | Cyber_Lab / Australia East |
| **Log Analytics Workspace** | log-cybergc1-lab |
| **VMs** | windows-vm (10.0.0.4, Windows) + linux-vm (10.0.0.5, Linux) |
| **Networking** | Lab-VNet, Lab-NSG |
| **Data Collection Rules** | Windows-DCR-Sentinel, lab-DCR |
| **Sentinel Solutions** | SecurityInsights, BehaviorAnalyticsInsights, Security, SecurityCenterFree |

---

## Documentation Index

Detailed documentation is organised by domain in the `/docs` folder:

| # | Document | Contents |
|---|---|---|
| 01 | [Environment Setup](docs/01-environment-setup.md) | Azure resources, VM configuration, Sentinel deployment, DCR setup |
| 02 | [Data Connectors](docs/02-data-connectors.md) | Enabled connectors, log tables, troubleshooting notes |
| 03 | [KQL Queries](docs/03-kql-queries.md) | Query catalogue, design approach, tuning notes |
| 04 | [Detection Rules](docs/04-detection-rules.md) | Custom analytic rules, MITRE mapping, triage procedures |
| 05 | [Incidents & Alerts](docs/05-incidents-and-alerts.md) | Alert pipeline, observed alerts, SOC workflow |
| 06 | [Vulnerability Management](docs/06-vulnerability-management.md) | Exposure score, device risk, remediation actions |
| 07 | [Threat Intelligence](docs/07-threat-intelligence.md) | TI features, threat actor profiles, MITRE ATT&CK coverage |
| 08 | [Identity & Access](docs/08-identity-and-access.md) | Activity monitoring, UEBA, Azure audit trail |

---

## KQL Detection Queries

All queries are stored as `.kql` files in the `/queries` folder, organised by category.

| Query File | Purpose | Lookback | MITRE |
|---|---|---|---|
| [impossible-travel-detection.kql](queries/identity/impossible-travel-detection.kql) | Sign-ins from different countries | 24h | T1078 |
| [brute-force-signin-1h.kql](queries/identity/brute-force-signin-1h.kql) | Failed logins ≥5 per IP (hunting) | 1h | T1110.001 |
| [brute-force-signin-5m.kql](queries/identity/brute-force-signin-5m.kql) | Failed logins ≥5 per IP (real-time) | 5m | T1110.001 |

> Additional queries covering endpoint, network, cloud activity, and threat hunting are in progress.

---

## Detection Rules

Three custom analytic rules are deployed in Sentinel:

| Rule | Severity | Status | KQL |
|---|---|---|---|
| Entra ID – User Sign-in from Different Location | High | Enabled | impossible-travel-detection.kql |
| RDP Brute Force Attack | Medium | Enabled | brute-force-signin-1h.kql |
| SSH Brute Force on Linux | Low | Enabled | brute-force-signin-5m.kql |

---

## Capabilities Covered

- Provisioning Azure Sentinel on a Log Analytics Workspace
- Configuring data connectors: Entra ID, MDE, Azure Activity, Windows Security Events, Linux Syslog
- Writing and tuning KQL detection queries
- Building scheduled analytic rules with MITRE ATT&CK mapping
- Incident management and triage workflows
- Vulnerability management and exposure scoring (MDVM)
- Threat intelligence integration (TI connector, actor profiles)
- Identity & access monitoring with UEBA
- Azure Activity log analysis and audit trail review
- Device inventory and onboarding (MDE)
- Workbooks and visual security monitoring

---

## Technologies Used

- Microsoft Azure Sentinel (SIEM/SOAR)
- Azure Log Analytics Workspace
- Microsoft Entra ID (Azure Active Directory)
- Microsoft Defender for Endpoint (MDE)
- Microsoft Defender Vulnerability Management (MDVM)
- KQL (Kusto Query Language)
- Azure Virtual Machines (Windows + Linux)
- Azure NSG, VNet, Data Collection Rules

---

## Repository Structure

```
Azure-Sentinel-Deployment/
├── README.md                          # This file — project overview and index
├── docs/
│   ├── 01-environment-setup.md
│   ├── 02-data-connectors.md
│   ├── 03-kql-queries.md
│   ├── 04-detection-rules.md
│   ├── 05-incidents-and-alerts.md
│   ├── 06-vulnerability-management.md
│   ├── 07-threat-intelligence.md
│   └── 08-identity-and-access.md
├── queries/
│   └── identity/
│       ├── impossible-travel-detection.kql
│       ├── brute-force-signin-1h.kql
│       └── brute-force-signin-5m.kql
└── screenshots/                       # (in progress)
```

---

## Training Context

This deployment was completed as a guided hands-on lab through the **CyberXcel** training program, designed to build practical skills in cloud security monitoring, threat detection, and incident response.

## Author

**IslandNC**
