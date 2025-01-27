---
title: Windows Update common errors and mitigation
description: Learn about some common issues you might experience with Windows Update
ms.prod: w10
ms.mktglfcycl: 
ms.sitesec: library
author: lomayor
ms.localizationpriority: medium
ms.author: lomayor
ms.date: 09/18/2018
ms.reviewer: 
manager: dansimp
ms.topic: article
---

# Windows Update common errors and mitigation

>Applies to: Windows 10

The following table provides information about common errors you might run into with Windows Update, as well as steps to help you mitigate them.

|Error Code|Message|Description|Mitigation|
|-|-|-|-| 
|0x8024402F|WU_E_PT_ECP_SUCCEEDED_WITH_ERRORS|External cab file processing completed with some errors|One of the reasons we see this issue is due to the design of a software called Lightspeed Rocket for Web filtering. <br>The IP addresses of the computers you want to get updates successfully on, should be added to the exceptions list of Lightspeed |
|0x80242006|WU_E_UH_INVALIDMETADATA|A handler operation could not be completed because the update contains invalid metadata.|Rename Software Redistribution Folder and attempt to download the updates again: <br>Rename the following folders to *.BAK: <br>- %systemroot%\system32\catroot2 <br><br>To do this, type the following commands at a command prompt. Press ENTER after you type each command.<br>- Ren %systemroot%\SoftwareDistribution\DataStore *.bak<br>- Ren %systemroot%\SoftwareDistribution\Download *.bak<br>Ren %systemroot%\system32\catroot2 *.bak |
|0x80070BC9|ERROR_FAIL_REBOOT_REQUIRED|The requested operation failed. A system reboot is required to roll back changes made.|Ensure that we do not have any policies that control the start behavior for the Windows Module Installer. This service should not be hardened to any start value and should be managed by the OS.|  
|0x80200053|BG_E_VALIDATION_FAILED|NA|Ensure that there is no Firewalls that filter downloads. The Firewall filtering may lead to invalid responses being received by the Windows Update Client.<br><br>If the issue still persists, run the [WU reset script](https://gallery.technet.microsoft.com/scriptcenter/Reset-Windows-Update-Agent-d824badc). |  
|0x80072EE2|WININET_E_TIMEOUT|The operation timed out|This error message can be caused if the computer isn't connected to Internet. To fix this issue, following these steps: make sure these URLs are not blocked: <br> http://*.update.microsoft.com<br>https://*.update.microsoft.com <br>http://download.windowsupdate.com  <br><br>Additionally , you can take a network trace and see what is timing out. <Refer to Firewall Troubleshooting scenario> |
|0x80072EFD <br>0x80072EFE <br>0x80D02002|TIME OUT ERRORS|The operation timed out|Make sure there are no firewall rules or proxy to block Microsoft download URLs. <br>Take a network monitor trace to understand better. <Refer to Firewall Troubleshooting scenario>| 
|0X8007000D|ERROR_INVALID_DATA|Indicates invalid data downloaded or corruption occurred.|Attempt to re-download the update and initiate installation. |
|0x8024A10A|USO_E_SERVICE_SHUTTING_DOWN|Indicates that the WU Service is shutting down.|This may happen due to a very long period of time of inactivity, a system hang leading to the service being idle and leading to the shutdown of the service. Ensure that the system remains active and the connections remain established to complete the upgrade. |
|0x80240020|WU_E_NO_INTERACTIVE_USER|Operation did not complete because there is no logged-on interactive user.|Please login to the system to initiate the installation and allow the system to be rebooted.  |
|0x80242014|WU_E_UH_POSTREBOOTSTILLPENDING|The post-reboot operation for the update is still in progress.|Some Windows Updates require the system to be restarted. Reboot the system to complete the installation of the Updates. |
|0x80246017|WU_E_DM_UNAUTHORIZED_LOCAL_USER|The download failed because the local user was denied authorization to download the content.|Ensure that the user attempting to download and install updates has been provided with sufficient privileges to install updates (Local Administrator).|
|0x8024000B|WU_E_CALL_CANCELLED|Operation was cancelled.|This indicates that the operation was cancelled by the user/service. You may also encounter this error when we are unable to filter the results. Run the [Decline Superseded PowerShell script](https://gallery.technet.microsoft.com/scriptcenter/Cleanup-WSUS-server-4424c9d6) to allow the filtering process to complete.| 
|0x8024000E|WU_E_XML_INVALID|Windows Update Agent found invalid information in the update's XML data.|Certain drivers contain additional metadata information in the update.xml, which could lead Orchestrator to understand it as invalid data. Ensure that you have the latest Windows Update Agent installed on the machine. | 
|0x8024D009|WU_E_SETUP_SKIP_UPDATE|An update to the Windows Update Agent was skipped due to a directive in the wuident.cab file.|You may encounter this error when WSUS is not sending the Self-update to the clients.<br><br>Review [KB920659](https://support.microsoft.com/help/920659/the-microsoft-windows-server-update-services-wsus-selfupdate-service-d) for instructions to resolve the issue.| 
|0x80244007|WU_E_PT_SOAPCLIENT_SOAPFAULT|SOAP client failed because there was a SOAP fault for reasons of WU_E_PT_SOAP_* error codes.|This issue occurs because Windows cannot renew the cookies for Windows Update.  <br><br>Review [KB2883975](https://support.microsoft.com/help/2883975/0x80244007-error-when-windows-tries-to-scan-for-updates-on-a-wsus-serv) for instructions to resolve the issue.| 
