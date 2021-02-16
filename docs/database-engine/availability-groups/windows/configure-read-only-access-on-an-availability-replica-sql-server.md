---
title: Konfigurieren des schreibgeschützten Zugriffs auf ein sekundäres Replikat einer Verfügbarkeitsgruppe
description: 'Konfigurieren Sie Ihr sekundäres Replikat, sodass der Lesezugriff für Ihre Always On-Verfügbarkeitsgruppe möglich ist. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 22387419-22c4-43fa-851c-5fecec4b049b
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab1e25991acdfc1b22310a8ffba603898adcf56e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342465"
---
# <a name="configure-read-only-access-to-a-secondary-replica-of-an-always-on-availability-group"></a>Konfigurieren des schreibgeschützten Zugriffs auf ein sekundäres Replikat einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Standardmäßig sind sowohl der Lese-/Schreibzugriff als auch der Zugriff für beabsichtigte Lesevorgänge für das primäre Replikat zulässig, während für sekundäre Replikate einer AlwaysOn-Verfügbarkeitsgruppe keine Verbindungen zulässig sind. In diesem Thema wird beschrieben, wie der Verbindungszugriff für ein Verfügbarkeitsreplikat einer AlwaysOn-Verfügbarkeitsgruppe in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] unter Verwendung von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell konfiguriert wird.  
  
 Informationen dazu, welche Auswirkungen die Aktivierung des schreibgeschützten Zugriffs für ein sekundäres Replikat hat, und eine Einführung in den Verbindungszugriff finden Sie unter [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md) und [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Voraussetzungen und Einschränkungen  
  
-   Um einen anderen Verbindungszugriff zu konfigurieren, benötigen Sie eine Verbindung zur Serverinstanz, die das primäre Replikat hostet.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
|Aufgabe|Berechtigungen|  
|----------|-----------------|  
|So konfigurieren Sie Replikate beim Erstellen einer Verfügbarkeitsgruppe|Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.|  
|So ändern Sie ein Verfügbarkeitsreplikat|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So konfigurieren Sie den Zugriff auf einem Verfügbarkeitsreplikat**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie auf die Verfügbarkeitsgruppe, deren Replikat geändert werden soll.  
  
4.  Klicken Sie mit der rechten Maustaste auf das Verfügbarkeitsreplikat, und klicken Sie auf **Eigenschaften**.  
  
5.  Im Dialogfeld **Eigenschaften des Verfügbarkeitsreplikats** können Sie den Verbindungszugriff für die primäre Rolle und die sekundäre Rolle wie folgt ändern:  
  
    -   Wählen Sie für die sekundäre Rolle aus der Dropdownliste für die **lesbare sekundäre** Rolle wie folgt einen neuen Wert aus:  
  
         **Nein**  
         Es werden keine Verbindungen mit sekundären Datenbanken dieses Replikats zugelassen. Sie sind für den Lesezugriff nicht verfügbar. Dies ist die Standardeinstellung.  
  
         **Nur beabsichtigte Lesevorgänge**  
         Es sind nur schreibgeschützte Verbindungen zu sekundären Datenbanken dieses Replikats zulässig. Die sekundären Datenbanken sind alle für Lesezugriff verfügbar.  
  
         **Ja**  
         Alle Verbindungen zu sekundären Datenbanken dieses Replikats sind zugelassen, aber nur für Lesezugriff. Die sekundären Datenbanken sind alle für Lesezugriff verfügbar.  
  
    -   Wählen Sie für die primäre Rolle aus der Dropdownliste für **Verbindungen in der primären Rolle** einen neuen Wert wie folgt aus:  
  
         **Alle Verbindungen zulassen**  
         Für die Datenbanken im primären Replikat sind alle Verbindungen zugelassen. Dies ist die Standardeinstellung.  
  
         **Verbindungen mit Lese-/Schreibzugriff zulassen**  
         Wenn die Eigenschaft für die Anwendungsabsicht auf **ReadWrite** festgelegt ist oder keine Verbindungseigenschaft für die Anwendungsabsicht festgelegt wurde, wird die Verbindung zugelassen. Verbindungen, bei denen die Verbindungseigenschaft für die Anwendungsabsicht auf **ReadOnly** festgelegt ist, werden zugelassen. Dies kann verhindern, dass Kunden mit dem primären Replikat versehentlich eine leseintensive Arbeitsauslastung verbinden. Weitere Informationen zur Verbindungseigenschaft für die Anwendungsabsicht finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So konfigurieren Sie den Zugriff auf einem Verfügbarkeitsreplikat**  
  
> [!NOTE]  
>  Ein Beispiel für diese Prozedur finden Sie weiter unten in diesem Abschnitt unter [Beispiel (Transact-SQL)](#TsqlExample).  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Wenn Sie ein Replikat für eine neue Verfügbarkeitsgruppe angeben, verwenden Sie die [-Anweisung](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Verwenden Sie zum Hinzufügen oder Ändern eines Replikats für eine vorhandene Verfügbarkeitsgruppe die [-Anweisung](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Geben Sie zum Konfigurieren des Verbindungszugriffs für die sekundäre Rolle in der ADD REPLICA- bzw. MODIFY REPLICA WITH-Klausel die SECONDARY_ROLE-Option wie folgt an:  
  
         SECONDARY_ROLE **(** ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL } **)**  
  
         Erläuterungen:  
  
         Nein  
         Es werden keine direkten Verbindungen mit sekundären Datenbanken dieses Replikats zugelassen. Sie sind für den Lesezugriff nicht verfügbar. Dies ist die Standardeinstellung.  
  
         READ_ONLY  
         Es sind nur schreibgeschützte Verbindungen zu sekundären Datenbanken dieses Replikats zulässig. Die sekundären Datenbanken sind alle für Lesezugriff verfügbar.  
  
         ALL  
         Alle Verbindungen zu sekundären Datenbanken dieses Replikats sind zugelassen, aber nur für Lesezugriff. Die sekundären Datenbanken sind alle für Lesezugriff verfügbar.  
  
3.  Geben Sie zum Konfigurieren des Verbindungszugriffs für die primäre Rolle in der ADD REPLICA- bzw. MODIFY REPLICA WITH-Klausel die PRIMARY_ROLE-Option wie folgt an:  
  
     PRIMARY_ROLE **(** ALLOW_CONNECTIONS **=** { READ_WRITE | ALL } **)**  
  
     Erläuterungen:  
  
     READ_WRITE  
     Verbindungen, bei denen die Verbindungseigenschaft für den Anwendungszweck auf **ReadOnly** festgelegt ist, werden nicht zugelassen.  Wenn die Eigenschaft für die Anwendungsabsicht auf **ReadWrite** festgelegt ist oder keine Verbindungseigenschaft für die Anwendungsabsicht festgelegt wurde, wird die Verbindung zugelassen. Weitere Informationen zur Verbindungseigenschaft für die Anwendungsabsicht finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
     ALL  
     Für die Datenbanken im primären Replikat sind alle Verbindungen zugelassen. Dies ist die Standardeinstellung.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL)  
 Im folgenden Beispiel wird einer Verfügbarkeitsgruppe namens *AG2* ein sekundäres Replikat hinzugefügt. Zum Hosten des neuen Verfügbarkeitsreplikats wurde die eigenständige Serverinstanz *COMPUTER03\HADR_INSTANCE* angegeben. Dieses Replikat ist zum ausschließlichen Zulassen von Verbindungen mit Lese-/Schreibzugriff für die primäre Rolle sowie zum ausschließlichen Zulassen von Verbindungen mit beabsichtigten Lesevorgängen konfiguriert.  
  
```  
ALTER AVAILABILITY GROUP AG2   
   ADD REPLICA ON   
      'COMPUTER03\HADR_INSTANCE' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03:7022',  
         PRIMARY_ROLE ( ALLOW_CONNECTIONS = READ_WRITE ),  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY )  
         );   
GO  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So konfigurieren Sie den Zugriff auf einem Verfügbarkeitsreplikat**  
  
> [!NOTE]  
>  Ein Codebeispiel finden Sie weiter unten in diesem Abschnitt unter [Beispiel (PowerShell)](#PSExample).  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie zum Hinzufügen eines Verfügbarkeitsreplikats zu einer Verfügbarkeitsgruppe das Cmdlet **New-SqlAvailabilityReplica** . Verwenden Sie zum Ändern eines vorhandenen Verfügbarkeitsreplikats das Cmdlet **Set-SqlAvailabilityReplica** . Die relevanten Parameter lauten wie folgt:  
  
    -   Um den Verbindungszugriff für die sekundäre Rolle zu konfigurieren, geben Sie den Parameter **ConnectionModeInSecondaryRole**_secondary_role_keyword_ an, wobei *secondary_role_keyword* einem der folgenden Werte entspricht:  
  
         **AllowNoConnections**  
         Für die Datenbanken im sekundären Replikat sind keine direkten Verbindungen zugelassen, und die Datenbanken sind für den Lesezugriff nicht verfügbar. Dies ist die Standardeinstellung.  
  
         **AllowReadIntentConnectionsOnly**  
         Verbindungen mit den Datenbanken im sekundären Replikat sind nur zulässig, wenn die Eigenschaft für die Anwendungsabsicht auf **ReadOnly** festgelegt ist. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         Für alle Verbindungen mit den Datenbanken im sekundären Replikat ist der schreibgeschützte Zugriff zugelassen.  
  
    -   Um den Verbindungszugriff für die primäre Rolle zu konfigurieren, geben Sie **ConnectionModeInPrimaryRole**_primary_role_keyword_ an, wobei *primary_role_keyword* einem der folgenden Werte entspricht:  
  
         **AllowReadWriteConnections**  
         Verbindungen, bei denen die Verbindungseigenschaft für die Anwendungsabsicht auf ReadOnly festgelegt ist, werden nicht zugelassen. Wenn die Eigenschaft für die Anwendungsabsicht auf ReadWrite festgelegt ist oder keine Verbindungseigenschaft für die Anwendungsabsicht festgelegt wurde, wird die Verbindung zugelassen. Weitere Informationen zur Verbindungseigenschaft für die Anwendungsabsicht finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         Für die Datenbanken im primären Replikat sind alle Verbindungen zugelassen. Dies ist die Standardeinstellung.  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Beispiel (PowerShell)  
 Im folgenden Beispiel wird sowohl der **ConnectionModeInSecondaryRole** -Parameter als auch der **ConnectionModeInPrimaryRole** -Parameter auf **AllowAllConnections** festgelegt.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
Set-SqlAvailabilityReplica -ConnectionModeInSecondaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ConnectionModeInPrimaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
  
```  
  
##  <a name="follow-up-after-configuring-read-only-access-for-an-availability-replica"></a><a name="FollowUp"></a>Nächster Schritt: Nach der Konfiguration des schreibgeschützten Zugriffs für ein Verfügbarkeitsreplikat  
 **Schreibgeschützter Zugriff auf ein lesbares sekundäres Replikat**  
  
-   Bei Verwendung der Hilfsprogramme [bcp](../../../tools/bcp-utility.md) oder [sqlcmd](../../../tools/sqlcmd-utility.md)können Sie den schreibgeschützten Zugriff für jedes sekundäre Replikat angeben, das den schreibgeschützten Zugriff unterstützt, indem Sie den Switch **-K ReadOnly** angeben.  
  
-   So ermöglichen Sie, dass Clientanwendungen eine Verbindung mit lesbaren sekundären Replikaten herstellen können  
  
|Voraussetzung|Link|  
|------------------|----------|  
|Stellen Sie sicher, dass die Verfügbarkeitsgruppe über einen Listener verfügt.|[Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Konfigurieren Sie das schreibgeschützte Routing für eine Verfügbarkeitsgruppe.|[Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)|  
  
 **Faktoren, die sich auf Trigger und Aufträge nach einem Failover auswirken können**  
  
 Wenn Sie Trigger und Aufträge haben, die beim Ausführen auf einer nicht lesbaren sekundären Datenbank oder einer lesbaren sekundären Datenbank fehlschlagen, müssen Sie ein Skript für die Trigger und Aufträge erstellen, die auf einem angegebenen Replikat kontrolliert werden sollen, um zu bestimmen, ob die Datenbank eine primäre Datenbank oder eine lesbare sekundäre Datenbank ist. Um diese Informationen abzurufen, verwenden Sie die [DATABASEPROPERTYEX](../../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion, um die **Updatability**-Eigenschaft der Datenbank zurückzugeben. Um eine schreibgeschützte Datenbank zu identifizieren, geben Sie READ_ONLY wie folgt als Wert an:  
  
```  
DATABASEPROPERTYEX([db name],'UpdateAbility') = N'READ_ONLY'  
```  
  
 Um eine Datenbank mit Lese-/Schreibzugriff zu identifizieren, geben Sie READ_WRITE als Wert an.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Always On: Value Proposition of Readable Secondary (Always On: Leistungsversprechen lesbarer sekundärer Replikate)](/archive/blogs/sqlserverstorageengine/alwayson-value-proposition-of-readable-secondary)  
  
-   [Always On: Why there are two options to enable a secondary replica for read workload? (Always On: Warum gibt es zwei Optionen zum Aktivieren eines sekundären Replikats für die Lesearbeitslast?)](/archive/blogs/sqlserverstorageengine/alwayson-why-there-are-two-options-to-enable-a-secondary-replica-for-read-workload)  
  
-   [Always On: Setting up Readable Secondary Replica (Always On: Einrichten eines lesbaren sekundären Replikats)](/archive/blogs/sqlserverstorageengine/alwayson-setting-up-readable-seconary-replica)  
  
-   [Always On: I just enabled Readable Secondary but my query is blocked? (Always On: Ich habe gerade lesbare sekundäre Replikate aktiviert, meine Abfrage wird jedoch blockiert.)](/archive/blogs/sqlserverstorageengine/alwayson-i-just-enabled-readable-secondary-but-my-query-is-blocked)  
  
-   [Always On: Making latest statistics available on Readable Secondary, Read-Only database and Database Snapshot (Always On: Zur Verfügung stellen aktueller Statistiken auf lesbaren sekundären Replikaten, schreibgeschützten Datenbanken und Datenbankmomentaufnahmen)](/archive/blogs/sqlserverstorageengine/alwayson-making-latest-statistics-available-on-readable-secondary-read-only-database-and-database-snapshot)  
  
-   [Always On: Challenges with statistics on ReadOnly database, Database Snapshot and Secondary Replica (Always On: Herausforderungen bei Statistiken auf schreibgeschützten Datenbanken, Datenbank-Momentaufnahmen und sekundären Replikaten)](/archive/blogs/sqlserverstorageengine/alwayson-challenges-with-statistics-on-readonly-database-database-snapshot-and-secondary-replica)  
  
-   [Always On: Impact on the primary workload when you run reporting workload on the secondary replica (Always On: Auswirkungen auf die primäre Arbeitslast, wenn die Berichterstellungsarbeitslast auf dem sekundären Replikat ausgeführt wird)](/archive/blogs/sqlserverstorageengine/alwayson-impact-on-the-primary-workload-when-you-run-reporting-workload-on-the-secondary-replica)  
  
-   [Always On: Impact of mapping reporting workload on Readable Secondary to Snapshot Isolation (Always On: Auswirkungen der Zuordnung der Berichterstellungsarbeitslast vom lesbaren sekundären Replikat zur Momentaufnahmeisolation)](/archive/blogs/sqlserverstorageengine/alwayson-impact-of-mapping-reporting-workload-on-readable-secondary-to-snapshot-isolation)  
  
-   [Always On: Minimizing blocking of REDO thread when running reporting workload on Secondary Replica (Always On: Minimieren der Blockierung von REDO-Threads beim Ausführen der Berichterstellungsarbeitslast auf sekundären Replikaten)](/archive/blogs/sqlserverstorageengine/alwayson-minimizing-blocking-of-redo-thread-when-running-reporting-workload-on-secondary-replica)  
  
-   [Always On: Readable Secondary and data latency (Always On: Lesbares sekundäres Replikat und Datenlatenz)](/archive/blogs/sqlserverstorageengine/alwayson-readable-secondary-and-data-latency)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
