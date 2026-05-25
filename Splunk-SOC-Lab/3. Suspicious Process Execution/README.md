# Suspicious Process Execution Detection

## Objective
Detect suspicious command execution on Linux endpoints using auditd telemetry integrated into Splunk.

## Environment
- Ubuntu Server
- Splunk Enterprise
- auditd
- Splunk Universal Forwarder

## Attack Simulation
Suspicious commands and network tools were executed to simulate post-exploitation behavior.

## Data Source
- `/var/log/audit/audit.log`
- sourcetype: `linux_audit`
- index: `main`

## auditd Configuration

```bash
sudo auditctl -a always,exit -F arch=b64 -S execve
```

## Detection Logic

### Process Execution Events

```spl
index=main sourcetype=linux_audit type=EXECVE
```

### Suspicious Tool Execution

```spl
index=main sourcetype=linux_audit type=EXECVE ("curl" OR "wget")
```

### Full Command Arguments

```spl
index=main sourcetype=linux_audit type=EXECVE
| table _time host a0 a1 a2 a3
```

## Findings
- Command execution telemetry successfully collected
- Suspicious network tools detected
- Full command arguments visible in Splunk
- Visibility into post-exploitation activity

## MITRE ATT&CK
- T1059 — Command and Scripting Interpreter
- T1105 — Ingress Tool Transfer

## Impact
Potential malware download or external command-and-control communication.

## Classification
True Positive

## Recommendations
- Restrict unnecessary binaries
- Monitor process execution activity
- Implement application allowlisting
- Correlate process execution with network telemetry
