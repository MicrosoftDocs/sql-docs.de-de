---
title: Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe
description: Leiten Sie schreibgeschützten Datenverkehr automatisch an ein sekundäres Replikat mit schreibgeschütztem Routing für Ihre Always On-Verfügbarkeitsgruppe weiter – mithilfe von Transact-SQL (T-SQL) oder PowerShell.
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51bbc2416b84f5e3288a1e011d5dde8232c8b699
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343537"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>Konfigurieren des schreibgeschützten Routing für eine Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]können Sie eine AlwaysOn-Verfügbarkeitsgruppe mit [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder mit PowerShell für schreibgeschütztes Routing konfigurieren. *Schreibgeschütztes Routing* bezeichnet die Fähigkeit von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , schreibgeschützte Verbindungsanforderungen an ein verfügbares [lesbares sekundäres AlwaysOn-Replikat](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) weiterzuleiten (das heißt, an ein Replikat, das unter der sekundären Rolle für schreibgeschützte Arbeitsauslastungen konfiguriert ist). Um schreibgeschütztes Routing zu unterstützen, muss die Verfügbarkeitsgruppe einen [Verfügbarkeitsgruppenlistener](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)besitzen. Schreibgeschützte Clients müssen die eigenen Verbindungsanforderungen an diesen Listener weiterleiten, und in den Verbindungszeichenfolgen des Clients muss die Anwendungsabsicht als "schreibgeschützt" angeben sein. Es muss sich also um *Verbindungsanforderungen für beabsichtigte Lesevorgänge* handeln.  

Schreibgeschütztes Routing ist in [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] und höher verfügbar.

> [!NOTE]  
>  Informationen zum Konfigurieren eines lesbaren sekundären Replikats finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)besitzen.  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Die Verfügbarkeitsgruppe muss über einen Verfügbarkeitsgruppenlistener verfügen. Weitere Informationen finden Sie unter [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   Mindestens ein Verfügbarkeitsreplikat muss so konfiguriert werden, dass es in der sekundären Rolle schreibgeschützte Verbindungen akzeptiert (das heißt, ein [lesbares sekundäres Replikat](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)ist). Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)besitzen.  
  
-   Sie müssen mit der Serverinstanz verbunden sein, auf der das aktuelle primäre Replikat gehostet wird.  

-   Wenn Sie eine SQL-Anmeldung verwenden, stellen Sie sicher, dass das Konto ordnungsgemäß konfiguriert ist. Weitere Informationen finden Sie unter [Verwaltung von Anmeldungen und Aufträgen für die Datenbanken einer Verfügbarkeitsgruppe (SQL Server)](logins-and-jobs-for-availability-group-databases.md).
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> Welche Replikateigenschaften müssen Sie konfigurieren, um schreibgeschütztes Routing zu unterstützen?  
  
-   Für jedes lesbare sekundäre Replikat, das schreibgeschütztes Routing unterstützen soll, müssen Sie eine *URL für schreibgeschütztes Routing* angeben. Diese URL wird nur wirksam, wenn das lokale Replikat unter der sekundären Rolle ausgeführt wird. Die URL für schreibgeschütztes Routing muss nach Bedarf replikatweise angegeben werden. Jede URL für schreibgeschütztes Routing wird zum Weiterleiten von Verbindungsanforderungen für beabsichtigte Lesevorgänge an ein bestimmtes lesbares sekundäres Replikat verwendet. In der Regel wird jedem lesbaren sekundären Replikat eine URL für schreibgeschütztes Routing zugewiesen.  
  
     Informationen zum Berechnen der schreibgeschützten Routing-URL für ein Verfügbarkeitsreplikat finden Sie unter [Berechnen von read_only_routing_url für AlwaysOn](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   Für jedes Verfügbarkeitsreplikat, das als primäres Replikat schreibgeschütztes Routing unterstützen soll, müssen Sie eine *Liste für schreibgeschütztes Routing* angeben. Eine Liste für schreibgeschütztes Routing wird nur wirksam, wenn das lokale Replikat unter der primären Rolle ausgeführt wird. Diese Liste muss nach Bedarf replikatweise angegeben werden. Normalerweise enthält jede Liste für schreibgeschütztes Routing jede URL für schreibgeschütztes Routing, wobei die URL des lokalen Replikats am Ende der Liste steht.  
  
    > [!NOTE]  
    >  Verbindungsanforderungen für beabsichtigte Lesevorgänge werden an den ersten verfügbaren Eintrag auf der Liste für schreibgeschütztes Routing des aktuellen primären Replikats weitergeleitet. Lastenausgleich über schreibgeschützte Replikate hinweg wird allerdings unterstützt. Weitere Informationen finden Sie unter [Konfigurieren von Lastenausgleich über schreibgeschützte Replikate hinweg](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
> [!NOTE]  
>  Weitere Informationen zu Verfügbarkeitsgruppenlistenern und zum schreibgeschützten Routing finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)besitzen.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
|Aufgabe|Berechtigungen|  
|----------|-----------------|  
|So konfigurieren Sie Replikate beim Erstellen einer Verfügbarkeitsgruppe|Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.|  
|So ändern Sie ein Verfügbarkeitsreplikat|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>Konfigurieren einer schreibgeschützten Routingliste  
 Verwenden Sie die folgenden Schritte, um schreibgeschütztes Routing mit Transact-SQL zu konfigurieren. Ein Codebeispiel finden Sie weiter unten in diesem Abschnitt unter [Beispiel (Transact-SQL)](#TsqlExample).  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Wenn Sie ein Replikat für eine neue Verfügbarkeitsgruppe angeben, verwenden Sie die [-Anweisung](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Verwenden Sie zum Hinzufügen oder Ändern eines Replikats für eine vorhandene Verfügbarkeitsgruppe die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung.  
  
    -   Geben Sie zum Konfigurieren des schreibgeschützten Routings für die sekundäre Rolle in der ADD REPLICA- bzw. MODIFY REPLICA WITH-Klausel die SECONDARY_ROLE-Option wie folgt an:  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         Die URL für das schreibgeschützte Routing verfügt über die folgenden Parameter:  
  
         *system-address*  
         Ist eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die das Zielcomputersystem eindeutig identifiziert.  
  
         *port*  
         Ist eine Portnummer, die von der Datenbank-Engine der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz verwendet wird.  
  
         Beispiel:   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         In einer MODIFY REPLICA-Klausel ist ALLOW_CONNECTIONS optional, wenn das Replikat bereits so konfiguriert worden ist, dass es schreibgeschützte Verbindungen zulässt.  
  
         Weitere Informationen finden Sie unter [Berechnen von read_only_routing_url für AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Geben Sie zum Konfigurieren des schreibgeschützten Routings für die primäre Rolle in der ADD REPLICA- bzw. MODIFY REPLICA WITH-Klausel die PRIMARY_ROLE-Option wie folgt an:  
  
         PRIMARY_ROLE **(** READ_ONLY_ROUTING_LIST **=('** _server_ **'** [ **,** ...*n* ] **))**  
  
         wobei *server* eine Serverinstanz identifiziert, die ein schreibgeschütztes sekundäres Replikat in der Verfügbarkeitsgruppe hostet.  
  
         Beispiel:   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  Sie müssen die URL für das schreibgeschützte Routing festlegen, bevor Sie die schreibgeschützte Routingliste festlegen.  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> Konfigurieren von Lastenausgleich über schreibgeschützte Replikate hinweg  
 Ab [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)]können Sie den Lastenausgleich über eine Reihe von schreibgeschützten Replikaten hinweg konfigurieren. Zuvor wurde der Datenverkehr beim schreibgeschützten Routing an das erste verfügbare Replikat in der Routingliste geleitet. Um dieses Feature nutzen zu können, verwenden Sie eine Ebene geschachtelter Klammern um die **READ_ONLY_ROUTING_LIST** -Serverinstanzen der Befehle **CREATE AVAILABILITY GROUP** oder **ALTER AVAILABILITY GROUP** .  
  
 Die folgende Routingliste führt beispielsweise einen Lastenausgleich für Verbindungsanfragen mit Leseabsicht über zwei schreibgeschützte Replikate, `Server1` und `Server2`, hinweg aus. Die geschachtelten Klammern, die diese Server umgeben, identifizieren den Satz mit Lastenausgleich. Wenn keines der Replikate in diesem Satz verfügbar ist, wird versucht, sequenziell eine Verbindung zu den anderen Replikaten ( `Server3` und `Server4`) in der schreibgeschützten Routingliste herzustellen.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 Beachten Sie, dass jeder Eintrag in der Routingliste selbst ein Satz mit schreibgeschützten Lastenausgleichsreplikaten sein kann. Das wird im folgenden Beispiel veranschaulicht.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 Nur eine Ebene geschachtelter Klammern wird unterstützt.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL)  
 Im folgenden Beispiel werden zwei Verfügbarkeitsreplikate einer vorhandenen Verfügbarkeitsgruppe `AG1` geändert, sodass schreibgeschütztes Routing unterstützt wird, wenn eines dieser Replikate die primäre Rolle besitzt. In diesem Beispiel werden die Instanznamen `COMPUTER01` und `COMPUTER02` zur Identifikation der Serverinstanzen angegeben, die das Verfügbarkeitsreplikat hosten.  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>Konfigurieren einer schreibgeschützten Routingliste  
 Verwenden Sie die folgenden Schritte, um schreibgeschütztes Routing mit PowerShell zu konfigurieren. Ein Codebeispiel finden Sie weiter unten in diesem Abschnitt unter [Beispiel (PowerShell)](#PSExample).  
  
1.  Legen Sie mit (**cd**) die Serverinstanz als Standard fest, die das primäre Replikat hostet.  
  
2.  Verwenden Sie zum Hinzufügen eines Verfügbarkeitsreplikats zu einer Verfügbarkeitsgruppe das Cmdlet **New-SqlAvailabilityReplica** . Verwenden Sie zum Ändern eines vorhandenen Verfügbarkeitsreplikats das Cmdlet **Set-SqlAvailabilityReplica** . Die relevanten Parameter lauten wie folgt:  
  
    -   Um das schreibgeschützte Routing für die sekundäre Rolle zu konfigurieren, legen Sie den Parameter **ReadonlyRoutingConnectionUrl"** _url_ **"** fest.  
  
         wobei *url* für den vollqualifizierten Domänennamen (FQDN) und Port der Verbindung steht, die beim Routing zum Replikat für schreibgeschützte Verbindungen verwendet werden. Beispiel: `-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         Weitere Informationen finden Sie unter [Berechnen von read_only_routing_url für AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Um den Verbindungszugriff für die primäre Rolle zu konfigurieren, geben Sie **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ] an, wobei *server* eine Serverinstanz identifiziert, die in der Verfügbarkeitsgruppe ein schreibgeschütztes sekundäres Replikat hostet. Beispiel: `-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  Sie müssen die URL für das schreibgeschützte Routing für ein Replikat festlegen, bevor Sie dessen schreibgeschützte Routingliste festlegen.  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>Einrichten und Verwenden des SQL Server PowerShell-Anbieters  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Beispiel (PowerShell)  
 Im folgenden Beispiel werden das primäre Replikat und ein sekundäres Replikat in einer Verfügbarkeitsgruppe für das schreibgeschützte Routing konfiguriert. Im Beispiel wird zuerst jedem Replikat eine URL für das schreibgeschützte Routing zugewiesen. Anschließend wird die Liste für schreibgeschütztes Routing auf dem primären Replikat festgelegt. Verbindungen, für die in der Verbindungszeichenfolge die ReadOnly-Eigenschaft festgelegt wurde, werden an das sekundäre Replikat umgeleitet. Wenn dieses sekundäre Replikat nicht gelesen werden kann (durch die **ConnectionModeInSecondaryRole** -Einstellung vorgegeben), wird die Verbindung wiederum zurück an das primäre Replikat geleitet.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren von schreibgeschütztem Routing  
 Sobald das aktuelle primäre Replikat und die lesbaren sekundären Replikate konfiguriert worden sind, sodass sie schreibgeschütztes Routing in beiden Rollen unterstützen, können die lesbaren sekundären Replikate Anforderungen für Leseverbindungen von Clients empfangen, die über den Verfügbarkeitsgruppenlistener eine Verbindung herstellen.  
  
> [!TIP]  
>  Bei Verwendung der Hilfsprogramme [bcp](../../../tools/bcp-utility.md) oder [sqlcmd](../../../tools/sqlcmd-utility.md)können Sie den schreibgeschützten Zugriff für jedes sekundäre Replikat angeben, das den schreibgeschützten Zugriff unterstützt, indem Sie den Switch **-K ReadOnly** angeben.  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> Anforderungen und Empfehlungen für Clientverbindungszeichenfolgen  
 Damit eine Clientanwendung schreibgeschütztes Routing verwendet, muss seine Verbindungszeichenfolge die folgenden Anforderungen erfüllen:  
  
-   Verwendung des TCP-Protokolls.  
  
-   Festlegen des Attributs bzw. der Eigenschaft für die Anwendungsabsicht auf schreibgeschützt  
  
-   Verweis auf den Listener einer Verfügbarkeitsgruppe, der zur Unterstützung des schreibgeschützten Routing konfiguriert worden ist.  
  
-   Verweis auf eine Datenbank in dieser Verfügbarkeitsgruppennamen.  
  
 Außerdem empfiehlt es sich, dass Verbindungszeichenfolgen Multisubnetzfailover aktivieren, das in jedem Subnetz einen parallelen Clientthread für jedes Replikat unterstützt. Dadurch wird die Zeitspanne minimiert, die nach einem Failover zum Wiederherstellen der Verbindung mit dem Client erforderlich ist.  
  
 Die Syntax für eine Verbindungszeichenfolge hängt vom SQL Server-Anbieter ab, den eine Anwendung verwendet. Die folgende Beispielverbindungszeichenfolge für den .NET Framework-Datenanbieter 4.0.2 für SQL Server veranschaulicht die Teile einer Verbindungszeichenfolge, die für schreibgeschütztes Routing erforderlich und empfohlen sind.  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 Weitere Informationen zur schreibgeschützten Anwendungsabsicht und zum schreibgeschützten Routing finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)besitzen.  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>Wenn schreibgeschütztes Routing nicht ordnungsgemäß funktioniert  
 Informationen zum Durchführen einer Problembehandlung an einer schreibgeschützten Routingkonfiguration finden Sie unter [Schreibgeschütztes Routing funktioniert nicht ordnungsgemäß](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR).  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a> Nächste Schritte 
**So zeigen Sie schreibgeschützte Routingkonfigurationen an**  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) (**read_only_routing_url**-Spalte)  
  
**So konfigurieren Sie den Clientverbindungszugriff**  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**So verwenden Sie Verbindungszeichenfolgen in Anwendungen**  
  
-   [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen mit SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**Blogs:**  
  
-    [Berechnen von „read_only_routing_url“ für AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
-    [CSS SQL Server-Technikblogs](/archive/blogs/psssql/)  
  
**Whitepaper:**  
  
-    [Microsoft-Whitepapers für SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [Whitepapers des SQL Server-Kundenberatungsteams](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**Zusätzliche Inhalte**

- [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)