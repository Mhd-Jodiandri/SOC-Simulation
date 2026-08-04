# Triage Notes

## Alert Summary

**Scenario:** PowerShell Malware Execution (Simulation)

**Host:** windows10-lab

**Detection Sources:**
- Sysmon
- Wazuh

## Triggered Rules

- Rule ID 92027
  - Powershell process spawned powershell instance

- Rule ID 92213
  - Executable file dropped in folder commonly used by malware

## Classification

True Positive

## MITRE ATT&CK

- T1059.001 - PowerShell

## Impact Assessment

No malicious payload was executed.

The activity was intentionally performed inside an isolated SOC laboratory.

## Recommendation

- Continue monitoring PowerShell executions.
- Review command-line parameters used by PowerShell.
- Investigate similar activity on production endpoints.