🛡️ Wazuh SIEM/XDR Homelab
Stage 1 — Enterprise-Style Deployment & Log Integration
🎯 Executive Summary

This project demonstrates the end-to-end deployment of a SIEM/XDR platform in a controlled lab environment, simulating a real Security Operations Center (SOC).

The focus of this stage:

Build a segmented enterprise network
Deploy Wazuh SIEM/XDR
Onboard endpoints (Windows & Linux)
Integrate network security telemetry (pfSense, IDS/IPS, Threat Intel)
Validate centralized log ingestion and visibility

This is not a basic lab — it reflects real-world SOC architecture and workflows.

🧱 Architecture Overview
                        ┌────────────────────────────┐
                        │        Kali Linux          │
                        │   (Attack Simulation)      │
                        │     192.168.60.101         │
                        └────────────┬───────────────┘
                                     │
                                     ▼
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
🖥️ Infrastructure Inventory
Asset	Role	Key Function
pfSense	Firewall / Router	Network security + log source
Windows Server 2022	Domain Controller	Authentication + AD logs
Windows Client	Endpoint	User activity simulation
Ubuntu Server	Wazuh SIEM/XDR	Centralized detection
Kali Linux	Attacker Machine	Adversary simulation
🔐 Security Stack (Defense-in-Depth)
Layer	Tool	Purpose
Network Perimeter	pfSense	Traffic control & segmentation
IDS/IPS	Snort / Suricata	Threat detection
Threat Intelligence	pfBlockerNG	Malicious IP/domain blocking
Endpoint Security	Wazuh Agent	Host-based detection
SIEM/XDR	Wazuh	Centralized analysis
⚙️ Wazuh Deployment
✔ Deployment Model
All-in-One (Manager + Indexer + Dashboard)
Installed on Ubuntu Server 24.04 LTS
✔ Key Actions
Installed Wazuh via official quickstart
Configured required ports:
1514 (agent communication)
1515 (registration)
55000 (API)
443 (dashboard)
Verified service health
Accessed dashboard successfully
🖥️ Endpoint Onboarding
✔ Systems Integrated
Windows Server 2022 (Domain Controller)
Windows Client (Domain-Joined)
Kali Linux (Linux Agent)
✔ Result
Agents successfully registered
Real-time logs flowing
System inventory visible
📊 Validation Results
✔ Agents reporting to Wazuh
✔ Event logs searchable
✔ Endpoint visibility established
✔ SIEM dashboard operational
🔗 Network Log Integration (Critical Capability)
🎯 Objective

Ingest logs from:

Firewall (pfSense)
IDS/IPS (Snort / Suricata)
Threat Intelligence (pfBlockerNG)
🔧 Implementation Summary
✔ pfSense Configuration
Enabled remote syslog forwarding

Sent logs to:

192.168.60.50:514
✔ Wazuh Configuration
<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>192.168.60.1</allowed-ips>
</remote>
✔ IDS/IPS Logging
Snort → System logs
Suricata → Syslog + EVE JSON
📈 Outcome

The SIEM now ingests:

Firewall traffic (allow/deny)
IDS alerts (intrusion attempts)
Threat intelligence blocks
Endpoint logs (Windows + Linux)

➡️ This enables correlation between network and endpoint activity, which is essential in real SOC environments.

🧠 Key Skills Demonstrated
🔹 SIEM Engineering
Wazuh deployment and configuration
Log ingestion pipeline design
Data normalization (syslog)
🔹 Blue Team Operations
Endpoint monitoring
Threat visibility
Detection readiness
🔹 Network Security Integration
IDS/IPS telemetry integration
Firewall log centralization
Threat intelligence correlation
🔹 Troubleshooting & Validation
Service verification
Agent connectivity debugging
Log flow validation
📌 What Makes This Project Stand Out
Not tool-focused — architecture-focused
Combines:
Endpoint telemetry
Network telemetry
Threat intelligence
Mirrors real SOC data flow
Designed for attack simulation (next stage)
🎥 Demonstration

📌 [Insert Video Walkthrough Here]

📸 Evidence

📌 [Insert Wazuh Dashboard Screenshot]
📌 [Insert Agent Deployment Screenshot]
📌 [Insert Log Search Screenshot]

🚀 Lessons Learned
SIEM value depends on data quality and integration, not just installation
Network logs + endpoint logs together provide true visibility
Default configurations are not enough — tuning is required
Centralized logging is the foundation for threat detection