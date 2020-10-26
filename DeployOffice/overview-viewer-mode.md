---
title: "Overview of viewer mode for Microsoft 365 Apps"
ms.author: mattphil
author: MJP-MSFT
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: o365-proplus-itpro
localization_priority: Normal
ms.collection: 
- Ent_O365
- M365-modern-desktop
ms.custom: Ent_Office_ProPlus
description: "Overview of viewer mode for Microsoft 365 Apps."
---

# Overview of viewer mode for Microsoft 365 Apps

Viewer mode allows unlicensed users of Microsoft 365 Apps to view and print Office files without being prompted to activate Office. To use viewer mode, you must use a supported version of Microsoft 365 Apps for enterprise.

Viewer mode is designed for users who need to view or print Office files without creating or editing. For example, a doctor or nurse can use this mode to open files in Office from a health-care line-of-business application. Viewer mode is a benefit to users of Office for the web.

When viewer mode is enabled, an unlicensed user of Microsoft 365 Apps for enterprise is not prompted to activate Office when they open Word, Excel, or PowerPoint. Instead, they see a banner message underneath the ribbon in their Office apps that informs them Office is in viewer mode:

:::image type="content" source="images/viewermode2.png" alt-text="Viewer mode enabled in Word":::

:::image type="content" source="images/viewermode1.png" alt-text="Viewer mode enabled in Microsoft 365 Apps":::

## Enable viewer mode with Group Policy

To enable viewer mode for unlicensed users in your organization with Group Policy:

- Enable the **Use Viewer Mode** policy setting. You can find this policy setting under Computer Configuration\Policies\Administrative Templates\Microsoft Office 2016 (Machine)\Licensing Settings.

- Disable the **Automatically activate Office with federated organization credentials** policy setting. You can find this setting under User Configuration\Policies\Administrative Templates\Microsoft Office 2016\Subscription Activation. By disabling this setting, you ensure that Office is not automatically activated with federated organization credentials.

To use these policy settings, download and install the [Administrative Template files (ADMX/ADML) for Office](https://www.microsoft.com/download/details.aspx?id=49030) from the Microsoft Download Center.

## Considerations when using viewer mode

- Viewer mode is currently supported for Word, Excel, PowerPoint(Version 1902 or later), Project and Visio (Version 2005 or later). 
- Updates, such as new features and security updates, are still applied when Office is in viewer mode.
- For information about the availability of specific versions in the update channels for Microsoft 365 Apps for enterprise, see [Update history for Microsoft 365 Apps](https://docs.microsoft.com/officeupdates/update-history-microsoft365-apps-by-date).
- The Activate Product button under File > Account is hidden when Office is in viewer mode.
- Even when viewer mode is enabled, users need to accept the license agreement when Office is opened for the first time. You can automatically accept the license agreement for your users when you deploy Office by configuring the [AcceptEULA attribute](office-deployment-tool-configuration-options.md#accepteula-attribute-part-of-display-element).
