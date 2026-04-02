# 🔐 SSH Brute Force Detection Lab

## 🎯 Project Overview

This project demonstrates a complete Security Information and Event Management (SIEM) implementation using Splunk to detect and visualize SSH brute force attacks. A simulated attack was launched from Kali Linux using Hydra against an Ubuntu target, with Splunk ingesting, analyzing, and visualizing the attack patterns in real-time.

## 🛠️ Technologies Used

| Tool | Purpose |
|------|---------|
| Splunk Enterprise | SIEM platform for log ingestion and analysis |
| Kali Linux | Attacker VM running Hydra |
| Ubuntu 22.04 | Target host with SSH service |
| Hydra | Password brute force tool |
| KVM | Virtualization platform |

## 📊 Attack Simulation Details

- Attack Type: SSH Password Brute Force
- Target User: ubuntu
- Source IP: 192.168.122.109 (Kali VM)
- Target IP: 192.168.122.1 (Ubuntu Host)
- Attack Time: April 2, 2026 - 17:09:54 (5:09 PM)
- Total Attempts: 22 failed logins
- Attack Tool: Hydra v9.6

## 📈 Dashboard Features

| Panel | Visualization | Purpose |
|-------|---------------|---------|
| Failed SSH Attempts | Line Chart | Attack intensity over time |
| Top Attackers | Bar Chart | Identify malicious source IPs |
| Attacks Over Time | Table | Detailed forensic evidence |

## 🖼️ Dashboard Screenshots

###  Lab Environment -Ubuntu Auth.log
![Ubuntu Auth.log](01-ubuntu-auth.log-terminal.png)
*Ubuntu host logging SSH failed password attempts from kali VM*

### Attack Simulation -Kali Hydra
![Kali Hydra](02-kali-hydra-attack.png)
*Kali Linux running Hydra brute force attack against Ubuntu SSH*

### splunk Search Results
![Splunk Search](splunk-search-result.png)
*Splunk search showing ingested SSH failure logs with timestap*

### Full Dashboard View
![Full Dashboard](03-dashboard-full-view.png)
*Complete Splunk dashboard with all three visualization panels*

### Attack Spike Detection (Line Chart)
![Attack Spike](04-line-chart-spike.png)
*Line chart showing attack spike at 17:09 with 11 failed attempts*

### Top Attacker Identification (Bar Chart)
![Top Attacker](05-bar-chart-top-attacker.png)
*Bar chart identifying 192.168.122.109 as source of 22 attempts*

### Detailed Attack Log (Table)
![Attack Details](06-table-attack-details.png)
*Timestamped evidence of each failed authentication attempt*

## 🔍 Sample SPL Queries

### Time-based Attack Analysis
```spl
index=security_logs sourcetype=linux_secure "Failed password"
| timechart count


### Top Attackers Identification
```spl
index=security_logs sourcetype=linux_secure "Failed password"
| stats count by host
| rename host as "Source IP"

### Detailed Attack Table
```spl
index=security_logs sourcetype=linux_secure "Failed password"
| rex "^(?<log_time>\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2})"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| eval Timestamp = strftime(strptime(log_time, "%Y-%m-%dT%H:%M:%S"), "%Y-%m-%d %H:%M:%S")
| table Timestamp, src_ip
| rename src_ip as "Source IP"
| head 20

### Project Structure
├── README.md                 # Project overview
├── screenshots/              # All dashboard screenshots
└── queries/                  # SPL query files

### How to Recreate This Lab
1. set up ubuntu target with SSH enabled
2. configure Splunk to monitor /var/log/auth.log
3. Launch Kali VM with network access to ubuntu
4. Run Hydra attack:
   hydra -l ubuntu -P password.txt ssh://[TARGET IP]
5. Verify logs in Splunk search & Reporting
6. Create dashboard using dashboard studio
7. Add Visualization (Line chart, Bar chart,Table)

## 📚 Skills Demonstrated

- ✅ SIEM implementation and configuration
- ✅ Security log analysis
- ✅ Splunk SPL (Search Processing Language)
- ✅ Dashboard design and visualization
- ✅ Attack simulation (Red Team)
- ✅ Threat detection and alerting
- ✅ Technical documentation

## 📚 What I Learned
Technical Skills:
✅ Splunk SPL (Search Processing Language): Learned to write queries like timechart, stats count by host, and rex for regex extraction
✅ Log Analysis: Understood how Linux auth.log captures authentication events (SSH, sudo, PAM)
✅ SIEM Implementation: Configured Splunk Universal Forwarder to monitor and forward logs
✅ Dashboard Design: Built multi-panel dashboards with different visualization types (line, bar, table)
✅ Attack Simulation: Used Hydra to simulate real-world brute force attacks
✅ Regex Extraction: Extracted IP addresses and usernames from unstructured log data
Security Concepts:
✅ Brute Force Detection: How to identify password spraying attacks in real-time
✅ Source IP Attribution: Tracing attacks back to specific IP addresses
✅ Attack Patterns: Understanding attack timing, frequency, and targeting
✅ Forensic Evidence: Using timestamped logs as proof of attack activity
Soft Skills:
✅ Problem Solving: Debugged Splunk configuration and data ingestion issues
✅ Documentation: Created professional technical documentation for portfolio
✅ Lab Building: Designed and executed complete security monitoring lab


## 🔧 Recommendations

Short-term (Next 24 hours):
1.Block the attacking IP using Ubuntu firewall:
(sudo ufw deny from 192.168.122.109)
2.Set up fail2ban to automatically block repeated failures
Medium-term (Next week):
3. Add alerting: Configure Splunk alerts when >10 failures occur in 5 minutes
4. Add more dashboards: Monitor successful logins, root access, and sudo attempts
5. Geolocation mapping: Plot attack sources on a world map
Long-term (Future lab improvements):
6. Integrate with TheHive for automated incident response
7. Add MFA detection - monitor for authentication after brute force
8. Create weekly security reports using Splunk scheduled reports
9. Add threat intelligence feeds to compare attacking IPs against known malicious databases
For Production Environment:
Enforce key-based authentication (disable password auth)
Implement rate-limiting on SSH connections
Use a VPN for administrative access
Regular security audits of failed login patterns

## 📅 Project Date

April 2026
