---
title: "Tools to manage volume activation of Office"
ms.author: danbrown
author: DHB-MSFT
manager: laurawi
audience: ITPro
ms.topic: get-started-article
ms.prod: office-perpetual-itpro
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Ent_Office_VL
description: "Provides Office admins with informaiton about the ospp.vbs and the slmgr.vbs scripts, and the Volume Activation Management Tool (VAMT)."
---

# Tools to manage volume activation of Office

 ***Applies to:*** *Volume licensed versions of Office 2019 and Office 2016, including Project and Visio*

  
The Office Software Protection Platform script (ospp.vbs), the Software License Manager script (slmgr.vbs), and the Volume Activation Management Tool (VAMT) can help you configure and test volume licensed versions of Office, including Project and Visio. Before you read this article, we recommend that you read [Overview of volume activation of Office](plan-volume-activation-of-office.md), [Activate volume licensed versions of Office by using MAK](activate-office-by-using-mak.md), and [Activate volume licensed versions of Office by using Active Directory](activate-office-by-using-active-directory.md).
  
  
<a name="ospp"> </a>
  
## The ospp.vbs script


The Office Software Protection Platform script (ospp.vbs) enables you to configure volume licensed versions of Office products, including Project and Visio. The opss.vbs script is located in the Program Files\Microsoft Office\Office16 folder. If you installed the 32-bit version of Office on a 64-bit operating system, go to the Program Files (x86)\Microsoft Office\Office16 folder. 
  
Running ospp.vbs requires the cscript.exe script engine. To see the Help file, type the following command, and then press ENTER:
  
 **cscript ospp.vbs /?**
  
The general syntax is as follows:
  
 **cscript ospp.vbs [Option:Value] [ComputerName] [User] [Password]**
  
 **Option** Specifies the option and value to use to activate a product, install or uninstall a product key, install and display license information, set KMS host name and port, and remove KMS host name and port. The options and values are listed in the tables in this section. 
  
 **ComputerName** Name of the remote computer. If a computer name is not provided, the local computer is used. 
  
 **User** Account that has the required permission on the remote computer. 
  
 **Password** Password for the account. If a user account and password are not provided, the current credentials are used. 
  
> [!IMPORTANT]
>  Before you run the ospp.vbs, ensure that:
> - If you want to run the script on a remote computer, the Windows firewall allows Windows Management Instrumentation (WMI) traffic on the remote computer. 
> - The user account you will use is a member of the Administrators group on the computer on which you run the script. 
> - You run ospp.vbs script from an elevated command prompt. 
  
**Global options for ospp.vbs**

|**Global option**|**Description**|
|:-----|:-----|
| _/act_ <br/> |Activates installed Office product keys.  <br/> |
| _/inpkey:value_ <br/> |Installs a product key (replaces existing key) with a user-provided product key. Value option is required.  <br/> |
| _/unpkey:value_ <br/> |Uninstalls an installed product key with the last five digits of the product key to uninstall (as displayed by the  _/dstatus_ option). Value parameter is required.  <br/> |
| _/inslic:value_ <br/> |Installs a license with user-provided path of the .xrm-ms license. Value parameter is required.  <br/> |
| _/dstatus_ <br/> |Displays license information for installed product keys.  <br/> |
| _/dstatusall_ <br/> |Displays license information for all installed licenses.  <br/> |
| _/dhistoryacterr_ <br/> |Displays the failure history for MAK/retail activation.  <br/> |
| _/dinstid_ <br/> |Displays Installation ID for offline activation.  <br/> |
| _/actcid:value_ <br/> |Activates product with user-provided Confirmation ID. Value parameter is required.  <br/> |
| _/rearm_ <br/> |Resets the licensing status for all installed Office product keys.  <br/> |
| _/rearm:value_ <br/> |Resets the licensing status for an Office license with a user-provided SKUID value. Value parameter is required.  <br/><br/> Use this option with the SKUID value specified by using the  _/dstatus_ option if you have run out of rearms and have activated Office through KMS or Active Directory-based activation to gain an additional rearm.  <br/> |
| _/ddescr:value_ <br/> |Displays the description for a user-provided error code. Value parameter is required.  <br/> |
   
The following table describes the ospp.vbs options for configuring the KMS client.
  
**KMS client options for ospp.vbs**

|**KMS client option**|**Description**|
|:-----|:-----|
| _/dhistorykms_ <br/> |Displays KMS client activation history.  <br/> |
| _/dcmid_ <br/> |Displays KMS client computer ID (CMID).  <br/> |
| _/sethst:value_ <br/> |Sets a KMS host name with a user-provided host name. Value parameter is required.  <br/> <br/>This sets **HKLM\Software\Microsoft\OfficeSoftwareProtectionPlatform\KeyManagementServiceName (REG_SZ)**.  <br/> |
| _/setprt:value_ <br/> |Sets a KMS port with a user-provided port number. The default port number is 1688. Value parameter is required.  <br/><br/> This sets **HKLM\Software\Microsoft\OfficeSoftwareProtectionPlatform\KeyManagementServicePort (REG_SZ)**.  <br/> |
| _/remhst_ <br/> |Removes KMS host name and sets port to default. The default port is 1688.  <br/> |
| _/cachst:value_ <br/> |Allows or denies KMS host caching. Value parameter is required (TRUE or FALSE).  <br/> |
| _/actype:value_ <br/> |(Windows 8.1 and later versions only) Sets volume activation type. Value parameter is required.  <br/><br/> Values: 1 (for Active Directory-based), 2 (for KMS), 0 (for both).  <br/> |
| _/skms-domain:value_ <br/> |(Windows 8.1 and later versions only) Sets the specific DNS domain in which all KMS SRV records can be found. This setting has no effect if the specific single KMS host name is set by the  _/sethst_ option. Value parameter is the Fully Qualified Domain Name (FQDN) and is required.  <br/> |
| _/ckms-domain_ <br/> |(Windows 8.1 and later versions only) Clears the specific DNS domain in which all KMS SRV records can be found. The specific KMS host is used if it is set by the  _/sethst_ option. Otherwise, auto-discovery of the KMS host is used.  <br/> |
   
### Scenarios that use ospp.vbs

The following scenarios assume that you are running ospp.vbs in an elevated command prompt.
  
> [!NOTE]
> Any changes that you make affect only the Office client products. You have to configure the Windows client separately by using the **slmgr.vbs** script described in [The slmgr.vbs script](tools-to-manage-volume-activation-of-office.md#BK_Theslmgrscript) later in this article. 
  
### To change the product key to a MAK key on a remote computer

1. If the remote computer is named contoso1, run the following command to enter the product key. This assumes that you have administrator credentials on the remote computer. If your credentials on the remote computer differ from your logon name and password, you must type your logon name and password at the command line, and then press ENTER:
    
    **cscript ospp.vbs /inpkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx contoso1**
    
2. To activate the remote computer, type the following command, and then press ENTER: 
    
    **cscript ospp.vbs /act contoso1**
    
### To diagnose KMS activation errors

1. If the computer has the KMS client key installed, check the licensing status. To do this, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /dstatusall**
    
2. To check the KMS activation history, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /dhistorykms**
    
3. To trigger activation, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /act**
    
4. Check the error message for error code 0xC004F042. If in the notification dialog boxes you see an error code that is shown to users, you can also use that code to check the error message. To do this, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /ddescr:0xC004F042**
    
### To turn on or off KMS host caching on the KMS client

1. On the KMS client computer, to turn caching on, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /cachst:TRUE**
    
2. On the KMS client computer, to turn caching off, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /cachst:FALSE**
    
### To test KMS host name, and then set to auto-discovery

1. If you are testing a KMS host named kmstest.contoso.com, and you want to specify it in the KMS client, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /sethst:kmstest.contoso.com**
    
    > [!NOTE]
    > If you change the default port on the KMS host computer, you must run **ospp.vbs** with the  _/setprt_ option. > For example: **cscript ospp.vbs /setprt:1750**
  
2. To trigger activation, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /act**
    
3. To check the KMS activation history to see whether the host was successfully contacted, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /dhistorykms**
    
4. Now you want this KMS client to use Domain Name System (DNS) for auto-discovery of the production KMS host. To remove the KMS host name, type the following command, and then press ENTER:
    
    **cscript ospp.vbs /remhst**
    
<a name="BK_Theslmgrscript"> </a>

## The slmgr.vbs script

The Software License Manager (slmgr.vbs) script is used to configure and retrieve volume activation information for the Office KMS host and the Windows host. For more information, see [Configuring KMS Hosts](https://go.microsoft.com/fwlink/p/?LinkID=311942) for Windows. 
  
You can run the script locally on the target computer or remotely from another computer. You have to be an administrator to use the script. If a standard user runs **slmgr.vbs**, some license data might be missing or incorrect, and many operations are prohibited. 
  
The slmgr.vbs script can use the Windows-based script host, wscript.exe, or the command-based script host, cscript.exe, and administrators can specify which script engine to use. If no script engine is specified, slmgr.vbs runs by using the default script engine, wscript.exe. We recommend the cscript.exe script engine.
  
You must restart the Software Licensing Service for any changes to take effect. To restart the Software Licensing Service, use the Microsoft Management Console (MMC) Services snap-in or run the following command:
  
 **net stop sppsvc &amp;&amp; net start sppsvc**
  
The slmgr.vbs script requires at least one parameter. If you run the script without any parameters, it displays Help information. The following table lists slmgr.vbs command-line options, and a description of each. Most of the parameters in the table configure the KMS host computer. However, the parameters **/sai** and **/sri** are passed to KMS clients after the KMS clients contact the KMS host computer. The general syntax of slmgr.vbs is as follows (assuming you are using the cscript.exe as the script engine): 
  
 **cscript slmgr.vbs /parameter**
  
 **cscript slmgr.vbs [ComputerName] [User] [Password] [Option]**
  
 **ComputerName** Name of the remote computer. If you do not provide a computer name, the local computer is used. 
  
 **User** Account with required permission on the remote computer. 
  
 **Password** Password for the account. If you do not provide a user account and password, current credentials are used. 
  
 **Option** Options are shown in the following table. 
  
**Slmgr.vbs command options — standard**

|**Option**|**Description**|
|:-----|:-----|
|/ _ipk [ProductKey]_ <br/> |Installs the product key for Windows (default) or other application identified by the product key.  <br/> |
|/ _ato [ActivationID]_ <br/> |Activates the KMS host for Windows (default), or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7.  <br/> |
|/ _dti [ActivationID]_ <br/> |Displays the Installation ID that is used for telephone activation of the KMS host computer for Windows (default) or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7. You enter the Installation ID into the telephone to receive the Confirmation ID, which is used for activating the KMS host computer by using the  _/atp_ parameter.  <br/> |
|/ _atp [ConfirmationID][ActivationID]_ <br/> |After the Confirmation ID is received, activates the KMS host for Windows (default) or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7.  <br/> |
|/ _dlv [ActivationID]_ <br/> |Displays detailed license information for Windows (default), or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7.  <br/> <br/>For example, run the following command at the elevated command prompt, in the C:\windows\system32 folder to get the status of the Office KMS host: <br/> <br/> cscript slmgr.vbs /dlv 70512334-47B4-44DB-A233-BE5EA33B914C <br/> |
|/ _dli [ActivationID]_ <br/> |Displays license information for Windows (default), or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7.  <br/> |
|/ _upk [ActivationID]_ <br/> |Uninstalls the product key for Windows (default) or the application that is identified when its Activation ID is provided. The Office 2019 Activation ID is 70512334-47B4-44DB-A233-BE5EA33B914C, and the Office 2016 Activation ID is 98EBFE73-2084-4C97-932C-C0CD1643BEA7.  <br/> <br/> **CAUTION:** If you intend to uninstall the product key for Office, for example, and forget to enter the Activation ID, all installed product keys are uninstalled. This includes the product key for Windows.           |
|/ _xpr [ActivationID]_ <br/> |Displays the expiration date for the current license state.  <br/> |
|/ _sprt [PortNumber]_ <br/> |Sets the TCP communications port on a KMS host computer. It replaces  _PortNumber_ with the TCP port number to use. The default setting is **1688**.  <br/> |
|/ _cdns_ <br/> |Disables automatic DNS publishing by a KMS host computer.  <br/> |
|/ _sdns_ <br/> |Enables automatic DNS publishing by the KMS host computer.  <br/> |
|/ _cpri_ <br/> |Lowers the priority of KMS host computer processes.  <br/> |
|/ _sai [ActivationInterval]_ <br/> |Changes how often a KMS client attempts to activate itself when it cannot find a KMS host computer. It replaces  _ActivationInterval_ with a value expressed in minutes. The default setting is **120** minutes.  <br/> |
|/ _sri [RenewalInterval]_ <br/> |Changes how often a KMS client attempts to renew its activation by contacting a KMS host computer. It replaces  _RenewalInterval_ with a value expressed in minutes. The default setting is **10080** minutes (7 days). This setting overrides the local KMS client settings.  <br/> |
   
The following table shows the command options for activating an AD DS forest by using Active Directory-based activation.
  
**Slmgr.vbs command options — Active Directory-based activation**

|**Option**|**Description**|
|:-----|:-----|
|/ _ad-activation-online [ProductKey]_ <br/> |Activates an AD DS forest through the user-provided product key.  <br/> |
|/ _ad-activation-apply-get-iid [ProductKey]_ <br/> |Displays installation ID for AD DS forest.  <br/> |
|/ _ad-activation-apply-cid [ProductKey][ConfirmationID]_ <br/> |Activates an AD DS forest through the user-provided product key and the Confirmation ID.  <br/> |
   
<a name="vamt"> </a>

## Volume Activation Management Tool (VAMT)

Volume Activation Management Tool (VAMT) provides a graphical user interface to manage volume activation. For more information about VAMT, see [Volume Activation Management Tool (VAMT) Technical Reference](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-management-tool).

> [!NOTE]
> Office 2019 requires at least VAMT 3.1.
  
## Related topics

- [Overview of volume activation of Office](plan-volume-activation-of-office.md)
- [Activate volume licensed versions of Office by using MAK](activate-office-by-using-mak.md)
- [Activate volume licensed versions of Office by using Active Directory](activate-office-by-using-active-directory.md)

