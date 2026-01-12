# SIEM-Splunk-Linux-Lab
Detecting failed SSH logins on a Linux Virtual Machine using Splunk SIEM
# 🛡️ Splunk SIEM Lab – Linux SSH Brute Force Detection

This project demonstrates the deployment and configuration of **Splunk Enterprise** as a SIEM solution on an **Ubuntu 24.04 LTS** virtual machine.  
The lab focuses on ingesting Linux authentication logs and detecting **failed SSH login attempts**, simulating real-world SOC monitoring and alerting.

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
- Created an Ubuntu Server 24.04 VM in VirtualBox
- Installed system updates and required dependencies
- Configured SSH service for local authentication testing

📸 *Screenshot:*  
`screenshots/Linux_Server_VM_Success_Creation.png`

---

### 2. Splunk Enterprise Installation
- Downloaded official Splunk Enterprise `.deb` package
- Installed using `dpkg`
- Started and verified Splunk services
- Enabled Splunk to start at boot

📸 *Screenshots:*  
- `screenshots/Splunk_Download_Success.png`  
- `screenshots/Splunkd_Running.png`  
- `screenshots/Splunk_Login_Success.png`

---

## 🔐 Log Ingestion Configuration

### Linux Authentication Logs
Ubuntu 24.04 does not always log SSH authentication events to `/var/log/auth.log` by default.

To ensure reliable ingestion:
- Installed and enabled **rsyslog**
- Restarted SSH service
- Verified authentication logs were being written to `/var/log/auth.log`

```bash
sudo apt install rsyslog -y
sudo systemctl enable rsyslog
sudo systemctl restart ssh
