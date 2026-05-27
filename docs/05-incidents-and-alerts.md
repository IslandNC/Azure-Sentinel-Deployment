# 05 — Incidents and Alerts

## Overview

This document covers the incident management and alert triage workflows practiced in this deployment. Microsoft Sentinel automatically creates incidents from analytic rule triggers, enabling a structured investigation workflow aligned with real-world SOC practices.

## Alert Pipeline

```
Log Source (VM / Entra ID / Azure)
    ↓
    Log Analytics Workspace (log-cybergc1-lab)
        ↓
        KQL Analytic Rule (scheduled, 5m / 1h lookback)
            ↓
            Alert Generated
                ↓
                Incident Created (with entity mapping)
                    ↓
                    SOC Analyst Triage
                    ```

                    ## Observed Alerts

                    ### Brute Force Attack on Linux (x2)
                    - **Alert Name:** Brute force attack on Linux detected
                    - **Severity:** Low
                    - **Status:** New
                    - **Category:** Credential Access
                    - **Source Rule:** SSH Brute Force on Linux (custom detection rule)
                    - **Investigation State:** Not started
                    - **Context:** Two identical alerts were generated, indicating multiple bursts of SSH brute force activity against `linux-vm`. The alerts confirm that the analytic rule is correctly firing on real telemetry ingested from the Linux VM via the Syslog data connector.

                    ## Alert Management Configuration

                    ### Built-in Alert Tuning
                    Sentinel's built-in alert tuning was enabled to prioritise high-fidelity, actionable alerts. This feature automatically suppresses lower-confidence alerts to reduce analyst fatigue.

                    ### Alert Filters Applied
                    During investigation, alerts were filtered by **Status: New, In progress** to focus triage on active, unresolved alerts. This mirrors standard SOC queue management practice.

                    ## Incident Investigation Workflow

                    For each generated incident, the following triage process was followed:

                    1. **Review alert details** — Name, severity, category, time of generation
                    2. **Examine entities** — Identify the affected IPs, users, hosts
                    3. **Check investigation state** — Was the attack ongoing or historical?
                    4. **Query related logs** — Use KQL to pull additional context from `Syslog`, `SecurityEvent`, or `SigninLogs`
                    5. **Assess impact** — Did any brute force attempts succeed? Was there lateral movement?
                    6. **Classify and close** — Mark as True Positive / False Positive / Benign Positive with notes

                    ## Incident Management Features Explored

                    - **Incidents queue** — Centralised view of all generated incidents with severity, status, and owner
                    - **Alert grouping** — Related alerts grouped into a single incident via incident correlation
                    - **Entity investigation** — Pivot from incident to IP address, user, or host entity pages
                    - **Incident timeline** — Chronological view of events associated with an incident
                    - **Comments and audit trail** — Adding analyst notes during investigation
                    - **Status management** — Transitioning incidents through New → In Progress → Closed

                    ## SOC Workflow Alignment

                    This lab mirrors Tier 1 SOC analyst workflows:

                    | SOC Activity | Sentinel Feature Used |
                    |---|---|
                    | Alert triage | Incidents & Alerts queue |
                    | Entity investigation | Entity pages (IP, User, Host) |
                    | Log deep-dive | Log Analytics KQL queries |
                    | Threat context | Threat Intelligence integration |
                    | Escalation documentation | Incident comments and notes |
                    | Metrics tracking | Workbooks and dashboards |

                    ## Notes on Lab Alert Volume

                    In this lab environment, alert volume is intentionally low due to the limited number of endpoints (2 VMs). In a production SOC, the same detection rules would generate significantly more alerts. Alert tuning thresholds (e.g., `FailedAttempts >= 5`) would need to be calibrated based on baseline noise levels in the environment.
                    
