# 02 — Data Connectors

## Overview

Data connectors are the pipeline that brings telemetry from various sources into the Microsoft Sentinel Log Analytics Workspace (`log-cybergc1-lab`). Each connector populates specific log tables that can then be queried with KQL and used in analytic rules.

## Connectors Enabled

### 1. Microsoft Entra ID (Azure Active Directory)
- **Log Tables:** `SigninLogs`, `AuditLogs`, `AADNonInteractiveUserSignInLogs`
- **Purpose:** Captures all authentication events, sign-in attempts (successful and failed), MFA activity, and directory changes
- **Setup:** Enabled via Entra ID Diagnostic Settings, routed to `log-cybergc1-lab`
- **Key Use Case:** Impossible travel detection, brute force sign-in detection, privilege escalation via role assignment changes
- **Note:** Diagnostic Settings must be explicitly configured in Entra ID to export `SignInLogs` to the workspace — this is a common setup oversight

### 2. Microsoft Defender for Endpoint (MDE)
- **Log Tables:** `DeviceEvents`, `DeviceProcessEvents`, `DeviceNetworkEvents`, `DeviceLogonEvents`, `DeviceFileEvents`, `AlertEvidence`, `AlertInfo`
- **Purpose:** Endpoint telemetry from onboarded VMs — process creation, network connections, logon events, file activity
- **Setup:** Both `windows-vm` and `linux-vm` onboarded to MDE; devices visible in Defender Device Inventory
- **Key Use Case:** Brute force detection on endpoints, lateral movement, malicious process execution

### 3. Azure Activity Logs
- **Log Tables:** `AzureActivity`
- **Purpose:** Captures all control-plane operations in the Azure subscription — resource creation/deletion, role assignments, policy changes
- **Setup:** Connected via the Azure Activity data connector in Sentinel
- **Key Use Case:** Detecting unauthorized resource creation, privilege escalation, subscription-level changes

### 4. Windows Security Events (via DCR)
- **Log Tables:** `SecurityEvent`
- **Purpose:** Windows event log data from `windows-vm` — logon events (Event ID 4624, 4625), process creation (4688), account management (4720, 4728)
- **Setup:** Configured via `Windows-DCR-Sentinel` Data Collection Rule targeting `windows-vm`
- **Key Use Case:** RDP brute force detection, account creation/modification, privilege use

### 5. Linux Syslog (via DCR)
- **Log Tables:** `Syslog`
- **Purpose:** System logs from `linux-vm` — auth logs, SSH activity, sudo usage, service events
- **Setup:** Configured via `lab-DCR` Data Collection Rule targeting `linux-vm`
- **Key Use Case:** SSH brute force detection, privilege escalation via sudo, suspicious cron jobs

### 6. Microsoft Defender for Cloud
- **Log Tables:** `SecurityAlert`, `SecurityRecommendation`
- **Purpose:** Cloud security alerts and hardening recommendations from Defender for Cloud
- **Setup:** Connected via Microsoft Defender for Cloud connector in Sentinel
- **Key Use Case:** Vulnerability alerts, missing patches, misconfigured resources

## Log Table Reference

| Log Table | Source | Key Fields |
|---|---|---|
| `SigninLogs` | Entra ID | UserPrincipalName, IPAddress, ResultType, LocationDetails |
| `AuditLogs` | Entra ID | OperationName, InitiatedBy, TargetResources |
| `SecurityEvent` | Windows VM (DCR) | EventID, Account, Computer, LogonType |
| `Syslog` | Linux VM (DCR) | SyslogMessage, HostName, Facility |
| `AzureActivity` | Azure Subscription | OperationName, Caller, ResourceGroup, Level |
| `DeviceEvents` | MDE | DeviceName, ActionType, InitiatingProcessName |
| `DeviceLogonEvents` | MDE | DeviceName, AccountName, LogonType, RemoteIP |
| `SecurityAlert` | Defender for Cloud | AlertName, Severity, CompromisedEntity |

## Troubleshooting Notes

- **SigninLogs returning 0 results:** Most common cause is Entra ID Diagnostic Settings not configured to export to the workspace. Verify under Entra ID > Monitoring > Diagnostic Settings
- **SecurityEvent not ingesting:** Verify the Windows DCR is associated with the correct VM and the VM is in Running state
- **Syslog gaps:** The Linux VM must be running and the AMA (Azure Monitor Agent) must be installed and reporting healthy
- **VM deallocated = no data:** When VMs are in Stopped (deallocated) state, agents stop reporting and log ingestion pauses entirely

## UEBA (User and Entity Behaviour Analytics)

The `BehaviorAnalyticsInsights` solution is deployed, enabling UEBA which enriches entities (users, hosts, IPs) with behavioural baselines. This powers the `BehaviorAnalytics` and `IdentityInfo` tables used in advanced hunting queries.
