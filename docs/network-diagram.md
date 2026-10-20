# Network Diagram

The lab runs on a hypervisor (VMware / Proxmox) with the lab network isolated from my home network and the internet restricted to update mirrors only. Addresses below are intentionally generic — no real IPs are published.

## Topology

```mermaid
flowchart TB
    INET([Internet - update mirrors only])
    FW[Firewall / Router\nVLAN segmentation + egress filtering]

    subgraph MGMT[Management VLAN]
        SIEM[Wazuh SIEM\nUbuntu Server]
    end

    subgraph LAB[Lab VLAN - domain]
        DC[Windows Server 2025\nAD Domain Controller + DNS]
        FS[Windows Server\nFile / member server]
        WS[Windows 11\nDomain-joined workstation]
        LNX[Ubuntu Server\nServices / victim]
    end

    subgraph REDVLAN[Isolated Attacker VLAN]
        ATK[Kali Linux\nAuthorized adversary emulation]
    end

    INET --- FW
    FW --- MGMT
    FW --- LAB
    FW --- REDVLAN

    DC -. Sysmon + agent logs .-> SIEM
    FS -. Sysmon + agent logs .-> SIEM
    WS -. Sysmon + agent logs .-> SIEM
    LNX -. agent logs .-> SIEM
    ATK -. "controlled, authorized traffic" .-> LAB
```

## Segmentation rationale
- **Management VLAN** isolates the SIEM so that collection/alerting keeps working even when the lab VLAN is intentionally compromised during an exercise.
- **Lab VLAN** holds the Active Directory domain and the systems under test.
- **Attacker VLAN** is firewalled so adversary emulation only reaches the lab on purpose, and never my real network.
- Egress is restricted to OS/software update sources so nothing in the lab reaches the public internet uncontrolled.

## ASCII fallback
```
[ Internet (updates only) ]
            |
      [ Firewall ] --- VLAN segmentation + egress filtering
       /    |    \
 [MGMT]  [LAB]   [ATTACKER]
   |       |         |
 Wazuh   DC/FS/WS/Linux   Kali (isolated)
   ^       |
   |_______|  endpoint telemetry (Sysmon + agents) -> Wazuh
```
