---
title: Build dynamic, lean, universal packages for Microsoft 365 Apps for enterprise
author: manoth-msft
ms.author: manoth
manager: laurawi
audience: ITPro 
ms.topic: article 
ms.service: o365-proplus-itpro
localization_priority: Normal
description: "Field best practices: How to provide ways for your users to automatically install additional language packs, proofing tools, products like Visio and Project, or other components of Microsoft 365 Apps (previously named Office 365 Business or Office 365 ProPlus)"
ms.custom: 
- Ent_Office_ProPlus
- Ent_Office_FieldNotes
ms.collection: 
- Ent_O365
- M365-modern-desktop
---

# Best practices from the field: Build dynamic, lean, and universal packages for Microsoft 365 Apps

> [!NOTE]
> This article was written by Microsoft experts in the field who work with enterprise customers to deploy Office.
   
As an admin, you might have to deploy Microsoft 365 Apps (previously named Office 365 Business or Office 365 ProPlus) in your organization. But such a deployment is more than just Office: After the initial migration to Microsoft 365 Apps, you might have to provide ways for your users to automatically install additional language packs, proofing tools, products like Visio and Project, or other components. We often refer to these scenarios as **2nd installs**, while the initial upgrade to Microsoft 365 Apps from a legacy Office is called **1st install**. For 1st install scenarios, have a look at the [install options](install-options.md) as well as the best way to [right-size your deployment](right-sizing-initial-deployment.md).

This article shows you how to build dynamic, lean, and universal packages for Microsoft 365 Apps. This method can greatly reduce long-term maintenance costs and effort in managed environments.
 
## The challenge!
When you plan your upgrade to Microsoft 365 Apps, the actual upgrade from a legacy version to the always-current Microsoft 365 Apps is front and center (1st install scenario). But looking beyond the initial deployment, there are other scenarios you’ll need to cover as an admin (2nd install). Sometimes, after you upgrade your users, they might need any of the following components:
 
- Additional language packs
- Proofing tools
- Visio
- Project

Historically, each of these scenarios was addressed by creating a dedicated installation package for automatic, controlled installation for users. Usually, an admin would combine the necessary source files (of ~2.5 gigabytes) and a copy of the Office Deployment Tool (ODT) together with a configuration file into a package for each of these components.

But, especially in larger organizations, you often don't have a single configuration set of Microsoft 365 Apps. You might have a mix of update channels, often Semi-Annual Enterprise Channel and Semi-Annual Enterprise Channel (Preview). And maybe you're currently transitioning from 32-bit to 64-bit, and maybe you'll have to support both architectures for quite some time.

So in the end, you wouldn't have *1* package per component but *4*, covering each possible permutation of Semi-Annual Enterprise Channel/Semi-Annual Enterprise Channel (Preview) and x86/x64. The end result would be:

- A large number of packages. The 4 listed components would result in 16 or more packages.
- High-bandwidth consumption, as a client might get the full 2.5-GB package pushed down before installation.
- High maintenance costs to keep embedded source files current.
- High user impact, if you haven’t kept the source files current and installing a component will perform a downgrade just to perform an update to the current version soon after.
- Low satisfaction for users who have to pick the matching package from many options presented in the software portal.
 
While the initial upgrade to Microsoft 365 Apps is a one-time activity, the scenarios described previously will be applicable over a longer period. Users might need additional components days, weeks, or even years after the initial deployment.

So, how do you build packages that are less costly to maintain over a long time frame and avoid the downsides?

## The solution: Dynamic, lean, and universal packages

You can resolve these issues by implementing self-adjusting, small, and universal packages. Let's cover the basic concepts before we dive into sample scenarios.

Build **dynamic** packages where you don’t hard-code anything. Use features of the [Office Deployment Tool (ODT)](https://go.microsoft.com/fwlink/p/?LinkID=626065) to enable the packages to self-adjust to the requirements:
- Use [Version=MatchInstalled](https://techcommunity.microsoft.com/t5/Office-365-Blog/New-feature-Make-changes-to-Office-deployments-without-changing/ba-p/816482) to prevent unexpected updates and stay in control of the version installed on a client. No hard coding of a build number, which gets outdated quickly, is required.
- Use [Language=MatchInstalled](https://techcommunity.microsoft.com/t5/Office-365-ProPlus/Dynamically-match-already-existing-languages-when-installing/m-p/716927) to instruct e.g. Visio or Project to install with the same set of languages as Office is already using. No need to list them or build a script that injects the required languages.
 
Build **lean** packages by removing the source files from the packages. This has multiple benefits:

- Package size is much smaller, from 2.5 GB down to less than 10 megabytes for the ODT and its configuration file.
- Instead of pushing a 2.5-GB install package to clients, you let clients pull what they need on demand from Office Content Delivery Network (CDN), which saves bandwidth.
   - When you add Project to an existing Microsoft 365 Apps installation, you need to download less than 50 megabytes, as Office shared components are already installed.
   - Visio installs are typically 100-200 megabytes, based on the number of languages, as the templates/stencils are a substantial part of the download.
   - Installing proofing tools is typically 30-50 megabytes, versus a full language pack, which is 200-300 megabytes.
- A second install scenario is often less frequent, which lowers the internet traffic burden, ultimately reducing the impact.
- You don’t have to update the source files every time Microsoft releases new features or security and quality fixes.
 
Build **universal** packages by not hard coding things like the architecture or update channel. ODT will dynamically match the existing install, so your packages work across all update channels and architectures. Instead of having 4 packages to install Visio, for example, you'll have a single, universal package that will work across all permutations of update channels and architectures.
- Leaving out [OfficeClientEdition](https://techcommunity.microsoft.com/t5/Office-365-ProPlus/Insights-into-OfficeClientEdition-and-how-to-make-it-work-for/m-p/767577) makes your package universal for mixed x86/x64 environments.
- Leaving out [Channel](https://techcommunity.microsoft.com/t5/Office-365-ProPlus/Understanding-the-Channel-attribute-of-the-Office-Deployment/m-p/813604) makes your package universal across update channels.
 
## How to build and benefit from building dynamic, lean, and universal packages

The idea is to not hard code everything in the configuration file, but to instead utilize the cleverness of the Office Deployment Tool as much as possible.

Let’s have a look at a "classic" package that was built to add Project to an existing install of Microsoft 365 Apps. We have the source files (of ~2.5 gigabytes) and a configuration file, which explicitly states what we want to achieve:

![Sample package](../images/lean5-pic1.jpg)

```xml
<Configuration>
 <Add OfficeClientEdition="64" Channel="SemiAnnual">
  <Product ID="ProjectProRetail">
   <Language ID="en-us" />
  </Product>
 </Add>
 <Display Level="None" />
</Configuration>
```
 
When we apply the concepts of dynamic, lean, and universal packages, the result would look like this:
 
![Lean sample package](../images/lean5-pic2.jpg)

```xml
<Configuration>
 <Add Version="MatchInstalled">
  <Product ID="ProjectProRetail">
   <Language ID="MatchInstalled" TargetProduct="O365ProPlusRetail" />
  </Product>
 </Add>
 <Display Level="None" />
</Configuration>
```

So what have we changed, and what are the benefits?

- We removed the OfficeClientEdition-attribute, as the ODT will automatically match the installed version.
   - Benefit: The configuration file now works for both x86 and x64 scenarios.
- We removed the channel for the same reason. ODT will automatically match the already-assigned update channel.
   - Benefit I: The package works for all update channels (Current Channel, Monthly Enterprise Channel, Semi-Annual Enterprise Channel, and others).
   - Benefit II: It also works for update channels you don’t offer as central IT. Some users are running Current Channel, some are on Insider builds? Don’t worry, it just works.
- We added *Version=MatchInstalled*, which ensures that ODT will install the same version that's already installed.
   - Benefit: You're in control of versions deployed, with no unexpected updates.
- We added *Language ID="MatchInstalled"*  and *TargetProduct* to match the currently installed languages, replacing a hard-coded list of languages to install.
   - Benefit I: The user will have the same languages for Project as were already installed for Office.
   - Benefit II: No need to re-request language pack installs.
   - Benefit III: Also works for rarely used languages that you as the central IT admin don’t offer, which makes users happy.
- We removed the source files. The ODT will fetch the correct set of source files from the Office CDN just in time.
   - Benefit I: The package never gets outdated. No maintenance of source files is needed.
   - Benefit II: The download is about 50 megabytes instead of about 2.5 GB.
 
## Another example: Add language packs and proofing tools the dynamic, lean, and  universal way

Let’s have a brief look at other scenarios as well, like adding language packs and proofing tools. The classic configuration file to install the German Language Pack might look like this:
 
```xml
<Configuration>
 <Add OfficeClientEdition="64" Channel="SemiAnnual">
  <Product ID="LanguagePack">
   <Language ID="de-de" />
  </Product>
 </Add>
 <Display Level="None" />
</Configuration>
```

If you’re running Semi-Annual Enterprise Channel as well as Semi-Annual Enterprise Channel (Preview) and have an x86/x64 mixed environment, you'd need three additional files to cover the remaining configuration permutations. Or, you just go the dynamic, lean, and universal way:

```xml
<Configuration>
 <Add Version="MatchInstalled">
  <Product ID="LanguagePack">
   <Language ID="de-de" />
  </Product>
 </Add>
 <Display Level="None" />
</Configuration>
```
 
This single configuration file will work across x86/x64 and all update channels, such as Current Channel, Monthly Enterprise Channel, Semi-Annual Enterprise Channel, and others. So, if you want to offer five additional languages in your environment, just build five of these "config file + ODT" packages. For proofing tools, you just change the ProductID to "ProofingTools".

## Build your own configuration

The above concept is universally applicable to all Click-To-Run-based installations and products, as long as the ODT is used. You can change the specified Product ID to your scenario. Please check out the [list of supported Product IDs](https://docs.microsoft.com/office365/troubleshoot/installation/product-ids-supported-office-deployment-click-to-run) for more information.

## Prerequisites/Notes

Here are some prerequisites you must meet to make this concept work in your environment and some notes:
- Use [Office Deployment Tool](https://go.microsoft.com/fwlink/p/?LinkID=626065) 16.0.11615.33602 or later to enable *Version=MatchInstalled* to work.
- The ODT must be able to locate the matching source files on the Office CDN.
- Make sure that the context you're using for running the install can traverse the proxy. For details, see [Office 365 ProPlus Deployment and Proxy Server Guidance](https://techcommunity.microsoft.com/t5/Office-365-Blog/Office-365-ProPlus-Deployment-and-Proxy-Server-Guidance/ba-p/849164).
- Make sure that the account (user or system) that's used to install the apps can connect to the internet.
- The tailored configuration files shown above are good for installing the products (with the /configure switch), but do not work with the /download switch. This is expected, as the ODT is missing some details to perform a download (like architecture). For the above concept, there is no need to download the files beforehand.
