## 08 — Identity & Access Monitoring
### Overview

Identity and access monitoring is a core function of this Sentinel deployment. The Identity & Access section within Microsoft Defender provides deep visibility into authentication events, account activity, and access patterns across the Azure environment.

---

### Activity Volume (Last 7 Days)

| Metric | Count | Source | Notes |
|---|---|---|---|
| Total Activities | 8,330 | Mixed | Across all identity and endpoint data sources |
| New Process Created (Event 4688) | 2,370 | Windows Security Events | Process creation audit events from windows-vm |
| Event 8030 | 2,190 | MDE Telemetry | Microsoft Defender for Endpoint internal telemetry event; not a Windows Security Event — represents AV/EDR scan activity |
| Process Was Accessed (Event 8002) | 2,190 | MDE Telemetry | MDE internal telemetry event for process access; captured via the `DeviceEvents` table, not `SecurityEvent` |
| Event 4624 (Account Logon) | 319 | Windows Security Events | Successful logon events from windows-vm |

> **Note on Events 8030 and 8002:** These are Microsoft Defender for Endpoint (MDE) internal telemetry events, not standard Windows Security Events. They appear in the Defender identity view via the `DeviceEvents` table and reflect AV/EDR monitoring activity rather than Windows audit events. High volumes are expected on an active, MDE-onboarded endpoint.

The high activity volume from a single windows-vm (3,150 machine activity counts) demonstrates that even a minimal lab generates substantial telemetry.

---

### Top User Activities

| Account | Activity Count | Notes |
|---|---|---|
| system | 944 | Local system account |
| nt authority/system | 906 | Windows built-in service account |
| admin~1 | 164 | Local admin account |
| cybergc1 | 131 | Primary lab user |
| jarrod.page | 66 | Lab test account |
| local service | 62 | Windows service account |
| windows-vm/cybergc1 | 62 | Domain/local account format |
| nt authority/local service | 58 | Windows service account |
| network service | 49 | Windows network service account |

---

### Machine Activities

| Machine | Activity Count |
|---|---|
| windows-vm | 3,150 |

Linux VM activity appears in `Syslog` rather than the Identity & Access view, as Linux authentication events use a different data path (Syslog connector → `Syslog` table).

---

### Azure Caller Activities (Control-Plane Audit)

| Caller | Deletions | Creations | Updates | Total |
|---|---|---|---|---|
| 600eb99a-... (Service Principal) | 0 | 28 | 28 | 28 |
| xxx.xxx@outlook.com | 12 | 2,194 | 2,194 | 2,425 |
| 21143f50-... | 0 | 6 | 6 | 6 |
| 8d6351b6-... | 0 | 3 | 3 | 3 |

The `xxx.xxx@outlook.com` account is the primary administrator responsible for 2,425 Azure control-plane activities during lab setup.

---

### Activity Log Level Distribution (5-day period)

| Log Level | Count |
|---|---|
| Informational | 51 |
| Error | 11 |
| Warning | 0 |

The 11 error events should be investigated to confirm they are expected lab setup errors rather than security events.

---

### Identity Investigation Capabilities Used

#### Entity Pages

Sentinel entity pages provide a unified view of all activity for a specific user, IP, or host including sign-in history, MFA status, group memberships, related alerts, and UEBA risk score.

#### UEBA (User and Entity Behavior Analytics)

With BehaviorAnalyticsInsights deployed, UEBA enriches entities with:

- Peer group comparisons
- Activity timeline anomalies
- Risk scores based on behavioural deviations
- Integration with Sentinel incidents

#### Cloud Inventory Identity & Access View

Provides breakdown of all identity-related activities: user vs machine activities, top callers by operation count, activity by log level over time, and filter by operation type.

---

### Key Security Observations

- High `system`/`SYSTEM` activity (944 and 906 events) is normal for an active Windows VM but should be baselined to detect anomalous spikes
- `cybergc1` account activity (131 events) is consistent with lab usage
- Azure admin activity (2,425 operations) is expected during initial lab setup but should reduce significantly post-deployment
- 11 error-level Azure Activity events warrant investigation
- MDE telemetry events (8030, 8002) contribute significantly to overall activity counts — normal behaviour for an MDE-onboarded endpoint

---

### Recommended Additional Monitoring

- Alert on new high-privilege role assignments in Entra ID (T1078.004)
- Alert on new user account creation outside business hours (T1136)
- Monitor for service principal secret creation/rotation
- Track privileged account sign-ins with UEBA risk scoring
