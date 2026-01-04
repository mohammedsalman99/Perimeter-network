# Perimeter Network Security Challenge

## Overview

This repository contains resources for a cybersecurity incident response challenge based on a simulated perimeter breach at **Initech Corp**, a mid-sized financial services company.

The scenario focuses on analyzing network logs to identify:

- Adversary techniques  
- Initial access  
- Lateral movement  
- Command and Control (C2) activity  
- Data exfiltration  

The challenge is intended for security analysts practicing log analysis using **Linux command-line tools** or **Splunk**.

---

## Key Elements

### Logs Provided

- `firewall.log` – Firewall logs  
- `ids_alerts.log` – WAF / IDS alerts  
- `vpn_auth.log` – VPN authentication logs  

### Network Assets

A reference table containing internal hosts, IP addresses, roles, operating systems, teams, and criticality levels.

### Analysis Methods

- Manual analysis (`grep`, `cat`, `awk`)
- Splunk-based analysis

---

## Prerequisites

- Basic Linux command-line knowledge  
- Splunk instance access (see PDF instructions)  

---

## Usage

### Method 1: Manual Log Analysis

Filter blocked reconnaissance attempts:

```bash
cat firewall.log | grep "BLOCK" | head
```

Check for successful VPN logins:

```bash
cat vpn_auth.log | grep "SUCCESS"
```

Identify lateral SMB activity:

```bash
cat ids_alerts.log | grep "SMB" | grep "Exploit"
```

---

### Method 2: Splunk Analysis

Search for C2 beaconing:

```text
index="network_logs" source=ids_alerts.json "TROJAN Possible C2 Beaconing"
```


## Challenge Questions

1. Which external IP performed the most reconnaissance?
2. Which internal host was targeted by firewall scans?
3. Which username was targeted in VPN authentication logs?
4. What internal IP was assigned after a successful VPN login?
5. Which port was used for lateral SMB attempts?
6. Which host beaconed to the C2 server?
7. Which IP was associated with C2 activity?
8. Which host showed exfiltration attempts?

---

## Network Assets Reference

| IP Address | Hostname | Role | OS | Team | Criticality |
|-----------|----------|------|----|------|-------------|
| 10.0.0.20 | FINANCE-SRV1 | File / Finance Server (SMB) | Windows Server | Finance IT | High |
| 10.0.0.50 | VPN-GW | VPN Gateway | Linux | NetOps | Critical |
| 10.0.0.51 | APP-WEB-01 | Internal Web/App Server | Linux | Apps Team | High |
| 10.0.0.60 | WORKSTATION-60 | Employee Workstation | Windows 10 | Sales | Medium |
| 10.8.0.23 | VPN-CLIENT-ATTK | VPN Assigned Client (Ephemeral) | N/A | N/A | Critical |
| 10.0.1.10 | DMZ-WEB | DMZ Web Server | Linux | NetOps | Medium |

---

## Attack Flow Summary

- **Reconnaissance**: External IP scans internal hosts (blocked by firewall)
- **Initial Access**: VPN brute-force against a service account
- **Lateral Movement**: SMB exploitation on port 445
- **C2 Establishment**: Beaconing to external command-and-control server
- **Exfiltration**: Large HTTP POST uploads

---

