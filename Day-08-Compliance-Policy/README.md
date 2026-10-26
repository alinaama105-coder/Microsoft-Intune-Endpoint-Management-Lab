# Day 8 – Windows 11 Device Compliance Policy Configuration

**Microsoft Intune Endpoint Management Lab | 26 October 2026**

## 1. Objective and environment

Day 8 focused on building a Windows 11 **device compliance policy** in Microsoft Intune. The goal was to define measurable security conditions, configure a response when those conditions are not met, and target the policy at a controlled Microsoft Entra ID pilot group.

This followed earlier lab work covering tenant setup, group management, Windows 11 enrollment and device configuration. A configuration profile attempts to apply settings, while a compliance policy **evaluates** whether a device satisfies requirements. The two mechanisms complement one another, but policy creation alone does not demonstrate successful enforcement.

**Environment:** Microsoft Intune admin center, Microsoft Entra ID, Azure-hosted Windows 11 lab VM, and a pilot security group.

**Evidence 01** — Screenshot from this configuration stage.

![Day 8 evidence 01](../Day08-01.png)

**Evidence 02** — Screenshot from this configuration stage.

![Day 8 evidence 02](../Day08-02.png)

**Evidence 03** — Screenshot from this configuration stage.

![Day 8 evidence 03](../Day08-03.png)

**Evidence 04** — Screenshot from this configuration stage.

![Day 8 evidence 04](../Day08-04.png)

**Evidence 05** — Screenshot from this configuration stage.

![Day 8 evidence 05](../Day08-05.png)

## 2. Creating the Windows compliance policy

From **Devices → Compliance → Policies**, I began creating a new policy for **Windows 10 and later**. I selected the Windows platform so that Intune would expose the appropriate device-health and system-security categories.

I named the policy **`WIN11-Compliance-Policy-Pilot`** and described its purpose as assessing encryption, Secure Boot, TPM and other security requirements. The explicit pilot name helps distinguish a controlled lab policy from any tenant-wide configuration. Clear naming and descriptions are important for change control, auditing and troubleshooting when several policies coexist.

**Evidence 06** — Screenshot from this configuration stage.

![Day 8 evidence 06](../Day08-06.png)

**Evidence 07** — Screenshot from this configuration stage.

![Day 8 evidence 07](../Day08-07.png)

**Evidence 08** — Screenshot from this configuration stage.

![Day 8 evidence 08](../Day08-08.png)

**Evidence 09** — Screenshot from this configuration stage.

![Day 8 evidence 09](../Day08-09.png)

**Evidence 10** — Screenshot from this configuration stage.

![Day 8 evidence 10](../Day08-10.png)

**Evidence 11** — Screenshot from this configuration stage.

![Day 8 evidence 11](../Day08-11.png)

**Evidence 12** — Screenshot from this configuration stage.

![Day 8 evidence 12](../Day08-12.png)

## 3. Device health: encryption and boot integrity

The Device Health section establishes baseline checks for core Windows protections. I configured **BitLocker**, **Secure Boot** and **Code Integrity** as **Require**.

**BitLocker** protects stored data through volume encryption, reducing exposure if a device or storage drive is lost. A compliance requirement checks encryption state; it is not itself a command to turn encryption on.

**Secure Boot** uses UEFI trust checks to help prevent untrusted startup components from loading. **Code Integrity** assesses operating-system integrity protections. Together, these checks address data-at-rest protection and the trustworthiness of the Windows startup environment. They are especially relevant when deciding whether an endpoint should be trusted with organisational information.

**Evidence 13** — Screenshot from this configuration stage.

![Day 8 evidence 13](../Day08-13.png)

**Evidence 14** — Screenshot from this configuration stage.

![Day 8 evidence 14](../Day08-14.png)

**Evidence 15** — Screenshot from this configuration stage.

![Day 8 evidence 15](../Day08-15.png)

**Evidence 16** — Screenshot from this configuration stage.

![Day 8 evidence 16](../Day08-16.png)

**Evidence 17** — Screenshot from this configuration stage.

![Day 8 evidence 17](../Day08-17.png)

**Evidence 18** — Screenshot from this configuration stage.

![Day 8 evidence 18](../Day08-18.png)

## 4. Password and session security

I configured the available password and lock requirements, including a required password, blocking simple passwords, alphanumeric/complexity options, a minimum length of **eight characters**, and a **15-minute inactivity** threshold. A **90-day expiration** option was also configured as part of the exercise.

The objective was to explore how password requirements appear within Windows compliance settings and how a managed device can be assessed against an organisation's authentication baseline. An inactivity lock helps limit unauthorised use of an unattended workstation. Password complexity and length address weak credential selection, although strong authentication also depends on phishing-resistant methods, account protections and good password practices.

Some Windows compliance settings have legacy or mobile-device-specific applicability. Their presence in the portal does **not** establish that every configured password option is enforced on a Windows 11 desktop. Device reporting is needed to determine the effective result.

**Evidence 19** — Screenshot from this configuration stage.

![Day 8 evidence 19](../Day08-19.png)

**Evidence 20** — Screenshot from this configuration stage.

![Day 8 evidence 20](../Day08-20.png)

**Evidence 21** — Screenshot from this configuration stage.

![Day 8 evidence 21](../Day08-21.png)

**Evidence 22** — Screenshot from this configuration stage.

![Day 8 evidence 22](../Day08-22.png)

**Evidence 23** — Screenshot from this configuration stage.

![Day 8 evidence 23](../Day08-23.png)

**Evidence 24** — Screenshot from this configuration stage.

![Day 8 evidence 24](../Day08-24.png)

**Evidence 25** — Screenshot from this configuration stage.

![Day 8 evidence 25](../Day08-25.png)

**Evidence 26** — Screenshot from this configuration stage.

![Day 8 evidence 26](../Day08-26.png)

## 5. Endpoint protection and operating-system security

The System Security section was used to define additional requirements for **storage encryption, firewall, TPM, antivirus, antispyware, Microsoft Defender antimalware, security intelligence and real-time protection**.

A host firewall limits unwanted network connections. **TPM** provides hardware-backed security capabilities used by Windows protections such as key safeguarding. Antivirus and antispyware requirements assess whether endpoint malware defenses are present and active. **Real-time protection** helps identify suspicious files and activity as they are encountered, while security intelligence updates support recognition of known threats.

These requirements are intended to produce a more complete endpoint-health assessment rather than relying on a single control. The compliance policy checks relevant conditions; it does not replace a dedicated antivirus deployment, firewall configuration profile or incident-response process.

**Evidence 27** — Screenshot from this configuration stage.

![Day 8 evidence 27](../Day08-27.png)

**Evidence 28** — Screenshot from this configuration stage.

![Day 8 evidence 28](../Day08-28.png)

**Evidence 29** — Screenshot from this configuration stage.

![Day 8 evidence 29](../Day08-29.png)

**Evidence 30** — Screenshot from this configuration stage.

![Day 8 evidence 30](../Day08-30.png)

**Evidence 31** — Screenshot from this configuration stage.

![Day 8 evidence 31](../Day08-31.png)

**Evidence 32** — Screenshot from this configuration stage.

![Day 8 evidence 32](../Day08-32.png)

**Evidence 33** — Screenshot from this configuration stage.

![Day 8 evidence 33](../Day08-33.png)

**Evidence 34** — Screenshot from this configuration stage.

![Day 8 evidence 34](../Day08-34.png)

## 6. Microsoft Defender for Endpoint and WSL

I explored the **Microsoft Defender for Endpoint** device-risk setting and selected a **High** risk threshold. When Defender for Endpoint integration and device-risk signals are available, Intune can use those signals in compliance decisions. Selecting the threshold alone does not prove that Defender integration or risk reporting was active in this lab.

I also configured **Windows Subsystem for Linux (WSL)** compliance requirements for **Debian**, using the lab's version-range entries. WSL extends the Windows endpoint with a Linux execution environment, so distribution and version checks can be relevant to organisations managing developer workstations. The exercise demonstrates policy configuration; it does not establish that Debian was installed or that WSL compliance was evaluated on the VM.

**Evidence 35** — Screenshot from this configuration stage.

![Day 8 evidence 35](../Day08-35.png)

**Evidence 36** — Screenshot from this configuration stage.

![Day 8 evidence 36](../Day08-36.png)

**Evidence 37** — Screenshot from this configuration stage.

![Day 8 evidence 37](../Day08-37.png)

**Evidence 38** — Screenshot from this configuration stage.

![Day 8 evidence 38](../Day08-38.png)

**Evidence 39** — Screenshot from this configuration stage.

![Day 8 evidence 39](../Day08-39.png)

**Evidence 40** — Screenshot from this configuration stage.

![Day 8 evidence 40](../Day08-40.png)

## 7. Actions for noncompliance and notification

In **Actions for noncompliance**, I retained **Mark device noncompliant — Immediately**. This sets the policy's response once Intune determines that an applicable requirement has failed, rather than allowing an additional grace period before the device is marked noncompliant.

I also created an email notification template and configured an email action. The notification was designed to explain that a managed Windows device may not meet required security settings and to direct the user toward remediation or IT support.

This stage highlights that compliance management is not only about detecting a problem: administrators must communicate what is wrong and provide a route to resolution. A notification template can make this process more consistent and reduce repeated manual support work.

**Evidence 41** — Screenshot from this configuration stage.

![Day 8 evidence 41](../Day08-41.png)

**Evidence 42** — Screenshot from this configuration stage.

![Day 8 evidence 42](../Day08-42.png)

**Evidence 43** — Screenshot from this configuration stage.

![Day 8 evidence 43](../Day08-43.png)

**Evidence 44** — Screenshot from this configuration stage.

![Day 8 evidence 44](../Day08-44.png)

**Evidence 45** — Screenshot from this configuration stage.

![Day 8 evidence 45](../Day08-45.png)

**Evidence 46** — Screenshot from this configuration stage.

![Day 8 evidence 46](../Day08-46.png)

**Evidence 47** — Screenshot from this configuration stage.

![Day 8 evidence 47](../Day08-47.png)

## 8. Pilot-group targeting and policy review

I targeted the policy to the Microsoft Entra ID pilot group **`SG-Intune-Pilot`**, rather than selecting all users or all devices. The group view showed **one device and one user**. A restricted pilot assignment is good practice because it allows an administrator to examine policy behaviour before wider deployment.

I reviewed the policy's configured sections and the assignment before completing creation. Reviewing the scope matters because an unexpectedly broad compliance policy can cause devices to be reported as noncompliant, potentially affecting access if Conditional Access is configured to require compliant devices.

The screenshots document the assignment workflow and final review of the policy configuration.

**Evidence 48** — Screenshot from this configuration stage.

![Day 8 evidence 48](../Day08-48.png)

**Evidence 49** — Screenshot from this configuration stage.

![Day 8 evidence 49](../Day08-49.png)

**Evidence 50** — Screenshot from this configuration stage.

![Day 8 evidence 50](../Day08-50.png)

**Evidence 51** — Screenshot from this configuration stage.

![Day 8 evidence 51](../Day08-51.png)

**Evidence 52** — Screenshot from this configuration stage.

![Day 8 evidence 52](../Day08-52.png)

**Evidence 53** — Screenshot from this configuration stage.

![Day 8 evidence 53](../Day08-53.png)

## 9. Creation, monitoring and findings

The policy was created as **`WIN11-Compliance-Policy-Pilot`**. Its initial monitoring dashboard showed **Compliant: 0; Noncompliant: 0; Others: 0; Total: 0**.

This is evidence that the policy exists and its monitoring view is accessible, **not** evidence that the Windows 11 VM passed every security check. Device-level compliance would require an enrolled device to check in, process the policy and report results. No compliant or noncompliant device outcome is claimed here.

**What I learned:** device configuration and device compliance are separate administrative tasks; security checks need appropriate targeting; notification actions form part of remediation; and monitoring must be interpreted carefully rather than assuming that a created policy has already been evaluated.

**Evidence 54** — Screenshot from this configuration stage.

![Day 8 evidence 54](../Day08-54.png)

**Evidence 55** — Screenshot from this configuration stage.

![Day 8 evidence 55](../Day08-55.png)

**Evidence 56** — Screenshot from this configuration stage.

![Day 8 evidence 56](../Day08-56.png)

## Conclusion

Day 8 completed the practical configuration of a Windows 11 compliance policy, including device-health checks, endpoint protection conditions, password options, WSL and Defender-related settings, noncompliance actions, email notifications, pilot assignment and initial monitoring. The work demonstrates Intune policy administration and evidence-based reporting without claiming an unverified device-compliance result.

## Microsoft Learn references

- [Get started with device compliance policies](https://learn.microsoft.com/en-us/intune/intune-service/protect/device-compliance-get-started)
- [Windows compliance policy settings](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-create-windows)
- [Actions for noncompliance](https://learn.microsoft.com/en-us/intune/intune-service/protect/actions-for-noncompliance)
- [Monitor device compliance](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-monitor)
