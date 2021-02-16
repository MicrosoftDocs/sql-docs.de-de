---
title: Verwalten einer replizierten Verlegerdatenbank als Teil einer Verfügbarkeitsgruppe
description: Dieser Artikel enthält eine Beschreibung für das Verwalten und Warten einer Datenbank, die als Verleger in einer SQL-Replikation fungiert und auch in einer Always On-Verfügbarkeitsgruppe beteiligt ist.
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3068eaae089c4a9e7cd6ab2599eb04695d56aa6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348434"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>Verwalten einer replizierten Verlegerdatenbank als Teil einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema werden besondere Überlegungen zum Verwalten einer Veröffentlichungsdatenbank bei Verwendung von Always On-Verfügbarkeitsgruppen erläutert.  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> Verwalten einer veröffentlichten Datenbank in einer Verfügbarkeitsgruppe  
 Die Wartung einer Always On-Veröffentlichungsdatenbank entspricht im Wesentlichen der Wartung einer standardmäßigen Veröffentlichungsdatenbank, wobei jedoch die folgenden Überlegungen zu berücksichtigen sind:  
  
-   Die Verwaltung muss beim primären Replikathost erfolgen. In [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]werden Veröffentlichungen unter dem Ordner **Lokale Veröffentlichungen** für den primären Replikathost und auch für lesbare sekundäre Replikate angezeigt. Nach einem Failover müssen Sie unter Umständen [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] manuell aktualisieren, um die Änderung widerzuspiegeln, wenn das sekundäre Replikat, das zum primären Replikat höher gestuft wurde, nicht lesbar war.  
  
-   Der Replikationsmonitor zeigt immer Veröffentlichungsinformationen unter dem ursprünglichen Verleger an. Diese Informationen können jedoch von einem beliebigen Replikat aus im Replikationsmonitor angezeigt werden, indem der ursprüngliche Verleger als Server hinzugefügt wird.  
  
-   Bei Verwendung von gespeicherten Prozeduren oder Replikationsverwaltungsobjekten (RMO, Replication Management Objects) zum Verwalten der Replikation im primären Replikat müssen Sie in Fällen, in denen Sie den Verlegernamen angeben, den Namen der Instanz angeben, auf der die Datenbank für die Replikation aktiviert wurde. Den entsprechenden Namen ermitteln Sie mithilfe der **PUBLISHINGSERVERNAME** -Funktion. Wenn eine Veröffentlichungsdatenbank einer Verfügbarkeitsgruppe beitritt, sind die in den sekundären Datenbankreplikaten gespeicherten Replikationsmetadaten mit denen im primären Replikat identisch. Demzufolge entspricht für Veröffentlichungsdatenbanken, die in der primären Replikatdatenbank zur Replikation aktiviert wurden, der in den Systemtabellen des sekundären Replikats gespeicherte Verlegerinstanzname dem Namen der primären Replikatdatenbank und nicht dem Namen der sekundären Replikatdatenbank. Dies wirkt sich auf die Replikationskonfiguration und -wartung aus, wenn ein Failover der Veröffentlichungsdatenbank zur sekundären Replikatdatenbank erfolgt. Wenn Sie z. B. die Replikation mit gespeicherten Prozeduren für ein sekundäres Replikat nach einem Failover konfigurieren und ein Pullabonnement für eine Veröffentlichungsdatenbank aktivieren möchten, die für ein anderes Replikat aktiviert wurde, müssen Sie als *\@publisher*-Parameter von **sp_addpullsubscription** oder **sp_addmergepullsubscription** den Namen des ursprünglichen anstelle des aktuellen Verlegers angeben. Wenn Sie jedoch eine Veröffentlichungsdatenbank nach einem Failover aktivieren, entspricht der in den Systemtabellen gespeicherte Verlegerinstanzname dem Namen des aktuellen primären Hosts. In diesem Fall würden Sie den Hostnamen des aktuellen primären Replikats für den *\@publisher*-Parameter verwenden.  
  
    > [!NOTE]  
    >  Bei einigen Prozeduren, z. B. **sp_addpublication**, wird der *\@publisher*-Parameter nur für Verleger unterstützt, die keine Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sind. In diesen Fällen ist er für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Always On nicht relevant.  
  
-   Synchronisieren Sie die Pullabonnements vom Abonnenten und die Pushabonnements vom aktiven Verleger aus, um ein Abonnement in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] nach einem Failover zu synchronisieren.  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> Entfernen einer veröffentlichten Datenbank aus einer Verfügbarkeitsgruppe  
 Berücksichtigen Sie die folgenden Probleme, wenn eine veröffentlichte Datenbank aus einer Verfügbarkeitsgruppe entfernt wird, oder wenn eine Verfügbarkeitsgruppe, die eine veröffentlichte Elementdatenbank aufweist, gelöscht wird.  
  
-   Wenn die Veröffentlichungsdatenbank beim ursprünglichen Verleger aus einem primären Replikat der Verfügbarkeitsgruppe entfernt wird, müssen Sie **sp_redirect_publisher** ausführen, ohne einen Wert für den *\@redirected_publisher*-Parameter anzugeben, um die Umleitung für das Verleger-/Datenbankpaar zu entfernen.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     Die Datenbank wird auf dem primären Replikat im Wiederherstellungsstatus belassen und muss wiederhergestellt werden. Sobald Sie dies tun, sollte die Replikation unverändert mit dem ursprünglichen Verleger funktionieren.  
  
-   Wenn für die Veröffentlichungsdatenbank ein Failover vom ursprünglichen Verleger zu einem Replikat ausgeführt und die Datenbank aus dem primären Replikat der Verfügbarkeitsgruppe entfernt wird, führen Sie die gespeicherte Prozedur **sp_redirect_publisher** aus, um den ursprünglichen Verleger zum neuen Verleger umzuleiten. Die Datenbank wird im Wiederherstellungsstatus belassen und muss wiederhergestellt werden. Sobald Sie dies tun, sollte die Replikation weiterhin so funktionieren wie zuvor in der Verfügbarkeitsgruppe.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     Entfernen Sie den Remoteserver für den ursprünglichen Verleger selbst dann nicht aus dem Verteiler, wenn nicht mehr auf den Server zugegriffen werden kann. Die Servermetadaten für den ursprünglichen Verleger werden beim Verteiler benötigt, um Abfragen von Veröffentlichungsmetadaten beantworten zu können  
  
-   Wenn eine vollständige Verfügbarkeitsgruppe entfernt wird, wirkt sich dies auf eine replizierte Mitgliedsdatenbank auf die gleiche Weise aus, wie das Entfernen einer veröffentlichten Datenbank aus einer Verfügbarkeitsgruppe. Die Replikation kann vom letzten primären Replikat fortgesetzt werden, sobald die Datenbank wiederhergestellt und die Umleitung geändert wurde. Wenn die Datenbank auf ihrem ursprünglichen Verleger wiederhergestellt wird, sollte die Umleitung entfernt werden. Wenn die Datenbank bei einem anderen Host wiederhergestellt wird, sollte Umleitung explizit an den neuen Host umgeleitet werden.  
  
    > [!NOTE]  
    >  Wenn eine Verfügbarkeitsgruppe entfernt wird, die über veröffentlichte Mitgliedsdatenbanken verfügt, oder wenn eine veröffentlichte Datenbank aus einer Verfügbarkeitsgruppe entfernt wird, werden alle Kopien der veröffentlichten Datenbanken im Wiederherstellungsstatus belassen. Nach der Wiederherstellung wird jede Datenbank als veröffentlichte Datenbank angezeigt. Nur eine Kopie sollte mit Veröffentlichungsmetadaten beibehalten werden. Um die Replikation für eine veröffentlichte Datenbankkopie zu deaktivieren, entfernen Sie zuerst alle Abonnements und Veröffentlichungen aus der Datenbank.  
  
     Führen Sie **sp_dropsubscription** aus, um die Veröffentlichungsabonnements zu entfernen. Vergewissern Sie sich, dass der Parameter *\@ignore_distributor* auf 1 festgelegt ist, um die Metadaten beim Verteiler für die aktive Veröffentlichungsdatenbank beizubehalten.  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     Führen Sie **sp_droppublication** aus, um alle Veröffentlichungen zu entfernen. Vergewissern Sie sich wieder, dass der Parameter *\@ignore_distributor* auf 1 festgelegt ist, um die Metadaten beim Verteiler für die aktive Veröffentlichungsdatenbank beizubehalten.  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     Führen Sie **sp_replicationdboption** aus, um die Replikation für die Datenbank zu deaktivieren.  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     An diesem Punkt kann die Kopie der veröffentlichten Datenbank beibehalten oder gelöscht werden.  

## <a name="remove-original-publisher"></a>Entfernen des ursprünglichen Herausgebers

Es kann Situationen geben (Austausch eines älteren Servers, Upgrade des Betriebssystems usw.), in denen Sie einen ursprünglichen Herausgeber aus einer Always On-Verfügbarkeitsgruppe entfernen möchten. Führen Sie die Schritte in diesem Abschnitt aus, um den Herausgeber aus der Verfügbarkeitsgruppe zu entfernen. 

Angenommen, Sie verfügen über die Server N1, N2 und D1, wobei N1 und N2 das primäre und sekundäre Replikat der Verfügbarkeitsgruppe AG1 sind, N1 der ursprüngliche Herausgeber einer Transaktionsveröffentlichung und D1 der Verteiler ist. Sie möchten den ursprünglichen Herausgeber N1 durch den neuen Herausgeber N3 ersetzen. 

Gehen Sie folgendermaßen vor, um den Herausgeber zu entfernen: 

1. Installieren und konfigurieren Sie SQL Server auf dem Knoten N3. Die Version von SQL Server muss dieselbe sein wie die des ursprünglichen Herausgebers. 
1. Fügen Sie N3 auf dem Verteilerserver D1 mithilfe von [sp_adddistpublisher](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) als Herausgeber hinzu. 
1. Konfigurieren Sie N3 als Herausgeber mit D1 als seinem Verteiler. 
1. Fügen Sie N3 als Replikat zur Verfügbarkeitsgruppe AG1 hinzu. 
1. Überprüfen Sie auf dem N3-Replikat, dass die Pushabonnenten für die Veröffentlichung als verknüpfte Server angezeigt werden. Verwenden Sie entweder [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) oder SQL Server Management Studio. 
1. Nachdem N3 synchronisiert ist, führen Sie einen Failover der Verfügbarkeitsgruppe zu N3 als primären Knoten durch. 
1. Entfernen Sie N1 aus der Verfügbarkeitsgruppe AG1. 

Berücksichtigen Sie folgende Punkte:
- Entfernen Sie weder den Remoteserver des ursprünglichen Herausgebers (in diesem Fall N1) noch damit verbundenen Metadaten aus dem Verteiler, auch wenn auf den Server nicht mehr zugegriffen werden kann. Die Servermetadaten für den ursprünglichen Herausgeber werden beim Verteiler benötigt, um Abfragen von Veröffentlichungsmetadaten beantworten zu können, und ohne sie tritt ein Fehler bei der Replikation auf. 
- Für SQL Server 2014 können Sie, sobald der ursprüngliche Herausgeber entfernt wurde, den ursprünglichen Herausgebernamen nicht mehr für die Verwaltung der Replikation im Replikationsmonitor verwenden. Wenn Sie versuchen, neue Replikate als Herausgeber im Replikationsmonitor zu registrieren, werden keine Informationen angezeigt, da keine Metadaten damit verbunden sind. Zur Verwaltung der Replikation in diesem Szenario müssen Sie in SQL Server Management Studio (SSMS) mit der rechten Maustaste auf einzelne Veröffentlichungen und Abonnements klicken.
- Für SQL Server 2016 SP2-CU3, SQL Server 2017 CU6 und höher registrieren Sie den Listener des Verfügbarkeitsgruppenherausgebers im Replikationsmonitor, um die Replikation mithilfe von SQL Server Management Studio Version 17.7 und höher zu verwalten. 
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren der Replikation für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Replikation, Änderungsnachverfolgung, Change Data Capture und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [Häufig gestellte Fragen für Replikationsadministratoren](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [Replikationsabonnenten und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [SQL Server-Replikation](../../../relational-databases/replication/sql-server-replication.md)  
  
  
