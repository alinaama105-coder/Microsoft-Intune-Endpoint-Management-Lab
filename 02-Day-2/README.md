Day 2 - Azure Windows 11 VM and Intune Enrollment Troubleshooting

Overview

Today I continued my Microsoft Intune lab. I created a Windows VM in Azure, connected to it remotely and started preparing it for Intune.

I also started practising some PowerShell commands. The aim was to understand what each command does and use them to check the VM while I was setting it up.

1. Created a Separate Lab Environment

I used a dedicated Microsoft/Azure lab environment so I could safely test Intune without affecting a production or business environment.

Lab account:

`AliNaama@AliNaamaLab155.onmicrosoft.com`

What I learned: Using a separate lab account means I can practise without affecting a real business environment.

2. Created the Azure Resource Group

I created:

`RG-Intune-Lab`

This keeps the resources associated with the Intune project organised in one location.

What I learned: A Resource Group helps me keep the Azure resources for this lab together.

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

I started with a PowerShell command to check basic information about the VM:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsArchitecture, CsName
```

Purpose: Check the Windows version, system type and computer name.

My first attempt failed because I entered the command in Command Prompt. I then learned that `Get-ComputerInfo` is a PowerShell command, switched to PowerShell and tried it again successfully.

What I learned: Command Prompt and PowerShell are different. Some commands only work in PowerShell.

4. Troubleshot the Azure VM Deployment

My first Azure VM deployment did not work because of a region restriction. I changed the Azure region and tried the deployment again, which worked.

What I learned: If an Azure deployment fails, the problem is not always the VM settings. The selected Azure region can also be the cause.

5. Troubleshot Remote Access and Networking

When attempting to connect through Remote Desktop, using:

`Intune-Win11-lab`

did not work because the VM name could not be resolved from the remote client.

I returned to Azure, obtained the appropriate connection information and configured the RDP connection using the VM's public IP.

I used PowerShell to look at the VM's network settings:

```powershell
Get-NetIPConfiguration
```

This showed me the Ethernet connection, private IP address, gateway and DNS server.

I then checked whether the network adapter was working:

```powershell
Get-NetAdapter | Select-Object Name, InterfaceDescription, Status, LinkSpeed
```

The network adapter showed a status of `Up`, which meant it was active.

What I learned: I can use PowerShell to check both the network settings and whether the network adapter is active.

6. Verified Microsoft Cloud Connectivity

I then tested whether the VM could connect to Microsoft's sign-in service on port 443:

```powershell
Test-NetConnection login.microsoftonline.com -Port 443
```

I also used PowerShell to check if DNS could find the Microsoft sign-in address:

```powershell
Resolve-DnsName login.microsoftonline.com
```

The command returned Microsoft addresses, so the DNS lookup was working.

What I learned: I learned how to check if DNS is working and whether the VM can connect to a Microsoft service.

7. Connected the Work Account

Inside the Windows VM I opened:

`Settings > Accounts > Access work or school`

and connected:

`AliNaama@AliNaamaLab155.onmicrosoft.com`

What I learned: Connecting the work account does not automatically mean the device is fully enrolled in Intune.

8. Attempted Intune Device Enrollment

I attempted to enroll the Windows endpoint into device management.

Windows returned an MDM discovery error because it could not automatically discover the required management endpoint.

Instead of changing random settings, I checked what was working first.

I used PowerShell to test whether the VM could reach the Intune enrollment service on port 443:

```powershell
Test-NetConnection enrollment.manage.microsoft.com -Port 443
```

The result returned:

```text
RemotePort       : 443
TcpTestSucceeded : True
```

The result was `True`, so the VM could reach the Intune enrollment service on port 443.

What I learned: The connection test worked, so I knew the VM could reach the Intune service. I then continued checking the Intune licence and account.

9. Investigated Intune Licensing

I checked the Microsoft 365 administration environment and found that an Intune licence was not yet available for assignment to the lab user.

Further investigation showed that the Microsoft account/subscription was still under review.

I stopped changing settings because the Intune licence was still not available.

What I learned: Azure credit and an Intune licence are separate. Having Azure credit does not automatically give me Intune.

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

I stopped at this point because the Intune licence/account review still needed to be completed before I could continue with enrollment.

Day 2 Status

Azure VM: Operational  
RDP: Working  
PowerShell validation: Completed  
Microsoft connectivity: Verified  
Work account: Connected  
Intune enrollment: Pending  
Microsoft account/licensing review: Pending
