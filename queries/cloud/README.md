# queries/cloud — Cloud Activity Detection Queries

> **Status: In Progress.** Queries in this folder are under development.

This folder will contain KQL queries for Azure control-plane and cloud activity threat detection, including:

| Planned Query | Purpose | MITRE |
|---|---|---|
| privileged-role-assignment.kql | New high-privilege role assigned in Entra ID | T1078.004 |
| new-account-off-hours.kql | User account created outside business hours | T1136 |
| service-principal-secret.kql | Service principal secret created or rotated | T1098.001 |
| azure-policy-change.kql | Azure Policy modified or disabled | T1562.001 |
| resource-deletion-spree.kql | Multiple resource deletions in short timeframe | T1485 |

These queries will target the `AzureActivity`, `AuditLogs`, and `SigninLogs` tables.
