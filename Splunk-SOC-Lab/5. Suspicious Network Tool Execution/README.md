# Suspicious Network Tool Execution

## Objective
Detect execution of network tools associated with remote downloads and possible command-and-control activity.

## Environment
- Ubuntu Server
- auditd
- Splunk Enterprise
- Splunk Universal Forwarder

## Attack Simulation
Commands such as curl and wget were executed against external domains to simulate suspicious outbound activity.

## Data Source
- `/var/log/audit/audit.log`
- sourcetype: `linux_audit`
- index: `main`

## Detection Logic

### Detect curl and wget

```spl
index=main sourcetype=linux_audit type=EXECVE ("curl" OR "wget")
```

### Command Argument Visibility

```spl
index=main sourcetype=linux_audit type=EXECVE
| table _time host a0 a1 a2 a3
```

### Basic Correlation

```spl
index=main (sourcetype=Ubuntu-Victima OR sourcetype=linux_audit)
("Accepted password" OR sudo OR curl OR wget)
| table _time host sourcetype _raw
```

## Findings
- Execution of curl and wget detected
- External communication attempts observed
- Full process arguments available for analysis
- Activity consistent with post-exploitation behavior

## MITRE ATT&CK
- T1105 — Ingress Tool Transfer
- T1059 — Command and Scripting Interpreter

## Impact
Potential:
- Malware download
- C2 communication
- Data exfiltration

## Classification
True Positive (controlled laboratory activity)

## Recommendations
- Restrict curl and wget usage
- Implement binary allowlisting
- Monitor outbound connections
- Correlate endpoint and network telemetry
