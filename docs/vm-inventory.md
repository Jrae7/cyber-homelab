# VM Inventory

A high-level inventory of the lab. **No IP addresses, host names, credentials, or license keys are published** — only OS and role, so the architecture is understandable without exposing anything sensitive.

> This is a representative inventory of an 8+ VM lab. Adjust the specifics to match your current build before relying on it.

| # | Role | Operating System | Purpose | Notes |
|---|------|------------------|---------|-------|
| 1 | Domain Controller | Windows Server 2025 | AD DS, DNS, Group Policy | Forest/domain root; audit policy + Sysmon GPO deployed |
| 2 | Member / File Server | Windows Server 2025 | File shares, service accounts | Used to generate lateral-movement and share-access telemetry |
| 3 | Workstation | Windows 11 | Domain-joined endpoint | Simulates a standard user host; Sysmon installed |
| 4 | SIEM | Ubuntu Server (LTS) | Wazuh manager + dashboard | Lives on the management VLAN |
| 5 | Linux Server | Ubuntu Server (LTS) | Web / service host | Target for service-level exercises |
| 6 | Attacker | Kali Linux | Authorized adversary emulation | Isolated VLAN; only reaches lab intentionally |
| 7 | Utility | Windows / Linux | Jump / tooling host | Snapshots, scripts, package mirror |
| 8 | Spare / Rebuild | varies | Clean-slate rebuilds | Kept for quick redeploys from template |

## Conventions I follow
- **Snapshots** before every exercise so hosts can be rolled back to a known-good state.
- **Templates / golden images** for fast rebuilds.
- **Least privilege** — dedicated, scoped service accounts rather than Domain Admin everywhere.
- **Telemetry on by default** — Sysmon + Windows audit policy on every Windows host, forwarded to Wazuh.
