# 🛡️ Splunk SIEM Lab – Linux SSH Brute Force Detection

This project demonstrates the deployment and configuration of **Splunk Enterprise** as a SIEM solution on an **Ubuntu 24.04 LTS** virtual machine.  
The lab focuses on ingesting Linux authentication logs and detecting **failed SSH login attempts**, simulating real-world SOC monitoring and alerting workflows.

---

## 🏗️ Lab Architecture

- **Host OS:** Windows
- **Virtualization:** Oracle VirtualBox
- **Guest OS:** Ubuntu Server 24.04 LTS
- **SIEM Platform:** Splunk Enterprise 9.2.x
- **Network Mode:** NAT with Port Forwarding
- **Access Method:** Web UI via `http://localhost:8000`

---

## ⚙️ Environment Setup

### 1. Linux Server Deployment
- Created an Ubuntu Server 24.04 virtual machine in VirtualBox
- Installed system updates and required dependencies
- Configured SSH service for authentication testing

Screenshots:
- `screenshots/Linux_Server_VM_Success_Creation.png`

---

### 2. Splunk Enterprise Installation
- Downloaded the official Splunk Enterprise `.deb` package
- Installed Splunk using `dpkg`
- Started and verified Splunk services
- Enabled Splunk to start at boot

Screenshots:
- `screenshots/Splunk_Download_Success.png`
- `screenshots/Splunkd_Running.png`
- `screenshots/Splunk_Login_Success.png`

---

## 🔐 Log Ingestion Configuration

### Linux Authentication Logs

Ubuntu 24.04 does not always log SSH authentication events to `/var/log/auth.log` by default.  
To ensure reliable log ingestion, additional configuration was required.

Actions taken:
- Installed and enabled `rsyslog`
- Restarted the SSH service
- Verified authentication logs were written to `/var/log/auth.log`
- Configured Splunk to monitor the authentication log file

Commands used:

sudo apt install rsyslog -y
sudo systemctl enable rsyslog
sudo systemctl restart ssh

Screenshots:
- `screenshots/update_dependencies.png`
- `screenshots/SSH_Relink.png`
- `screenshots/Add_Auth_Logs.png`

---

## 🔌 Splunk Data Input

Splunk was configured to ingest Linux authentication logs for security monitoring.

Input configuration:
- **Input Type:** Monitor – Files & Directories
- **Monitored File:** `/var/log/auth.log`
- **Index:** main
- **Sourcetype:** linux_secure
- **App Context:** Search & Reporting

Screenshot:
- `screenshots/Add_Auth_Logs.png`

---

## 🔍 Detection & Analysis

To simulate attacker behavior, repeated failed SSH login attempts were generated using an invalid username.

Attack simulation:

ssh fakeuser@localhost


These events were ingested into Splunk and analyzed using the following SPL query:



index=main sourcetype=linux_secure "Failed password"
| stats count by user, src
| sort -count


This detection logic:
- Identifies failed SSH authentication attempts
- Aggregates attempts by username and source IP
- Highlights repeated failures consistent with brute-force behavior

Screenshot:
- `screenshots/Failed_SSH_Detection.png`

---

## 🚨 Alerting

The SSH brute-force detection search can be converted into a Splunk alert to provide near real-time visibility into suspicious authentication activity.

Example alert configuration:
- **Alert Type:** Scheduled
- **Trigger Condition:** Number of results > 3
- **Time Window:** Last 5 minutes
- **Severity:** Medium

This alert enables proactive identification of credential-based attacks and mirrors common SOC alerting workflows.

---

## 🧠 Skills Demonstrated

- Splunk Enterprise SIEM deployment and configuration
- Linux system administration (Ubuntu Server 24.04 LTS)
- Log ingestion and normalization
- SSH authentication monitoring and analysis
- SPL (Search Processing Language)
- Brute-force attack detection techniques
- VirtualBox networking (NAT with port forwarding)
- Security troubleshooting and validation
- SOC-style detection testing

---

## 📌 Key Takeaways

- Modern Linux distributions may require additional configuration to expose authentication logs
- Effective SIEM detection depends on verified and reliable log ingestion
- Overly restrictive searches can hide legitimate security events
- Simulating attacks is critical for validating detection logic
- Splunk can function effectively as a host-based SIEM solution

---

## 🚀 Future Enhancements

- Convert detections into fully automated Splunk alerts
- Build dashboards to visualize SSH brute-force activity
- Ingest Windows Security Event Logs from additional endpoints
- Deploy Splunk Universal Forwarder on multiple systems
- Correlate SSH events with firewall logs
- Map detections to **MITRE ATT&CK (T1110 – Brute Force)**

---
