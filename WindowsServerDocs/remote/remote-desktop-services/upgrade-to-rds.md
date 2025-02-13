---
title: Upgrading Remote Desktop Services deployments
description: Learn how to upgrade your existing Remote Desktop Services deployments.
ms.author: spatnaik
ms.date: 03/07/2023
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
---
# Upgrading Remote Desktop Services deployments

>Applies to: Windows Server 2022, Windows Server 2019, and Windows Server 2016

In this article, learn about which Remote Desktop Services (RDS) versions can be upgraded and
the suggested order to upgrade your Remote Desktop (RD) role services.

## Supported OS upgrades with RDS role installed

Beginning with Windows Server 2012 R2, you can upgrade to a newer version of Windows Server by two versions at a time. For example, to upgrade to Windows Server 2022 from Windows Server 2012 R2, you would first need to upgrade to Windows Server 2016 or Windows Server 2019.

## Flow for deployment upgrades

In order to keep the down-time to a minimum, using the follow as a guide:

1. **RD Connection Broker servers** should be the first to be upgraded. If you have an active/active configuration, remove all but one server from the deployment and perform an in-place upgrade. Perform upgrades on the remaining RD Connection Broker servers offline and then readd them to the deployment. The deployment isn't available during the RD Connection Broker servers' upgrade.

   > [!NOTE]
   > It is mandatory to upgrade all RD Connection Broker servers. We do not support Windows Server RD Connection Broker servers in a mixed deployment. Once the RD Connection Broker server(s) are running a new Windows Server version the deployment will remain functional, even if the rest of the servers in the deployment are still running the previous version.

2. **RD License servers** should be upgraded before you upgrade your RD Session Host servers.
   > [!NOTE]
   > A RD license server from an older version of Windows Server will work with newer versions, but they can only process CALs from the older Windows Server version. They cannot use the newer Windows Server CALs. For more information about RD license servers, see [RDS CAL version compatibility](rds-client-access-license.md#rds-cal-version-compatibility).

3. **RD Session Host servers** can be upgraded next. Avoid down time during upgrade by splitting the servers to be upgraded in two steps as detailed. All will be functional after the upgrade. To upgrade, use the steps described in [Upgrading Remote Desktop Session Host servers](upgrade-to-rdsh.md).

4. **RD Virtualization Host servers** can be upgraded next. To upgrade, use the steps described in [Upgrading Remote Desktop Virtualization Host servers](upgrade-to-rdvh.md).

5. **RD Web Access servers** can be upgraded anytime.
   > [!NOTE]
   >
   > - Upgrading RD Web may reset IIS properties (such as any configuration files). To not lose your changes, make notes or copies of customizations done to the RD Web site in IIS.
   > - RD Web Access servers from an older version of Windows Server will work with newer versions.

6. **RD Gateway servers** can be upgraded anytime.
   > [!NOTE]
   >
   > - Windows Server 2016 and later does not include Network Access Protection (NAP) policies - they will have to be removed. The easiest way to remove the correct policies is by running the upgrade wizard. If there are any NAP policies you must delete, the upgrade will block and create a text file on the desktop that includes the specific policies. To manage NAP policies, open the Network Policy Server tool. After deleting them, click **Refresh** in the Setup tool to continue with the upgrade process.
   > - RD Gateway servers from an older version of Windows Server will work with newer versions.

## VDI deployment – supported guest OS upgrade

Administrators have the following options to upgrade of VM collections:

### Upgrade Managed Shared VM collections

Administrators need to create VM templates with the desired OS version and use it to patch all the VMs in the pool.

We support the following patching scenarios:

- Windows 10 can be patched to Windows 11

### Upgrade unmanaged shared VM collections

End users can't upgrade their personal desktops. Administrators should perform the upgrade. The exact steps are still to be determined.

## Known issues

**Issue:** If the RD deployment has RD Web Access (RDWA) Role already installed and has been upgraded from a previous windows installation, a new upgrade might fail. For example, if the deployment containing RDWA upgraded from Server 2012 R2 to Server 2019, another upgrade to Server 2022 might encounter a failure.

**Workaround:** Before migrating for the second time, check if the following registry key is present:
`HKLM\SOFTWARE\Microsoft\Terminal Server Web Access\IsInstalled`

If it isn't present, Open an elevated PowerShell prompt, then run the following commands:

  ```powershell
  $registryPath = "HKLM:SOFTWARE\Microsoft\Terminal Server Web Access\IsInstalled"
  New-Item -Path $registryPath
  New-ItemProperty -Path $registryPath  -Name Version -PropertyType String -Value "6.0"
  ```
