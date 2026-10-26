# Microsoft Intune Endpoint Management Lab

**Final project report | October 2026**  
**Status: Core objectives completed — project closed**  
**Duration: Over 30 days of practical work**  
**Scope: Nine documented stages (Days 1–9)**

## Overview

Over more than 30 days, I built and documented a hands-on Microsoft Intune endpoint management lab using Microsoft Azure, Windows 11, Microsoft Entra ID, Microsoft Graph PowerShell and Microsoft Intune. The goal was to develop practical skills in cloud identity, endpoint enrollment, security configuration, compliance administration, application assignment and technical troubleshooting.

The project combines practical configuration work with screenshots, administrative verification and technical documentation. Each stage is preserved in the linked day-by-day guides below.

## Project stages

1. **[Day 1 — Azure Environment Setup](01-Environment-Setup/README.md)**
2. **[Day 2 — Networking and Enrollment Troubleshooting](02-Day-2/README.md)**
3. **[Day 3 — Entra ID Users and Groups](03-Day-3/README.md)**
4. **[Day 4 — Security Group Administration](04-Day-4/README.md)**
5. **[Day 5 — Intune Licensing and Preparation](05-Day-5/README.md)**
6. **[Day 6 — Windows 11 Join and Enrollment](06-Day-6/README.md)**
7. **[Day 7 — Security Configuration Policy](07-Day-7/README.md)**
8. **[Day 8 — Device Compliance Policy](Day-08-Compliance-Policy/README.md)**
9. **[Day 9 — Application Management](Day-09-Application-Deployment/README.md)**

## Work completed

- **Azure and Windows 11:** Provisioned a Windows 11 lab virtual machine, configured remote access and examined network and platform security settings.
- **Connectivity and troubleshooting:** Investigated DNS, authentication, networking, registration and device enrollment issues using Windows tools and PowerShell.
- **Microsoft Entra ID:** Created and managed test identities, device records and security groups for structured administration and pilot targeting.
- **Microsoft Graph PowerShell:** Queried and verified users, groups, membership and devices using administrative commands.
- **Intune preparation:** Activated Intune Plan 1 licensing and prepared the pilot tenant and device for management.
- **Windows enrollment:** Successfully joined the Windows 11 endpoint to Microsoft Entra ID and established Intune MDM management, checking device registration with `dsregcmd /status` and admin-centre records.
- **Security configuration:** Created and assigned a Windows inactivity-lock policy using Intune Settings Catalog, with device-side verification.
- **Compliance administration:** Created and configured a Windows device compliance policy with security requirements, noncompliance actions and group targeting.
- **Application management:** Added Microsoft Company Portal from the Microsoft Store catalogue and configured an available-app assignment for the pilot group.
- **Evidence and reporting:** Captured and embedded screenshots, documented investigations and referenced official technical guidance across all nine stages.

## Key achievements

The central milestone was bringing together Azure infrastructure, Entra ID identities and licensing to enroll and manage a Windows 11 endpoint through Microsoft Intune. The lab also provided practical exposure to configuration policies, compliance administration and application assignment workflows.

Resolving registration, connectivity and enrollment problems was an important part of the learning process. I used repeatable checks, PowerShell commands, portal observations and Microsoft documentation to investigate problems and record outcomes.

## Skills demonstrated

Microsoft Azure · Windows 11 · Microsoft Intune · Microsoft Entra ID · Microsoft Graph PowerShell · Device enrollment · Identity and group administration · Endpoint security configuration · Compliance policy administration · Application assignment · Troubleshooting · Evidence-based technical documentation

## Documentation and evidence

Each linked stage contains the configuration walkthrough, screenshots and technical notes relevant to that part of the lab. These are practical lab records, not a claim of production deployment. The report summarises the documented work without adding a separate Day 10 exercise.

## Official references

- [Microsoft Intune documentation](https://learn.microsoft.com/en-us/intune/)
- [Windows device management deployment guide](https://learn.microsoft.com/en-us/mem/intune/fundamentals/deployment-guide-platform-windows)
- [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/)
- [Microsoft Graph PowerShell overview](https://learn.microsoft.com/en-us/powershell/microsoftgraph/overview)
- [Windows device join troubleshooting (dsregcmd)](https://learn.microsoft.com/en-us/entra/identity/devices/troubleshoot-device-dsregcmd)
- [Intune Settings Catalog](https://learn.microsoft.com/en-us/intune/device-configuration/settings-catalog/)
- [Intune compliance policies](https://learn.microsoft.com/en-us/intune/device-security/compliance/overview)
- [Windows compliance settings](https://learn.microsoft.com/en-us/intune/device-security/compliance/ref-windows-settings)
- [Microsoft Store apps in Intune](https://learn.microsoft.com/en-us/intune/app-management/deployment/add-microsoft-store)
- [Azure virtual machines documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/)

## Final reflection and project closure

This project took **over 30 days** of hands-on learning, configuration, investigation and documentation. Starting with a Windows 11 virtual machine, it developed into an integrated Microsoft cloud endpoint-management lab covering identity, enrollment, security configuration, compliance and application administration.

The work strengthened my confidence in investigating real technical issues, validating administrative changes and communicating results through a structured GitHub portfolio. The skills are relevant to junior cybersecurity, SOC analyst, IT support and endpoint administration roles.

**Project closure — 26 October 2026:** The core learning objectives have been achieved to my satisfaction. The Microsoft Intune Endpoint Management Lab is closed as a completed nine-stage portfolio project. No Day 10 lab work is required.
