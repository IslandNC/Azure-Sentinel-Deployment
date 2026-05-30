# Screenshots

This folder contains screenshots demonstrating the Azure Sentinel deployment in action.

> Screenshots are taken directly from the live lab environment. They reflect real telemetry, real alerts, and real configuration — not simulated data.

---

## Contents

| File | Description |
|---|---|
| [01-sentinel-overview.png](01-sentinel-overview.png) | Microsoft Defender Azure Activity view — Caller Activities table (cyber.gc1@outlook.com: 2,425 ops) and Activities by log level over time chart (Informational: 51, Error: 11, Warning: 0) |
| [02-detection-rules.png](02-detection-rules.png) | Microsoft Sentinel Detection Rules — all 3 custom analytic rules enabled: Entra ID Sign-in from Different Location (High), RDP Brute Force Attack (Medium), SSH Brute Force on Linux (Low) |
| [03-vulnerability-exposure-score.png](03-vulnerability-exposure-score.png) | Exposure Management dashboard — Top Initiatives (Zero Trust 14%, Ransomware Protection 26%), Vulnerability Management score 43/100 (Medium), score history (+43% over 6 days), device exposure distribution, and threat actor initiative drops panel (Storm-2885, Granite Typhoon, BadSuccessor, etc.) |
| [03-vulnerability-score.png](03-vulnerability-score.png) | MDVM Vulnerability Management Overview — Endpoint exposure score gauge showing 43/100 Medium with Low/Medium/High band legend and score history trend |
| [04-alerts.png](04-alerts.png) | Microsoft Sentinel Alerts queue — 2 active alerts: "Brute force attack on Linux detected" (Low severity, New status), filtered by Status: New, In Progress; confirms SSH Brute Force detection rule firing on real telemetry |
| [05-device-inventory.png](05-device-inventory.png) | Microsoft Defender Assets > Device Inventory — 2 total devices (windows-vm 10.0.0.4 and linux-vm 10.0.0.5), both Workstation type, 0 critical/high risk, 1 not onboarded, 2 newly discovered |
| [06-kql-log-analytics.png](06-kql-log-analytics.png) | Log Analytics workspace `log-cybergc1-lab` — brute-force-signin-5m.kql query open in editor with query history showing 4 prior executions with 0 results (confirming rule ran against live SigninLogs) |
| [07-identity-access.png](07-identity-access.png) | Cloud Inventory > Identity & Access — Top 10 activities: 8.33K total, 2.37K Event 4688 (process creation), 2.19K Event 8030 (MDE telemetry), 2.19K Event 8002 (MDE telemetry), 319 Event 4624 (logon); User activities table and Machine activities (windows-vm: 3,150) |
| [08-caller-activities.png](08-caller-activities.png) | Azure Portal > Recently Viewed Resources — showing recently accessed lab resources: log-cybergc1-lab (Log Analytics workspace), windows-vm, linux-vm, Cyber_Lab (resource group), NICs, public IPs, Lab-NSG, Lab-VNet |
| [09-resources.png](09-resources.png) | Azure Resource Manager > All Resources — full resource list for the lab: BehaviorAnalyticsInsights solution, lab-DCR, Lab-NSG, Lab-VNet, linux-vm, linux-vm-ip, linux-vm703 NIC, linux-vm OS disk, log-cybergc1-lab workspace, NetworkWatcher, Security solutions, SecurityCenterFree, SecurityInsights — all in Australia East |
| [10-vms-running.png](10-vms-running.png) | Azure Portal > Compute > Virtual Machines — linux-vm and windows-vm both in **Running** state, CYBER_LAB resource group, Australia East, Standard_D series |
| [11-defender-device-inventory.png](11-defender-device-inventory.png) | Microsoft Defender for Endpoint > Device Inventory — same 2 devices (windows-vm 10.0.0.4, linux-vm) shown in the Defender portal context; confirms successful MDE onboarding |
| [12-defender-for-cloud.png](12-defender-for-cloud.png) | Microsoft Defender for Cloud > Overview — 1 subscription, 6 assessed resources, 0 attack paths, 0 security alerts; Security Posture score 23% (Azure), 47 recommendations (High 18, Medium 4, Low 25) |

---

## Notes on Duplicate File Names

Two files use the `03-` prefix, uploaded with different names:

- `03-vulnerability-exposure-score.png` — the **Exposure Management** full dashboard (Top Initiatives, threat actor drops, device exposure)
- `03-vulnerability-score.png` — the **MDVM Vulnerability Management** gauge view (43/100 score detail)

Both relate to doc [06 — Vulnerability Management](../docs/06-vulnerability-management.md). Consider renaming `03-vulnerability-score.png` to `03b-vulnerability-score.png` to clarify ordering on future uploads.

---

## Category Index

### Environment & Infrastructure
`08-caller-activities.png`, `09-resources.png`, `10-vms-running.png`

Screenshots documenting the Azure resource group, VMs, networking, and recently viewed resources.

### Detection & Alerting
`02-detection-rules.png`, `04-alerts.png`

Screenshots of all 3 custom detection rules and the 2 confirmed brute force alerts.

### Vulnerability Management
`03-vulnerability-exposure-score.png`, `03-vulnerability-score.png`

MDVM exposure scoring dashboard showing the 43/100 Medium score and threat actor initiative drops.

### Identity & Access Monitoring
`01-sentinel-overview.png`, `07-identity-access.png`

Azure Activity caller activities and Cloud Inventory Identity & Access with 8.33K total events.

### Endpoint & Device Management
`05-device-inventory.png`, `11-defender-device-inventory.png`, `12-defender-for-cloud.png`

Device inventory from both Defender and Azure perspectives, and Defender for Cloud security posture.

### KQL Queries
`06-kql-log-analytics.png`

Log Analytics workspace showing the brute-force-signin-5m.kql query and execution history.

---

Screenshots align with the documentation sections in [`/docs`](../docs/).
