# Privilege Escalation Detection

## Objective
Detect privilege escalation activity through sudo execution on a Linux endpoint.

## Environment
- Ubuntu Server
- Splunk Enterprise
- Linux authentication logs

## Attack Simulation
Privilege escalation activity was simulated using sudo commands executed from a standard user account.

## Data Source
- `/var/log/auth.log`
- sourcetype: `Ubuntu-Victima`
- index: `main`

## Detection Logic

### Sudo Activity Detection

```spl
index=main sourcetype=Ubuntu-Victima sudo
```

### Privileged Command Execution

```spl
index=main sourcetype=Ubuntu-Victima sudo
| table _time host user _raw
```

### Frequency Analysis

```spl
index=main sourcetype=Ubuntu-Victima sudo
| stats count by user
```

## Findings
- Successful sudo executions detected
- Privileged commands executed by standard users
- Activity consistent with privilege escalation behavior

## MITRE ATT&CK
- T1548 — Abuse Elevation Control Mechanism

## Impact
Potential unauthorized privileged access and system manipulation.

## Classification
True Positive

## Recommendations
- Restrict sudo permissions
- Implement least privilege access
- Monitor privileged command execution
- Enable audit logging
