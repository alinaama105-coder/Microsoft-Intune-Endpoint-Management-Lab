Day 2 - Azure Windows 11 VM and Intune Enrollment Troubleshooting

Overview

Today I continued building my Microsoft Intune lab by creating a dedicated Windows 11 test endpoint in Azure, connecting to it remotely, and beginning the Intune enrollment process.

The main goal was to get the Windows 11 device ready for Intune management and document the troubleshooting process along the way.

1. Created a Separate Lab Environment

I used a dedicated Microsoft/Azure lab environment so I could safely test Intune without affecting a production or business environment.

**Lab account:** `AliNaama@AliNaamaLab155.onmicrosoft.com`

**What I learned:** A dedicated tenant gives me a safe place to test device enrollment, policies and configurations without affecting real users or production devices.

2. Created the Azure Resource Group

I created a dedicated Azure Resource Group:

`RG-Intune-Lab`

This keeps the Azure resources used for the project organised in one place.

**What I learned:** Resource Groups make it easier to manage related Azure resources together.

3. Created the Windows 11 Virtual Machine

I created a Windows 11 Azure VM called:

`Intune-Win11-lab`

The VM will act as the Windows endpoint used throughout the Intune project.

Configuration included:

- Windows 11
- Standard SSD
- Boot diagnostics enabled
- Auto-shutdown enabled
- No unnecessary extensions
- No additional data disks

**What I learned:** An Intune lab does not need an unnecessarily complicated VM. I need a suitable Windows endpoint that I can use for Microsoft Entra ID and Intune testing.

4. Troubleshot an Azure Region Deployment Failure

My first deployment attempt used **West Europe**, but Azure rejected the deployment because the region was not currently accepting the new subscription.

I identified that the issue was related to Azure regional eligibility rather than the VM configuration.

I changed the deployment region to:

**Sweden Central**

The VM then deployed successfully.

**What I learned:** A failed Azure deployment does not always mean the VM configuration is wrong. Subscription restrictions and regional availability can also prevent a deployment.

5. Connected to the Windows 11 VM

After deployment, I attempted to connect using Windows App / Remote Desktop.

Initially I entered the VM name:

`Intune-Win11-lab`

Remote Desktop could not locate the computer.

I returned to the Azure VM Overview page and used the VM's public IP address instead. The RDP connection then worked successfully.

**What I learned:** The Azure VM name was not being resolved externally in this setup, so I used the VM's public IP address to establish the RDP connection.

6. Connected the Work Account

Inside the Windows 11 VM I opened:

**Settings > Accounts > Access work or school**

I connected my lab account:

`AliNaama@AliNaamaLab155.onmicrosoft.com`

Evidence captured:

`01-Work-School-Account-Connected.png`

**What I learned:** Connecting a work account establishes a relationship between the Windows device and the Microsoft organisation, but this alone does not mean the device is fully managed by Intune.

7. Attempted Intune Device Enrollment

I then attempted to enroll the Windows 11 VM into device management.

Windows returned an MDM discovery error because it could not automatically discover a management endpoint for the username.

Instead of manually forcing an MDM server URL, I stopped and investigated the underlying issue.

Evidence captured:

`02-Intune-MDM-Auto-Discovery-Failure.png`

**What I learned:** Troubleshooting is part of the lab. Documenting a failure, investigating the cause and understanding the next action is just as useful as documenting a successful configuration.

8. Investigated the Intune Licence

I checked the Microsoft 365 Admin Center and found that an Intune licence was not yet available to assign to the lab user.

Further investigation showed that the Microsoft account/subscription was still under review.

Evidence captured:

`03-Microsoft-Tenant-Account-Under-Review.png`

**What I learned:** Azure subscriptions and Microsoft Intune licensing are separate. Having Azure credit available does not automatically provide an Intune licence.

9. Confirmed Azure Credit

The Azure credit is now active, which means the Azure side of the lab is operational.

Current status:

- Azure credit: Active
- Windows 11 Azure VM: Created
- RDP access: Working
- Work account: Connected
- Intune licence: Waiting
- Auto-claim: Enabled
- Intune enrollment: Paused until licensing is available

10. Paused at the Correct Troubleshooting Point

There is no benefit in repeatedly attempting Intune enrollment while the required licence is unavailable.

I stopped at this point rather than changing unrelated settings. The Azure VM can also be stopped/deallocated when it is not required so that unnecessary compute credit is not consumed.

Day 2 Status

The Windows 11 test environment is ready and remote access is working. I have started the Intune enrollment process and documented the MDM discovery and licensing issues encountered.

The current blocker is the availability of the Intune licence.

Next Steps - Day 3

Once the Intune licence becomes available:

1. Verify and assign the Intune licence
2. Configure the MDM user scope
3. Join the Windows 11 device to Microsoft Entra ID
4. Complete Intune enrollment
5. Verify the managed device in Intune
6. Create device/user groups
7. Begin configuration, compliance and endpoint security policies

**Project status:** Day 2 complete - Intune enrollment pending licensing.
