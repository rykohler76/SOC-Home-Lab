# PowerShell Network Activity Investigation

## Evidence

Today I simulated a web request to example.com through PowerShell and used Splunk to investigate what events were created from it. While investigating the web request, I noticed that the event order was different from what I originally expected. First, the PowerShell process was created, which showed up as Event ID 1. Then a temporary .ps1 file was created with Event ID 11, followed by a network connection to the destination IP with Event ID 3. Finally, Event ID 22 showed a DNS query for example.com. I was able to connect all four events because they shared the same ProcessGuid, showing that they were all associated with the same PowerShell process.

### Technical Evidence

ProcessGuid:
{d6875eff-a44c-6abe-0d08-000000001200}

Observed Sysmon Events:

- Event ID 1 - Process Creation
- Event ID 11 - File Creation
- Event ID 3 - Network Connection
- Event ID 22 - DNS Query

Network Connection:
Destination IP: 104.20.23.154
Destination Port: 443

DNS Query:
QueryName: example.com
QueryStatus: 0

## Analysis

Searching using Splunk can be very easy or very difficult, it really comes down to how you search. While I could have searched for powershell.exe to find the process I created, that would have given me way more data than I needed and taken longer to investigate. That's where the ProcessGuid search came in. By searching the specific ProcessGuid, I was able to narrow the results down to the exact PowerShell process I was investigating, making the investigation much easier and simpler.

## Disposition

This activity was created intentionally in my home lab to simulate a web request and investigate how the activity would appear in Splunk. Since I generated the activity myself and the investigation did not show evidence of malicious behavior, I classified it as benign/expected activity.

## Lessons Learned

- The order that events appear in isn't always set in stone, so I should use the actual telemetry instead of assuming what order events should happen in.
- Using specific fields like ProcessGuid can make Splunk searches much more focused and reduce the amount of unrelated data I have to investigate.
- I learned more about what different Sysmon Event IDs mean, especially Event ID 3 for network connections and Event ID 22 for DNS queries.

