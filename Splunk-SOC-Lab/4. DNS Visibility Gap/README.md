# DNS Visibility Gap Analysis

## Objective
Evaluate DNS visibility limitations in Linux system logs and identify monitoring gaps affecting threat detection.

## Environment
- Ubuntu Server
- Splunk Enterprise
- systemd-resolved

## Scenario
An attempt was made to identify suspicious DNS activity from Linux system logs.

## Data Source
- `/var/log/syslog`
- sourcetype: `syslog`
- index: `main`

## Detection Attempt

```spl
index=main sourcetype=syslog "query"
```

## Findings
- No useful DNS telemetry was available
- Observed events corresponded to kernel and ACPI activity
- DNS visibility was insufficient for security monitoring

## Analysis
The default Linux DNS resolver configuration did not provide enough logging visibility for threat detection purposes.

## Security Impact
Lack of DNS telemetry can prevent detection of:
- Malware communication
- DGA activity
- DNS tunneling
- External malicious domains

## Classification
Visibility Gap / Monitoring Limitation

## Recommendations
- Implement DNS logging solutions
- Integrate DNS telemetry into Splunk
- Deploy dnsmasq or BIND logging
- Incorporate network security monitoring
