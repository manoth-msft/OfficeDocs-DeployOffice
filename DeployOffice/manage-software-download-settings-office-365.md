---
title: "Manage software download settings in Office 365"
ms.author: danbrown
author: DHB-MSFT
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: o365-proplus-itpro
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Ent_Office_ProPlus
description: "Provides Office 365 admins with information to help them configure software download settings in the admin center."
---


# Manage software download settings in Office 365

As an Office 365 admin, you can control which Office software your users can download and install from Office 365. The choices you make on the **Office software download settings** page determine which software users can install from the **My account** > **Apps & devices** page in Office 365. Whichever choices you make, they apply to all users in your organization.

> [!TIP]
> To get to the **Office software download settings** page, sign in to the Microsoft 365 admin center with your admin account, and then go to **Show all** > **Settings** > **Org settings** > **Services**.
  
If you choose to make some software unavailable to users, they see a message on their **My account** > **Apps & devices** page instead of an **Install** button. For example, if you choose to make Office unavailable, users see this message: 
  
*Your admin has turned off Office installs. Contact your admin for more information about how to get Office in your organization.*
  
## Choose how often to get feature updates for Office

For Microsoft 365 Apps, you must choose how often you want users to get feature updates. For example, users can get new features to Microsoft 365 Apps as soon as they're ready, or once a month (on the second Tuesday of the month), or twice a year (in January and July, on the second Tuesday).
  
If you want some users to get feature updates every month, but have other users get feature updates only every six months, you can manually download and install the Office apps for your users by using the Office Deployment Tool. This gives you the control to select different feature update settings for different groups of users.
  
For more information, [Overview of update channels for Microsoft 365 Apps](overview-update-channels.md).
  
## Manually download and install the Office apps by using the Office Deployment Tool
If you don't want your users to install software themselves from Office 365, you can download the software to your local network. Then, you can deploy the software to your users by using your existing software deployment tools and processes.
  
To manually deploy Office, as well as Project or Visio, you need to use the Office Deployment Tool. You can download the Office Deployment Tool for free from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkID=626065).
  
You use the Office Deployment Tool to both download and deploy Office, such as Microsoft 365 Apps, to your users. If you already use other tools, such as Microsoft Endpoint Configuration Manager, to deploy software to your users, you can use the Office Deployment Tool along with those tools to deploy Office.
  
For more information, see [Overview of the Office Deployment Tool](overview-office-deployment-tool.md).
  
## Manually download InfoPath 2013 and SharePoint Designer 2013
In addition to Office, you can download InfoPath 2013 and SharePoint Designer 2013 in order to deploy them to your users. You can download them from the Microsoft Download Center by using the following links:
  
- [InfoPath 2013](https://go.microsoft.com/fwlink/p/?LinkID=626623) (32-bit and 64-bit versions) 
    
- [SharePoint Designer 2013](https://go.microsoft.com/fwlink/p/?LinkID=626622) (32-bit and 64-bit versions) 
    
After you download the software, you can use your existing software deployment tools and processes to deploy InfoPath or SharePoint Designer to your users.
  
## Additional considerations about managing software download settings in Office 365
- The list of Office software that appears on the **Office software download settings** page depends on the type of Office 365 (or Microsoft 365) plan that your organization has. 
    
- Users have to be local administrators on their computers to install Office software. If users aren't local administrators, you'll have to install Office for them.
    
- For users to be able to install Office, you need to [assign them a license](https://docs.microsoft.com/microsoft-365/admin/manage/assign-licenses-to-users).
    
- By default, Office installs all the apps that are included in your version of Office on the user's computer. If you want some users to get fewer apps, such as only Word and PowerPoint, you need to use the Office Deployment Tool to deploy Office to your users without those apps.
