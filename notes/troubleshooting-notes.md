# Troubleshooting Notes

## Issue 1 - Wazuh Dashboard Unreachable

### Symptom

Unable to access:

```text
https://192.168.203.133
```

### Verification

```bash
sudo systemctl status wazuh-dashboard
```

### Resolution

Confirmed dashboard service was running and verified VM networking.

---

## Issue 2 - No IP Address Assigned

### Symptom

```bash
hostname -I
```

returned no IP address.

### Verification

```bash
ip addr show ens160
```

Result:

```text
NO-CARRIER
```

### Resolution

Reviewed VMware virtual networking configuration and restored adapter connectivity.

---

## Issue 3 - Agent Cannot Reach Manager

### Symptom

```powershell
Test-NetConnection 192.168.203.133 -Port 1514
```

Result:

```text
TcpTestSucceeded : False
```

### Verification

Manager listening on TCP 1514.

```bash
sudo ss -tulpn | grep 1514
```

### Resolution

Temporarily stopped firewall.

```bash
sudo systemctl stop firewalld
```

Retested:

```text
TcpTestSucceeded : True
```

---

## Issue 4 - Incorrect Agent Configuration

### Symptom

Agent did not appear in Wazuh Dashboard.

### Cause

Incorrect manager address in ossec.conf.

### Resolution

Updated:

```xml
<server>
  <address>192.168.203.133</address>
</server>
```

Restarted Wazuh Agent service.

---

## Issue 5 - PowerShell Command Not Found

### Symptom

Commands such as:

```powershell
ipconfig
msiexec
```

returned command not found errors.

### Resolution

Use:

```powershell
.\ipconfig.exe

.\msiexec.exe
```

or use Command Prompt.

---

## Verification Checklist

- [x] Wazuh Manager Running
- [x] Dashboard Running
- [x] Port 1514 Listening
- [x] Connectivity Verified
- [x] Agent Installed
- [x] Agent Configured
- [x] Agent Service Running
- [x] Agent Enrolled
- [x] Agent Active

---

## Lessons Learned

- Verify connectivity before troubleshooting enrollment.
- Check manager listening ports.
- Validate firewall rules early.
- Confirm correct manager IP configuration.
- Verify agent status after configuration changes.
