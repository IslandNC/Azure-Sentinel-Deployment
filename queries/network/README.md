# queries/network — Network Detection Queries

> **Status: In Progress.** Queries in this folder are under development.

This folder will contain KQL queries for network-based threat detection, including:

| Planned Query | Purpose | MITRE |
|---|---|---|
| lateral-movement-smb.kql | SMB lateral movement between VMs | T1021.002 |
| dns-beaconing.kql | High-frequency DNS queries to single domain | T1071.004 |
| port-scan-detection.kql | Rapid sequential port connections from single IP | T1046 |
| nsg-flow-anomaly.kql | Unusual outbound traffic volume or destination | T1041 |

These queries will target the `CommonSecurityLog`, `AzureNetworkAnalytics_CL`, and `DeviceNetworkEvents` tables.
