---
title: Konfigurieren der Replikation mit Verfügbarkeitsgruppen
description: In diesem Artikel erfahren Sie mehr über den ausführlichen Prozess zum Konfigurieren der SQL Server-Replikation mit Ihrer AlwaysOn-Verfügbarkeitsgruppe.
ms.custom: seodec18
ms.date: 01/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 4e001426-5ae0-4876-85ef-088d6e3fb61c
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 727542650a3a310e0cdfecace5997ca31d015176
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343357"
---
# <a name="configure-replication-with-always-on-availability-groups"></a>Konfigurieren der Replikation mit Always On-Verfügbarkeitsgruppen

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Die Konfiguration der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Replikation und von Always On-Verfügbarkeitsgruppen umfasst sieben Schritte. Jeder dieser Schritte wird in den folgenden Abschnitten detailliert beschrieben.  
  
##  <a name="1-configure-the-database-publications-and-subscriptions"></a><a name="step1"></a> 1. Konfigurieren der Datenbankveröffentlichungen und Abonnements  
 **Konfigurieren des Verteilers**  
  
 Die Verteilungsdatenbank kann nicht in eine Verfügbarkeitsgruppe mit SQL Server 2012 und SQL Server 2014 eingefügt werden. Die Platzierung der Verteilungsdatenbank in einer Verfügbarkeitsgruppe wird mit SQL 2016 und höher unterstützt, mit Ausnahme von Verteilungsdatenbanken, die in Mergetopologien, bidirektionalen Topologien oder Peer-zu-Peer-Replikationstopologien verwendet werden. Weitere Informationen finden Sie unter [Konfigurieren einer Verteilungsdatenbank in einer Verfügbarkeitsgruppe](../../../relational-databases/replication/configure-distribution-availability-group.md).
  
1.  Konfigurieren Sie Verteilung beim Verteiler. Wenn gespeicherte Prozeduren zur Konfiguration verwendet werden, führen Sie **sp_adddistributor** aus. Verwenden Sie den *\@password* -Parameter, um das Kennwort zu identifizieren, das verwendet wird, wenn ein Remoteverleger eine Verbindung mit dem Verteiler herstellt. Das Kennwort wird auch bei jedem Remoteverleger benötigt, wenn der Remoteverteiler eingerichtet wird.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = '**Strong password for distributor**';  
    ```  
  
2.  Erstellen Sie die Verteilungsdatenbank beim Verteiler. Wenn gespeicherte Prozeduren zur Konfiguration verwendet werden, führen Sie **sp_adddistributiondb** aus.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributiondb  
        @database = 'distribution',  
        @security_mode = 1;  
    ```  
  
3.  Konfigurieren Sie den Remoteverleger. Wenn gespeicherte Prozeduren zur Konfiguration des Verteilers verwendet werden, führen Sie **sp_adddistpublisher** aus. Mit dem Parameter *\@security_mode* wird festgelegt, wie die gespeicherte Prozedur zur Verlegerüberprüfung, die von den Replikations-Agents ausgeführt wird, eine Verbindung mit dem aktuellen primären Replikat herstellt. Wenn der Parameter auf 1 festgelegt ist, wird die Windows-Authentifizierung verwendet, um eine Verbindung mit dem aktuellen primären Replikat herzustellen. Wenn er auf 0 festgelegt ist, wird die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Authentifizierung mit den angegebenen *\@login*- und *\@password*-Werten verwendet. Die Anmeldedaten und das Kennwort, die angegeben wurden, müssen bei jedem sekundären Replikat gültig sein, damit die gespeicherte Prozedur zur Überprüfung erfolgreich eine Verbindung mit diesem Replikat herstellen kann.  
  
    > [!NOTE]  
    >  Wenn geänderte Replikations-Agents auf einem anderen Computer als dem Verteiler ausgeführt werden, dann ist bei Verwendung der Windows-Authentifizierung zum Herstellen einer Verbindung zum primären Replikat erforderlich, dass die Kerberos-Authentifizierung für die Kommunikation zwischen den Replikathostcomputern konfiguriert wird. Bei Verwendung einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldung zum Herstellen einer Verbindung mit dem aktuellen primären Replikat ist keine Kerberos-Authentifizierung erforderlich.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistpublisher  
        @publisher = 'AGPrimaryReplicaHost',  
        @distribution_db = 'distribution',  
        @working_directory = '\\MyReplShare\WorkingDir',  
        @login = 'MyPubLogin',  
        @password = '**Strong password for publisher**';  
    ```  
  
 Weitere Informationen finden Sie unter [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
 **Konfigurieren des Verlegers beim ursprünglichen Verleger**  
  
1.  Konfigurieren Sie die Remoteverteilung. Wenn gespeicherte Prozeduren zur Konfiguration des Verlegers verwendet werden, führen Sie **sp_adddistributor** aus. Geben Sie den Wert für *\@password* an, der verwendet wurde, als **sp_adddistrbutor** beim Verteiler ausgeführt wurde, um die Verteilung einzurichten.  
  
    ```  
    exec sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = 'MyDistPass'  
    ```  
  
2.  Aktivieren Sie die Datenbank für die Replikation. Wenn gespeicherte Prozeduren zur Konfiguration des Verlegers verwendet werden, führen Sie **sp_replicationdboption** aus. Wenn sowohl Transaktions- als auch Mergereplikation für die Datenbank konfiguriert werden sollen, müssen beide aktiviert werden.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'true';  
  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'merge publish',  
        @value = 'true';  
    ```  
  
3.  Erstellen Sie die Replikationsveröffentlichung, Artikel und Abonnements. Weitere Informationen zum Konfigurieren der Replikation finden Sie unter "Veröffentlichen von Daten und Datenbankobjekten".  
  
##  <a name="2-configure-the-always-on-availability-group"></a><a name="step2"></a> 2. Konfigurieren der Always On-Verfügbarkeitsgruppe  
 Erstellen Sie beim vorgesehenen primären Replikat die Veröffentlichungsgruppe, und ordnen Sie ihr die veröffentlichte (oder zu veröffentlichende) Datenbank als Elementdatenbank zu. Wenn Sie den Verfügbarkeitsgruppen-Assistenten verwenden, können Sie es entweder dem Assistenten erlauben, die sekundären Replikatdatenbanken zum ersten Mal zu synchronisieren, oder Sie können die Initialisierung mit Sicherung und Wiederherstellung manuell ausführen.  
  
 Erstellen Sie einen DNS-Listener für die Verfügbarkeitsgruppe, die von den Replikations-Agents verwendet wird, um eine Verbindung mit dem aktuellen Primären herzustellen. Der angegebene Listenername wird als Umleitungsziel für das aus ursprünglichem Verleger und veröffentlichter Datenbank bestehende Paar verwendet. Wenn Sie die Verfügbarkeitsgruppe beispielsweise mithilfe von DDL konfigurieren, kann das folgende Codebeispiel zur Angabe eines Verfügbarkeitsgruppenlisteners für eine vorhandene Verfügbarkeitsgruppe mit dem Namen `MyAG` verwendet werden:  
  
```  
ALTER AVAILABILITY GROUP 'MyAG'   
    ADD LISTENER 'MyAGListenerName' (WITH IP (('10.120.19.155', '255.255.254.0')));  
```  
  
 Weitere Informationen finden Sie unter [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md).  

  
##  <a name="3-ensure-that-all-of-the-secondary-replica-hosts-are-configured-for-replication"></a><a name="step3"></a> 3. Stellen Sie sicher, dass alle sekundären Replikathosts für die Replikation konfiguriert wurden.  
 Überprüfen Sie bei jedem sekundären Replikathost, ob [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] so konfiguriert wurde, dass die Replikation unterstützt wird. Die folgende Abfrage kann auf jedem sekundären Replikathost ausgeführt werden, um zu bestimmen, ob die Replikation installiert wurde:  
  
```  
USE master;  
GO  
DECLARE @installed int;  
EXEC @installed = sys.sp_MS_replication_installed;  
SELECT @installed;  
```  
  
 Wenn *\@installed* gleich 0 ist, muss die Replikation der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Installation hinzugefügt werden.  
  
##  <a name="4-configure-the-secondary-replica-hosts-as-replication-publishers"></a><a name="step4"></a> 4. Konfigurieren des sekundären Replikathosts als Replikationsverleger  
 Ein sekundäres Replikat kann nicht als Replikationsverleger oder Neuverleger fungieren, aber die Replikation kann so konfiguriert werden, dass das sekundäre Replikat nach einem Failover die Rolle übernehmen kann. Konfigurieren Sie beim Verteiler die Verteilung für jeden sekundären Replikathost. Geben Sie die Verteilungsdatenbank und das Arbeitsverzeichnis an, die angegeben wurden, als der ursprüngliche Verleger dem Verteiler hinzugefügt wurde. Wenn Sie gespeicherte Prozeduren zum Konfigurieren der Verteilung verwenden, führen Sie **sp_adddistpublisher** aus, um die Remoteverleger dem Verteiler zuzuordnen. Wenn *\@login* und *\@password* für den ursprünglichen Verleger verwendet wurden, geben Sie die gleichen Werte für die einzelnen sekundären Replikathosts an, die Sie als Verleger hinzufügen.  
  
```  
EXEC sys.sp_adddistpublisher  
    @publisher = 'AGSecondaryReplicaHost',  
    @distribution_db = 'distribution',  
    @working_directory = '\\MyReplShare\WorkingDir',  
    @login = 'MyPubLogin',  
    @password = '**Strong password for publisher**';  
```  
  
 Konfigurieren Sie die Verteilung auf jedem sekundären Replikathost. Identifizieren Sie den Verteiler des ursprünglichen Verlegers als Remoteverteiler. Verwenden Sie das Kennwort, das bei der ursprünglichen Ausführung von **sp_adddistributor** auf dem Verteiler verwendet wurde. Wenn gespeicherte Prozeduren zum Konfigurieren der Verteilung verwendet werden, wird der Parameter *\@password* von **sp_adddistributor** verwendet, um das Kennwort anzugeben.  
  
```  
EXEC sp_adddistributor   
    @distributor = 'MyDistributor',  
    @password = '**Strong password for distributor**';  
```  
  
 Stellen Sie bei jedem sekundären Replikathost sicher, dass die Pushabonnenten der Datenbankveröffentlichungen als Verbindungsserver angezeigt werden. Wenn gespeicherte Prozeduren zum Konfigurieren der Remoteverleger verwendet werden, führen Sie **sp_addlinkedserver** aus, um den Verlegern die Abonnenten (sofern nicht bereits vorhanden) als Verbindungsserver hinzuzufügen.  
  
```  
EXEC sys.sp_addlinkedserver   
    @server = 'MySubscriber';  
```  
  
##  <a name="5-redirect-the-original-publisher-to-the-ag-listener-name"></a><a name="step5"></a> 5. Umleiten des ursprünglichen Verlegers zum Namen des Verfügbarkeitsgruppenlisteners  
 Führen Sie auf dem Verteiler in der Verteilungsdatenbank die gespeicherte Prozedur **sp_redirect_publisher** aus, um den ursprünglichen Verleger und dem Namen des Verfügbarkeitsgruppenlisteners der Verfügbarkeitsgruppe die veröffentlichte Datenbank zuzuordnen.  
  
```  
USE distribution;  
GO  
EXEC sys.sp_redirect_publisher   
@original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = 'MyAGListenerName';  
```  
  
##  <a name="6-run-the-replication-validation-stored-procedure-to-verify-the-configuration"></a><a name="step6"></a> 6. Ausführen der gespeicherten Prozedur zur Replikationsüberprüfung, um die Konfiguration zu überprüfen  
 Führen Sie auf dem Verteiler in der Verteilungsdatenbank die gespeicherte Prozedur **sp_validate_replica_hosts_as_publishers** aus, um zu überprüfen, ob alle Replikathosts bereits so konfiguriert worden sind, um als Verleger für die veröffentlichte Datenbank zu fungieren.  
  
```  
USE distribution;  
GO  
DECLARE @redirected_publisher sysname;  
EXEC sys.sp_validate_replica_hosts_as_publishers  
    @original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = @redirected_publisher output;  
```  
  
 Die gespeicherte Prozedur **sp_validate_replica_hosts_as_publishers** sollte von einer Anmeldung mit ausreichender Autorisierung bei jedem Host des Verfügbarkeitsgruppenreplikats ausgeführt werden, um Informationen zur Verfügbarkeitsgruppe abzufragen. Im Gegensatz zu **sp_validate_redirected_publisher** verwendet diese gespeicherte Prozedur die Anmeldeinformationen des Aufrufers und nicht die in „msdb.dbo.MSdistpublishers“ gespeicherte Anmeldung , um eine Verbindung mit den Verfügbarkeitsgruppenreplikaten herzustellen.  
  
> [!NOTE]  
>  **sp_validate_replica_hosts_as_publishers** schlägt bei der Überprüfung sekundärer Replikathosts, die keinen Lesezugriff zulassen oder die Angabe der Leseabsicht erfordern, mit dem folgenden Fehler fehl.  
>   
>  Meldung 21899, Ebene 11, Status 1, Prozedur **sp_hadr_verify_subscribers_at_publisher**, Zeile 109  
>   
>  Die Abfrage beim umgeleiteten Verleger „MyReplicaHostName“ zur Bestimmung, ob sysserver-Einträge für die Abonnenten des ursprünglichen Verlegers „MyOriginalPublisher“ vorliegen, ist mit Fehler 976 und folgender Meldung fehlgeschlagen: „Fehler 976, Stufe 14, Status 1, Meldung: The target database, 'MyPublishedDB', is participating in an availability group and is currently not accessible for queries. (Die Zieldatenbank „MyPublishedDB“ ist an einer Verfügbarkeitsgruppe beteiligt, und Abfragen können derzeit nicht darauf zugreifen.) Entweder die Datenverschiebung wurde angehalten, oder für das Verfügbarkeitsreplikat wurde kein Schreibzugriff aktiviert. Um schreibgeschützten Zugriff auf diese und andere Datenbanken in der Verfügbarkeitsgruppe zuzulassen, aktivieren Sie den Lesezugriff auf mindestens ein sekundäres Verfügbarkeitsreplikat in der Gruppe.  Weitere Informationen finden Sie in der **ALTER AVAILABILITY GROUP** -Anweisung in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
>   
>  Es sind ein oder mehrere Verlegerüberprüfungsfehler für Replikathost 'MyReplicaHostName' aufgetreten.  
  
 Dieses Verhalten wird erwartet. Sie müssen das Vorhandensein der Abonnentenservereinträge bei diesen sekundären Replikathosts überprüfen, indem Sie die sysserver-Einträge im Host direkt abfragen.  
  
##  <a name="7-add-the-original-publisher-to-replication-monitor"></a><a name="step7"></a> 7. Hinzufügen des ursprünglichen Verlegers zum Replikationsmonitor  
 Fügen Sie dem Replikationsmonitor bei jedem Verfügbarkeitsgruppenreplikat den ursprünglichen Verleger hinzu.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **Replikation**  
  
-   [Warten einer Always On-Veröffentlichungsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [Replikation, Änderungsnachverfolgung, Change Data Capture und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [Häufig gestellte Fragen für Replikationsadministratoren](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **So erstellen und konfigurieren Sie eine Verfügbarkeitsgruppe**  
  
-   [Verwenden des Assistenten für Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
- [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
- [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
- [Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
- [SQL Server-Replikation](../../../relational-databases/replication/sql-server-replication.md)  
- Wenn Sie vom Standard abweichende Ports verwenden, finden Sie weitere Informationen unter [Walkthrough Publisher, Distributor, Subscriber in AlwaysOn Availability Groups (Exemplarische Vorgehensweise für Herausgeber, Verteiler und Abonnenten in AlwaysOn-Verfügbarkeitsgruppen)](https://repltalk.com/2019/03/09/walkthrough-publisher-distributor-subscriber-in-alwayson-availability-groups).
