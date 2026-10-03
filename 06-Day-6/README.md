# Day 6 – Microsoft Intune Device Enrollment & Validation

Day 6 continues directly from the identity, security-group and licensing preparation completed during Days 4 and 5. By this point, the Microsoft Entra environment had been prepared, the Intune security groups were in place, an Intune Plan 1 license had been assigned, and the existing Windows 11 Azure VM was ready to move from preparation into active endpoint management.

The objective was to take the existing **Intune-Win11-lab** endpoint through the complete enrollment journey: review its current organisational connection, troubleshoot the initial enrollment failures, validate licensing and MDM configuration, join the device to Microsoft Entra ID, establish Microsoft Intune MDM management, and confirm that the endpoint ultimately appeared as a managed and compliant device.

![Day 4 and Day 5 preparation flow](../Lab%20Preparation%20Flow_%20Day%204_5%20to%20Day%206.png)

---

## Continuing from Day 4 and Day 5

The work began on the existing Windows 11 VM rather than creating a new endpoint. In **Settings → Accounts → Access work or school**, the organisational account was already visible, providing the starting point for the Day 6 enrollment work.

![Initial Access work or school state](../01-Access-Work-School-Initial.png)

The work or school account connection was reviewed to confirm that Windows already had an organisational identity associated with the device.

![Work or school account connected](../02-Work-School-Account-Connected.png)

From the same area, the device-management enrollment option was used to begin moving the endpoint towards Microsoft Intune management.

![Device management enrollment option](../03-Device-Management-Enrollment-Option.png)

The first enrollment attempt did not complete successfully. Windows returned an enrollment server error, showing that having the organisational account present on the endpoint was not, by itself, enough to complete MDM enrollment.

![Enrollment server error](../04-Enrollment-Server-Error.png)

Rather than rebuilding the VM, the existing cloud and endpoint configuration was investigated. The first check was licensing. Microsoft Intune licensing was confirmed as assigned so that the account being used for enrollment had the required Intune entitlement.

![Intune license assigned](../05-Intune-License-Assigned.png)

The licensed-user view was also checked to confirm the Intune licensing state across the lab users.

![Intune licensed users](../06-Intune-Licensed-Users.png)

A further enrollment attempt exposed an MDM autodiscovery problem. This was useful troubleshooting evidence because it showed that the next step was to validate the relationship between the Windows endpoint, Microsoft Entra ID and the Intune enrollment configuration rather than repeatedly attempting the same enrollment.

![MDM autodiscovery error](../07-MDM-Autodiscovery-Error.png)

The Intune enrollment configuration was therefore reviewed before continuing.

![Intune enrollment configuration](../08-Intune-Enrollment-Configuration.png)

## Validating the Device Identity State

Attention then moved back to the Windows endpoint. The device registration state was examined using Windows registration information to understand how the VM was currently associated with Microsoft Entra ID.

![DSREGCMD state before Entra Join](../09-DSREGCMD-Before-Entra-Join.png)

This check highlighted an important distinction in the lab: adding a work account, joining a device to Microsoft Entra ID, and enrolling that device into Microsoft Intune are related stages, but they are not the same operation.

The existing VM therefore needed to progress through the Microsoft Entra Join stage. From Windows, **Join this device to Microsoft Entra ID** was selected.

![Join device to Microsoft Entra ID](../10-Join-Device-To-Entra-ID.png)

During authentication, the first sign-in attempt exposed an account or tenant naming issue.

![Entra sign-in troubleshooting](../11-Entra-Sign-In-Troubleshooting.png)

Instead of continuing with the incorrect identity information, the user account was checked directly in Microsoft Entra ID. This allowed the correct User Principal Name and tenant details to be verified against the cloud identity.

![Entra user account verification](../12-Entra-User-Account-Verification.png)

With the correct organisational identity confirmed, Windows recognised the intended organisation and displayed the final confirmation before joining the endpoint.

![Confirm organisation for Entra Join](../13-Confirm-Organization-Entra-Join.png)

The Microsoft Entra Join then completed successfully.

![Microsoft Entra Join success](../14-Entra-Join-Success.png)

At this stage, the same Windows 11 Azure VM that had been used throughout the project had successfully progressed into the organisational device environment.

## Reconnecting and Checking the Existing VM

Because the identity configuration of the endpoint had changed, remote access was checked before continuing. The existing **Intune-Win11-lab** machine remained available through Windows App.

![Windows App remote PC](../15-Windows-App-Remote-PC.png)

The VM was opened again and the Windows desktop remained accessible.

![Entra joined VM desktop](../16-Entra-Joined-VM-Desktop.png)

PowerShell was then used to verify the account running the current Windows session:

```powershell
whoami
```

![WHOAMI local Azure user](../17-WHOAMI-Local-AzureUser.png)

The result showed the existing local **azureuser** session. This provided useful evidence that the device identity and the interactive Windows user session are separate concepts: the endpoint could be connected to the organisation while the current session continued under the existing local VM account.

The registration and enrollment prerequisites were then checked again to validate the endpoint state before proceeding with MDM enrollment.

![DSREGCMD enrollment prerequisites](../18-DSREGCMD-Enrollment-Prerequisites.png)

Remote-PC configuration was also reviewed through Windows App.

![Windows App remote PC settings](../19-Windows-App-Remote-PC-Settings.png)

During this check, the PC-name field showed that Windows App expected a valid computer name or IP address rather than an organisational sign-in identity.

![Windows App PC name validation](../20-Windows-App-PC-Name-Validation.png)

The Azure VM itself was also reviewed. Although the portal displayed a VM-agent status warning, the machine remained accessible and the endpoint-management work could continue.

![Azure VM extensions status](../21-Azure-VM-Extensions-Status.png)

## Establishing the Intune Baseline

Before the final MDM enrollment, **Microsoft Intune Admin Center → Devices → All devices** was checked.

At this point the portal showed **0 devices**.

![Intune before enrollment – no devices](../22-Intune-Before-Enrollment-No-Devices.png)

This created a clear baseline. The endpoint had progressed through the identity work, but it had not yet appeared in the Intune device inventory. It also demonstrated why Microsoft Entra connectivity alone should not be treated as proof of successful Intune enrollment.

Back on the Windows VM, **Access work or school** was opened again. The organisational account was present and the endpoint was now ready for the management stage.

![Access work or school before MDM](../23-Access-Work-School-Before-MDM.png)

The MDM enrollment process was started. Windows displayed **Setting up your device**, indicating that the endpoint was establishing its management relationship with the organisation.

![MDM setting up device](../24-MDM-Setting-Up-Device.png)

After the process completed, Windows showed the MDM connection to the lab organisation.

![MDM connection confirmed](../25-MDM-Connection-Confirmed.png)

This was the first clear endpoint-side confirmation that an MDM relationship had been established. The Intune portal, however, did not immediately display the endpoint.

![Intune device pending](../26-Intune-Device-Pending.png)

Rather than assuming that enrollment had failed, the Windows endpoint was checked locally for evidence that the MDM components had been provisioned.

## Validating MDM Components with PowerShell

PowerShell was used to inspect the scheduled tasks created under Windows Enterprise Management:

```powershell
Get-ScheduledTask -TaskPath "\Microsoft\Windows\EnterpriseMgmt\*" |
Select-Object TaskName, State
```

![EnterpriseMgmt scheduled tasks](../27-EnterpriseMgmt-Scheduled-Tasks.png)

The presence of the Enterprise Management tasks provided additional endpoint-side evidence that Windows had created the components used for ongoing MDM communication and policy processing. This was particularly useful during the short period in which Windows showed the management connection but the endpoint had not yet appeared in the Intune Admin Center.

## Successful Microsoft Intune Enrollment

After allowing the enrollment process to complete, **Intune Admin Center → Devices → All devices** was refreshed again.

This time the result had changed.

![Intune device enrolled and compliant](../28-Intune-Device-Enrolled-Compliant.png)

The Windows VM was now present in Microsoft Intune. The portal showed the endpoint as **Managed by Intune**, with **Personal** ownership, **Compliant** status, Windows as the operating system, OS version **10.0.26200.9457**, an associated primary user, and a successful recent check-in.

The same **Intune-Win11-lab** endpoint that began this stage outside the Intune device inventory had now become an actively managed and compliant Windows endpoint.

## Day 6 Outcome

Day 6 brought together the preparation completed during Days 4 and 5 and converted it into a working endpoint-management environment.

The project now follows one continuous path:

**Entra users and groups → Intune licensing → Windows 11 VM preparation → identity validation → enrollment troubleshooting → Microsoft Entra Join → Intune MDM enrollment → Enterprise Management validation → Intune check-in → compliant managed endpoint**

The troubleshooting evidence has been retained deliberately. It demonstrates not only the successful final configuration, but also how an endpoint can be investigated across Windows, Microsoft Entra ID, Microsoft Azure and Microsoft Intune when enrollment does not immediately complete as expected.

At the end of Day 6, the Windows 11 Azure VM had successfully transitioned from a prepared lab endpoint into a **Microsoft Intune-managed and compliant Windows device**.

This provides the foundation for the next stage of the project: applying practical endpoint-management controls such as configuration profiles, compliance policies, endpoint security settings and application deployments.

---

## References

- [Microsoft Intune documentation](https://learn.microsoft.com/mem/intune/)
- [Windows enrollment methods in Microsoft Intune](https://learn.microsoft.com/mem/intune/enrollment/windows-enrollment-methods)
- [Microsoft Entra joined devices](https://learn.microsoft.com/entra/identity/devices/concept-directory-join)
- [Troubleshoot devices by using dsregcmd](https://learn.microsoft.com/entra/identity/devices/troubleshoot-device-dsregcmd)
- [MDM enrollment of Windows devices](https://learn.microsoft.com/windows/client-management/mdm-enrollment-of-windows-devices)
