Day 2 - Azure Windows 11 VM and Intune Enrollment Troubleshooting

Overview

Today I continued building my Microsoft Intune lab by creating a dedicated Windows test endpoint in Azure, connecting to it remotely, preparing it for Intune management, and troubleshooting the issues encountered during enrollment.

I also used PowerShell as part of the Day 2 validation and troubleshooting process to inspect the endpoint, verify networking, test Microsoft cloud connectivity and investigate the Intune enrollment issue.

1. Created a Separate Lab Environment

I used a dedicated Microsoft/Azure lab environment so I could safely test Intune without affecting a production or business environment.

Lab account:

`AliNaama@AliNaamaLab155.onmicrosoft.com`

What I learned: A dedicated tenant provides a controlled environment where I can practise endpoint management without affecting production users or devices.

2. Created the Azure Resource Group

I created:

`RG-Intune-Lab`

This keeps the resources associated with the Intune project organised in one location.

What I learned: Azure Resource Groups provide a logical way of organising and managing related cloud resources.

3. Created the Windows Test VM

I created the Azure VM:

`Intune-Win11-lab`

The VM acts as the Windows endpoint for the Intune project.

Configuration included:

- Windows 11
- Standard SSD
- Boot diagnostics enabled
- Auto-shutdown enabled
- No unnecessary extensions
- No additional data disks

As part of Day 2 validation, I used PowerShell to retrieve key system information:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsArchitecture, CsName
```

Purpose: Verify the operating system information, architecture and computer name of the endpoint.

My first attempt was made from Command Prompt and failed because `Get-ComputerInfo` is a PowerShell cmdlet. I identified the incorrect command-line environment, switched to PowerShell and successfully executed the command.

What I learned: Windows administration commands can depend on the shell being used. PowerShell cmdlets cannot normally be executed directly from standard Command Prompt.

4. Troubleshot the Azure VM Deployment

The initial Azure deployment encountered a regional availability/eligibility issue.

After identifying that the problem was related to the Azure region rather than the VM configuration itself, I changed the deployment location and successfully deployed the VM.

What I learned: Azure deployment failures are not always caused by incorrect VM configuration. Subscription eligibility and regional availability can also affect resource deployment.

5. Troubleshot Remote Access and Networking

When attempting to connect through Remote Desktop, using:

`Intune-Win11-lab`

did not work because the VM name could not be resolved from the remote client.

I returned to Azure, obtained the appropriate connection information and configured the RDP connection using the VM's public IP.

I then used PowerShell inside the VM to inspect its network configuration:

```powershell
Get-NetIPConfiguration
```

This showed the active Ethernet interface, private IPv4 address, default gateway and DNS server.

I also checked the virtual network adapter:

```powershell
Get-NetAdapter | Select-Object Name, InterfaceDescription, Status, LinkSpeed
```

The Microsoft Hyper-V Network Adapter reported a status of `Up`.

What I learned: `Get-NetIPConfiguration` and `Get-NetAdapter` provide different levels of network troubleshooting information. One exposes IP configuration while the other helps verify the state of the network interface.

6. Verified Microsoft Cloud Connectivity

Because the endpoint will communicate with Microsoft cloud services, I tested HTTPS connectivity to Microsoft's authentication infrastructure:

```powershell
Test-NetConnection login.microsoftonline.com -Port 443
```

I then tested DNS resolution:

```powershell
Resolve-DnsName login.microsoftonline.com
```

The DNS query successfully returned Microsoft service records and IPv4/IPv6 addresses.

What I learned: Successful DNS resolution and TCP 443 connectivity help establish that the endpoint can locate and communicate with Microsoft's authentication services.

7. Connected the Work Account

Inside the Windows VM I opened:

`Settings > Accounts > Access work or school`

and connected:

`AliNaama@AliNaamaLab155.onmicrosoft.com`

What I learned: Connecting a work account establishes a relationship with the Microsoft organisation, but it does not by itself prove that the endpoint is fully enrolled and managed by Intune.

8. Attempted Intune Device Enrollment

I attempted to enroll the Windows endpoint into device management.

Windows returned an MDM discovery error because it could not automatically discover the required management endpoint.

Rather than manually forcing configuration changes, I investigated the environment.

As part of the troubleshooting process, I tested connectivity directly to the Microsoft Intune enrollment service:

```powershell
Test-NetConnection enrollment.manage.microsoft.com -Port 443
```

The result returned:

```text
RemotePort       : 443
TcpTestSucceeded : True
```

This confirmed that the VM could establish basic HTTPS connectivity to the Intune enrollment endpoint.

What I learned: The successful TCP test helped narrow the investigation. The enrollment failure was less likely to be caused by a simple TCP 443 connectivity problem, so I continued investigating the Intune configuration and licensing.

9. Investigated Intune Licensing

I checked the Microsoft 365 administration environment and found that an Intune licence was not yet available for assignment to the lab user.

Further investigation showed that the Microsoft account/subscription was still under review.

This explained why continuing to change endpoint settings would not have been an appropriate troubleshooting step.

What I learned: Azure services and Microsoft Intune licensing are separate. Having Azure credit and a functioning Azure VM does not automatically provide an active Intune licence.

10. Day 2 Troubleshooting Outcome

By the end of Day 2 I had:

- Created the Azure Windows test endpoint
- Established RDP access
- Verified the endpoint using PowerShell
- Inspected IP and adapter configuration
- Verified Microsoft DNS resolution
- Verified HTTPS connectivity to Microsoft authentication services
- Connected the work account
- Attempted Intune enrollment
- Reproduced the MDM discovery problem
- Verified TCP 443 connectivity to the Intune enrollment service
- Identified licensing/account review as the current blocker

Rather than repeatedly attempting enrollment or changing unrelated settings, I stopped troubleshooting at the point where the external licensing dependency had been identified.

Day 2 Status

Azure VM: Operational  
RDP: Working  
PowerShell validation: Completed  
Microsoft connectivity: Verified  
Work account: Connected  
Intune enrollment: Pending  
Microsoft account/licensing review: Pending
