# Wazuh-Lab-01-Windows-Agent-Enrollment

## Overview

This lab demonstrates the deployment and enrollment of a Windows endpoint into a Wazuh SIEM environment.

The objective was to install the Wazuh Agent on a Windows host, establish communication with the Wazuh Manager running on Rocky Linux, and verify successful enrollment through the Wazuh Dashboard.

---

## Lab Environment

| Component | Details |
|------------|------------|
| Wazuh Manager | Rocky Linux 9 |
| Wazuh Dashboard | Wazuh Dashboard |
| Agent | Windows 11 Host |
| Virtualization | VMware Workstation |
| Network | NAT (VMnet8) |

---

## Objectives

- Install Wazuh Agent on Windows
- Configure Manager communication
- Verify agent connectivity
- Validate agent enrollment
- Confirm endpoint visibility in Wazuh Dashboard

---

## Architecture

```text
Windows Host (Agent)
        |
        | TCP 1514
        |
Rocky Linux VM
(Wazuh Manager + Dashboard)
```

---

## Deployment Steps

### 1. Verify Manager Status

```bash
sudo systemctl status wazuh-manager
```

### 2. Verify Manager Listening Port

```bash
sudo ss -tulpn | grep 1514
```

Expected:

```text
LISTEN 0.0.0.0:1514
```

### 3. Verify Connectivity From Windows

```powershell
Test-NetConnection 192.168.203.133 -Port 1514
```

Expected:

```text
TcpTestSucceeded : True
```

### 4. Install Wazuh Agent

Download and install the latest Windows Wazuh Agent.

### 5. Configure Agent

Edit:

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Update:

```xml
<server>
  <address>192.168.203.133</address>
  <port>1514</port>
  <protocol>tcp</protocol>
</server>
```

### 6. Restart Agent

```text
Services → Wazuh Agent → Restart
```

### 7. Verify Enrollment

Navigate to:

```text
Wazuh Dashboard → Agents
```

Confirm the Windows host appears as:

```text
Status: Active
```

---

## Results

✅ Wazuh Agent installed successfully

✅ Agent communication established

✅ TCP 1514 connectivity verified

✅ Endpoint enrolled successfully

✅ Agent visible in Wazuh Dashboard

---

## Screenshots

| Screenshot | Description |
|------------|------------|
| dashboard-overview.png | Wazuh Dashboard |
| active-agent.png | Active Windows Agent |
| connectivity-test.png | Port 1514 Connectivity Test |
| agent-details.png | Agent Details |

---

## Skills Demonstrated

- Wazuh Administration
- Endpoint Onboarding
- SIEM Operations
- Network Troubleshooting
- Security Monitoring

---

## Key Takeaway

Endpoint onboarding is one of the most fundamental SOC activities. Proper agent deployment ensures endpoint telemetry is collected and available for monitoring, threat hunting, and incident response.
