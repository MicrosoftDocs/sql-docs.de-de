---
title: Abonnementeinstellungen und ein Dateifreigabekonto (Configuration Manager) | Microsoft-Dokumentation
description: Über die Seite „Abonnementeinstellungen“ im Berichtsserver-Konfigurations-Manager können Sie ein Dateifreigabekonto für Berichtsserver im einheitlichen Modus und Dateifreigabeabonnements konfigurieren.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5239174f08c9922a31204b4f476f923a7e931567
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236228"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>Abonnementeinstellungen und ein Dateifreigabekonto (Berichtsserver-Konfigurations-Manager)
  Verwenden Sie die Seite **Abonnementeinstellungen** im Konfigurations-Manager für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , um ein Dateifreigabekonto für Berichtsserver im einheitlichen Modus und Dateifreigabeabonnements zu konfigurieren. Mit dem Dateifreigabekonto können Sie einen einzelnen Anmeldeinformationssatz in mehreren Abonnements verwenden, die Berichte an eine Dateifreigabe übermitteln. Wenn die Anmeldeinformationen geändert werden müssen, konfigurieren Sie die Änderung für das Dateifreigabekonto. So müssen Sie nicht jedes einzelne Abonnement aktualisieren.  
  
 Es gibt zwei Workflows für Dateifreigabeabonnements in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Im [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] -Release kann Ihr [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Administrator neuerdings eine einzelne Dateifreigabe konfigurieren, die für ein oder mehrere Abonnements verwendet wird. Konfigurieren Sie dazu **Ein Dateifreigabekonto angeben**, und wählen Sie auf den Seiten für die Abonnementkonfiguration **Dateifreigabekonto verwenden** aus.  
  
-   Sie können einzelne Abonnements mit spezifischen Anmeldeinformationen für die Zieldateifreigabe konfigurieren.  
  
-   Sie können beide Ansätze auch kombinieren, sodass einige Dateifreigabeabonnements das zentrale Dateifreigabekonto verwenden, während andere Abonnements bestimmte Anmeldeinformationen nutzen.  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im einheitlichen Modus  
  
## <a name="specify-a-file-share-account"></a>Ein Dateifreigabekonto angeben  
 Wenn diese Option ausgewählt ist, können Sie ein Konto für den Zugriff auf Dateifreigaben vom Berichtsserver bereitstellen. Wenn Sie das Dateifreigabekonto konfigurieren, können alle Benutzer das Konto für alle Abonnements auswählen, die für die Übermittlung von Berichten an eine Dateifreigabe konfiguriert sind. Wenn diese Option nicht ausgewählt ist, wird das Dateifreigabekonto **nicht** in allen Abonnements zur Verfügung stehen.  
  
 Beachten Sie, dass Sie überprüfen müssen, ob das als Dateifreigabekonto konfigurierte Konto Lese- und Schreibberechtigungen für alle Dateifreigaben besitzt, die die Benutzer für die Dateifreigabeübermittlung verwenden werden.  
  
 Die folgende Abbildung zeigt, was Benutzer in Abonnements sehen, die für eine Dateifreigabeübermittlung konfiguriert sind. **Dateifreigabekonto verwenden** ist deaktiviert, wenn kein Dateifreigabekonto konfiguriert wurde.  
  
 ![Configuration Manager-Dateifreigabekonto](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "Configuration Manager-Dateifreigabekonto")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>Verhindern von Berechtigungsausweitung oder erhöhten Rechten  
  
> [!IMPORTANT]
> Das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienstkonto steuert die Abonnementübermittlung und interagiert mit dem für Dateifreigabeabonnements verwendeten Konto. Die Windows-Sicherheitsfunktionen beschränken Kombinationen von 1) dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienstkonto und 2) dem für Dateifreigabekonten verwendeten Konto. Wenn beispielsweise ein integriertes Konto im Betriebssystem für das Dateifreigabekonto verwendet wird, muss das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienstkonto ein anderes Dienstkonto mit Identitätswechselberechtigungen sein. Wenn ein explizites Dateifreigabekonto und ein Kennwort konfiguriert sind, benötigt das Dateifreigabekonto das Recht, sich auf dem Computer anzumelden, auf dem der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienst ausgeführt wird. Wenn das Dateifreigabekonto nicht über die erforderlichen Berechtigungen verfügt, schlagen Abonnements fehl, die dieses Dateifreigabekonto verwenden. Die Fehlermeldung ähnelt der folgenden:  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>PowerShell-Beispiel zum Überwachen der Verwendung des Dateifreigabekontos  
 Führen Sie das folgende Windows PowerShell-Skript aus, um alle [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements aufzulisten, die für die Verwendung des **Dateifreigabekontos** konfiguriert sind. Aktualisieren Sie `SERVERNAME` auf einen für den Berichtsserver geeigneten Wert.  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 Die Ausgabe des Skripts sieht etwa wie folgt aus:  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dateifreigabeübermittlung in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Create and Manage Subscriptions for Native Mode Report Servers (Erstellen und Verwalten von Abonnements für Berichtsserver im einheitlichen Modus)](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
