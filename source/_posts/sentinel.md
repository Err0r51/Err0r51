---
title: Fine-tuning Sysmon detections with Atomic Red Team and Microsoft Sentinel
date: 2025-10-19 14:46:25
tags:
---

Defending Windows systems effectively starts with high-fidelity telemetry — and few tools are as proven for this as Sysmon. I wanted to to revisit our Sentinel Alert rules and identify blind spots in our Windows coverage. In order to achieve this I utilized the [Atomic Red library](https://github.com/redcanaryco/atomic-red-team), which offers a quick way to simulate different attacks patterns (including clean-up). The goal of this article is to walk you through connecting Sysmon to Microsoft Sentinel, testing detections with Atomic Red Team, and building practical KQL analytics mapped to the MITRE ATT&CK framework.

---

## 1. Why Sysmon and Sentinel belong together

Sysmon has long been the go-to collector for Windows endpoint telemetry. It provides visibility into process creation, file writes, network connections, and registry changes — all crucial for detection engineering.

For most environments, the [SwiftOnSecurity Sysmon configuration](https://github.com/SwiftOnSecurity/sysmon-config) or one of its community extensions provides an excellent baseline. It balances visibility and noise and is updated frequently to match modern attack techniques.

Once Sysmon is deployed, the next step is getting its telemetry into Microsoft Sentinel. The easiest and most reliable way (if your machines run in Azure) is the Microsoft Sentinel Data Connector for Windows via the Azure Monitor Agent (AMA). Otherwise you would need to utilize Azure Arc, which is outside the scope of this article. 

---

## 2. Connecting Sysmon to Sentinel via the Data Connector

Microsoft Sentinel provides a native Windows Security Events and Sysmon Data Connector that handles ingestion through the Azure Monitor Agent.

**High-level steps:**

1. In the Sentinel portal, open your workspace → *Content Management* → *Data Connectors* → search for “Windows Security Events via AMA.”
2. Under the Sysmon section (Microsoft-Windows-Sysmon/Operational), enable collection for the desired machines or resource groups.
3. When creating the Data Collection Rule for the connector, select custom and use the following event log location *Microsoft-Windows-Sysmon/Operational!** 
3. Use the portal flow to deploy the AMA agent and bind the connector — ensuring that dependencies and ASIM schemas are correctly applied.

💡 Tip: Avoid pre-installing AMA manually with ad-hoc CLI commands. Let the Sentinel connector handle agent deployment to prevent ingestion or schema mapping issues.

For on-prem systems, onboard them with Azure Arc first, then use the same connector flow to connect them to Sentinel.

Further reading about sysmon onboarding on [Jeffrey Appel's excellent blog](https://jeffreyappel.nl/deploy-sysmon-and-collect-data-with-sentinel-and-the-ama-agent/) 

---

## 3. Confirming Sysmon ingestion

After connecting, generate a few benign process events (open Notepad or run `certutil /?`) and check that data is flowing by running this query in Sentinel Logs:

```kql
WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational"
| project TimeGenerated, Computer, EventID, EventData
| sort by TimeGenerated desc
```

If events appear, ingestion is active and ready for validation.

---

## 4. Validating detections with Atomic Red Team

With Sysmon telemetry in Sentinel, the next step is to test and validate your detections. Atomic Red Team is a modular framework that safely simulates specific attack techniques mapped to MITRE ATT&CK.

Here’s how to set it up in a lab environment:

```powershell
Install-Module Invoke-AtomicRedTeam,powershell-yaml -Scope CurrentUser -Force
Import-Module Invoke-AtomicRedTeam
Invoke-Expression (Invoke-WebRequest https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1).Content
Install-AtomicRedTeam -GetAtomics -InstallPath 'C:\AtomicRedTeam' -NoPayloads -Force
$Env:PathToAtomicsFolder = 'C:\AtomicRedTeam\atomics'
```

Run a concrete test — PowerShell encoded command execution (ATT&CK Technique T1059.001):

```powershell
Invoke-AtomicTest T1059.001 -TestNumbers 1 -PathToAtomicsFolder 'C:\AtomicRedTeam\atomics' -Confirm:$false
```

---

## 5. Creating analytic rules with MITRE mapping

Now create an analytic rule in Sentinel that detects suspicious PowerShell execution. The query below targets encoded PowerShell command usage and maps to T1059.001 (PowerShell / Execution):

```kql
WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational" and EventID == 1
| where EventData has "powershell.exe" and (EventData has "-EncodedCommand" or EventData has "-e ")
| project TimeGenerated, Computer, EventData
| sort by TimeGenerated desc
```

When saving the analytic rule in Sentinel, add the following ATT&CK metadata:

* attack.tactic: Execution
* attack.technique: T1059
* attack.subtechnique: T1059.001

This ensures your alerts map to MITRE ATT&CK, improving visibility in coverage dashboards.

---

## 6. Practical KQL examples for Sysmon detections

Detect Certutil encode/decode usage:

```kql
WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational" and EventID == 1
| where EventData has "certutil" and (EventData has "encode" or EventData has "decode")
| project TimeGenerated, Computer, EventData
| sort by TimeGenerated desc
```

Detect executable written to Temp (possible staged payload):

```kql
WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational" and EventID == 11
| where tolower(EventData) has "\\temp\\" and tolower(EventData) has ".exe"
| project TimeGenerated, Computer, EventData
| sort by TimeGenerated desc
```

Correlate certutil and Temp .exe creation within 5 minutes:

```kql
let cert = WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational" and EventID == 1
| where EventData has "certutil" and (EventData has "encode" or EventData has "decode")
| project CertTime=TimeGenerated, Computer;
let exe = WindowsEvent
| where EventLog == "Microsoft-Windows-Sysmon/Operational" and EventID == 11
| where tolower(EventData) has "\\temp\\" and tolower(EventData) has ".exe"
| project FileTime=TimeGenerated, Computer;
cert
| join kind=inner exe on Computer
| where FileTime between (CertTime .. CertTime + 5m)
| project Computer, CertTime, FileTime
```

---

## 7. Good practices for analytic rules

* Add MITRE ATT&CK identifiers to each rule.
* Start with simple detections and build correlations gradually.
* Document rule purpose and known benign triggers.
* Validate with Atomic Red Team after every rule change.

---

## 8. Next steps and references

At this point you have:

1. Sysmon collecting telemetry with a proven config (SwiftOnSecurity).
2. Microsoft Sentinel ingesting and normalizing events via AMA and ASIM.
3. Atomic Red Team safely simulating ATT&CK techniques.
4. KQL rules mapped to MITRE ATT&CK for visibility and reporting.

Continue by automating validation (e.g., run Atomic tests on a schedule and verify alerts) and refining detections based on results.
