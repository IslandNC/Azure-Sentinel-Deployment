# Screenshots

This folder contains annotated screenshots demonstrating the Azure Sentinel deployment in action.

> **Note:** The screenshots shown are representative examples. Significantly more work has been completed across all capability areas documented in this repository.

## Contents

| File | Description |
|------|-------------|
| `01-sentinel-overview.png` | Microsoft Sentinel workspace overview and data ingestion summary |
| `02-detection-rules.png` | Custom analytic rules — Entra ID, RDP, and SSH brute force detections |
| `03-vulnerability-score.png` | MDVM endpoint exposure score (43/100 Medium) and score history |
| `04-alerts.png` | Active alerts view — brute force attack detections (Low severity) |
| `05-device-inventory.png` | Device inventory showing windows-vm and linux-vm onboarded to MDE |
| `06-kql-log-analytics.png` | Log Analytics KQL query workspace with query history |
| `07-identity-access.png` | Cloud Inventory Identity & Access view with 8.33k total activities |
| `08-caller-activities.png` | Azure control-plane caller activities (cyber.gc1@outlook.com — 2,425 ops) |
| `09-resources.png` | Full Azure resource list — Cyber_Lab resource group (18 resources) |
| `10-vms-running.png` | Both VMs in Running state (CYBER_LAB, Australia East) |
| `11-zero-trust-score.png` | Zero Trust (14%) and Ransomware Protection (26%) initiative scores |
| `12-mitre-coverage.png` | MITRE ATT&CK workbook coverage visualisation |

## Categories

### Environment & Infrastructure
Screenshots documenting the Azure resource group, VMs, networking, and Log Analytics workspace setup.

### Detection & Alerting
Screenshots of custom detection rules, fired alerts, and incident pipeline in Microsoft Sentinel.

### Vulnerability Management
MDVM exposure scoring screenshots showing progression from 0 to 43/100 during the lab.

### Identity & Access Monitoring
Cloud Inventory Identity & Access screenshots, UEBA entity pages, and Azure Activity audit trail.

### KQL Queries
Log Analytics workspace screenshots showing query execution and results.

---

*Screenshots are organised to align with the `/docs` documentation sections.*
