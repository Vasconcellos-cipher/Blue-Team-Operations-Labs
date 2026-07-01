# Incident Report: Malicious File/Script Download Attempt (SOC137)

## Executive Summary
* **Incident Date/Time:** Mar 14, 2021, 07:15 PM
* **Event ID:** 76
* **Rule Triggered:** SOC137 - Malicious File/Script Download Attempt
* **Severity:** Medium
* **Target Host:** HOST-PRD-037 (Internal Segment)
* **Artifact Name:** INVOICE_PACKAGE_DOWNLOAD_LINK.docm
* **MD5 Hash:** F2d0C66B801244C059F636D08A474079
* **Classification:** True Positive

---

## 1. MITRE ATT&CK Mapping
This incident leverages specific tactics and techniques aimed at endpoint execution rather than exploiting web-facing vulnerabilities:

* **Tactic: Phishing ([TA0001](https://attack.mitre.org/tactics/TA0001/))** -> **Technique: Spearphishing Attachment ([T1566.001](https://attack.mitre.org/techniques/T1566/001/)):** The adversary delivered the malicious payload as a weaponized email attachment disguised as a financial invoice.
* **Tactic: Execution ([TA0002](https://attack.mitre.org/tactics/TA0002/))** -> **Technique: User Execution ([T1204.002](https://attack.mitre.org/techniques/T1204/002/)):** The attack relies on the victim opening the malicious `.docm` file and enabling macros.
* **Tactic: Execution ([TA0002](https://attack.mitre.org/tactics/TA0002/))** -> **Technique: Command and Scripting Interpreter: PowerShell ([T1059.001](https://attack.mitre.org/techniques/T1059/001/)):** The embedded `"AutoOpen"` macro was engineered to invoke PowerShell to execute hidden background download strings.

---

## 2. Threat Intelligence Analysis
A reverse lookup of the file hash was conducted using reputable third-party sandbox engines to evaluate the core capabilities of the payload:

* **VirusTotal Analytics:** 39/65 security vendors flagged the hash as malicious. Static analysis insights confirmed the embedded macro code designed to spawn a hidden PowerShell download execution.
* **Hybrid Analysis:** The file scored a Threat Level of 100/100. Behavioral analysis confirmed active indicators for `#macros-on-open` and generic Trojan downloaders/droppers.

---

## 3. Playbook Execution & Investigation Steps

### Endpoint Mitigation Status
The perimeter security controls and endpoint detection mechanisms registered the device action as **Blocked**. The system successfully intercepted and neutralized the weaponized `.docm` file upon delivery. As a result, the malicious macro structure was completely prevented from executing the secondary PowerShell payload on the targeted workstation.

### Network Traffic & C2 Verification
A comprehensive audit of the central **Log Management** console was conducted for the affected subnet within the alert's timeframe. No anomalous outbound traffic, suspicious DNS queries, or established connections to external Command and Control (C2) infrastructures were identified. This confirms the threat was fully contained at the pre-execution delivery phase.

---

## 4. Post-Incident Review & Analyst Notes
Although the automated platform playbook expects a rigid classification of the containment status, industry-standard incident response workflows dictate that any corporate asset encountering sophisticated macro droppers should undergo an isolated credential verification check and a full, offline endpoint scan. 

The incident has been verified as a **True Positive** with zero lateral movement or internal systems compromise. The MD5 hash has been successfully integrated into the enterprise centralized blocklist repository for preventative defense-in-depth hardening.
