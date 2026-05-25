# Brute Force SSH Detection

## Objective
Detect repeated failed SSH authentication attempts against a Linux server using Splunk.

## Environment
- Ubuntu Server
- Splunk Enterprise
- Linux auth logs
- SSH service enabled

## Attack Simulation
Multiple failed SSH login attempts were generated from an attacker machine against the target Linux host to simulate brute force activity.

## Data Source
- `/var/log/auth.log`
- sourcetype: `Ubuntu-Victima`
- index: `main`

## Detection Logic

### Failed SSH Attempts

```spl
index=main sourcetype=Ubuntu-Victima "Failed password"
| stats count by src,user
| sort - count
```

### Authentication Timeline

```spl
index=main sourcetype=Ubuntu-Victima "Failed password"
| timechart count
```

### Top Source IPs

```spl
index=main sourcetype=Ubuntu-Victima "Failed password"
| stats count by src
| sort - count
```

## Findings
- Multiple failed authentication attempts detected
- Repeated login failures from the same IP address
- Pattern consistent with brute force activity

## MITRE ATT&CK
- T1110 — Brute Force

## Impact
Risk of unauthorized access through compromised credentials.

## Classification
True Positive

## Recommendations
- Implement fail2ban
- Restrict SSH access by IP/VPN
- Disable password authentication
- Monitor authentication anomalies

## Screenshots
Add Splunk screenshots here.
