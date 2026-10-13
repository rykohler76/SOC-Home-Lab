# Encoded PowerShell Investigation

## Alert / Detection

Detection Name: Encoded PowerShell Command Detection

Data Source: Sysmon Event ID 1 - Process Creation

Detection Query:

index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1
Image="*\\powershell.exe"
(CommandLine="*-EncodedCommand*" OR CommandLine="*-enc *")

## Evidence

### Event 1 - Encoded PowerShell Process Creation

Timestamp: 09/22/2026 3:15:47.199 PM

EventCode: 1 - Process Creation

Image:
C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe

Behavior:
PowerShell was launched using the -NoProfile and -EncodedCommand parameters.

Decoded Command:
Write-Output "SOC Home Lab Test"

ProcessGuid:
{d6875eff-fe13-6ab2-9613-000000000e00}

### Event 2 - File Creation

Timestamp: 09/22/2026 3:15:47.251 PM

EventCode: 11 - File Create

Image:
C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe

TargetFilename:
C:\Users\SOCUsers\AppData\Local\Temp\__PSScriptPolicyTest_zps22kcp.dru.ps1

ProcessGuid:
{d6875eff-fe13-6ab2-9613-000000000e00}

### Correlation

The process creation and file creation events occurred approximately 52 milliseconds apart and share the same ProcessGuid, allowing the two Sysmon events to be correlated to the same PowerShell process.

## Analysis

The detection was triggered when powershell.exe was launched with an encoded command. The command was decoded and found to contain `Write-Output "SOC Home Lab Test"`. This command only outputs text and was run for the purpose of the home lab.

Event ID 11 showed that PowerShell created a temporary file. We connected this event to Event ID 1 by matching their ProcessGuids. Both events were associated with the same PowerShell process.

I also searched for additional events using the same ProcessGuid and only found the process creation and file creation events. No additional suspicious activity was identified in the collected Sysmon logs.

## Disposition

**Benign / Expected Activity**

This activity was intentionally generated to explore what an encoded PowerShell command would look like in Splunk. Since this was a user-controlled experiment in a controlled home-lab environment, the decoded command was harmless, and no additional suspicious activity was identified in the collected logs, I classified the activity as benign.

## Lessons Learned

- A detection firing does not automatically mean malicious activity occurred.
- Sysmon Event ID 1 records process creation, while Event ID 11 records file creation.
- ProcessGuid can be used to correlate multiple Sysmon events to the same process.
- Timestamps can identify potentially related activity, but timing alone does not prove that events are connected.
- Encoded PowerShell should be investigated by examining the command contents, parent process, user context, and surrounding activity.
- Raw Sysmon event data can be useful when Splunk's extracted fields are unclear or incomplete.