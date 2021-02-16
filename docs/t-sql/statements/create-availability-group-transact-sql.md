---
description: CREATE AVAILABILITY GROUP (Transact-SQL)
title: CREATE AVAILABILITY GROUP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AVAILABILITY GROUP
- CREATE_AVAILABILITY_TSQL
- CREATE_AVAILABILITY_GROUP_TSQL
- CREATE AVAILABILITY GROUP
- CREATE AVAILABILITY
- AVAILABILITY_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- CREATE AVAILABILITY GROUP statement
- Availability Groups [SQL Server], creating
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: a3d55df7-b4e4-43f3-a14b-056cba36ab98
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1a2878025e3b5b6aa8d303ebf94e7215c5590992
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344074"
---
# <a name="create-availability-group-transact-sql"></a>CREATE AVAILABILITY GROUP (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Erstellt eine neue Verfügbarkeitsgruppe, wenn die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]-Funktion aktiviert wird.  
  
> [!IMPORTANT]  
>  Führen Sie CREATE AVAILABILITY GROUP auf die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus, die als ursprüngliches primäres Replikat der neuen Verfügbarkeitsgruppe verwendet werden soll. Diese Serverinstanz muss sich in einem WSFC-Knoten (Windows Server Failover Clustering) befinden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
CREATE AVAILABILITY GROUP group_name  
   WITH (<with_option_spec> [ ,...n ] )  
   FOR [ DATABASE database_name [ ,...n ] ]  
   REPLICA ON <add_replica_spec> [ ,...n ]  
   AVAILABILITY GROUP ON <add_availability_group_spec> [ ,...2 ]  
   [ LISTENER 'dns_name' ( <listener_option> ) ]  
[ ; ]  
  
<with_option_spec>::=   
    AUTOMATED_BACKUP_PREFERENCE = { PRIMARY | SECONDARY_ONLY| SECONDARY | NONE }  
  | FAILURE_CONDITION_LEVEL  = { 1 | 2 | 3 | 4 | 5 }   
  | HEALTH_CHECK_TIMEOUT = milliseconds  
  | DB_FAILOVER  = { ON | OFF }   
  | DTC_SUPPORT  = { PER_DB | NONE }  
  | [ BASIC | DISTRIBUTED ]
  | REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = { integer }
  | CLUSTER_TYPE = { WSFC | EXTERNAL | NONE } 
  
<add_replica_spec>::=  
  <server_instance> WITH  
    (  
       ENDPOINT_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY },  
       FAILOVER_MODE = { AUTOMATIC | MANUAL | EXTERNAL }  
       [ , <add_replica_option> [ ,...n ] ]  
    )   
  
  <add_replica_option>::=  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL } ]   
        [,] [ READ_ONLY_ROUTING_URL = 'TCP://system-address:port' ]  
     } )  
     | PRIMARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { READ_WRITE | ALL } ]   
        [,] [ READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE } ]  
        [,] [ READ_WRITE_ROUTING_URL = { ( '<server_instance>' ) ] 
     } )  
     | SESSION_TIMEOUT = integer  
  
<add_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT },  
       FAILOVER_MODE = MANUAL,  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<listener_option> ::=  
   {  
      WITH DHCP [ ON ( <network_subnet_option> ) ]  
    | WITH IP ( { ( <ip_address_option> ) } [ , ...n ] ) [ , PORT = listener_port ]  
   }  
  
  <network_subnet_option> ::=  
     'ip4_address', 'four_part_ipv4_mask'    
  
  <ip_address_option> ::=  
     {   
        'ip4_address', 'pv4_mask'  
      | 'ipv6_address'  
     }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*group_name*  
Gibt den Namen der neuen Verfügbarkeitsgruppe an. *group_name* muss ein gültiger [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][-Bezeichner](../../relational-databases/databases/database-identifiers.md) und in allen Verfügbarkeitsgruppen im WSFC-Cluster eindeutig sein. Die maximale Länge eines Verfügbarkeitsgruppennamens beträgt 128 Zeichen.  

AUTOMATED_BACKUP_PREFERENCE **=** { PRIMARY \| SECONDARY_ONLY \| SECONDARY \| NONE }  
 Legt fest, wie ein Sicherungsauftrag das primäre Replikat auswerten soll, wenn ausgewählt wird, wo Sicherungen ausgeführt werden müssen. Sie können einen gegebenen Sicherungsauftrag erstellen, um die automatisierte Sicherungseinstellung zu berücksichtigen. Die Einstellung wird nicht von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erzwungen und weist deshalb keine Auswirkungen auf Ad-hoc-Sicherungen auf.  
  
 Die folgenden Werte werden unterstützt:  
  
 PRIMARY  
 Gibt an, dass die Sicherungen immer auf dem primären Replikat erfolgen müssen. Diese Option ist hilfreich, wenn Sie Sicherungsfunktionen benötigen, z. B. das Erstellen differenzieller Sicherungen, die nicht unterstützt werden, wenn die Sicherung auf einem sekundären Replikat ausgeführt wird.  
  
> [!IMPORTANT]  
>  Wenn Sie den Protokollversand verwenden möchten, um sekundäre Datenbanken auf eine Verfügbarkeitsgruppe vorzubereiten, legen Sie die Voreinstellung für automatisierte Sicherungen auf **Primär** fest, bis alle sekundären Datenbanken vorbereitet und mit der Verfügbarkeitsgruppe verknüpft worden sind.  
  
 SECONDARY_ONLY  
 Gibt an, dass Sicherungen nie auf dem primären Replikat ausgeführt werden dürfen. Wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt, darf keine Sicherung erfolgen.  
  
 SECONDARY  
 Gibt an, dass Sicherungen auf einem sekundären Replikat erfolgen müssen, außer wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt. In diesem Fall muss die Sicherung auf dem primären Replikat erfolgen. Dies ist das Standardverhalten.  
  
 Keine  
 Gibt an, dass Sicherungsaufträge die Rolle der Verfügbarkeitsreplikate ignorieren sollen, wenn sie das Replikat zum Durchführen der Sicherungen auswählen. Hinweis: Sicherungsaufträge können andere Faktoren auswerten, wie z. B. die Sicherungspriorität jedes Verfügbarkeitsreplikats in Verbindung mit seinem Betriebszustand und Verbindungsstatus.  
  
> [!IMPORTANT]  
>  Die Einstellung AUTOMATED_BACKUP_PREFERENCE wird nicht erzwungen. Die Interpretation dieser Einstellung hängt von der Logik ab, die Sie ggf. per Skript in Sicherungsaufträge für die Datenbanken in einer angegebenen Verfügbarkeitsgruppe integriert haben. Die Voreinstellung für die automatisierte Sicherung hat keine Auswirkungen auf Ad-hoc-Sicherungen. Weitere Informationen finden Sie unter [Konfigurieren der Sicherung auf Verfügbarkeitsreplikaten &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
> [!NOTE]  
>  Um die automatisierte Sicherungseinstellung einer vorhandenen Verfügbarkeitsgruppe anzuzeigen, wählen Sie die Spalten **automated_backup_preference** oder **automated_backup_preference_desc** der Katalogsicht [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) aus. Darüber hinaus kann [sys.fn_hadr_backup_is_preferred_replica &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) verwendet werden, um das bevorzugte Sicherungsreplikat zu bestimmen.  Diese Funktion gibt für mindestens eines der Replikate 1 zurück, sogar wenn `AUTOMATED_BACKUP_PREFERENCE = NONE` ist.  
  
 FAILURE_CONDITION_LEVEL **=** { 1 \| 2 \| **3** \| 4 \| 5 }  
 Gibt an, welche Fehlerbedingungen ein automatisches Failover für diese Verfügbarkeitsgruppe auslösen. FAILURE_CONDITION_LEVEL wird auf Gruppenebene festgelegt, ist aber nur auf Verfügbarkeitsreplikaten relevant, die für den Verfügbarkeitsmodus mit synchronen Commits (AVAILIBILITY_MODE **=** SYNCHRONOUS_COMMIT) konfiguriert sind. Weiterhin können Fehlerbedingungen nur ein automatisches Failover auslösen, wenn das primäre und das sekundäre Replikat für den automatischen Failovermodus konfiguriert sind (FAILOVER_MODE **=** AUTOMATIC) und das sekundäre Replikat gerade mit dem primären Replikat synchronisiert wird.  
  
 Die Fehlerbedingungsebenen (1–5) reichen von der Ebene 1 mit den wenigsten Einschränkungen bis zur Ebene 5 mit den meisten Einschränkungen. Jede Bedingungsebene umfasst stets auch sämtliche weniger restriktiven Ebenen. Daher schließt die strengste Bedingungsebene 5 die vier Bedingungsebenen mit weniger Einschränkungen (1-4) ein, Ebene 4 schließt die Ebenen 1-3 ein usw. In der folgenden Tabelle wird die Fehlerbedingung beschrieben, die der jeweiligen Ebene entspricht.  
  
|Ebene|Fehlerbedingung|  
|-----------|-----------------------|  
|1|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> – Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst ist ausgefallen.<br /><br /> – Die Lesedauer der Verfügbarkeitsgruppe für die Verbindung mit dem WSFC-Cluster läuft ab, da keine ACK-Meldung von der Serverinstanz empfangen wird. Weitere Informationen finden Sie unter [How It Works: SQL Server Always On Lease Timeout (Funktionsweise: SQL Server Always On-Leasetimeout)](/archive/blogs/psssql/how-it-works-sql-server-alwayson-lease-timeout).|  
|2|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> – Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt keine Verbindung mit dem Cluster her, und der vom Benutzer angegebene HEALTH_CHECK_TIMEOUT-Schwellenwert der Verfügbarkeitsgruppe wurde überschritten.<br /><br /> – Das Verfügbarkeitsreplikat weist einen fehlerhaften Status auf.|  
|3|Gibt an, dass ein automatisches Failover bei kritischen internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. verwaisten Spinlocks, schwerwiegenden Schreibzugriffsverletzungen oder zu vielen Sicherungen.<br /><br /> Dies ist das Standardverhalten.|  
|4|Gibt an, dass ein automatisches Failover bei mittelschweren internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. bei dauerhaft unzureichendem Arbeitsspeicher im internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcenpool.|  
|5|Gibt an, dass ein automatisches Failover bei sämtlichen qualifizierten Fehlerbedingungen initiiert werden soll, einschließlich:<br /><br /> – Erschöpfung der SQL Engine-Arbeitsthreads.<br /><br /> – Erkennung eines unlösbaren Deadlocks.|  
  
> [!NOTE]  
>  Das Fehlen einer Reaktion auf Clientanforderungen durch eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz ist für Verfügbarkeitsgruppen nicht relevant.  
  
 Der FAILURE_CONDITION_LEVEL- und der HEALTH_CHECK_TIMEOUT-Wert definieren eine *flexible Failoverrichtlinie* für eine angegebene Gruppe. Diese flexible Failoverrichtlinie bietet eine präzise Kontrolle der Bedingungen, die ein automatisches Failover verursachen müssen. Weitere Informationen finden Sie unter [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md).  
  
 HEALTH_CHECK_TIMEOUT **=** *milliseconds*  
 Gibt die Wartezeit (in Millisekunden) für die gespeicherte Systemprozedur [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) an, um Informationen über den Serverzustand zurückzugeben, ehe das WSFC-Cluster annimmt, dass die Serverinstanz langsam ist oder nicht reagiert. HEALTH_CHECK_TIMEOUT wird auf Gruppenebene festgelegt, ist aber nur für Verfügbarkeitsreplikate relevant, die für den Verfügbarkeitsmodus für synchrone Commits mit automatischem Failover (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT) konfiguriert sind.  Weiterhin kann ein Integritätsprüfungstimeout nur ein automatisches Failover auslösen, wenn das primäre und das sekundäre Replikat für den automatischen Failovermodus konfiguriert sind (FAILOVER_MODE **=** AUTOMATIC) und das sekundäre Replikat gerade mit dem primären Replikat synchronisiert wird.  
  
 Der standardmäßige HEALTH_CHECK_TIMEOUT-Wert beträgt 30.000 Millisekunden (30 Sekunden). Der minimale Wert beträgt 15.000 Millisekunden (15 Sekunden) und der maximale Wert 4.294.967.295 Millisekunden.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** führt keine Integritätsprüfungen auf Datenbankebene aus.  
  
 DB_FAILOVER  **=** { ON | OFF }  
 Gibt die Antwort an, die akzeptiert wird, wenn eine Datenbank auf dem primären Replikat offline ist. Wenn diese Option auf ON festgelegt ist, löst jeder Status außer ONLINE für eine Datenbank in der Verfügbarkeitsgruppe ein automatisches Failover aus. Wenn diese Option auf OFF festgelegt ist, wird nur die Integrität der Instanz verwendet, um ein automatisches Failover auszulösen.  
  
  Weitere Informationen zu dieser Einstellung finden Sie unter [Integritätserkennung auf Datenbankebene](../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). 
  
 DTC_SUPPORT  **=** { PER_DB | NONE }  
 Gibt an, ob datenbankübergreifende Transaktionen vom Distributed Transaction Coordinator (DTC) unterstützt werden. Datenbankübergreifende Transaktionen werden erst ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] unterstützt. PER_DB erstellt die Verfügbarkeitsgruppe mit Unterstützung für diese Transaktionen. Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
 BASIC  
 Dient zum Erstellen einer Basis-Verfügbarkeitsgruppe. Basis-Verfügbarkeitsgruppen sind auf eine Datenbank und zwei Replikate beschränkt: ein primäres und ein sekundäres Replikat. Diese Option ist ein Ersatz für veraltete Datenbankspiegelungsfeatures in SQL Server Standard Edition. Weitere Informationen finden Sie unter [Basis-Verfügbarkeitsgruppen &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md). Basis-Verfügbarkeitsgruppen werden ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] unterstützt.  

 DISTRIBUTED  
 Dient zum Erstellen einer verteilten Verfügbarkeitsgruppe. Diese Option wird mit dem Parameter AVAILABILITY GROUP ON verwendet, um die Verbindung von zwei Verfügbarkeitsgruppen in separaten Windows Server-Failoverclustern herzustellen.  Weitere Informationen finden Sie unter [Verteilte Verfügbarkeitsgruppen &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md). Verteilte Verfügbarkeitsgruppen werden ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] unterstützt. 

 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT   
 Eingeführt in SQL Server 2017. Dient zum Festlegen einer minimalen Anzahl synchroner sekundärer Replikate, um einen Commit auszuführen, bevor das primäre Replikat einen Commit für die Transaktion ausführt. Garantiert, dass die SQL Server-Transaktion wartet, bis die Transaktionsprotokolle für die minimale Anzahl von sekundären Replikaten aktualisiert werden. Der Standardwert ist 0. Dies bietet das gleiche Verhalten wie SQL Server 2016. Der Mindestwert beträgt 0. Der maximale Wert ist die Anzahl der Replikate minus 1. Diese Option bezieht sich auf Replikate im synchronen Commitmodus. Wenn sich Replikate im synchronen Commitmodus befinden, warten Schreibvorgänge auf dem primären Replikat, bis Schreibvorgänge auf dem zweiten synchronen Replikat an das Transaktionsprotokoll der Replikatsdatenbank übergeben werden. Wenn ein SQL Server, der ein sekundäres synchronisiertes Replikat hostet, nicht mehr reagiert, markiert der SQL Server, der das erste primäre Replikat hostet, dieses sekundäre Replikat als NOT SYNCHRONIZED und setzt den Vorgang fort. Wenn die nicht reagierende Datenbank wieder online geschaltet wird, befindet sie sich im Zustand „nicht synchronisiert“, und das Replikat wird als fehlerhaft markiert, bis das primäre Replikat den Zustand „synchron“ wiederherstellen kann. Diese Einstellung stellt sicher, dass das primäre Replikat wartet, bis die minimale Anzahl der Replikate ein Commit für jede Transaktion ausgeführt hat. Wenn die minimale Anzahl der Replikate nicht verfügbar ist, schlagen Commits auf dem primären Replikat fehl. Für den Clustertyp `EXTERNAL` wird diese Einstellung geändert, wenn die Verfügbarkeitsgruppe der Clusterressource hinzugefügt wird. Weitere Informationen finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](../../linux/sql-server-linux-availability-group-ha.md).

 CLUSTER_TYPE  
 Eingeführt in SQL Server 2017. Wird verwendet, um festzustellen, ob sich die Verfügbarkeitsgruppe auf einem Windows Server-Failovercluster (WSFC) befindet.  Die Option ist auf „WSFC“ festgelegt, wenn sich die Verfügbarkeitsgruppe auf einer Failoverclusterinstanz befindet, die zu einem Windows-Failovercluster gehört. Sie ist auf EXTERNAL festgelegt, wenn der Cluster von einem Cluster-Manager verwaltet wird, bei dem es sich nicht um einen Windows Server-Failovercluster handelt, z.B. Linux Pacemaker. Sie ist auf NONE festgelegt, wenn die Verfügbarkeitsgruppe für die Clusterkoordination keinen WSFC verwendet. Dies ist z.B. der Fall, wenn eine Verfügbarkeitsgruppe Linux-Server ohne Cluster-Manager enthält. 

 DATABASE *database_name*  
 Gibt eine Liste mit mindestens einer Benutzerdatenbank auf der lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an (der Serverinstanz, auf der Sie die Verfügbarkeitsgruppe erstellen). Sie können mehrere Datenbanken für eine Verfügbarkeitsgruppe angeben, aber jede Datenbank kann nur zu einer Verfügbarkeitsgruppe gehören. Informationen über die Datenbanktypen, für die eine Verfügbarkeitsgruppe Unterstützung bietet finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen&#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). In der Spalte **replica_id** in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht können Sie überprüfen, welche lokalen Datenbanken bereits zu einer Verfügbarkeitsgruppe gehören.  
  
 Die DATABASE-Klausel ist optional. Wenn sie weggelassen wird, ist die neue Verfügbarkeitsgruppe leer.  
  
 Stellen Sie nach dem Erstellen der Verfügbarkeitsgruppe eine Verbindung zu jeder Serverinstanz her, die ein sekundäres Replikat hostet, und bereiten Sie anschließend alle sekundären Datenbankn vor und verknüpfen Sie sie mit der Verfügbarkeitsgruppe. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
> [!NOTE]  
>  Später können Sie berechtigte Datenbanken auf der Serverinstanz hinzufügen, die das primäre Replikat für eine Verfügbarkeitsgruppe hostet. Sie können zudem eine Datenbank aus einer Verfügbarkeitsgruppe entfernen. Weitere Informationen finden Sie unter [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)hinzugefügt wird.  
  
 REPLICA ON  
 Gibt eine bis fünf SQL Server-Instanzen als Hostverfügbarkeitsreplikate in der neuen Verfügbarkeitsgruppe an.  Jedes Replikat wird von seiner Serverinstanzadresse gefolgt von einer WITH (…)-Klausel angegeben. Sie müssen mindestens die lokale Serverinstanz angeben, die als ursprüngliches primäres Replikat fungieren soll. Optional können Sie auch bis zu vier sekundäre Replikate angeben.  
  
 Sie müssen jedes sekundäre Replikat mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie unter [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)hinzugefügt wird.  
  
> [!NOTE]  
>  Wenn Sie weniger als vier sekundäre Replikate beim Erstellen einer Verfügbarkeitsgruppe angeben, können Sie jederzeit mit der [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ein zusätzliches sekundäres Replikat hinzufügen. Außerdem können Sie mit dieser Anweisung alle sekundären Replikate aus einer vorhandenen Verfügbarkeitsgruppe entfernen.  
  
 \<server_instance> gibt die Adresse der Instanz von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, die als Host für ein Replikat fungiert. Das Adressformat hängt davon ab, ob die Instanz die Standardinstanz oder eine benannte Instanz ist und ob es eine eigenständige Instanz oder eine Failoverclusterinstanz (FCI) ist, wie dies im Folgenden beschrieben ist:  
  
 { '*system_name*[\\*instance_name*]' | '*FCI_network_name*[\\*instance_name*]' }  
  
 Diese Adresse weist die folgenden Komponenten auf:  
  
 *system_name*  
 Der NetBIOS-Name des Computersystems, auf dem sich eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Zielinstanz befindet. Dieser Computer muss ein WSFC-Knoten sein.  
  
 *FCI_network_name*  
 Ist der Netzwerkname, der verwendet wird, um auf einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failovercluster zuzugreifen. Verwenden Sie diesen Namen, wenn die Serverinstanz als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverpartner beteiligt ist. Wenn SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) in einer FCI-Serverinstanz ausgeführt wird, wird die gesamte '*FCI_network_name*[\\*instance_name*]'-Zeichenfolge zurückgegeben (dabei handelt es sich um den vollständigen Replikatnamen).  
  
 *instance_name*  
 Der Name einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, die von *system_name* oder *FCI_network_name* gehostet wird und für die der HADR-Dienst aktiviert ist. Bei einer Standardserverinstanz ist *instance_name* optional. Bei dem Instanznamen wird die Groß-/Kleinschreibung berücksichtigt. In einer benannten Serverinstanz stimmt der Name dieses Werts mit dem Wert überein, der beim Ausführen von `select ServerProperty(N'InstanceName');` zurückgegeben wird.  
  
 \  
 Ist eine Trennzeichen, das nur bei der Angabe von *instance_name* verwendet wird, um dieses Element von *system_name* oder *FCI_network_name* zu trennen.  
  
 Informationen zu den Voraussetzungen für WSFC-Knoten und Serverinstanzen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen&#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 ENDPOINT_URL **='** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt den URL-Pfad des [Datenbankspiegelungsendpunkts](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an, die das Verfügbarkeitsreplikat hostet, das Sie in der aktuellen REPLICA ON-Klausel definieren.  
  
 Die ENDPOINT_URL-Klausel ist erforderlich. Weitere Informationen finden Sie unter [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)zu unterstützen.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt eine URL zum Bestimmen einer Endpunkt-URL oder einer URL für das schreibgeschützte Routing an. Die URL-Parameter lauten wie folgt:  
  
 *system-address*  
 Ist eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die das Zielcomputersystem eindeutig identifiziert.  
  
 *port*  
 Ist eine Portnummer, die dem Spiegelungsendpunkt der Partnerserverinstanz (für die ENDPOINT_URL-Option) oder der Portnummer, die von [!INCLUDE[ssDE](../../includes/ssde-md.md)] der Serverinstanz (für die READ_ONLY_ROUTING_URL-Option) verwendet wird, zugeordnet ist.  
  
 AVAILABILITY_MODE **=** { {SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 SYNCHRONOUS_COMMIT oder ASYNCHRONOUS_COMMIT geben an, ob das primäre Replikat auf das sekundäre Replikat warten muss, um das Verstärken (Schreiben) der Protokolldatensätze auf einem Datenträger zu bestätigen, bevor das primäre Replikat die Transaktion auf einer bestimmten primären Datenbank ausführen kann. Die Transaktionen auf anderen Datenbanken über dasselbe primäre Replikat können unabhängig einen Commit ausführen. SQL Server 2017 CU 1 führt CONFIGURATION_ONLY ein. Das CONFIGURATION_ONLY-Replikat gilt nur für Verfügbarkeitsgruppen mit CLUSTER_TYPE = EXTERNAL oder CLUSTER_TYPE = NONE. 
  
 SYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat mit der Ausführung von Transaktionen wartet, bis sie auf diesem sekundären Replikat (Modus mit synchronem Commit) verstärkt wurden. Sie können SYNCHRONOUS_COMMIT für bis zu drei Replikate angeben, einschließlich des primären Replikats.  
  
 ASYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat einen Commit für Transaktionen ausführt, ohne zu warten, bis dieses sekundäre Replikat das Protokoll verstärkt (Verfügbarkeitsmodus mit synchronem Commit). Sie können ASYNCHRONOUS_COMMIT für bis zu fünf Verfügbarkeitsreplikate angeben, einschließlich des primären Replikats.  

 CONFIGURATION_ONLY gibt an, dass das primäre Replikat ein synchrones Commit für Metadaten von Verfügbarkeitsgruppen an die Masterdatenbanken auf diesem Replikat durchführt. Das Replikat enthält keine Benutzerdaten. Diese Option:

- Kann auf einer beliebigen Edition von SQL Server, darunter Express Edition gehostet werden.
- Erfordert, dass der Datenbankspiegelungs-Endpunkt des CONFIGURATION_ONLY-Replikats vom Typ `WITNESS` ist.
- Kann nicht geändert werden.
- Ist nicht gültig, wenn `CLUSTER_TYPE = WSFC`.
- Die Optionen `failover_mode` und `seeding_mode` werden nicht unterstützt, wenn `availability_mode` für ein Replikat auf `configuration_only` festgelegt ist. Ein Beispiel finden Sie [hier](../../linux/sql-server-linux-availability-group-configure-ha.md).

   Weitere Informationen finden Sie unter [Configuration only replica (Configuration only-Replikat)](../../linux/sql-server-linux-availability-group-ha.md).
  
 Die AVAILABILITY_MODE-Klausel ist erforderlich. Weitere Informationen finden Sie unter [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)ausgetauscht werden.  
  
 FAILOVER_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt den Failovermodus des Verfügbarkeitsreplikats an, das Sie definieren.  
  
 AUTOMATIC  
 Aktiviert das automatische Failover. Diese Option wird nur unterstützt, wenn Sie auch AVAILABILITY_MODE = SYNCHRONOUS_COMMIT angeben. Sie können AUTOMATIC für zwei Verfügbarkeitsreplikate angeben, einschließlich des primären Replikats.  
  
> [!NOTE]  
>  SQL Server-Failoverclusterinstanzen (FCIs) unterstützen kein automatisches Failover durch Verfügbarkeitsgruppen. Daher können die Verfügbarkeitsreplikate, die von einer FCI gehostet werden, nur für manuelles Failover konfiguriert werden.  
  
 MANUAL  
 Ermöglicht ein geplantes manuelles Failover oder ein erzwungenes manuelles Failover (üblicherweise als *erzwungenes Failover* bezeichnet) durch den Datenbankadministrator.  
  
 Die FAILOVER_MODE-Klausel ist erforderlich. Die beiden Typen des manuellen Failovers, manuelles Failover ohne Datenverlust und erzwungenes Failover (mit möglichem Datenverlust), werden unter unterschiedlichen Bedingungen unterstützt. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Failover und Failovermodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt an, wie für das sekundäre Replikat zuerst ein Seeding durchgeführt wird.  
  
 AUTOMATIC  
 Ermöglicht direktes Seeding. Diese Methode startet das sekundäre Replikat über das Netzwerk. Mit dieser Methode ist es nicht mehr erforderlich, eine Kopie der primären Datenbank zu sichern und auf dem Replikat wiederherzustellen.  
  
> [!NOTE]  
>  Um das direkte Seeding zu ermöglichen, müssen Sie die Datenbankerstellung auf jedem sekundären Replikat zulassen, indem Sie den Befehl **ALTER AVAILABILITY GROUP** mit der Option **GRANT CREATE ANY DATABASE** aufrufen.  
  
 MANUAL  
 Gibt das manuelle Seeding an (Standard). Bei dieser Methode müssen Sie eine Sicherungskopie der Datenbank auf dem primären Replikat erstellen und diese manuell auf dem sekundären Replikat wiederherstellen.  
  
 BACKUP_PRIORITY **=** *n*  
 Gibt die Priorität für die Ausführung von Sicherungen auf diesem Replikat in Relation zu den anderen Replikaten in derselben Verfügbarkeitsgruppe an. Der Wert liegt im Bereich von 0 bis 100 und ist eine ganze Zahl. Diese Werte haben die folgenden Bedeutungen:  
  
-   1..100 gibt an, dass das Verfügbarkeitsreplikat zum Ausführen von Sicherungen ausgewählt werden könnte. 1 gibt die niedrigste Priorität und 100 die höchste Priorität an. Wenn BACKUP_PRIORITY = 1, würde das Verfügbarkeitsreplikat nur zum Ausführungen von Sicherungen ausgewählt werden, wenn gerade keine höheren Prioritätsverfügbarkeitsreplikate verfügbar sind.  
  
-   0 gibt an, dass dieses Verfügbarkeitsreplikat nicht für das Ausführen von Sicherungen ausgewählt ist. Dies ist zum Beispiel für ein Remoteverfügbarkeitsreplikat hilfreich, für das keine Failover bei Sicherungen auftreten sollen.  
  
 Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
 SECONDARY_ROLE **(** ... **)**  
 Gibt rollenspezifische Einstellungen an, die wirksam werden, wenn dieses Verfügbarkeitsreplikat gerade die sekundäre Rolle besitzt (d.h. wenn es gerade ein sekundäres Replikat ist). Geben Sie innerhalb der Klammern eine oder beide sekundäre Rollenoptionen an. Wenn Sie beide angeben, verwenden Sie eine durch Trennzeichen getrennte Liste.  
  
 Folgende Optionen stehen für die sekundäre Rolle zur Verfügung:  
  
 ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL }  
 Gibt an, ob die Datenbanken eines bestimmten Verfügbarkeitsreplikats, das die sekundäre Rolle einnimmt (das heißt, als sekundäres Replikat dient), Verbindungen von Clients akzeptieren können, z. B.:  
  
 Nein  
 Es werden keine Verbindungen mit sekundären Datenbanken dieses Replikats zugelassen. Sie sind für den Lesezugriff nicht verfügbar. Dies ist das Standardverhalten.  
  
 READ_ONLY  
 Verbindungen mit den Datenbanken im sekundären Replikat sind nur zulässig, wenn die Eigenschaft für die Anwendungsabsicht auf **ReadOnly** festgelegt ist. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Für alle Verbindungen mit den Datenbanken im sekundären Replikat ist der schreibgeschützte Zugriff zugelassen.  
  
 Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt die URL an, die zum Weiterleiten von Verbindungsanforderungen für beabsichtigte Lesevorgänge zu diesem Verfügbarkeitsreplikat verwendet werden soll. Dies ist die URL, die die SQL Server-Datenbank-Engine überwacht. In der Regel überwacht die Standardinstanz der SQL Server-Datenbank-Engine auf TCP-Port 1433.  
  
 Für eine benannte Instanz können Sie die Portnummer durch das Abfragen der Spalten **port** und **type_desc** der dynamischen [sys.dm_tcp_listener_states](../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)-Verwaltungssicht abrufen. Die Serverinstanz verwendet den Transact-SQL-Listener (**type_desc='TSQL'** ).  
  
 Weitere Informationen zum Berechnen der schreibgeschützten Routing-URL für ein Replikat finden Sie unter [Calculating read_only_routing_url for Always On (Berechnen von „read_only_routing_url“ für Always On)](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
> [!NOTE]  
>  Für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollte der Transact-SQL-Listener konfiguriert werden, um einen bestimmten Port zu verwenden. Weitere Informationen finden Sie unter [Konfigurieren eines Servers zur Überwachung eines bestimmten TCP-Ports &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 PRIMARY_ROLE **(** ... **)**  
 Gibt rollenspezifische Einstellungen an, die wirksam werden, wenn dieses Verfügbarkeitsreplikat zu diesem Zeitpunkt die sekundäre Rolle besitzt (d.h. wenn es gerade ein sekundäres Replikat ist). Geben Sie innerhalb der Klammern eine oder beide primäre Rollenoptionen an. Wenn Sie beide angeben, verwenden Sie eine durch Trennzeichen getrennte Liste.  
  
 Folgende Optionen stehen für die primäre Rolle zur Verfügung:  
  
 ALLOW_CONNECTIONS **=** { READ_WRITE | ALL }  
 Gibt den Verbindungstyp an, den die Datenbanken eines bestimmten Verfügbarkeitsreplikats, das die primäre Rolle einnimmt (das heißt, als primäres Replikat dient), von Clients akzeptieren können, z. B.:  
  
 READ_WRITE  
 Verbindungen, bei denen die Verbindungseigenschaft für den Anwendungszweck auf **ReadOnly** festgelegt ist, werden nicht zugelassen.  Wenn die Eigenschaft für die Anwendungsabsicht auf **ReadWrite** festgelegt ist oder keine Verbindungseigenschaft für die Anwendungsabsicht festgelegt wurde, wird die Verbindung zugelassen. Weitere Informationen zur Verbindungseigenschaft für die Anwendungsabsicht finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Für die Datenbanken im primären Replikat sind alle Verbindungen zugelassen. Dies ist das Standardverhalten.  
  
 READ_ONLY_ROUTING_LIST **=** { **('** \<server_instance> **'** [ **,** ...*n* ] **)** | NONE } gibt eine durch Trennzeichen getrennte Liste von Serverinstanzen an, die Verfügbarkeitsreplikate für diese Verfügbarkeitsgruppe hosten, die beim Ausführen unter der sekundären Rolle die folgenden Anforderungen erfüllen:  
  
-   Wird konfiguriert, um alle Verbindungen oder schreibgeschützte Verbindungen (siehe das obige ALLOW_CONNECTIONS-Argument der SECONDARY_ROLE-Option) zuzulassen.  
  
-   Die schreibgeschützte Routing-URL wurde definiert (siehe das obige READ_ONLY_ROUTING_URL-Argument der SECONDARY_ROLE-Option).  
  
 Die READ_ONLY_ROUTING_LIST-Werte lauten wie folgt:  
  
 \<server_instance> gibt die Adresse der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an, die als Host für ein Replikat fungiert, das ein lesbares sekundäres Replikat ist, wenn es unter der sekundären Rolle ausgeführt wird.  
  
 Verwenden Sie eine durch Trennzeichen getrennte Liste, um alle Serverinstanzen anzugeben, die ein lesbares sekundäres Replikat hosten könnten. Schreibgeschütztes Routing erfolgt in der Reihenfolge, in der Serverinstanzen in der Liste angegeben werden. Wenn Sie die Hostserverinstanz eines Replikats auf der schreibgeschützten Routingliste des Replikats einschließen, ist es eine empfohlene Vorgehensweise, diese Serverinstanz am Ende der Liste zu platzieren, damit Verbindungen für beabsichtigte Lesevorgänge bei Verfügbarkeit zu einem sekundären Replikat wechseln.  
  
 Beginnend mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie einen Lastenausgleich für Anforderungen von beabsichtigten Lesevorgängen über lesbare sekundäre Replikate durchführen. Dies können Sie angeben, indem Sie die Replikate in geschachtelten Klammern innerhalb der schreibgeschützten Routingliste platzieren. Weitere Informationen und Beispiele finden Sie unter [Configure load-balancing across read-only replicas (Konfigurieren des Lastenausgleichs für mehrere schreibgeschützte Replikate)](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
 Keine  
 Gibt an, dass schreibgeschütztes Routing nicht unterstützt wird, wenn dieses Verfügbarkeitsreplikat das primäre Replikat ist. Dies ist das Standardverhalten.  

 READ_WRITE_ROUTING_URL **=** { **('** \<server_instance> **')** }  
 Gilt für: SQL Server (ab SQL Server 2019 (15.x)) 

 Gibt bei Ausführung unter der primären Rolle Serverinstanzen an, die Verfügbarkeitsreplikate für diese Verfügbarkeitsgruppe hosten, die die folgenden Anforderungen erfüllen:
-   Die Replikatspezifikation PRIMARY_ROLE enthält READ_WRITE_ROUTING_URL.
-   Die Verbindungszeichenfolge ist ReadWrite, entweder indem ApplicationIntent als ReadWrite definiert wird oder indem ApplicationIntent nicht festgelegt und somit der Standard (ReadWrite) wirksam wird.

Weitere Informationen finden Sie unter [Umleitung von Lese-/Schreibverbindungen vom sekundären zum primären Replikat (Always On-Verfügbarkeitsgruppen)](../../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).

 SESSION_TIMEOUT **=** *integer*  
 Gibt den Zeitraum für das Sitzungstimeout in Sekunden an. Wenn Sie die Option nicht angeben, beträgt der Timeoutzeitraum standardmäßig 10 Sekunden. Der Wert muss mindestens 5 Sekunden betragen.  
  
> [!IMPORTANT]  
>  Es wird empfohlen, einen Timeoutzeitraum von 10 Sekunden oder mehr zu wählen.  
  
 Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 AVAILABILITY GROUP ON  
 Gibt zwei Verfügbarkeitsgruppen an, die eine *verteilte Verfügbarkeitsgruppe* bilden. Jede Verfügbarkeitsgruppe ist Teil ihres eigenen Windows Server-Failoverclusters (WSFC). Wenn Sie eine verteilte Verfügbarkeitsgruppe erstellen, wird die Verfügbarkeitsgruppe auf der aktuellen SQL Server-Instanz zur primären Verfügbarkeitsgruppe. Die zweite Verfügbarkeitsgruppe wird zur sekundären Verfügbarkeitsgruppe.  
  
 Sie müssen die sekundäre Verfügbarkeitsgruppe mit dem verteilten Cluster verknüpfen. Weitere Informationen finden Sie unter [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)hinzugefügt wird.  
  
 \<ag_name> gibt den Namen der Verfügbarkeitsgruppe an, die eine Hälfte der verteilten Verfügbarkeitsgruppe ausmacht.  
  
 LISTENER **='** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt den URL-Pfad für den Listener an, der der Verfügbarkeitsgruppe zugeordnet ist.  
  
 Die LISTENER-Klausel ist erforderlich.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt eine URL für den Listener an, der der Verfügbarkeitsgruppe zugeordnet ist. Die URL-Parameter lauten wie folgt:  
  
 *system-address*  
 Eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die den Listener eindeutig identifiziert.  
  
 *port*  
 Eine Portnummer, die dem Spiegelungsendpunkt der Verfügbarkeitsgruppe zugeordnet ist. Beachten Sie, dass dies nicht der Port des Listeners ist.  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 Gibt an, ob das primäre Replikat auf die sekundäre Verfügbarkeitsgruppe warten muss, um das Verstärken (Schreiben) der Protokolldatensätze auf einem Datenträger zu bestätigen, bevor das primäre Replikat die Transaktion auf einer bestimmten primären Datenbank ausführen kann.  
  
 SYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat mit der Ausführung von Transaktionen wartet, bis diese auf dieser sekundären Verfügbarkeitsgruppe verstärkt wurden. Sie können SYNCHRONOUS_COMMIT für bis zu zwei Verfügbarkeitsgruppen angeben, einschließlich der primären Verfügbarkeitsgruppe.  
  
 ASYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat Transaktionen ausführt, ohne zu warten, bis diese sekundäre Verfügbarkeitsgruppe das Protokoll verstärkt. Sie können ASYNCHRONOUS_COMMIT für bis zu zwei Verfügbarkeitsgruppen angeben, einschließlich der primären Verfügbarkeitsgruppe.  
  
 Die AVAILABILITY_MODE-Klausel ist erforderlich.  
  
 FAILOVER_MODE **=** { MANUAL }  
 Gibt den Failovermodus der verteilten Verfügbarkeitsgruppe an.  
  
 MANUAL  
 Ermöglicht ein geplantes manuelles Failover oder ein erzwungenes manuelles Failover (üblicherweise als *erzwungenes Failover* bezeichnet) durch den Datenbankadministrator.  
  
 Die FAILOVER_MODE-Klausel ist erforderlich, und die einzige Option ist MANUAL. Das automatische Failover auf die sekundäre Verfügbarkeitsgruppe wird nicht unterstützt.  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt an, wie für die sekundäre Verfügbarkeitsgruppe zuerst ein Seeding durchgeführt wird.  
  
 AUTOMATIC  
 Ermöglicht direktes Seeding. Diese Methode startet die sekundäre Verfügbarkeitsgruppe über das Netzwerk. Mit dieser Methode ist es nicht mehr erforderlich, eine Kopie der primären Datenbank zu sichern und auf den Replikaten der sekundären Verfügbarkeitsgruppe wiederherzustellen.  
  
 MANUAL  
 Gibt das manuelle Seeding an (Standard). Bei dieser Methode müssen Sie eine Sicherungskopie der Datenbank auf dem primären Replikat erstellen und diese manuell auf dem Replikat/den Replikaten der sekundären Verfügbarkeitsgruppe wiederherstellen.  
  
 LISTENER **'** _dns\_name_ **'(** \<listener_option\> **)** definiert einen neuen Verfügbarkeitsgruppenlistener für diese Verfügbarkeitsgruppe. LISTENER ist ein optionales Argument.  
  
> [!IMPORTANT]
>  Vor dem Erstellen Ihres ersten Listeners wird dringend empfohlen, den Artikel [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md) zu lesen.  
> 
>  Nachdem Sie einen Listener für eine Verfügbarkeitsgruppe erstellt haben, empfehlen wir dringend, folgende Schritte auszuführen:  
> 
>  -   Bitten Sie den Netzwerkadministrator, die IP-Adresse des Listeners zur exklusiven Verwendung zu reservieren.  
> -   Geben Sie den DNS-Hostnamen des Listeners an Anwendungsentwickler weiter, damit diese den Namen in Verbindungszeichenfolgen zum Anfordern von Clientverbindungen mit dieser Verfügbarkeitsgruppe verwenden.  
  
 *dns_name*  
 Gibt den DNS-Hostnamen des Verfügbarkeitsgruppenlisteners an. Der DNS-Name des Listeners muss in der Domäne und NetBIOS eindeutig sein.  
  
 *dns_name* ist ein Zeichenfolgenwert. Dieser Name darf nur alphanumerische Zeichen, Bindestriche (-) und Unterstriche (_) enthalten (in beliebiger Reihenfolge). Bei DNS-Hostnamen muss die Groß-/Kleinschreibung beachtet werden. Die maximale Länge beträgt 63 Zeichen.  
  
 Wir empfehlen, dass Sie eine sinnvolle Zeichenfolge angeben. Für eine Verfügbarkeitsgruppe mit dem Namen `AG1`wäre ein sinnvoller DNS-Hostname z. B. `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS erkennt nur die ersten 15 Zeichen im dns_name. Wenn Sie zwei WSFC-Cluster verwenden, die vom gleichen Active Directory-Dienst gesteuert werden, und Sie versuchen, Verfügbarkeitsgruppenlistener in beiden Clustern mit Namen mit mehr als 15 Zeichen und einem identischen 15-Zeichen-Präfix zu erstellen, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass die VNN-Ressource nicht online geschaltet werden konnte. Informationen zu Präfix-Benennungsregeln für DNS-Namen finden Sie unter [Zuweisen von Domänennamen](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
 \<listener_option> LISTENER verwendet eine der folgenden \<listener_option>-Optionen: 
  
 WITH DHCP [ ON { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')** } ]  
 Gibt an, dass der Verfügbarkeitsgruppenlistener das Dynamic Host Configuration-Protokoll (DHCP) verwendet.  Verwenden Sie die ON-Klausel optional, um das Netzwerk zu identifizieren, auf dem dieser Listener erstellt wird. DHCP ist auf ein einzelnes Subnetz beschränkt, das für alle Serverinstanzen verwendet wird, die ein Replikat in der Verfügbarkeitsgruppe hosten.  
  
> [!IMPORTANT]  
>  DHCP wird in einer Produktionsumgebung nicht empfohlen. Wenn die DHCP-IP-Leasedauer bei einer Downtime abläuft, ist eine Verlängerung erforderlich, um die neue IP-Adresse des DHCP-Netzwerks zu registrieren, die dem DNS-Namen des Listeners zugeordnet ist, was sich auf die Clientkonnektivität auswirkt. DHCP eignet sich jedoch gut zum Einrichten der Entwicklungs- und Testumgebung, um grundlegende Funktionen von Verfügbarkeitsgruppen und die Integration in Ihre Anwendungen zu überprüfen.  
  
 Beispiel:  
  
 `WITH DHCP ON ('10.120.19.0','255.255.254.0')`  
  
 WITH IP **(** { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')**  |  **('** _ipv6\_address_ **')** } [ **,** ...*n* ] **)** [ **,** PORT **=** _listener\_port_ ]  
 Gibt an, dass der Verfügbarkeitsgruppenlistener statt DHCP mindestens eine statische IP-Adresse verwendet. Um eine Verfügbarkeitsgruppe über mehrere Subnetze zu erstellen, erfordert jedes Subnetz in der Listenerkonfiguration eine statische IP-Adresse. Für ein angegebenes Subnetz kann die statische IP-Adresse entweder eine IPv4-Adresse oder eine IPv6-Adresse sein. Wenden Sie sich an Ihren Netzwerkadministrator, um eine statische IP-Adresse für jedes Subnetz zu erhalten, das ein Replikat für die neue Verfügbarkeitsgruppe hostet.  
  
 Beispiel:  
  
 `WITH IP ( ('10.120.19.155','255.255.254.0') )`  
  
 *ip4_address*  
 Gibt eine vierteilige IPv4-Adresse für einen Verfügbarkeitsgruppenlistener an. Beispiel: `10.120.19.155`.  
  
 *ipv4_mask*  
 Gibt eine vierteilige IPv4-Maske für einen Verfügbarkeitsgruppenlistener an. Beispiel: `255.255.254.0`.  
  
 *ipv6_address*  
 Gibt eine IPv6-Adresse für einen Verfügbarkeitsgruppenlistener an. Beispiel: `2001::4898:23:1002:20f:1fff:feff:b3a3`.  
  
 PORT **=** *listener_port*  
 Gibt die Portnummer (*listener_port*) an, die von einem Verfügbarkeitsgruppenlistener verwendet wird, der anhand einer WITH IP-Klausel angegeben wird. PORT ist optional.  
  
 Die Standardportnummer 1433 wird unterstützt. Wenn Sie jedoch Sicherheitsbedenken hegen, empfehlen wir die Verwendung einer anderen Portnummer.  
  
 Beispiel: `WITH IP ( ('2001::4898:23:1002:20f:1fff:feff:b3a3') ) , PORT = 7777`  
  
## <a name="prerequisites-and-restrictions"></a>Voraussetzungen und Einschränkungen  
 Informationen zu den Voraussetzungen zum Erstellen von Verfügbarkeitsgruppen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen&#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 Informationen zu Einschränkungen bei den AVAILABILITY GROUP-Transact-SQL-Anweisungen finden Sie unter [Übersicht über Transact-SQL-Anweisungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-configuring-backup-on-secondary-replicas-flexible-failover-policy-and-connection-access"></a>A. Konfigurieren der Sicherung für sekundäre Replikate, flexible Failoverrichtlinien und Verbindungszugriff  
 Im folgenden Beispiel wird eine Verfügbarkeitsgruppe mit dem Namen `MyAg` für die zwei Benutzerdatenbanken `ThisDatabase` und `ThatDatabase` erstellt. In der folgenden Tabelle werden die Werte zusammengefasst, die für die Optionen angegeben wurden, die für die Verfügbarkeitsgruppe als Ganzes festgelegt sind.  
  
|Gruppenoption|Einstellung|BESCHREIBUNG|  
|------------------|-------------|-----------------|  
|AUTOMATED_BACKUP_PREFERENCE|SECONDARY|Diese automatisierte Sicherungseinstellung gibt an, dass Sicherungen auf einem sekundären Replikat erfolgen sollen, außer wenn das primäre Replikat das einzige Replikat online ist (dies ist das Standardverhalten). Damit die AUTOMATED_BACKUP_PREFERENCE-Einstellung in Kraft tritt, müssen Sicherungsaufträge in Verfügbarkeitsdatenbanken so verfasst werden, dass die automatische Sicherungseinstellung berücksichtigt wird.|  
|FAILURE_CONDITION_LEVEL|3|Diese Einstellung für die Fehlerbedingungsebene gibt an, dass ein automatisches Failover bei kritischen internen SQL Server-Fehlern initiiert werden soll, z. B. verwaisten Spinlocks, ernsten Schreibzugriffsverletzungen oder zu vielen Sicherungen.|  
|HEALTH_CHECK_TIMEOUT|600000|Ein Timeoutwert für die Integritätsprüfung von 60 Sekunden besagt, dass der WSFC-Cluster 60.000 Millisekunden wartet, bis die gespeicherte Systemprozedur [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) Informationen über den Serverzustand über eine Serverinstanz zurückgibt, die ein Replikat mit synchronem Commit automatisch hostet, bevor der Cluster annimmt, dass die Hostserverinstanz langsam ist oder nicht reagiert. (Der Standardwert beträgt 30.000 Millisekunden.)|  
  
 Drei Verfügbarkeitsreplikate müssen von den Standardserverinstanzen auf Computern mit der Bezeichnung `COMPUTER01`, `COMPUTER02` und `COMPUTER03` gehostet werden. In der folgenden Tabelle werden die für die Replikatoptionen jedes Replikats angegebenen Werte zusammengefasst.  
  
|Replikatoption|Einstellung auf `COMPUTER01`|Einstellung auf `COMPUTER02`|Einstellung auf `COMPUTER03`|BESCHREIBUNG|  
|--------------------|-----------------------------|-----------------------------|-----------------------------|-----------------|  
|ENDPOINT_URL|TCP://*COMPUTER01:5022*|TCP://*COMPUTER02:5022*|TCP://*COMPUTER03:5022*|In diesem Beispiel weisen die Systeme dieselbe Domäne auf. Daher können die Endpunkt-URLs den Namen des Computersystems als Systemadresse verwenden.|  
|AVAILABILITY_MODE|SYNCHRONOUS_COMMIT|SYNCHRONOUS_COMMIT|ASYNCHRONOUS_COMMIT|Zwei der Replikate verwenden den Modus mit synchronem Commit. Nach der Synchronisierung unterstützen sie Failover ohne Datenverlust. Das dritte Replikat verwendet den Verfügbarkeitsmodus mit asynchronem Commit.|  
|FAILOVER_MODE|AUTOMATIC|AUTOMATIC|MANUAL|Die Replikate mit synchronem Commit unterstützen automatisches Failover und geplantes manuelles Failover. Das Verfügbarkeitsmodusreplikat mit synchronem Commit unterstützt nur erzwungenes manuelles Failover.|  
|BACKUP_PRIORITY|30|30|90|Dem Replikat mit asynchronem Commit wird eine höhere Priorität (90) als dem Replikat mit synchronem Commit zugewiesen. Sicherungen erfolgen tendenziell auf der Serverinstanz, die das Replikat mit asynchronem Commit hostet.|  
|SECONDARY_ROLE|( ALLOW_CONNECTIONS = NO,<br /><br /> READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01:1433' )|( ALLOW_CONNECTIONS = NO,<br /><br /> READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02:1433' )|( ALLOW_CONNECTIONS = READ_ONLY, <br />READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03:1433' )|Nur das Replikat mit asynchronem Commit fungiert als lesbares sekundäres Replikat.<br /><br /> Gibt den Computernamen und die standardmäßige Datenbank-Engine-Portnummer (1433) an.<br /><br /> Dieses Argument ist optional.|  
|PRIMARY_ROLE|( ALLOW_CONNECTIONS = READ_WRITE, <br />READ_ONLY_ROUTING_LIST = (COMPUTER03) )|( ALLOW_CONNECTIONS = READ_WRITE, <br />READ_ONLY_ROUTING_LIST = (COMPUTER03) )|( ALLOW_CONNECTIONS = READ_WRITE, <br />READ_ONLY_ROUTING_LIST = NONE )|In der primären Rolle lehnen alle Replikate Verbindungsversuche für beabsichtigte Lesevorgänge ab.<br /><br /> Verbindungsanforderungen für beabsichtigte Lesevorgänge werden an COMPUTER03 weitergeleitet, wenn das lokale Replikat unter der sekundären Rolle ausgeführt wird. Wenn dieses Replikat unter der primären Rolle ausgeführt wird, wird das schreibgeschützte Routing deaktiviert.<br /><br /> Dieses Argument ist optional.|  
|SESSION_TIMEOUT|10|10|10|In diesem Beispiel wird der Timeoutwert für die Standardsitzung (10) angegeben. Dieses Argument ist optional.|  
  
 Im Beispiel wird schließlich die optionale LISTENER-Klausel angegeben, um einen Verfügbarkeitsgruppenlistener für die neue Verfügbarkeitsgruppe zu erstellen. Der eindeutige DNS-Name `MyAgListenerIvP6`wird für diesen Listener angegeben. Die zwei Replikate befinden sich auf anderen Subnetzen, daher muss der Listener statische IP-Adressen verwenden. Für die beiden Verfügbarkeitsreplikate gibt die WITH IP-Klausel jeweils eine statische IP-Adresse an, nämlich `2001:4898:f0:f00f::cf3c` und `2001:4898:e0:f213::4ce2`, die das IPv6-Format verwenden. In diesem Beispiel wird zudem das optionale PORT-Argument verwendet, um Port `60173` als Listenerport anzugeben.  
  
```SQL
CREATE AVAILABILITY GROUP MyAg   
   WITH (  
      AUTOMATED_BACKUP_PREFERENCE = SECONDARY,  
      FAILURE_CONDITION_LEVEL  =  3,   
      HEALTH_CHECK_TIMEOUT = 600000  
       )  
  
   FOR   
      DATABASE  ThisDatabase, ThatDatabase   
   REPLICA ON   
      'COMPUTER01' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC,  
         BACKUP_PRIORITY = 30,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = NO,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01:1433' ),
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER03) ),  
         SESSION_TIMEOUT = 10  
         ),   
  
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC,  
         BACKUP_PRIORITY = 30,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = NO,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER03) ),  
         SESSION_TIMEOUT = 10  
         ),   
  
      'COMPUTER03' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03:5022',  
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
         FAILOVER_MODE =  MANUAL,  
         BACKUP_PRIORITY = 90,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = NONE ),  
         SESSION_TIMEOUT = 10  
         );
GO  
ALTER AVAILABILITY GROUP [MyAg]
  ADD LISTENER 'MyAgListenerIvP6' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173 );   
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Verwenden des Assistenten für Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten für Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [DROP AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-availability-group-transact-sql.md)   
 [Problembehandlung für die Always On-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
