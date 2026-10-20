# cyber-homelab

Sanitized documentation for my personal cybersecurity home lab — an 8+ VM environment I use to practice **detection engineering, incident response, and Active Directory security** against authorized, self-contained attacks.

> ⚠️ **Scope & safety:** Everything in this lab is isolated on a private, internet-restricted virtual network. This repo contains **architecture and process documentation only** — no IP addresses, credentials, host names, or offensive payloads. All exercises are run against systems I own.

## What this lab demonstrates
- Building and operating a **Windows Server 2025 Active Directory** domain from scratch
- Joining and managing a **Windows 11** endpoint in the domain
- Centralized log collection and alerting with a **Wazuh** SIEM + **Sysmon** endpoint telemetry
- Writing and validating **MITRE ATT&CK-mapped detections** (see [`wazuh-detections`](https://github.com/Jrae7/wazuh-detections))
- Safe, authorized adversary emulation to generate realistic telemetry for blue-team practice

## Architecture at a glance
See [`docs/network-diagram.md`](docs/network-diagram.md) for the full diagram.

```
          ┌──────────────┐
          │   Firewall   │  (segmented, egress-restricted)
          └──────┬───────┘
         ┌────────┴─────────┐
   Management VLAN      Lab VLAN
         │                   │
   ┌─────┴─────┐      ┌──────┴───────────────────────┐
   │ Wazuh SIEM│◀─────│ DC · Workstation · Servers · │
   └───────────┘ logs │ Attacker (isolated)          │
                      └──────────────────────────────┘
```

## Documentation
| Doc | Contents |
|-----|----------|
| [`docs/network-diagram.md`](docs/network-diagram.md) | Network topology and VLAN segmentation (Mermaid) |
| [`docs/vm-inventory.md`](docs/vm-inventory.md) | Each VM, its OS, and its role (no IPs / creds) |
| [`docs/setup-notes.md`](docs/setup-notes.md) | High-level steps I followed to build the domain + telemetry |

## Screenshots
<!-- Add sanitized screenshots here (blur or omit host names / IPs / usernames). -->
_Screenshots coming soon — dashboards and topology, sanitized._

## Related repos
- [`wazuh-detections`](https://github.com/Jrae7/wazuh-detections) — detections tested in this lab
- [`log-parser`](https://github.com/Jrae7/log-parser) — tooling for triaging the logs this lab produces
