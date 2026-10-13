# SOC Home Lab

## Project Overview

The main reason I built this SOC home lab was to expand my cybersecurity knowledge beyond earning certifications. While certifications are valuable, I believe hands-on experience is just as important when preparing to enter the cybersecurity field.

Through this lab, I expanded my understanding of cybersecurity, specifically the responsibilities of a SOC analyst. I found this experience extremely helpful because it not only improved my understanding of security concepts but also gave me the opportunity to put those concepts into practice.

My goal was to demonstrate that I can apply what I've learned by working with security tools, investigating events, and analyzing real telemetry generated in my own lab environment.

## Lab Environment

I used VirtualBox to create a virtual environment where I could safely experiment with cybersecurity tools without directly affecting my main operating system.

Inside VirtualBox, I installed Windows 11 and used tools like PowerShell to generate activity that I could investigate.

I used Sysmon to collect detailed information about what was happening inside Windows, including process creation, file creation, and network connections.

Finally, I used Splunk to search and analyze the collected events, helping me understand what the activity meant and how a SOC analyst would investigate it.

## Skills Demonstrated

One of the most important skills I developed was understanding what different Sysmon Event IDs mean and how they relate to system activity.

I also learned how to use Splunk to filter large amounts of event data and narrow my searches to the information relevant to an investigation. This helped me investigate activity more efficiently and understand how different events could be connected.

## Projects and Investigations

### Detection Rules

- [PowerShell System Discovery Detection](detections/powershell-system-discovery.txt) — Identifies PowerShell process creation involving system discovery commands.
- [Encoded PowerShell Detection](detections/powershell-encoded-command.txt) — Identifies PowerShell processes launched with encoded command parameters.

### Investigations

- [Investigation 001: Encoded PowerShell](investigations/001-encoded-powershell-investigation.md) — Investigated encoded PowerShell execution and correlated process creation and file creation events.
- [Investigation 002: PowerShell Network Activity](investigations/002-powershell-network-investigation.md) — Investigated PowerShell network activity using Sysmon process, file, network, and DNS events.

## Investigation Screenshots

### PowerShell System Discovery
- [Detection Query](screenshots/01-powershell-detection-query.png)
- [Detection Event](screenshots/02-powershell-detection-event.png)

### Encoded PowerShell
- [Detection Query](screenshots/03-encoded-powershell-query.png)
- [Detection Event](screenshots/04-encoded-powershell-event.png)

### PowerShell Network Investigation
- [Event Timeline](screenshots/05-powershell-network-timeline.png)
- [Network Connection Evidence](screenshots/06-powershell-network-event.png)

