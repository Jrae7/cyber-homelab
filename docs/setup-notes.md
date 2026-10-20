# Setup Notes (High Level)

A high-level record of how I built the lab. This is intentionally **conceptual, not a copy-paste runbook** — it shows the process and decisions without exposing my environment. Fill in or expand sections as your build evolves.

## 1. Hypervisor & networking
1. Provision the hypervisor (VMware Workstation/ESXi or Proxmox).
2. Create isolated virtual networks / VLANs: **management**, **lab**, and **attacker**.
3. Stand up a firewall/router VM to segment the VLANs and restrict egress to update sources only.

## 2. Active Directory domain
1. Install **Windows Server 2025**; set a static (internal) address and rename the host.
2. Promote to a **Domain Controller** (new forest) and install the **DNS** role.
3. Create an Organizational Unit (OU) structure (Workstations, Servers, Users, Service Accounts).
4. Create test users and **scoped service accounts** — avoid using Domain Admin for routine tasks.
5. Define **Group Policy Objects** for baseline hardening and logging.

## 3. Endpoints
1. Install **Windows 11**; join it to the domain.
2. Add a **member/file server** for shares and service-account exercises.
3. Apply the Sysmon + audit-policy GPOs so endpoints emit rich telemetry.

## 4. Telemetry & SIEM
1. Deploy an **Ubuntu Server** on the management VLAN and install the **Wazuh** manager + dashboard.
2. Install **Sysmon** on Windows hosts with a curated configuration (process creation, network, image load, registry).
3. Enable Windows **audit policies** (logon events, process creation with command line, Kerberos, object access as needed).
4. Install Wazuh agents on all hosts and confirm events are arriving in the dashboard.

## 5. Validate detections
1. Run **authorized** adversary-emulation steps from the attacker VLAN against lab hosts I own.
2. Confirm the expected telemetry appears in Wazuh.
3. Build/tune detection rules — see [`wazuh-detections`](https://github.com/Jrae7/wazuh-detections).

## Housekeeping
- Snapshot before each exercise; roll back after.
- Keep service accounts least-privileged.
- Never bridge the lab to a production or home network.
