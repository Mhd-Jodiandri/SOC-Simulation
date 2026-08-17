# Scenario 5 — LSASS Credential Access

## Overview

This scenario evaluates the detection of suspicious access to the Windows **Local Security Authority Subsystem Service (LSASS)** process.

The test was mapped to **MITRE ATT&CK T1003.001 — OS Credential Dumping: LSASS Memory** and performed using `LsassAccessTester.exe` on the Windows 10 endpoint.

The primary objective was to verify whether:

1. `LsassAccessTester.exe` could access `lsass.exe`.
2. Sysmon would record the activity as **Event ID 10 (Process Access)**.
3. Wazuh would generate a corresponding security alert.

The test confirmed the expected endpoint telemetry. However, Wazuh did not generate an alert for the specific activity, including during a subsequent retest. The result is therefore documented as a **Detection Gap**.

---

## Lab Environment

| Component          | Configuration                       |
| ------------------ | ----------------------------------- |
| Attacker           | Kali Linux                          |
| Target             | Windows 10                          |
| SIEM / Monitoring  | Ubuntu Server + Wazuh               |
| Endpoint Telemetry | Sysmon                              |
| Test Application   | `LsassAccessTester.exe`             |
| Relevant Event     | Sysmon Event ID 10 — Process Access |
| MITRE ATT&CK       | T1003.001 — LSASS Memory            |
| Network            | VirtualBox Host-Only                |

All testing was conducted within the isolated SOC simulation lab.

---

## Objective

The expected detection path was:

```text
LsassAccessTester.exe
        |
        v
   Access lsass.exe
        |
        v
Sysmon Event ID 10
        |
        v
   Wazuh Agent
        |
        v
 Wazuh Detection
        |
        v
      Alert
```

The test was considered successful from an endpoint-visibility perspective if Sysmon captured the source process, target process, and access information.

---

## Test Execution

`LsassAccessTester.exe` was executed on the Windows 10 endpoint.

The Sysmon Operational log was then queried for Event ID 10 within the test time window and filtered for events containing `LsassAccessTester.exe`:

```powershell
$event = Get-WinEvent -FilterHashtable @{
    LogName = "Microsoft-Windows-Sysmon/Operational"
    Id = 10
    StartTime = [datetime]"2026-08-17 22:10:40"
    EndTime   = [datetime]"2026-08-17 22:10:50"
} | Where-Object {
    $_.Message -match "LsassAccessTester.exe"
} | Select-Object -First 1
```

The event was exported in XML format using:

```powershell
$event.ToXml()
```

The query returned a matching Sysmon Event ID 10.

---

## Sysmon Evidence

The captured event contains the following metadata:

```text
Event ID       : 10
Event RecordID : 25129
Computer       : WIN10-SOC
UTC Time       : 2026-08-17 15:10:43.393
```

The corresponding local time was:

```text
2026-08-17 22:10:43.393 WIB
```

### Source Process

```text
SourceProcessId : 6740

SourceImage :
C:\Users\Joyz\AppData\Local\Temp\dotnet\runfile\
LsassAccessTester-1dd42b1db0a7a9c0927fa282254825e54001520
f3dcd6617701dac6b2d58809d\bin\debug\LsassAccessTester.exe
```

### Target Process

```text
TargetProcessId : 680

TargetImage :
C:\Windows\system32\lsass.exe
```

### Access Information

```text
GrantedAccess : 0x1000
```

### User Context

```text
SourceUser : WIN10-SOC\Joyz
TargetUser : NT AUTHORITY\SYSTEM
```

The key fields from the event are:

```text
Event ID      : 10
SourceImage   : LsassAccessTester.exe
TargetImage   : C:\Windows\system32\lsass.exe
GrantedAccess : 0x1000
SourceUser    : WIN10-SOC\Joyz
TargetUser    : NT AUTHORITY\SYSTEM
```

This provides direct evidence that `LsassAccessTester.exe` accessed the LSASS process.

---

## Call Trace

The event also contains a call trace associated with the process access:

```text
ntdll.dll
KERNELBASE.dll
coreclr.dll
hostpolicy.dll
hostfxr.dll
LsassAccessTester.exe
```

The presence of `LsassAccessTester.exe` in both the `SourceImage` and the call trace provides additional context for attributing the process-access event to the test application.

---

## Analysis

The captured Sysmon event confirms the following:

- `LsassAccessTester.exe` executed on the Windows endpoint.
- The process accessed `lsass.exe`.
- Sysmon recorded the activity as Event ID 10.
- The source and target processes were identified.
- The requested access was recorded as `0x1000`.
- The source and target security contexts were recorded.
- A call trace associated with the .NET runtime and test executable was captured.

This is sufficient to establish **LSASS process access**.

However, the event alone does not demonstrate that credential material was successfully extracted from LSASS.

For that reason, the evidence is described as **LSASS access telemetry**, rather than definitive proof of successful credential dumping.

---

## Wazuh Detection

The corresponding activity was investigated in Wazuh after the Sysmon event was confirmed on the endpoint.

No Wazuh alert corresponding to the `LsassAccessTester.exe` activity was identified.

A second test was performed to determine whether the result was reproducible.

The retest produced the same outcome:

```text
Sysmon Event ID 10
        |
        |  Event generated
        v
LsassAccessTester.exe -> lsass.exe
        |
        |  No corresponding alert
        v
      Wazuh
```

Therefore, the missing alert was treated as a repeatable detection issue rather than a one-time observation.

---

## Detection Result

| Detection Stage                   | Result          |
| --------------------------------- | --------------- |
| `LsassAccessTester.exe` execution | ✅ Confirmed    |
| Access to `lsass.exe`             | ✅ Confirmed    |
| Sysmon Event ID 10                | ✅ Confirmed    |
| Source process identification     | ✅ Confirmed    |
| Raw XML evidence                  | ✅ Available    |
| Wazuh alert for the activity      | ❌ Not detected |
| Retest                            | ❌ No alert     |
| Detection Gap                     | ⚠️ Confirmed    |

### Final Detection Status

```text
Attack Simulation  : COMPLETED
Endpoint Telemetry : CONFIRMED
Wazuh Detection    : NOT DETECTED
Detection Gap      : CONFIRMED
```

---

## Detection Gap

The main issue identified during this scenario was the gap between **endpoint telemetry** and **SIEM detection**.

Sysmon successfully generated an Event ID 10 containing the required information:

```text
Source : LsassAccessTester.exe
Target : lsass.exe
Access : 0x1000
```

However, the activity did not result in a corresponding Wazuh alert.

The same result was observed during the retest.

The scenario was therefore not extended into further Wazuh troubleshooting. Instead, the result was recorded as a detection gap based on the evidence collected during the testing process.

This distinction is important:

```text
Telemetry exists
        ≠
SIEM alert exists
```

In this case, endpoint visibility was available, but the specific activity was not surfaced as a Wazuh alert.

---

## SOC Analyst Perspective

An Event ID 10 involving `lsass.exe` should not automatically be classified as credential dumping.

Legitimate applications and Windows components may access LSASS during normal operation. The source process, access rights, user context, process ancestry, and surrounding activity should therefore be reviewed before assigning malicious intent.

For this investigation, the following fields are particularly relevant:

| Field           | Value                   |
| --------------- | ----------------------- |
| Source Process  | `LsassAccessTester.exe` |
| Target Process  | `lsass.exe`             |
| Granted Access  | `0x1000`                |
| Source User     | `WIN10-SOC\Joyz`        |
| Target User     | `NT AUTHORITY\SYSTEM`   |
| Event ID        | `10`                    |
| Event Record ID | `25129`                 |
| Host            | `WIN10-SOC`             |

In a production environment, this event would ideally be correlated with process-creation telemetry, parent-process information, executable reputation or hash data, and other events occurring around the same timestamp.

---

## MITRE ATT&CK Mapping

| Attribute     | Mapping               |
| ------------- | --------------------- |
| Tactic        | Credential Access     |
| Technique     | OS Credential Dumping |
| Sub-technique | LSASS Memory          |
| MITRE ID      | **T1003.001**         |

The mapping reflects the objective of the simulation: accessing the LSASS process as part of an LSASS credential-access scenario.

The Sysmon event itself should be treated as supporting telemetry and not as proof of successful credential extraction.

---

## Evidence

The raw Sysmon event was exported directly from the Windows endpoint in XML format.

A recommended repository structure is:

```text
scenario-5/
├── README.md
├── screenshots/
│   ├── code-execution.png
│   ├── sysmon-event-10-lsassaccesstester.png
│   └── wazuh-no-alert.png
└── logs/
    └── sysmon-event-10-lsassaccesstester.xml
```

The XML file should be retained alongside the screenshots because it preserves the original event fields and provides stronger evidence than a screenshot alone.

---

## Lessons Learned

This scenario highlighted an important distinction in the detection pipeline.

Generating useful endpoint telemetry does not necessarily result in a SIEM alert.

The investigation also showed the value of preserving raw event data. Because the Sysmon XML was available, it was possible to establish that:

- the test process executed;
- LSASS was accessed;
- Sysmon recorded the activity; and
- the expected Wazuh alert was still absent after a retest.

This made it possible to identify the issue as a **detection gap** rather than an endpoint-visibility failure.

---

## Final Result

The scenario produced the expected endpoint evidence for LSASS process access.

The most relevant event was Sysmon Event ID 10:

```text
SourceImage  : LsassAccessTester.exe
TargetImage  : C:\Windows\system32\lsass.exe
GrantedAccess: 0x1000
```

However, Wazuh did not generate a corresponding alert during the initial test or the subsequent retest.

The final outcome is therefore:

> **Sysmon successfully captured the simulated LSASS access, while Wazuh failed to surface the specific activity as a security alert. The result is documented as a confirmed Detection Gap.**

No claim is made that credentials were successfully extracted from LSASS, because the available evidence does not establish successful credential extraction.
