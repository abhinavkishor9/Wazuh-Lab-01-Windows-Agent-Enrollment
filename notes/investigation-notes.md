# Investigation Notes

## Objective

Enroll a Windows endpoint into a Wazuh environment and validate communication with the Wazuh Manager.

---

## Validation Process

### Step 1 - Manager Verification

Verified Wazuh Manager service:

```bash
sudo systemctl status wazuh-manager
```

Result:

```text
active (running)
```

---

### Step 2 - Port Verification

Verified manager listening on TCP 1514:

```bash
sudo ss -tulpn | grep 1514
```

Result:

```text
LISTEN 0.0.0.0:1514
```

---

### Step 3 - Connectivity Testing

From Windows:

```powershell
Test-NetConnection 192.168.203.133 -Port 1514
```

Initial Result:

```text
TcpTestSucceeded : False
```

---

### Step 4 - Firewall Investigation

Observed successful ping but failed TCP connectivity.

Hypothesis:

```text
Host firewall blocking TCP 1514.
```

Stopped firewalld for testing.

```bash
sudo systemctl stop firewalld
```

Retested connectivity.

Result:

```text
TcpTestSucceeded : True
```

---

### Step 5 - Agent Configuration

Updated manager IP in:

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Configured:

```xml
<address>192.168.203.133</address>
```

---

### Step 6 - Agent Enrollment

Restarted Wazuh Agent service.

Verified endpoint appeared in:

```text
Wazuh Dashboard → Agents
```

Status:

```text
Active
```

---

## Findings

- Wazuh Manager was functioning correctly.
- TCP 1514 was listening.
- Firewall prevented agent communication.
- Agent configuration required correct manager IP.
- Enrollment succeeded after connectivity was restored.

---

## Conclusion

The Windows endpoint successfully enrolled and established communication with the Wazuh Manager, enabling security telemetry collection and monitoring.
