---
title:  December 2023 OS security update (KB 5033383) for Azure Stack HCI, version 23H2
description: Read about the December 2023 OS security update (KB 5033383) for Azure Stack HCI, version 23H2.
author: alkohli
ms.topic: conceptual
ms.date: 03/20/2024
ms.author: alkohli
ms.reviewer: alkohli
---

# December 2023 OS security update (KB 5033383) for Azure Stack HCI, version 23H2

[!INCLUDE [applies-to](../includes/hci-applies-to-23h2.md)]

This article describes the security update for Azure Stack HCI, version 23H2 that was released on December 12, 2023 and applies to OS build 25398.584.

<!--For an overview of Azure Stack HCI version 23H2 release notes, see the [update history](https://support.microsoft.com/topic/release-notes-for-azure-stack-hci-version-23h2-018b9b10-a75b-4ad7-b9d1-7755f81e5b0b).-->

## Improvements

This security update includes quality improvements. When you install this KB:

- This update changes the English name of the former Republic of Turkey. The new, official name is the Republic of Türkiye.

- This update supports the currency change in Croatia from the Kuna to the Euro.

- This update affects the Netherlands time zone. It adds the recent artificial landmass outside of Rotterdam to the shape files.

For more information about security vulnerabilities, see the [Security Update Guide](https://msrc.microsoft.com/update-guide/) and the [December 2023 Security Updates](https://msrc.microsoft.com/update-guide/releaseNote/2023-Dec).

<!--## Servicing stack update - 25398.521

This update makes quality improvements to the servicing stack, which is the component that installs Windows updates. Servicing stack updates (SSU) ensure that you have a robust and reliable servicing stack so that your devices can receive and install Microsoft updates.-->

## Known issues

Microsoft isn't currently aware of any issues with this update.

## To install this update

Microsoft now combines the latest servicing stack update (SSU) for your operating system with the latest cumulative update (LCU). For general information about SSUs, see [Servicing stack updates](/windows/deployment/update/servicing-stack-updates) and [Servicing Stack Updates (SSU): Frequently Asked Questions](https://support.microsoft.com/topic/servicing-stack-updates-ssu-frequently-asked-questions-06b62771-1cb0-368c-09cf-87c4efc4f2fe).

<!--To install the LCU on your Azure Stack HCI cluster, see [Update Azure Stack HCI via PowerShell](../update/update-via-powershell-23h2.md).

| Release Channel | Available | Next Step |
|----|----|----|
| Windows Update and Microsoft Update | Yes | None. This update is downloaded automatically from Windows Update. |
| Windows Update for Business | Yes | None. This update is downloaded automatically from Windows Update in accordance with configured policies. |
| Microsoft Update Catalog | Yes | To get the standalone package for this update, go to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=KB5032202). |
| Windows Server Update Services (WSUS) | Yes | This update automatically syncs with WSUS if you configure Products and Classifications as follows:<br>**Product**: Azure Stack HCI<br>**Classification**: Security Updates |-->

<!--## To remove the LCU

To remove the LCU after installing the combined SSU and LCU package, use the DISM [Remove-WindowsPackage](/powershell/module/dism/remove-windowspackage) command line option with the LCU package name as the argument. You can find the package name by using this command: `DISM /online /get-packages`.

Running [Windows Update Standalone Installer](https://support.microsoft.com/topic/description-of-the-windows-update-standalone-installer-in-windows-799ba3df-ec7e-b05e-ee13-1cdae8f23b19) (wusa.exe) with the `/uninstall` switch on the combined package won't work because the combined package contains the SSU. You can't remove the SSU from the system after installation.-->

## File list

For a list of the files that are provided in this update, download the file information for [cumulative update 5033383](https://go.microsoft.com/fwlink/?linkid=2255814).

<!--For a list of the files that are provided in the servicing stack update, download the file information for the [SSU - version 25398.521](https://go.microsoft.com/fwlink/?linkid=2252176).-->

## Next steps

- [Install updates via PowerShell](../update/update-via-powershell-23h2.md) for Azure Stack HCI, version 23H2.
- [Install updates via Azure Update Manager in Azure portal](../update/azure-update-manager-23h2.md) for Azure Stack HCI, version 23H2.