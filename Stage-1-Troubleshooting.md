# pfSense → Wazuh Syslog Integration (Troubleshooting & Analysis Report)

## Objective

The goal of this stage was to forward firewall and security logs from pfSense to the Wazuh SIEM using syslog (UDP, custom port 5514), and make them searchable within the Wazuh Dashboard (Discover).

Target logs:
- Firewall logs (filterlog)
- Suricata IDS alerts
- pfBlockerNG events

---

## Lab Architecture

- pfSense Firewall: 192.168.60.1
- Wazuh Manager: 192.168.60.50
- Syslog Protocol: UDP
- Custom Port: 5514

---

## Configuration Steps

### 1. pfSense Syslog Configuration

**Path:**

Status → System Logs → Settings

**Configured:**
- Remote Log Server: `192.168.60.50:5514`
- Transport: UDP
- Enabled:
  - Firewall Events
  - System Logs
  - DNS
  - DHCP

---

### 2. Wazuh Syslog Listener Configuration

**File:**
```
/var/ossec/etc/ossec.conf
```

**Configured:**
```xml
<remote>
  <connection>syslog</connection>
  <port>5514</port>
  <protocol>udp</protocol>
  <allowed-ips>192.168.60.1</allowed-ips>
</remote>

```
---

### 3. Enabled Full Log Storage
```
xml
<logall>yes</logall>
<logall_json>yes</logall_json>
```

---

### 4. Decoder Implementation

File:
```
</> Bash
/var/ossec/etc/decoders/local_decoder.xml
```

Final working configuration:
```
xml
<decoder name="pfsense">
  <program_name>filterlog</program_name>
</decoder>
```

---

### 5. Custom Rule

File:
```
</> Bash
/var/ossec/etc/rules/local_rules.xml
```
```
<group name="pfsense,syslog,">
  <rule id="100100" level="5">
    <decoded_as>pfsense</decoded_as>
    <description>pfSense firewall log detected</description>
  </rule>
</group>
```
---

### 6. Validation Steps | Network-Level Validation
```
</> Bash
tcpdump -A -i any port 5514
```
Result:

Confirmed presence of filterlog entries
Verified pfSense → Wazuh connectivity

---

### 7. Service Validation
```
</> Bash
lsof -i :5514
```
Result:

Wazuh listening on UDP `port 5514`

---

### 8. Log Pipeline Validation
```
</> Bash
tail -f /var/ossec/logs/archives/archives.log
```
```
</> Bash
tail -f /var/ossec/logs/alerts/alerts.log
```

### ✅Successfully Achieved

| Component                      | Status |
| ------------------------------ | ------ |
| pfSense log generation         | ✅      |
| Syslog transport               | ✅      |
| Network connectivity           | ✅      |
| Wazuh ingestion (port binding) | ✅      |
| Decoder configuration          | ✅      |
| Rule creation                  | ✅      |

### ❌Not Achieved

| Component                        | Status |
| -------------------------------- | ------ |
| pfSense logs in alerts.log       | ❌      |
| pfSense logs in Wazuh Dashboard  | ❌      |
| wazuh-alerts-* index for pfSense | ❌      |

### Current State in Dashboard

●Only the following logs are visible:
```
Windows Security Events ✅
Agent-based logs ✅
```

Missing:
```
filterlog ❌
Suricata ❌
pfBlockerNG ❌
```
---
## Technical Analysis
Key Finding

Even though:

●pfSense logs are confirmed via tcpdump

●Wazuh is listening and receiving traffic

●Decoder and rules are present

`[No alerts are generated]`

---
## Interpretation

This indicates:

●Wazuh receives syslog traffic but does not process it into alerts.

Possible reasons:

1.Logs are not fully parsed beyond program_name

2.No field extraction (src_ip, dst_ip, action)

3.Rule matching is too generic

4.Log format incompatibility with Wazuh parsing engine

---
## Important Observation

Wazuh does not behave as a traditional SIEM:

●It does not index raw syslog automatically

●It requires successful decoding + rule matching to generate alerts

---
## Lessons Learned

●SIEM ingestion ≠ detection

●Network visibility does not guarantee indexing

●Wazuh depends heavily on decoders and rules

●Regex limitations can break configurations entirely

●Structured fields (program_name) are more reliable than regex

---
## Conclusion

This stage successfully demonstrated:

●End-to-end syslog transport validation

●Wazuh ingestion configuration

●Decoder and rule development

●Systematic troubleshooting methodology

However:

●pfSense logs were not successfully transformed into alerts or indexed data.
