# Wazuh-SIEM-XDR-Homelab
<div>
    <img src="https://img.shields.io/badge/-Wazuh-2E57B4?&style=for-the-badge&logo=Wazuh&logoColor=white" />
</div>

---
**This project demonstrates the end-to-end deployment of a SIEM/XDR platform in a controlled lab environment, simulating a real Security Operations Center**
---
<div>
    <img src="https://img.shields.io/badge/-Kali%20Linux-557C94?&style=for-the-badge&logo=Kali%20Linux&logoColor=white" />
</div>
---

<div>
    <img src="https://img.shields.io/badge/-pfSense-4D4D4D?&style=for-the-badge&logo=pfSense&logoColor=white" />
</div>
<div>
    <img src="https://img.shields.io/badge/-Snort-FF0000?&style=for-the-badge&logo=Snort&logoColor=white" />
</div>
<div>
    <img src="https://img.shields.io/badge/-Suricata-EF3939?&style=for-the-badge&logo=Suricata&logoColor=white" />
</div>
<div>
    <img src="https://img.shields.io/badge/-pfBlocker%20NG-1E90FF?&style=for-the-badge&logo=pfBlockerNG&logoColor=white" />
</div>
<div>
    <img src="https://img.shields.io/badge/-Syslog-555555?&style=for-the-badge&logo=syslog&logoColor=white" />
</div>
---
<!-- Windows Server with Server logo (if preferred) -->
<div>
    <img src="https://img.shields.io/badge/-Windows%20Server-0078D4?&style=for-the-badge&logo=microsoft&logoColor=white" />
</div>

<!-- Windows 11 Enterprise with distinct color -->
<div>
    <img src="https://img.shields.io/badge/-Windows%2011%20Enterprise-2575C3?&style=for-the-badge&logo=windows11&logoColor=white" />
</div>
---


                        ┌────────────────────────────┐   
                        │        Kali Linux          │   
                        │   (Attack Simulation)      │
                        │     192.168.60.101         │
                        └────────────┬───────────────┘
                                     │
                                     |
    ┌──────────────────────────────────────────────────────────────┐
    │                        pfSense Firewall                      │
    │                      192.168.60.1/24                         │
    │                                                              │
    │  • Snort (IDS/IPS)                                           │
    │  • Suricata (IDS/IPS)                                        │
    │  • pfBlockerNG (Threat Intel)                                │
    │  • Syslog-ng (Log Forwarding)                                │
    └──────────────┬───────────────────────────────┬───────────────┘
               │                               │
               ▼                               ▼
    ┌──────────────────────┐        ┌────────────────────────┐
    │  Windows Server 2022 │        │   Windows Client       │
    │  Domain Controller   │        │   Domain Joined        │
    │  192.168.60.10       │        │   192.168.60.100       │
    └──────────┬───────────┘        └──────────┬─────────────┘
              │                               │
              └──────────────┬────────────────┘
                             ▼
                ┌────────────────────────────┐
                │   Wazuh SIEM/XDR Server    │
                │   Ubuntu 24.04 LTS         │
                │   192.168.60.50            │
                │                            │
                │  • Wazuh Manager           │
                │  • Wazuh Indexer           │
                │  • Wazuh Dashboard         │
                └────────────────────────────┘
