# SOC Analyst Capstone — Full Incident Investigation

## Overview

This repository contains my SOC Analyst internship capstone investigation into a phishing-delivered malware execution chain.

The investigation covers the complete incident lifecycle from initial detection through endpoint investigation, timeline reconstruction, indicator identification, MITRE ATT&CK mapping, threat-intelligence validation, and recommended remediation actions.

> **Training/Lab Environment:** This investigation was conducted using a controlled cybersecurity training environment. The indicators documented in this repository belong to the lab scenario and should not be interpreted as evidence from a real-world client incident.

---

## Incident Summary

The investigation began with a suspicious email containing the attachment `perspiciatism.zip`.

Endpoint telemetry showed the following execution chain:

```text
Phishing Email
      ↓
perspiciatism.zip
      ↓
PERSPICIATISM.iso
      ↓
Open_Document.exe
      ↓
cmd.exe
      ↓
data\document.rtf
      ↓
curl.exe
      ↓
3291.png
      ↓
rundll32.exe
```
The investigation established evidence of:
- Phishing-delivered malware
- Malicious file execution
- Windows command-shell activity
- External payload transfer
- Suspicious use of rundll32.exe
- Malicious/phishing infrastructure identified through threat intelligence

Investigation Timeline

Time	    Event
08:11:36	Suspicious email received containing perspiciatism.zip.
09:16:08	Chrome created perspiciatism.zip in the Downloads directory.
09:16:29	7-Zip created PERSPICIATISM.iso.
09:16:43	Open_Document.exe and edputil.dll were created.
09:16:53	Open_Document.exe was executed.
09:16:54	cmd.exe was spawned to execute data\document.rtf.
09:16:55	curl.exe retrieved 3291.png from an external URL.
09:17:10	rundll32.exe executed 3291.png using GetModuleProp.

## MITRE ATT&CK Mapping

- **T1566.001 — Phishing: Spearphishing Attachment**
  - Evidence: Suspicious email containing `perspiciatism.zip`.

- **T1204.002 — User Execution: Malicious File**
  - Evidence: Execution of `Open_Document.exe`.

- **T1059.003 — Command and Scripting Interpreter: Windows Command Shell**
  - Evidence: `cmd.exe` used during the execution chain.

- **T1105 — Ingress Tool Transfer**
  - Evidence: `curl.exe` retrieved `3291.png`.

- **T1218.011 — System Binary Proxy Execution: Rundll32**
  - Evidence: `rundll32.exe` executed `3291.png`.

## Key Indicators of Compromise

- **Email Sender:** `support[@]mail[.]westcapitalreserve[.]com`
- **Sender IP:** `192.227.130.26`
- **Attachment:** `perspiciatism.zip`
- **Executable:** `Open_Document.exe`
- **DLL:** `edputil.dll`
- **Downloaded File:** `C:\wnd\3291.png`
- **URL:** `hxxps://yourunitedlaws[.]com/mrD/4462`
- **Domain:** `yourunitedlaws[.]com`
- **IP:** `50.3.132.236`
- **Affected Host:** `172.16.17.122`

Threat Intelligence

The identified payload delivery URL was investigated using VirusTotal.
At the time of investigation, 12 of 92 security vendors flagged the URL, with classifications including phishing, malware, malicious activity, and suspicious activity.
The payload hash did not have an available public VirusTotal analysis during the investigation, so no malware-family attribution was made solely from the hash.

Recommendations

The investigation resulted in the following recommended actions:
- Isolate the affected endpoint.
- Remove or quarantine identified malicious artifacts.
- Block identified malicious network indicators.
- Search the environment for related files, hashes, domains, and URLs.
- Review authentication and lateral-movement activity.
- Reset credentials if additional investigation establishes credential exposure.
- Strengthen email filtering for suspicious archive and executable attachments.
- Improve detections for suspicious chains involving Office applications, cmd.exe, curl.exe, and rundll32.exe.
- Reinforce phishing awareness and reporting procedures.

Skills Demonstrated

This investigation demonstrates practical experience with:
- Security alert investigation
- Endpoint log analysis
- Process-tree analysis
- Incident timeline reconstruction
- IOC identification
- Threat intelligence
- VirusTotal analysis
- MITRE ATT&CK mapping
- Phishing analysis
- Malware investigation
- Incident response recommendations
- SOC documentation and reporting

Full Investigation Report

The complete capstone investigation report is included in this repository as a PDF.
Environment: LetsDefend training environment
Investigation Type: SOC / Blue Team Incident Investigation
Scenario: Phishing-delivered malware
