# 03 — KQL Detection Queries

## Overview

All KQL queries were developed and tested in the `log-cybergc1-lab` Log Analytics Workspace. Queries are organised by detection category and stored as individual `.kql` files in the `/queries` folder. This document serves as a reference catalogue.

## Query Catalogue

| File | Category | Purpose | Lookback | MITRE Technique |
|---|---|---|---|---|
| `queries/identity/impossible-travel-detection.kql` | Identity | Sign-ins from different countries | 24h | T1078 — Valid Accounts |
| `queries/identity/brute-force-signin-1h.kql` | Identity | Failed logins ≥5 per IP (hunting) | 1h | T1110 — Brute Force |
| `queries/identity/brute-force-signin-5m.kql` | Identity | Failed logins ≥5 per IP (real-time) | 5m | T1110 — Brute Force |

> Additional queries covering endpoint, network, cloud activity, and threat hunting are stored in the `/queries` subfolders.

---

## Identity Queries

### Impossible Travel Detection
**File:** `queries/identity/impossible-travel-detection.kql`  
**MITRE:** T1078 — Valid Accounts  
**Lookback:** 24 hours  
**Description:** Detects users who have signed in from two different countries within a 24-hour window. This is a classic indicator of a compromised account or credential sharing.

**Key Logic:**
- Uses `make_list(..., 2)` to retrieve the two most recent sign-ins per user
- Compares `PreviousCountry` vs `NewCountry` — fires only when they differ
- Enriches results with device, OS, browser, city, and IP for investigation context
- `array_length(SigninHistory) == 2` ensures at least two sign-ins exist before comparison

**Tuning Notes:**
- In single-user lab environments, this returns 0 results unless sign-ins occur from different geographic locations
- Threshold can be tightened by adding a `TimeDelta` check (e.g., impossible travel within 2 hours)
- Consider adding MFA status field for enrichment

---

### Brute Force Sign-in Detection (1 Hour — Hunting)
**File:** `queries/identity/brute-force-signin-1h.kql`  
**MITRE:** T1110 — Brute Force  
**Lookback:** 1 hour  
**Description:** Identifies IP addresses with 5 or more failed Entra ID sign-in attempts within the past hour. Suited for retrospective hunting and manual investigation.

**Key Logic:**
- Filters `ResultType != 0` (non-zero = failed authentication)
- Groups by `IPAddress` and `UserPrincipalName`
- Captures `FirstAttempt` and `LastAttempt` timestamps for timeline analysis
- Threshold: `FailedAttempts >= 5`

**Tuning Notes:**
- Add `| extend TimeDelta = LastAttempt - FirstAttempt` to show attack velocity
- Adjust threshold based on environment — 5 is appropriate for lab; production may need 10+
- Cross-reference IPs with threat intelligence feeds via `ThreatIntelligenceIndicator` table

---

### Brute Force Sign-in Detection (5 Minutes — Real-Time Alerting)
**File:** `queries/identity/brute-force-signin-5m.kql`  
**MITRE:** T1110 — Brute Force  
**Lookback:** 5 minutes  
**Description:** Tight-window variant used as the basis for the scheduled analytic rule in Sentinel. Designed for real-time alerting with minimal latency.

**Key Logic:**
- Identical structure to the 1-hour version but with `ago(5m)` lookback
- Summarises by `IPAddress` and `bin(TimeGenerated, 10m)` for temporal grouping
- Captures the set of targeted `UserPrincipalName` values per IP per time bucket

**When to Use Which:**
- **5-minute version:** Use in Sentinel analytic rules scheduled to run every 5–10 minutes for real-time alerting
- **1-hour version:** Use in hunting notebooks or manual investigation for broader pattern detection

---

## Query Development Approach

All queries follow a consistent structure:

1. **Source table** — identify the relevant log table
2. **Time filter** — scope to relevant lookback window
3. **Pre-filter** — remove noise early (e.g., `ResultType != 0`, `isnotempty(IPAddress)`)
4. **Enrich** — extend fields for human-readable output (geo, device, OS)
5. **Summarise** — aggregate by entity (IP, user, host)
6. **Threshold** — apply minimum count filter to reduce false positives
7. **Order** — sort by most severe/recent for analyst triage

This structure mirrors production SOC KQL practices and maps directly to the Sentinel analytic rule pipeline.
