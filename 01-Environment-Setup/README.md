01 - Microsoft Intune Environment Setup

Overview

This stage establishes a cloud-based Windows 11 endpoint that will be used for hands-on Microsoft Intune administration and testing.

Environment

- Microsoft Azure
- Microsoft Entra ID
- Microsoft Intune
- Windows 11 Pro 25H2
- Azure Virtual Machine
- Remote Desktop Protocol (RDP)

Azure VM Configuration

- VM Name: Intune-Win11-lab
- Operating System: Windows 11 Pro 25H2
- Architecture: x64
- VM Size: Standard D2as v7
- Region: West Europe
- Security Type: Trusted Launch
- Secure Boot: Enabled
- vTPM: Enabled

Work Completed

- Created a dedicated Azure resource group for the Intune lab
- Deployed a Windows 11 Pro Azure virtual machine
- Configured Trusted Launch, Secure Boot and vTPM
- Established remote access to the VM using RDP
- Verified Microsoft 365 Business Premium licensing
- Confirmed Microsoft Intune licensing is available
- Reviewed Microsoft Entra ID permissions
- Identified that the lab account requires the Intune Administrator role

Next Steps

- Assign the Intune Administrator role to the lab account
- Create a dedicated Intune lab device group
- Enroll the Windows 11 VM into Microsoft Intune
- Verify successful device enrollment
- Begin device compliance and configuration testing

References and Further Reading

Official Microsoft documentation relevant to the technologies and procedures demonstrated in this lab:

- [Azure Windows Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/)
- [Trusted Launch for Azure virtual machines](https://learn.microsoft.com/en-us/azure/virtual-machines/trusted-launch)
- [Role-based access control for Microsoft Intune](https://learn.microsoft.com/en-us/intune/intune-service/fundamentals/role-based-access-control)

