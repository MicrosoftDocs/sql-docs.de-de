---
title: Reporting Services mit Verfügbarkeitsgruppen
description: Erfahren Sie, wie Sie Reporting Services mit Always On-Verfügbarkeitsgruppen in SQL Server konfigurieren. Die unterstützten Funktionen unterscheiden sich je nach Szenario.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: d80b30323dfc38d31e385c88bd79e24c3d1c7184
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344545"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Reporting Services mit Always On-Verfügbarkeitsgruppen (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema enthält Informationen über die Konfiguration von [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] zur Verwendung mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] (AG) in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Die drei Szenarien zum Verwenden von [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] und [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sind Datenbanken für Berichtsdatenquellen, Berichtsserver-Datenbanken und Berichtsentwurf. Die unterstützten Funktionen und die erforderliche Konfiguration sind für die drei Szenarien unterschiedlich.  
  
 Ein entscheidender Vorteil bei der Verwendung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mit [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Datenquellen liegt darin, dass lesbare sekundäre Replikate als Berichtsdatenquelle genutzt werden, während gleichzeitig die sekundären Replikate ein Failover für eine primäre Datenbank bereitstellen.  
  
 Weitere Informationen zu [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Häufig gestellte Fragen zu Always On für SQL Server 2012 (../../../sql-server/index.yml)](../../../sql-server/index.yml).  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> Anforderungen für die Verwendung von Reporting Services und Always On-Verfügbarkeitsgruppen  
 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] und Power BI-Berichtsserver verwenden .NET Framework 4.0 und unterstützen die Verwendung der Verbindungszeichenfolgen-Eigenschaften von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mit Datenquellen.  
  
 Um [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mit  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 und früher zu verwenden, müssen Sie einen Hotfix für .NET 3.5 SP1 herunterladen und installieren. Der Hotfix fügt Unterstützung für Funktionen des SQL Client für Verfügbarkeitsgruppen und Unterstützung der Verbindungszeichenfolgeneigenschaften **ApplicationIntent** und **MultiSubnetFailover** hinzu. Wenn der Hotfix nicht auf jedem Computer installiert ist, der einen Berichtsserver hostet, dann sehen Benutzer, die versuchen, Berichte in der Vorschau anzuzeigen, eine Fehlermeldung wie die Folgende, und die Fehlermeldung wird in das Ablaufverfolgungsprotokoll des Berichtsservers geschrieben:  
  
> **Fehlermeldung:** "Keyword not supported 'applicationintent'" (Das Schlüsselwort wird nicht unterstützt: 'applicationintent')  
  
 Die Meldung wird ausgegeben, wenn Sie eine der [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Eigenschaften in die [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Verbindungszeichenfolge einschließen, aber der Server die Eigenschaft nicht erkennt. Die angegebene Fehlermeldung wird angezeigt, wenn Sie in den [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Benutzeroberflächen auf die Schaltfläche „Verbindung testen“ klicken, und wenn Sie den Bericht in der Vorschau anzeigen, sofern Remotefehler auf den Berichtsservern aktiviert sind.  
  
 Weitere Informationen zum erforderlichen Hotfix finden Sie in [KB 2654347A, Hotfix bietet Unterstützung für die Always On-Funktionen aus SQL Server 2012 in .NET Framework 3.5 SP1](https://go.microsoft.com/fwlink/?LinkId=242896).  
  
 Informationen zu anderen Anforderungen von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Konfigurationsdateien wie **RSreportserver.config** werden nicht als Teil der [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]-Funktion unterstützt. Wenn Sie manuelle Änderungen an einer Konfigurationsdatei auf einem der Berichtsserver vornehmen, müssen Sie die Replikate manuell aktualisieren.  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> Berichtsdatenquellen und Verfügbarkeitsgruppen  
 Das Verhalten von [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Datenquellen auf Grundlage von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] kann abhängig davon variieren, wie der Administrator die Verfügbarkeitsgruppenumgebung konfiguriert hat.  
  
 Um [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] für Berichtsdatenquellen zu verwenden, müssen Sie die Berichtsdatenquellenverbindungszeichenfolge konfigurieren, um die Verfügbarkeitsgruppe *DNS-Name des Listeners* zu verwenden. Die folgenden Datenquellen werden unterstützt:  
  
-   ODBC-Datenquelle mit SQL Native Client.  
  
-   SQL Client, mit dem auf den Berichtsserver angewendeten .NET-Hotfix.  
  
 Die Verbindungszeichenfolge kann auch neue Always On-Verbindungseigenschaften enthalten, mit denen die Berichtsabfrageanforderungen zum Verwenden sekundärer Replikate für die schreibgeschützte Berichterstellung konfiguriert werden. Durch die Verwendung von sekundären Replikaten für Berichtsanforderungen wird die Last für ein primäres Lese-/Schreib-Replikat verringert. Die folgende Abbildung ist ein Beispiel für eine Verfügbarkeitsgruppenkonfiguration mit drei Replikaten, in der die [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Datenquellenverbindungszeichenfolgen mit ApplicationIntent=ReadOnly konfiguriert wurden. In diesem Beispiel werden die Berichtsabfrageanforderungen an ein sekundäres Replikat und nicht das primäre Replikat gesendet.  
  
 
  
 Im Folgenden sehen Sie eine Beispielverbindungszeichenfolge, bei der [AvailabilityGroupListenerName] der **DNS-Name des Listeners** ist, der bei Erstellung der Replikate konfiguriert wurde:  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 Über die Schaltfläche **Verbindung testen** auf den [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Benutzeroberflächen wird überprüft, ob eine Verbindung hergestellt werden kann. Damit wird jedoch nicht die Verfügbarkeitsgruppenkonfiguration überprüft. Wenn Sie z. B. ApplicationIntent in eine Verbindungszeichenfolge zu einem Server einschließen, der kein Teil der Verfügbarkeitsgruppe ist, wird der zusätzliche Parameter ignoriert, und die Schaltfläche **Verbindung testen** überprüft nur, ob eine Verbindung zum angegebenen Server hergestellt werden kann.  
  
 Abhängig davon, wie die Berichte erstellt und veröffentlicht werden, wird bestimmt, wo Sie die Verbindungszeichenfolge bearbeiten:  
  
-   **Einheitlicher Modus :** Verwenden Sie das Webportal [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)] für freigegebene Datenquellen und Berichte, die bereits auf einem Berichtsserver im einheitlichen Modus veröffentlicht wurden.  
  
-   **SharePoint-Modus:** Verwenden Sie die SharePoint-Konfigurationsseiten innerhalb der Dokumentbibliotheken für Berichte, die bereits auf einem SharePoint-Server veröffentlicht wurden.  
  
-   **Berichtsentwurf:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] oder [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], wenn Sie neue Berichte erstellen. Weitere Informationen finden Sie in diesem Thema im Abschnitt „Berichtsentwurf“.  
  
 **Zusätzliche Ressourcen:**  
  
-   [Verwalten von Berichtsdatenquellen](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   Weitere Informationen zu den verfügbaren Verbindungszeichenfolgeneigenschaften finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
-   Weitere Informationen zu Verfügbarkeitsgruppenlistenern finden Sie unter [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Überlegungen:** Sekundäre Replikate empfangen Datenänderungen vom primären Replikat in der Regel mit Verzögerung. Die folgenden Faktoren können sich auf die Updatewartezeit zwischen den primären und sekundären Replikaten auswirken:  
  
-   Die Anzahl der sekundären Replikate. Die Verzögerung nimmt mit jedem sekundären Replikat zu, das der Konfiguration hinzugefügt wird.  
  
-   Geografischer Standort und Entfernung zwischen den primären und sekundären Replikaten. Die Verzögerung ist z. B. in der Regel größer, wenn die sekundären Replikate sich einem anderen Rechenzentrum befinden, als wenn sie sich im gleichen Gebäude wie das primäre Replikat befinden.  
  
-   Konfiguration des Verfügbarkeitsmodus für jedes Replikat. Der Verfügbarkeitsmodus legt fest, ob das primäre Replikat mit dem Commit der Transaktionen für eine Datenbank wartet, bis das sekundäre Replikat die Transaktion auf den Datenträger geschrieben hat. Weitere Informationen finden Sie im Abschnitt „Verfügbarkeitsmodi“ des Themas [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 Wenn ein schreibgeschütztes sekundäres Replikat als [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Datenquelle verwendet wird, muss sichergestellt werden, dass die Datenupdatewartezeit die Anforderungen der Berichtsbenutzer erfüllt.  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> Berichtsentwurf und Verfügbarkeitsgruppen  
 Beim Entwerfen von Berichten in [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] oder eines Berichtsprojekts in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]kann ein Benutzer eine Berichtsdatenquellenverbindungszeichenfolge so konfigurieren, dass sie von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]bereitgestellte Verbindungseigenschaften enthält. Die Unterstützung für die neuen Verbindungseigenschaften hängt davon ab, wo ein Benutzer den Bericht in der Vorschau anzeigt.  
  
-   **Lokale Vorschau:** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] und [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] verwenden .NET Framework 4.0 und unterstützen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Verbindungszeichenfolgen-Eigenschaften.  
  
-   **Remote- oder Servermodusvorschau:** Wenn nach dem Veröffentlichen von Berichten auf dem Berichtsserver oder bei einer Vorschau in [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] ein Fehler wie der Folgende angezeigt wird, ist dies ein Hinweis darauf, dass Sie Berichte vom Berichtsserver in der Vorschau anzeigen und dass der .NET Framework 3.5 SP1-Hotfix für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nicht auf dem Berichtsserver installiert wurde.  
  
> **Fehlermeldung:** "Keyword not supported 'applicationintent'" (Das Schlüsselwort wird nicht unterstützt: 'applicationintent')  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> Berichtsserver-Datenbanken und Verfügbarkeitsgruppen  
 Reporting Services und Power BI-Berichtsserver bieten eingeschränkte Unterstützung für die Verwendung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mit Berichtsserver-Datenbanken. Die Berichtsserver-Datenbanken können in Verfügbarkeitsgruppen als Teil eines Replikats konfiguriert werden, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] verwendet bei einem Failover jedoch nicht automatisch ein anderes Replikat für die Berichtsserver-Datenbanken. Die Verwendung von MultiSubnetFailover mit den Berichtsserver-Datenbanken wird nicht unterstützt.  
  
 Manuelle Aktionen oder benutzerdefinierte Automatisierungsskripts müssen verwendet werden, um das Failover und die Wiederherstellung abzuschließen. Bis diese Aktionen abgeschlossen sind, funktionieren einige Funktionen des Berichtsservers möglicherweise nicht ordnungsgemäß nach dem [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Failover.  
  
> [!NOTE]  
>  Wenn Sie Failover und Notfallwiederherstellung für die Berichtsserver-Datenbanken planen, sollten Sie immer eine Kopie des Berichtsserververschlüsselungsschlüssels sichern.  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> Unterschiede zwischen dem einheitlichen Modus und dem SharePoint-Modus  
 Dieser Abschnitt fasst die Unterschiede bei der Interaktion von Berichtsservern im SharePoint-Modus und im einheitlichen Modus mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]zusammen.  
  
 Ein SharePoint-Berichtsserver erstellt für jede **-Dienstanwendung, die Sie erstellen,** 3 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] Datenbanken. Die Verbindung mit den Berichtsserver-Datenbanken im SharePoint-Modus wird in der SharePoint-Zentraladministration konfiguriert, wenn Sie die Dienstanwendung erstellen. Die Standardnamen der Datenbanken enthalten eine GUID für die Dienstanwendung. Datenbanknamen für einen SharePoint-Modusberichtsserver können beispielsweise wie folgt aussehen:  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 Für Berichtsserver im einheitlichen Modus werden **2** Datenbanken verwendet. Datenbanknamen für einen Berichtsserver im einheitlichen Modus können beispielsweise wie folgt aussehen:  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 Der einheitliche Modus unterstützt bzw. verwendet Warnungsdatenbanken und zugehörige Funktionen nicht. Berichtsserver im einheitlichen Modus konfigurieren Sie im [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager. Im SharePoint-Modus konfigurieren Sie den Dienstanwendungsdatenbanknamen als Namen des „Clientzugriffspunkts“, den Sie als Teil der SharePoint-Konfiguration erstellten. Weitere Informationen zum Konfigurieren von SharePoint mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Konfigurieren und Verwalten von SQL Server-Verfügbarkeitsgruppen für SharePoint Server (/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14)).  
  
> [!NOTE]
>  Berichtsserver im SharePoint-Modus verwenden einen Synchronisierungsvorgang zwischen den [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Dienstanwendungsdatenbanken und den SharePoint-Inhaltsdatenbanken. Es ist wichtig, die Berichtsserver-Datenbanken und Inhaltsdatenbanken zusammen zu verwalten. Sie sollten erwägen, sie in den gleichen Verfügbarkeitsgruppen zu konfigurieren, damit sie bei Failover und Wiederherstellung als Satz behandelt werden. Nehmen Sie das folgende Szenario als Beispiel:  
> 
>  -   Sie führen eine Wiederherstellung oder ein Failover zu einer Kopie der Inhaltsdatenbank aus, die nicht die gleichen letzten Updates wie die Berichtsserver-Datenbank empfangen hat.  
> -   Der [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Synchronisierungsvorgang erkennt Unterschiede zwischen der Liste der Elemente in der Inhaltsdatenbank und den Berichtsserver-Datenbanken.  
> -   Der Synchronisierungsvorgang löscht oder aktualisiert Elemente in der Inhaltsdatenbank.  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> Vorbereiten von Berichtsserver-Datenbanken für Verfügbarkeitsgruppen  
 Im Folgenden sind die grundlegenden Schritte zum Vorbereiten und Hinzufügen der Berichtsserver-Datenbanken zu einer [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]beschrieben:  
  
-   Erstellen Sie die Verfügbarkeitsgruppe, und konfigurieren Sie einen *DNS-Namen des Listeners*.  
  
-   **Primäres Replikat:** Konfigurieren Sie die Berichtsserverdatenbanken als Teil einer einzelnen Verfügbarkeitsgruppe, und erstellen Sie ein primäres Replikat, das alle Berichtsserverdatenbanken einschließt.  
  
-   **Sekundäre Replikate:** Erstellen Sie mindestens ein sekundäres Replikat. Die übliche Methode zum Kopieren der Datenbanken vom primären Replikat zum sekundären Replikat besteht darin, die Datenbanken mit „RESTORE WITH NORECOVERY“ auf jedem sekundären Replikat wiederherzustellen. Weitere Informationen zum Erstellen von sekundären Replikaten und zum Überprüfen, ob die Datensynchronisierung funktioniert, finden Sie unter [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
-   **Berichtsserveranmeldeinformationen:** Sie müssen die entsprechenden Berichtsserveranmeldeinformationen auf den sekundären Replikaten erstellen, die Sie auf dem primären Replikat erstellt haben. Die genauen Schritte hängen davon ab, welchen Authentifizierungstyp Sie in der [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Umgebung verwenden; Windows-[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Dienstkonto, Windows-Benutzerkonto oder SQL Server-Authentifizierung. Weitere Informationen finden Sie unter [Konfigurieren einer Berichtsserver-Datenbankverbindung &#40;SSRS-Konfigurations-Manager&#41;](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Aktualisieren Sie die Datenbankverbindung, um den DNS-Namen des Listeners zu verwenden. Ändern Sie den **Berichtsserver-Datenbanknamen** für Berichtsserver im einheitlichen Modus im [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager. Ändern Sie den **Datenbankservernamen** für den SharePoint-Modus für die [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Dienstanwendung(en).  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> Schritte zum Abschluss der Notfallwiederherstellung von Berichtsserver-Datenbanken  
 Die folgenden Schritte müssen nach einem [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Failover zu einem sekundären Replikat ausgeführt werden:  
  
1.  Beenden Sie die Instanz des SQL Agent-Diensts, der von der primären Datenbank-Engine verwendet wurde, das die [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Datenbanken hostet.  
  
2.  Starten Sie den SQL Agent-Dienst auf dem Computer, der das neue primäre Replikat ist.  
  
3.  Beenden Sie den Berichtsserverdienst.  
  
     Wenn der Berichtsserver im einheitlichen Modus ausgeführt wird, beenden Sie den Windows-Berichtsserver mithilfe des [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] Konfigurations-Managers.  
  
     Wenn der Berichtsserver für den SharePoint-Modus konfiguriert ist, beenden Sie den freigegebenen [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Dienst in SharePoint-Zentraladministration.  
  
4.  Starten Sie den Berichtsserverdienst oder [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] SharePoint-Dienst.  
  
5.  Stellen Sie sicher, dass Berichte für das neue primäre Replikat ausgeführt werden können.  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> Berichtsserververhalten, wenn ein Failover auftritt  
 Bei einem Failover der Berichtsserver-Datenbanken in einer Berichtsserverumgebung, die für die Verwendung des neuen primären Replikats aktualisiert wurde, entstehen funktionale Probleme, die sich aus dem Failover und dem Wiederherstellungsvorgang ergeben. Die Auswirkungen dieser Probleme hängen von der [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Last zum Zeitpunkt des Failovers und von der Länge der Zeit ab, die die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] zum Failover auf ein sekundäres Replikat benötigt und die der Berichtsserver-Administrator zum Aktualisieren der Berichtsumgebung für die Verwendung des neuen primären Replikats braucht.  
  
-   Die Ausführung der Hintergrundverarbeitung tritt möglicherweise mehr als einmal auf. Dies liegt an der Wiederholungslogik und der Unfähigkeit des Berichtsservers, die geplante Arbeit während des Failoverzeitraums als abgeschlossen zu markieren.  
  
-   Die Ausführung der Hintergrundverarbeitung, die normalerweise für den Zeitraum des Failovers ausgelöst worden wäre, tritt nicht auf, da der SQL Server-Agent nicht in der Lage ist, Daten in die Berichtsserver-Datenbank zu schreiben und diese Daten nicht mit dem neuen primären Replikat synchronisiert werden.  
  
-   Nachdem das Datenbankfailover abgeschlossen wurde, und nachdem der Berichtsserverdienst neu gestartet wurde, werden Aufträge des SQL Server-Agents automatisch neu erstellt. Hintegrundausführungen, die dem SQL Server-Agent zugeordnet sind, werden so lange nicht verarbeitet, bis die SQL Agent-Aufträge neu erstellt werden. Hierzu zählen [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Abonnements, Zeitpläne und Momentaufnahmen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Erste Schritte mit Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen mit dem nativen Client von SQL Server](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)   
 [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
