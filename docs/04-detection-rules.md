## 04 — Detection Rules
### Overview

Custom detection rules were created in Microsoft Sentinel using scheduled KQL analytic rules. Each rule is configured to run on a defined schedule, generate incidents, and map to MITRE ATT&CK techniques.

All rules are of type **Custom detection rule** with **Incident correlation enabled** and **Scheduled execution**.

---

### Detection Rules Summary

| Rule Name | Severity | MITRE Technique | KQL File | Status |
|---|---|---|---|---|
| Entra ID – User Sign-in from Different Location | High | T1078 — Valid Accounts | [impossible-travel-detection.kql](../queries/identity/impossible-travel-detection.kql) | Enabled |
| RDP Brute Force Attack | Medium | T1110.001 — Password Guessing | [brute-force-signin-5m.kql](../queries/identity/brute-force-signin-5m.kql) | Enabled |
| SSH Brute Force on Linux | Low | T1110.001 — Password Guessing | [brute-force-signin-5m.kql](../queries/identity/brute-force-signin-5m.kql) | Enabled |

> Both RDP and SSH rules use `brute-force-signin-5m.kql` as their scheduled rule query. `brute-force-signin-1h.kql` is used for manual hunting and retrospective investigation.

---

### Rule Details

#### 1. Entra ID – User Sign-in from Different Location

- **Severity:** High
- **Rule Type:** Custom detection rule
- **Scheduling:** Runs every 1 hour, looks back 24 hours
- **Incident Correlation:** Enabled
- **MITRE ATT&CK:** T1078 — Valid Accounts
- **Description:** Detects when a user account signs in from two geographically different countries within a 24-hour window. This may indicate account compromise, credential theft, or use of anonymising proxies/VPNs.
- **Alert Trigger:** `PreviousCountry != NewCountry`
- **Entity Mapping:** UserPrincipalName, IPAddress, Country

**Triage Steps:**

1. Verify whether the user recently travelled
2. Check MFA status on both sign-ins
3. Review other activity from both source IPs
4. Confirm whether the sign-in device is known/managed
5. Escalate if sign-in is from a high-risk country or anonymous IP

---

#### 2. RDP Brute Force Attack

- **Severity:** Medium
- **Rule Type:** Custom detection rule
- **Scheduling:** Runs every 5 minutes, looks back 5 minutes
- **Incident Correlation:** Enabled
- **MITRE ATT&CK:** T1110.001 — Brute Force: Password Guessing
- **Description:** Detects repeated failed Remote Desktop Protocol (RDP) authentication attempts from a single source IP. RDP (port 3389) on `windows-vm` is intentionally exposed to generate realistic brute force telemetry in the lab.
- **Alert Trigger:** `FailedAttempts >= 5` within 5-minute window from same IP
- **Entity Mapping:** IPAddress, UserPrincipalName, Computer

**Triage Steps:**

1. Check source IP reputation (VirusTotal, AbuseIPDB, Sentinel TI)
2. Review whether any attempts succeeded (`ResultType == 0` after failures)
3. Check if the targeted account was locked out
4. If successful logon follows failed attempts — escalate as likely compromise
5. Consider blocking source IP via NSG rule

---

#### 3. SSH Brute Force on Linux

- **Severity:** Low
- **Rule Type:** Custom detection rule
- **Scheduling:** Runs every 5 minutes, looks back 5 minutes
- **Incident Correlation:** Enabled
- **MITRE ATT&CK:** T1110.001 — Brute Force: Password Guessing
- **Description:** Detects rapid failed SSH authentication attempts against `linux-vm`. SSH (port 22) is intentionally exposed in this lab to generate brute force alert data.
- **Alert Trigger:** `FailedAttempts >= 5` within 5-minute window
- **Entity Mapping:** IPAddress, UserPrincipalName
- **Lab Evidence:** Two *Brute force attack on Linux detected* alerts were generated with Low severity and New status, confirming the rule is firing on real telemetry

**Triage Steps:**

1. Check source IP against threat intelligence
2. Review `/var/log/auth.log` on linux-vm for targeted usernames
3. Confirm no successful SSH logon from the attacking IP
4. Correlate with any lateral movement indicators on other VMs
5. Block source IP via NSG if attack is ongoing

---

### Rule Configuration Notes

- All rules use **Scheduled** query type (not NRT/Near Real Time)
- Incident correlation is enabled on all rules to group related alerts into a single incident
- Rules were validated by observing generated alerts in the Sentinel Alerts view
- The 5-minute scheduling on both brute force rules is appropriate for real-time detection; the 1-hour hunting query (`brute-force-signin-1h.kql`) is suited for broader retrospective investigation
- MITRE ATT&CK technique tags are attached to each rule for integration with the MITRE ATT&CK workbook

---

### Expanding the Rule Set

Additional rules to consider for more complete detection coverage:

| MITRE | Technique | Description |
|---|---|---|
| T1078.004 | Cloud account abuse | New high-privilege role assignments in Entra ID |
| T1136 | Account creation | New user accounts created outside business hours |
| T1562 | Impair defences | MDE tamper protection disabled |
| T1059 | Command and Scripting Interpreter | PowerShell execution with encoded commands |
| T1003 | Credential dumping | LSASS access on windows-vm |
