---
title: Verwalten von Abonnementbesitzern und Ausführen von PowerShell-Abonnement | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über das programmgesteuerte Übertragen des Besitzes eines Reporting Services-Abonnements eines Benutzers an einen anderen.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 01/16/2020
ms.openlocfilehash: b0174f0b7705c9a7c7c678782a4b17fb4a1a74af
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91985886"
---
# <a name="manage-subscription-owners-and-run-subscription---powershell"></a>Verwalten von Abonnementbesitzern und Ausführen von PowerShell-Abonnement

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

Beginnend mit [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] können Sie den Besitz eines [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements programmgesteuert von einem Benutzer auf einen anderen übertragen. Dieses Thema enthält mehrere Windows PowerShell-Skripts, die Sie verwenden können, um den Besitz von Abonnements zu ändern oder einfach aufzulisten. Jedes Beispiel enthält Beispielsyntax sowohl für den einheitlichen als auch den SharePoint-Modus. Wenn Sie den Abonnementbesitzer ändern, wird das Abonnement dann im Sicherheitskontext des neuen Besitzers ausgeführt, und das User!UserID-Feld des Berichts zeigt den Wert für den neuen Besitzer an. Weitere Informationen zum Objektmodell des PowerShell-Beispielaufrufs finden Sie unter <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A>  

![PowerShell-Inhalt](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell-Inhalt")

##  <a name="in-this-topic"></a><a name="bkmk_top"></a> In diesem Thema:
  
- [Gewusst wie: Verwenden der Skripts](#bkmk_how_to)  
  
- [Skript: Auflisten der Besitzer aller Abonnements](#bkmk_list_ownership_all)  
  
- [Skript: Auflisten aller Abonnements im Besitz eines spezifischen Benutzers](#bkmk_list_all_one_user)  
  
- [Skript: Ändern des Besitzes aller Abonnements eines spezifischen Benutzers](#bkmk_change_all)  
  
- [Skript: Auflisten aller Abonnements im Zusammenhang mit einem bestimmten Bericht](#bkmk_list_for_1_report)  
  
- [Skript: Ändern des Besitzes eines bestimmten Abonnements](#bkmk_change_all_1_subscription)  
  
- [Skript: Ausführen (Auslösen) eines Einzelabonnements](#bkmk_run_1_subscription)  
  
## <a name="how-to-use-the-scripts"></a><a name="bkmk_how_to"></a> Gewusst wie: Verwenden der Skripts
  
### <a name="permissions"></a>Berechtigungen

In diesem Abschnitt werden die erforderlichen Berechtigungsstufen für die Verwendung der Methoden für den einheitlichen und den SharePoint-Modus [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]zusammengefasst. Die Skripts in diesem Thema verwenden die folgenden [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Methoden:  
  
- [ReportingService2010.ListSubscriptions Method](/dotnet/api/reportservice2010.reportingservice2010.listsubscriptions)  
  
- [ReportingService2010.ChangeSubscriptionOwner Method](/dotnet/api/reportservice2010.reportingservice2010.changesubscriptionowner)  
  
- [ReportingService2010.ListChildren](/dotnet/api/reportservice2010.reportingservice2010.listchildren)  
  
- Die Methode [ReportingService2010.FireEvent](/dotnet/api/reportservice2010.reportingservice2010.fireevent) wird nur im letzten Skript verwendet, um zu veranlassen, dass ein bestimmtes Abonnement ausgeführt wird. Wenn Sie dieses Skript nicht verwenden möchten, können Sie die Berechtigungsanforderungen für die FireEvent-Methode ignorieren.  
  
**Einheitlicher Modus :**
  
- Abonnements auflisten: [ReportOperation-Enumeration](/dotnet/api/microsoft.reportingservices.interfaces.reportoperation) im Bericht UND der Benutzer, der Besitzer des Abonnements ist, ODER ReadAnySubscription  
  
- Abonnements ändern: Der Benutzer muss ein Mitglied der Gruppe "BUILTIN\Administrators" sein.  
  
- Untergeordnete Elemente auflisten: ReadProperties für das Element  
  
- Ereignis auslösen: GenerateEvents (System)  
  
 **SharePoint-Modus:**
  
- Abonnements auflisten: ManageAlerts ODER [CreateAlerts](/previous-versions/office/sharepoint-server/ms412690(v=office.15)) im Bericht UND der Benutzer, der Abonnementbesitzer ist, dabei muss es sich um ein geplantes Abonnement handeln.  
  
- Abonnements ändern: ManageWeb  
  
- Untergeordnete Elemente auflisten: ViewListItems  
  
- Ereignis auslösen: ManageWeb  
  
 Weitere Informationen finden Sie unter [Vergleichen der Rollen und Aufgaben in Reporting Services mit SharePoint-Gruppen und -Berechtigungen](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md).  
  
### <a name="script-usage"></a>Verwenden von Skripts

**Erstellen von Skriptdateien (.ps1)**
  
1. Erstellen Sie einen Ordner mit dem Namen **c:\scripts**. Wenn Sie einen anderen Ordner wählen, passen Sie den Ordnernamen in den Befehlszeilensyntaxanweisungen aus dem Beispiel an.  
  
2. Erstellen Sie eine Textdatei für jedes Skript, und speichern Sie die Dateien im Ordner c:\scripts. Verwenden Sie den Namen aus der Beispielbefehlszeilensyntax, wenn Sie die PS1-Dateien erstellen.  
  
3. Öffnen Sie eine Eingabeaufforderung mit Administratorberechtigungen.  
  
4. Führen Sie alle Skriptdateien mithilfe der Befehlszeilensyntax aus, die in jedem Beispiel enthalten ist.  
  
**Getestete Umgebungen**
  
 Die Skripts in diesem Thema wurden mit Version 3 von PowerShell getestet sowie den folgenden Versionen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
## <a name="script-list-the-ownership-of-all-subscriptions"></a><a name="bkmk_list_ownership_all"></a> Skript: Auflisten der Besitzer aller Abonnements

Dieses Skript listet alle Abonnements auf einer Site auf. Sie können dieses Skript verwenden, um Ihre Verbindung zu testen oder um den Berichtspfad und die Abonnement-ID für die Verwendung in den anderen Skripts zu verifizieren. Dieses Skript ist außerdem hilfreich, wenn Sie einfach prüfen möchten, welche Abonnements vorhanden sind und wer diese besitzt.  
  
 **Syntax im einheitlichen Modus:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/reportserver" "/"  
```  
  
 **Syntax im SharePoint-Modus:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/_vti_bin/reportserver" "https://[server]"  
```  
  
 **Skript:**
  
```
# Parameters  
#    server   - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
  
Param(  
    [string]$server,  
    [string]$site  
   )  
  
$rs2010 += New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site); # use "/" for default native mode site  
  
Write-Host " "  
Write-Host "----- $server's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted, Status  
```  
  
> [!TIP]  
> Verwenden Sie das SharePoint-Cmdlet **Get-SPSite**, um Website-URLs im SharePoint-Modus zu verifizieren. Weitere Informationen finden Sie unter [Get-SPSite](https://msdn.microsoft.com/library/ff607950\(v=office.15\).aspx).  
  
##  <a name="script-list-all-subscriptions-owned-by-a-specific-user"></a><a name="bkmk_list_all_one_user"></a> Skript: Auflisten aller Abonnements im Besitz eines spezifischen Benutzers

Dieses Skript listet alle Abonnements auf, die ein bestimmter Benutzer besitzt. Sie können dieses Skript verwenden, um Ihre Verbindung zu testen oder um den Berichtspfad und die Abonnement-ID für die Verwendung in den anderen Skripts zu verifizieren. Dieses Skript ist hilfreich, wenn jemand Ihr Unternehmen verlässt und Sie prüfen möchten, welche Abonnements diese Person besessen hat, sodass Sie den Besitzer ändern oder das Abonnement löschen können.  
  
 **Syntax im einheitlichen Modus:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]" "[server]/reportserver" "/"  
```  
  
**Syntax im SharePoint-Modus:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]"  "[server]/_vti_bin/reportserver" "https://[server]"  
```  
  
**Skript:**
  
```
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
Param(  
    [string]$currentOwner,  
    [string]$server,  
    [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $currentOwner's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.owner -eq $currentOwner}  
```  
  
## <a name="script-change-ownership-for-all-subscriptions-owned-by-a-specific-user"></a><a name="bkmk_change_all"></a> Skript: Ändern des Besitzes aller Abonnements eines spezifischen Benutzers

Dieses Skript ändert den Besitz für alle Abonnements, die ein bestimmter Benutzer besitzt, zum neuen Besitzerparameter.  
  
**Syntax im einheitlichen Modus:**  
  
```powershell
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\current owner]" "[Domain]\[new owner]" "[server]/reportserver"  
```  
  
**Syntax im SharePoint-Modus:**  
  
```powershell
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\{current owner]" "[Domain]\[new owner]" "[server]/_vti_bin/reportserver"  
```  
  
**Skript:**  
  
```
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    newOwner      - DOMAIN\USER that will own the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver, myserver/reportserver_db2, myserver/_vti_bin/reportserver)
  
Param(  
    [string]$currentOwner,  
    [string]$newOwner,  
    [string]$server  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$items = $rs2010.ListChildren("/", $true);  
  
$subscriptions = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $curRepSubs = $rs2010.ListSubscriptions($item.Path);  
        ForEach ($curRepSub in $curRepSubs)  
        {  
            if ($curRepSub.Owner -eq $currentOwner)  
            {  
                $subscriptions += $curRepSub;  
            }  
        }  
    }  
}  
  
Write-Host " "  
Write-Host " "  
Write-Host -foregroundcolor "green" "-----  $currentOwner's Subscriptions changing ownership to $newOwner : "  
$subscriptions | select SubscriptionID, Owner, Path, Description,  Status  | format-table -AutoSize  
  
ForEach ($sub in $subscriptions)  
{  
    $rs2010.ChangeSubscriptionOwner($sub.SubscriptionID, $newOwner);  
}  
  
$subs2 = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $subs2 += $rs2010.ListSubscriptions($item.Path);  
    }  
}  
```  
  
## <a name="script-list-all-subscriptions-associated-with-a-specific-report"></a><a name="bkmk_list_for_1_report"></a> Skript: Auflisten aller Abonnements im Zusammenhang mit einem bestimmten Bericht  

Dieses Skript listet alle Abonnements auf, die mit einem bestimmten Bericht verknüpft sind. Die Berichtspfadsyntax unterscheidet sich vom SharePoint-Modus, der eine vollständige URL erfordert. In den Syntaxbeispielen wird der Berichtsname „title only“ verwendet, der ein Leerzeichen enthält und daher einfache Anführungszeichen vor und nach dem Berichtsnamen erfordert.  
  
**Syntax im einheitlichen Modus:**  
  
```powershell
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/reportserver" "'/reports/title only'" "/"  
```  
  
**Syntax im SharePoint-Modus:**  
  
```powershell
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/_vti_bin/reportserver"  "'https://[server]/shared documents/title only.rdl'" "https://[server]"  
```  
  
**Skript:**  
  
```
# Parameters:  
#    server      - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    reportpath  - path to report in the report server, including report name e.g. /reports/test report >> pass in  "'/reports/title only'"  
#    site        - use "/" for default native mode site  
Param  
(  
      [string]$server,  
      [string]$reportpath,  
      [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $reportpath 's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.path -eq $reportpath}  
```  
  
## <a name="script-change-ownership-of-a-specific-subscription"></a><a name="bkmk_change_all_1_subscription"></a> Skript: Ändern des Besitzes eines bestimmten Abonnements  
 Dieses Skript ändert den Besitz eines bestimmten Abonnements. Dieses Abonnement wird durch die SubscriptionID (Abonnement-ID) identifiziert, die Sie in das Skript übernehmen. Sie können eines der Skripts zur Auflistung von Abonnements verwenden, um die korrekte SubscriptionID zu bestimmen.  
  
 **Syntax im einheitlichen Modus:**  
  
```powershell
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/reportserver" "/" "ac5637a1-9982-4d89-9d69-a72a9c3b3150"  
```  
  
 **Syntax im SharePoint-Modus:**  
  
```powershell
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/_vti_bin/reportserver" "https://[server]" "9660674b-f020-453f-b1e3-d9ba37624519"  
```  
  
 **Skript:**  
  
```
# Parameters:  
#    newOwner       - DOMAIN\USER that will own the subscriptions you wish to change  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
#    subscriptionID - guid for the single subscription to change  
  
Param(  
    [string]$newOwner,  
    [string]$server,  
    [string]$site,  
    [string]$subscriptionid  
   )  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
  
$subscription += $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid};  
  
Write-Host " "  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
  
$rs2010.ChangeSubscriptionOwner($subscription.SubscriptionID, $newOwner)  
  
#refresh the list  
$subscription = $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid}; # use "/" for default native mode site  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
```  
  
## <a name="script-run-fire-a-single-subscription"></a><a name="bkmk_run_1_subscription"></a> Skript: Ausführen (Auslösen) eines Einzelabonnements  

Dieses Skript führt ein bestimmtes Abonnement mit der FireEvent-Methode aus. Das Skript führt das Abonnement sofort aus, unabhängig davon, welcher Zeitplan für das Abonnement konfiguriert ist. Der EventType (Ereignistyp) wird mit einem bekannten Satz an Ereignissen abgeglichen, die in der Konfigurationsdatei des Berichtsservers **rsreportserver.config** definiert sind. Das Skript verwendet den folgenden Ereignistyp für standardmäßige Abonnements:  
  
 `<Event>`  
  
 `<Type>TimedSubscription</Type>`  
  
 `</Event>`  
  
 Weitere Informationen zur Konfigurationsdatei finden Sie unter [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
 Das Skript enthält die Verzögerungslogik „`Start-Sleep -s 6`“, sodass nach Auslösen des Ereignisses Zeit bleibt, in der der aktualisierte Status mit der ListSubscription-Methode verfügbar ist.  
  
 **Syntax im einheitlichen Modus:**  
  
```powershell
powershell c:\scripts\FireSubscription.ps1 "[server]/reportserver" $null "70366e82-2d3c-4edd-a216-b97e51e26de9"  
```  
  
 **Syntax im SharePoint-Modus:**  
  
```powershell
powershell c:\scripts\FireSubscription.ps1 "[server]/_vti_bin/reportserver" "https://[server]" "c3425c72-580d-423e-805a-41cf9799fd25"  
```  
  
 **Skript:**  
  
```
  
# Parameters  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site           - use $null for a native mode server  
#    subscriptionid - subscription guid  
  
Param(  
  [string]$server,  
  [string]$site,  
  [string]$subscriptionid  
  )  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
#event type is case sensative to what is in the rsreportserver.config  
$rs2010.FireEvent("TimedSubscription",$subscriptionid,$site)  
  
Write-Host " "  
Write-Host "----- Subscription ($subscriptionid) status: "  
#get list of subscriptions and filter to the specific ID to see the Status and LastExecuted  
Start-Sleep -s 6 # slight delay in processing so ListSubscription returns the updated Status and LastExecuted  
$subscriptions = $rs2010.ListSubscriptions($site);   
$subscriptions | select Status, Path, report, Description, Owner, SubscriptionID, EventType, lastexecuted | where {$_.SubscriptionID -eq $subscriptionid}  
  
```  

## <a name="see-also"></a>Weitere Informationen  

- [ReportingService2010.ListSubscriptions Method](/dotnet/api/reportservice2010.reportingservice2010.listsubscriptions)  

- [ReportingService2010.ChangeSubscriptionOwner Method](/dotnet/api/reportservice2010.reportingservice2010.changesubscriptionowner)   

- [ReportingService2010.ListChildren](/dotnet/api/reportservice2010.reportingservice2010.listchildren)  

- [ReportingService2010.FireEvent](/dotnet/api/reportservice2010.reportingservice2010.fireevent)