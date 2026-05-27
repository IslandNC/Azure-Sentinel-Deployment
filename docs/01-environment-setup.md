# 01 — Environment Setup

## Azure Resource Group & Infrastructure

All resources are deployed in a dedicated resource group within a single Azure subscription.

| Resource | Name | Type | Location |
|---|---|---|---|
| Resource Group | Cyber_Lab | Resource group | Australia East |
| Log Analytics Workspace | log-cybergc1-lab | Workspace | Australia East |
| Virtual Network | Lab-VNet | Virtual network | Australia East |
| Network Security Group | Lab-NSG | NSG | Australia East |
| Windows VM | windows-vm | Virtual machine | Australia East |
| Linux VM | linux-vm | Virtual machine | Australia East |
| Windows NIC | windows-vm401 | Network Interface | Australia East |
| Linux NIC | linux-vm703 | Network Interface | Australia East |
| Windows Public IP | windows-vm-ip | Public IP address | Australia East |
| Linux Public IP | linux-vm-ip | Public IP address | Australia East |
| Windows OS Disk | linux-vm_OsDisk_1_5e899970eb954364b | Disk | Australia East |
| Network Watcher | NetworkWatcher_australiaeast | Network Watcher | NetworkWatcherRG |

## Virtual Machines

### windows-vm
- **OS:** Windows Server
- **Size:** Standard_D (series)
- **Private IP:** 10.0.0.4
- **Resource Group:** CYBER_LAB
- **Purpose:** Simulates a Windows endpoint for log generation, MDE onboarding, and Windows Security Event collection

### linux-vm
- **OS:** Linux (Ubuntu)
- **Size:** Standard_D (series)
- **Private IP:** 10.0.0.5
- **Resource Group:** CYBER_LAB
- **Purpose:** Simulates a Linux endpoint for Syslog collection and SSH brute force detection testing

## Microsoft Sentinel Deployment

Sentinel was deployed as a solution on top of the Log Analytics Workspace `log-cybergc1-lab`. The following solutions were installed:

| Solution | Name |
|---|---|
| Microsoft Sentinel | SecurityInsights(log-cybergc1-lab) |
| Security Centre Free | SecurityCenterFree(log-cybergc1-lab) |
| Behaviour Analytics | BehaviorAnalyticsInsights(log-cybergc1-lab) |
| Security (combined) | Security(log-cybergc1-lab) |

## Data Collection Rules (DCR)

Two Data Collection Rules were created to route telemetry from VMs into the Log Analytics Workspace:

| DCR Name | Purpose |
|---|---|
| Windows-DCR-Sentinel | Collects Windows Security Events from windows-vm |
| lab-DCR | General lab-wide data collection rule |

## Network Configuration

- Both VMs reside on `Lab-VNet` with private IPs in the `10.0.0.x` subnet
- `Lab-NSG` controls inbound/outbound traffic rules
- Public IPs assigned to both VMs for lab access and intentional exposure to test brute force detection
- **Note:** Exposing RDP (3389) and SSH (22) publicly is intentional in this lab context to generate realistic attack telemetry. In production these ports should be restricted via NSG or Azure Bastion.

## Deployment Steps

1. Created resource group `Cyber_Lab` in Australia East
2. Provisioned `Lab-VNet` virtual network and `Lab-NSG` network security group
3. Deployed `windows-vm` (Windows) and `linux-vm` (Linux) within the VNet
4. Created Log Analytics Workspace `log-cybergc1-lab`
5. Deployed Microsoft Sentinel on top of the workspace
6. Installed BehaviorAnalyticsInsights, Security, and SecurityCenterFree solutions
7. Created Data Collection Rules to route VM telemetry to the workspace
8. Onboarded both VMs to Microsoft Defender for Endpoint (MDE)
9. Verified devices appeared in Defender Device Inventory

## Notes

- VMs should be in **Running** state for agents to report and log data to flow
- When VMs are deallocated, MDE heartbeat stops and log ingestion pauses
- UEBA (User and Entity Behaviour Analytics) is enabled via the BehaviorAnalyticsInsights solution
