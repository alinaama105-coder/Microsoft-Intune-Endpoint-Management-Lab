Day 3 - Entra ID Preparation and Microsoft Graph Verification

Overview

Today I prepared the Microsoft Entra ID side of the Intune lab and verified the environment using PowerShell 7 and Microsoft Graph.

I created a dedicated test user, built separate security groups for Intune users and devices, added the correct members, checked the Windows 11 device before Intune enrollment, and then used Microsoft Graph to independently verify the configuration.

1. Creating the Test User

I created a dedicated lab user called:

`daniel carter`

This account will be used later for user-based Intune assignments, configuration profiles and application testing.

![Create Daniel Carter user](01-create-daniel-carter-user.png)

The user was created successfully in Microsoft Entra ID.

![Daniel Carter user created](02-daniel-carter-user-created.png)

What I learned: Using a dedicated test account keeps Intune testing separate from the administrator account and gives me a realistic user for policy assignments.

2. Creating the Intune User Security Group

I created a dedicated security group:

`SG-Intune-IT-Users`

![Create Intune IT users group](03-create-intune-it-users-group.png)

The group was created as an assigned security group.

![Intune IT users group created](04-intune-it-users-group-created.png)

I then added Daniel Carter to the group.

![Add Daniel Carter to user group](05-add-daniel-carter-to-user-group.png)

![Daniel Carter selected for user group](06-daniel-carter-selected-for-user-group.png)

The membership was confirmed in Entra ID.

![Daniel Carter user group member](07-daniel-carter-user-group-member.png)

What I learned: Separate security groups allow Intune policies and applications to be targeted at specific users instead of assigning everything to the whole tenant.

3. Creating the Intune Device Security Group

I created a second security group for Windows devices:

`SG-Intune-IT-Devices`

![Create Intune IT devices group](08-create-intune-it-devices-group.png)

The device group was created successfully.

![Intune device group created](09-intune-device-group-created.png)

I checked both dedicated Intune groups together.

![Intune user and device security groups](10-intune-user-and-device-security-groups.png)

What I learned: Keeping user and device assignments separate makes the lab easier to manage and reflects how endpoint policies can be targeted in a structured environment.

4. Checking the Windows Device Before Intune Enrollment

I checked the Windows 11 lab device in Microsoft Entra ID before Intune enrollment.

![Windows device before Intune enrollment](11-windows-device-before-intune-enrollment.png)

At this stage the device was visible in Entra ID but was not yet managed by Intune.

This gives me a useful baseline so I can compare the device state before and after Intune enrollment.

5. Adding the Windows Device to the Device Group

I opened the dedicated device group:

`SG-Intune-IT-Devices`

![Open Intune IT devices group](12-open-intune-it-devices-group.png)

I then added the Windows 11 lab device.

![Add Windows device to device group](13-add-windows-device-to-device-group.png)

The device was selected for membership.

![Windows device selected for device group](14-windows-device-selected-for-device-group.png)

I checked the group members after the change.

![Windows device group members](15-windows-device-group-members.png)

The Windows device membership was confirmed.

![Windows device group member confirmed](16-windows-device-group-member-confirmed.png)

What I learned: Device groups give me a controlled target for future Intune compliance, configuration and endpoint security policies.

6. Microsoft Graph PowerShell Troubleshooting

After preparing the Entra ID objects through the portal, I wanted to verify them independently using Microsoft Graph.

My first Microsoft Graph command was not available in the original PowerShell environment.

![Graph module initial error](01-Graph-Module-Initial-Error.png)

I checked and worked through the Microsoft Graph module installation.

![Graph module troubleshooting](02-Graph-Module-Troubleshooting.png)

Rather than hiding the failed attempts, I kept them as evidence of the troubleshooting process.

What I learned: A failed command can help identify whether the problem is with the tenant, permissions, PowerShell environment or a missing module.

7. Moving to PowerShell 7

I installed PowerShell 7 and verified the PowerShell version before continuing with Microsoft Graph.

```powershell
$PSVersionTable
```

![PowerShell version verification](03-PowerShell-Version-Verification.png)

What I learned: Checking the shell version is useful when troubleshooting module compatibility and authentication behaviour.

8. Connecting to Microsoft Graph

I connected to Microsoft Graph with delegated read permissions for users, groups and devices.

```powershell
Connect-MgGraph -Scopes "User.Read.All","Group.Read.All","Device.Read.All"
```

The connection completed successfully.

![Microsoft Graph connected](04-Microsoft-Graph-Connected.png)

9. Verifying the Microsoft Graph Context

I checked the active Microsoft Graph session.

```powershell
Get-MgContext
```

![Graph context verified](05-Graph-Context-Verified.png)

This confirmed that Microsoft Graph was connected to the lab tenant using the expected account and delegated permissions.

What I learned: `Get-MgContext` is a quick way to check the current Graph account, tenant, authentication type and granted scopes before running administrative queries.

10. Verifying the Test User Through Graph

I queried Microsoft Entra ID for Daniel Carter.

```powershell
Get-MgUser -Filter "displayName eq 'daniel carter'" | Select-Object DisplayName,UserPrincipalName,Id
```

![Daniel Carter user verified](06-Daniel-Carter-User-Verified.png)

The query returned the user successfully.

This proved that the account created through the portal could also be retrieved programmatically through Microsoft Graph.

11. Verifying the Intune Security Groups Through Graph

I queried the dedicated Intune security groups.

```powershell
Get-MgGroup -Filter "startsWith(displayName,'SG-Intune')" | Select-Object DisplayName,Id
```

![Intune security groups verified](07-Intune-Security-Groups-Verified.png)

Both groups were returned:

- `SG-Intune-IT-Users`
- `SG-Intune-IT-Devices`

What I learned: Microsoft Graph gives me another way to validate Entra ID configuration instead of relying only on what I can see in the portal.

12. Verifying the Windows Device Through Graph

I queried the Windows 11 lab device directly from Microsoft Graph.

```powershell
Get-MgDevice -Filter "displayName eq 'Intune-Win11-la'" | Select-Object DisplayName,OperatingSystem,OperatingSystemVersion,TrustType,AccountEnabled,Id
```

![Entra device verified](08-Entra-Device-Verified.png)

The query confirmed the Windows operating system, OS version, enabled state, device object ID and:

`TrustType : Workplace`

The Workplace trust type is consistent with the device being Microsoft Entra registered at this stage of the lab.

13. Verifying User Group Membership Through Graph

I verified that Daniel Carter was a member of the dedicated Intune user group.

```powershell
Get-MgGroupMember -GroupId (Get-MgGroup -Filter "displayName eq 'SG-Intune-IT-Users'").Id | Select-Object Id
```

![User group membership verified](09-User-Group-Membership-Verified.png)

The returned member object matched the Daniel Carter account created earlier.

14. Verifying Device Group Membership Through Graph

I also verified the Windows device membership in the dedicated Intune device group.

![Device group membership verified](10-Device-Group-Membership-Verified.png)

This confirmed through Microsoft Graph that the device was correctly positioned for future device-based Intune assignments.

15. Troubleshooting Summary

During the Microsoft Graph stage I encountered several problems, including the Graph command initially being unavailable, module installation issues and authentication problems.

I worked through the PowerShell environment and Microsoft Graph setup until the connection succeeded. I then verified the tenant context and queried the user, security groups and Windows device.

What I learned: Troubleshooting is not just about getting a command to work. I need to identify which layer is failing, make one change at a time and then verify the result.

16. Day 3 Outcome

By the end of Day 3 I had:

- Created a dedicated Entra ID test user
- Created separate Intune user and device security groups
- Added Daniel Carter to the user group
- Added the Windows 11 endpoint to the device group
- Recorded the Windows device state before Intune enrollment
- Installed and verified PowerShell 7
- Troubleshot Microsoft Graph PowerShell
- Connected successfully to Microsoft Graph
- Verified the active Graph tenant and permissions
- Verified the test user through Graph
- Verified both Intune security groups through Graph
- Verified the Windows device through Graph
- Verified user and device group membership

Day 3 Status

Entra ID test user: Ready  
Intune user security group: Ready  
Intune device security group: Ready  
Windows device in device group: Confirmed  
PowerShell 7: Working  
Microsoft Graph: Connected  
Graph user/group/device verification: Completed  
Intune enrollment: Pending  
Intune licensing/management access: Pending

The Entra ID foundation is now prepared and independently verified. The next stage will continue with Microsoft Intune enrollment and policy configuration once the required Intune management access is available.
