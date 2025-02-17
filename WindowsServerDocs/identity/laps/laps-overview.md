---
title: Windows LAPS overview
description: Get an overview of Windows Local Administrator Password Solution (Windows LAPS), including key scenarios and setup and management options.
author: jay98014
ms.author: jsimmons
ms.date: 11/08/2022
ms.topic: overview
---

# What is Windows LAPS?

Windows Local Administrator Password Solution (Windows LAPS) is a Windows feature that automatically manages and backs up the password of a local administrator account on your Azure Active Directory-joined or Windows Server Active Directory-joined devices. You also can use Windows LAPS to automatically manage and back up the Directory Services Restore Mode (DSRM) account password on your Windows Server Active Directory domain controllers. An authorized administrator can retrieve the DSRM password and use it.

## Windows LAPS supported platforms and Azure AD LAPS preview status

Windows LAPS is now available on the following OS platforms with the specified update or later installed:

- [Windows 11 22H2 - April 11 2023 Update](https://support.microsoft.com/help/5025239)
- [Windows 11 21H2 - April 11 2023 Update](https://support.microsoft.com/help/5025224)
- [Windows 10 - April 11 2023 Update](https://support.microsoft.com/help/5025221)
- [Windows Server 2022 - April 11 2023 Update](https://support.microsoft.com/help/5025230)
- [Windows Server 2019 - April 11 2023 Update](https://support.microsoft.com/help/5025229)

The  Windows LAPS on-premises Active Directory scenarios are fully supported as of the above updates.

> [!IMPORTANT]
> The Azure Active Directory LAPS scenario remains in private preview and is closed to new customers. The Azure Active Directory LAPS scenario is scheduled to enter public preview in Q2 2023. This documentation will be updated once a more precise date for the public preview is available.

### Legacy LAPS Interop issues with the April 11 2023 Update

> [!IMPORTANT]
> The April 11, 2023 update has two potential regressions related to interoperability with legacy LAPS scenarios. Please read the following to understand the scenario parameters plus possible workarounds.
>
> Issue #1: If you install the legacy LAPS CSE on a device patched with the April 11, 2023 security update and an applied legacy LAPS policy, both Windows LAPS and legacy LAPS will enter a broken state where neither feature will update the password for the managed account. Symptoms include Windows LAPS event log IDs 10031 and 10033, as well as legacy LAPS event ID 6. Microsoft is working on a fix for this issue.
>
> Two primary workarounds exist for the above issue:
>
> a. Uninstall the legacy LAPS CSE (result: Windows LAPS will take over management of the managed account)
>
> b. [Disable legacy LAPS emulation mode](laps-scenarios-legacy.md#disabling-legacy-microsoft-laps-emulation-mode) (result: legacy LAPS will take over management of the managed account)
>
> Issue #2: If you apply a legacy LAPS policy to a device patched with the April 11, 2023 update, Windows LAPS will immediately enforce\honor the legacy LAPS policy, which may be disruptive (for example if done during OS deployment workflow). [Disable legacy LAPS emulation mode](laps-scenarios-legacy.md#disabling-legacy-microsoft-laps-emulation-mode) may also be used to prevent those issues.

## Benefits of using Windows LAPS

Use Windows LAPS to regularly rotate and manage local administrator account passwords and get these benefits:

- Protection against pass-the-hash and lateral-traversal attacks
- Improved security for remote help desk scenarios
- Ability to sign in to and recover devices that are otherwise inaccessible
- A fine-grained security model (access control lists and optional password encryption) for securing passwords that are stored in Windows Server Active Directory
- Support for the Azure role-based access control model for securing passwords that are stored in Azure Active Directory

Watch this video to learn about Windows LAPS.

>[!Video https://www.youtube.com/embed/jdEDIXm4JgU]

## Key Windows LAPS scenarios

You can use Windows LAPS for several primary scenarios:

- Back up local administrator account passwords to [Azure Active Directory](/azure/active-directory/devices/concept-azure-ad-join) (for Azure Active Directory-joined devices)

- Back up local administrator account passwords to Windows Server Active Directory (for Windows Server Active Directory-joined clients and servers)

- Back up DSRM account passwords to Windows Server Active Directory (for Windows Server Active Directory domain controllers)

- Back up local administrator account passwords to Windows Server Active Directory by using legacy Microsoft LAPS

In each scenario, you can apply different policy settings.

## Understand device join state restrictions

Whether a device is joined to Azure Active Directory or Windows Server Active Directory determines how you can use Windows LAPS.

Devices that are joined only to [Azure Active Directory](/azure/active-directory/devices/concept-azure-ad-join) can back up passwords only to Azure Active Directory.

Devices that are joined only to Windows Server Active Directory can back up passwords only to Windows Server Active Directory.

Devices that are [hybrid-joined](/azure/active-directory/devices/concept-azure-ad-join-hybrid) (joined to both Azure Active Directory and Windows Server Active Directory) can back up their passwords either to Azure Active Directory or to Windows Server Active Directory. You can't back up passwords to both Azure Active Directory and Windows Server Active Directory.

Windows LAPS doesn't support Azure Active Directory workplace-joined clients.

## Set Windows LAPS policy

To set up and manage policy for your Windows LAPS deployment, you have multiple options:

- [Windows LAPS configuration service provider (CSP)](/windows/client-management/mdm/laps-csp)
- [Windows LAPS Group Policy](laps-management-policy-settings.md#windows-laps-group-policy)
- [Legacy Microsoft LAPS](https://www.microsoft.com/download/details.aspx?id=46899)

## Manage and monitor Windows LAPS

You also have various options to manage and monitor Windows LAPS.

Options for Windows include:

- The Windows Server Active Directory Users and Computers properties dialog
- A dedicated event log channel
- A Windows PowerShell module that's specific to Windows LAPS

Azure-based monitoring and reporting solutions are available when you back up passwords to Azure Active Directory.

## Windows LAPS vs. legacy Microsoft LAPS

You can still download an earlier version of Local Administrator Password Solution, [legacy Microsoft LAPS](https://www.microsoft.com/download/details.aspx?id=46899).

Windows LAPS inherits many design concepts from legacy Microsoft LAPS. If you're familiar with legacy Microsoft LAPS, many Windows LAPS features will be familiar. A key difference is that Windows LAPS is an entirely separate implementation that's native to Windows. Windows LAPS also adds many features that aren't available in legacy Microsoft LAPS. You can use Windows LAPS to back up passwords to Azure Active Directory, encrypt passwords in Windows Server Active Directory, and store your password history.

> [!IMPORTANT]
> Windows LAPS doesn't require you to install legacy Microsoft LAPS. You can fully deploy and use all Windows LAPS features without installing or referring to legacy Microsoft LAPS. But to help migrate an existing legacy Microsoft LAPS deployment, Windows LAPS offers [legacy Microsoft LAPS emulation mode](laps-scenarios-legacy.md).

## See also

[Legacy Microsoft LAPS](https://www.microsoft.com/download/details.aspx?id=46899)

## Next steps

- [Key concepts in Windows LAPS](laps-concepts.md)
- [Get started with Windows LAPS for Windows Server Active Directory](laps-scenarios-windows-server-active-directory.md)
- [Get started with Windows LAPS for Azure Active Directory](laps-scenarios-azure-active-directory.md)
- [Get started with Windows LAPS in legacy Microsoft LAPS emulation mode](laps-scenarios-legacy.md)
