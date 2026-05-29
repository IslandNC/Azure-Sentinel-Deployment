# Changelog

All notable changes to this project are documented here.

---

## [Unreleased]

### In Progress
- Additional KQL queries: network anomaly detection, cloud activity monitoring, threat hunting playbooks
- Screenshots for all documentation sections
- Additional detection rules: T1078.004, T1136, T1562, T1059 (encoded PowerShell), T1003 (LSASS)
- Placeholder subfolders for `queries/network/` and `queries/cloud/`

---

## [1.1.0] — 2026-05-30

### Added
- `queries/endpoint/process-creation-anomaly.kql` — LOLBin and suspicious process execution detection (T1059, T1218)
- `.gitignore` — macOS, Windows, editor file exclusions
- `CHANGELOG.md` — this file

### Changed
- **README.md** — Added status callout, added endpoint query to catalogue, corrected query-to-rule mapping (both RDP and SSH rules use `brute-force-signin-5m.kql`), updated repository structure tree
- **03-kql-queries.md** — Added `process-creation-anomaly.kql` to catalogue with full documentation; updated impossible travel section to note `sort by` requirement and `make_list` limitation; clarified 5m query powers both RDP and SSH rules
- **04-detection-rules.md** — Corrected rule scheduling to 5 minutes for brute force rules; fixed triage step formatting; updated KQL file links; added planned rules as table
- **05-incidents-and-alerts.md** — Added `text` language tag to alert pipeline code block; improved section structure
- **07-threat-intelligence.md** — Expanded MITRE ATT&CK coverage map with planned detection rules; added process-creation query as partial coverage
- **08-identity-and-access.md** — Added source column and explanation for MDE telemetry Events 8030/8002; expanded activity volume table

### Fixed
- **impossible-travel-detection.kql** — Added `| sort by TimeGenerated desc` before `summarize` to guarantee deterministic `make_list` ordering; documented `make_list(..., 2)` limitation in header comment
- **brute-force-signin-5m.kql** — Corrected severity comment to reflect both Medium (RDP) and Low (SSH) rules; added note that severity is set per rule, not in the query

---

## [1.0.0] — 2026-05-28

### Added
- Initial repository structure
- `docs/01-environment-setup.md` — Azure resource group, VMs, Sentinel deployment, DCR setup
- `docs/02-data-connectors.md` — Six enabled connectors, log table reference, troubleshooting notes
- `docs/03-kql-queries.md` — Query catalogue with identity detection queries
- `docs/04-detection-rules.md` — Three custom analytic rules with MITRE mapping and triage procedures
- `docs/05-incidents-and-alerts.md` — Alert pipeline, confirmed brute force alerts, SOC workflow
- `docs/06-vulnerability-management.md` — Exposure score 43/100, MDVM initiatives, remediation actions
- `docs/07-threat-intelligence.md` — TI features, threat actor profiles, initial MITRE coverage map
- `docs/08-identity-and-access.md` — 8,330 activity events, UEBA, Azure Activity audit trail
- `queries/identity/impossible-travel-detection.kql` — T1078 detection
- `queries/identity/brute-force-signin-1h.kql` — T1110.001 hunting query
- `queries/identity/brute-force-signin-5m.kql` — T1110.001 real-time alerting query
- `screenshots/README.md` — Screenshot index with 12 documented screenshots
