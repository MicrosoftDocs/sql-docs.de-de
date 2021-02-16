---
description: ALTER AVAILABILITY GROUP (Transact-SQL)
title: ALTER AVAILABILITY GROUP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/02/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AVAILABILITY_GROUP_TSQL
- ALTER_AVAILABILITY_TSQL
- ALTER AVAILABILITY GROUP
- ALTER AVAILABILITY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- ALTER AVAILABILITY GROUP statement
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: f039d0de-ade7-4aaf-8b7b-d207deb3371a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7dfa4ce102b0d53987ad782f6ab8883b2ff0037c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348553"
---
# <a name="alter-availability-group-transact-sql"></a>ALTER AVAILABILITY GROUP (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert eine vorhandene Always On-Verfügbarkeitsgruppe in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Die meisten ALTER AVAILABILITY GROUP-Argumente werden nur von dem aktuellen primäre Replikat unterstützt. Die JOIN-, FAILOVER- und FORCE_FAILOVER_ALLOW_DATA_LOSS-Argumente werden hingegen nur auf sekundären Replikaten unterstützt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
ALTER AVAILABILITY GROUP group_name   
  {  
     SET ( <set_option_spec> )   
   | ADD DATABASE database_name   
   | REMOVE DATABASE database_name  
   | ADD REPLICA ON <add_replica_spec>   
   | MODIFY REPLICA ON <modify_replica_spec>  
   | REMOVE REPLICA ON <server_instance>  
   | JOIN  
   | JOIN AVAILABILITY GROUP ON <add_availability_group_spec> [ ,...2 ]  
   | MODIFY AVAILABILITY GROUP ON <modify_availability_group_spec> [ ,...2 ]  
   | GRANT CREATE ANY DATABASE  
   | DENY CREATE ANY DATABASE  
   | FAILOVER  
   | FORCE_FAILOVER_ALLOW_DATA_LOSS   
   | ADD LISTENER 'dns_name' ( <add_listener_option> )  
   | MODIFY LISTENER 'dns_name' ( <modify_listener_option> )  
   | RESTART LISTENER 'dns_name'  
   | REMOVE LISTENER 'dns_name'  
   | OFFLINE  
  }  
[ ; ]  
  
<set_option_spec> ::=   
    AUTOMATED_BACKUP_PREFERENCE = { PRIMARY | SECONDARY_ONLY| SECONDARY | NONE }  
  | FAILURE_CONDITION_LEVEL  = { 1 | 2 | 3 | 4 | 5 }   
  | HEALTH_CHECK_TIMEOUT = milliseconds  
  | DB_FAILOVER  = { ON | OFF }   
  | DTC_SUPPORT  = { PER_DB | NONE }  
  | REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = { integer }
  
<server_instance> ::=   
 { 'system_name[\instance_name]' | 'FCI_network_name[\instance_name]' }  
  
<add_replica_spec>::=  
  <server_instance> WITH  
    (  
       ENDPOINT_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY },  
       FAILOVER_MODE = { AUTOMATIC | MANUAL }   
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
  
<modify_replica_spec>::=  
  <server_instance> WITH  
    (    
       ENDPOINT_URL = 'TCP://system-address:port'   
     | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }   
     | FAILOVER_MODE = { AUTOMATIC | MANUAL }   
     | SEEDING_MODE = { AUTOMATIC | MANUAL }   
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
          ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL }   
        | READ_ONLY_ROUTING_URL = 'TCP://system-address:port'   
          } )  
     | PRIMARY_ROLE ( {   
          ALLOW_CONNECTIONS = { READ_WRITE | ALL }   
        | READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE }   
          } )  
     | SESSION_TIMEOUT = seconds  
    )   
  
<add_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT },  
       FAILOVER_MODE = MANUAL,  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<modify_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER = 'TCP://system-address:port'  
       | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
       | SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<add_listener_option> ::=  
   {  
      WITH DHCP [ ON ( <network_subnet_option> ) ]  
    | WITH IP ( { ( <ip_address_option> ) } [ , ...n ] ) [ , PORT = listener_port ]  
   }  
  
  <network_subnet_option> ::=  
     'ipv4_address', 'ipv4_mask'    
  
  <ip_address_option> ::=  
     {   
        'four_part_ipv4_address', 'four_part_ipv4_mask'  
      | 'ipv6_address'  
     }  
  
<modify_listener_option>::=  
    {  
       ADD IP ( <ip_address_option> )   
     | PORT = listener_port  
    }  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *group_name*  
 Gibt den Namen der neuen Verfügbarkeitsgruppe an. *group_name* muss ein gültiger [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Bezeichner und in allen Verfügbarkeitsgruppen im WSFC-Cluster eindeutig sein.  
  
 AUTOMATED_BACKUP_PREFERENCE **=** { PRIMARY \| SECONDARY_ONLY \| SECONDARY \| NONE }  
 Legt fest, wie ein Sicherungsauftrag das primäre Replikat auswerten soll, wenn ausgewählt wird, wo Sicherungen ausgeführt werden müssen. Sie können einen gegebenen Sicherungsauftrag erstellen, um die automatisierte Sicherungseinstellung zu berücksichtigen. Die Einstellung wird nicht von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erzwungen und weist deshalb keine Auswirkungen auf Ad-hoc-Sicherungen auf.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
 Mit den Parametern werden folgende Werte angegeben:  
  
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
>  Um die automatisierte Sicherungseinstellung einer vorhandenen Verfügbarkeitsgruppe anzuzeigen, wählen Sie die Spalten **automated_backup_preference** oder **automated_backup_preference_desc** der Katalogsicht [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) aus. Darüber hinaus kann [sys.fn_hadr_backup_is_preferred_replica &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) verwendet werden, um das bevorzugte Sicherungsreplikat zu bestimmen.  Diese Funktion gibt immer 1 für mindestens eines der Replikate zurück, sogar wenn `AUTOMATED_BACKUP_PREFERENCE = NONE` ist.  
  
 FAILURE_CONDITION_LEVEL **=** { 1 \| 2 \| **3** \| 4 \| 5 }  
 Gibt an, welche Fehlerbedingungen ein automatisches Failover für diese Verfügbarkeitsgruppe auslösen. FAILURE_CONDITION_LEVEL wird auf Gruppenebene festgelegt, ist aber nur auf Verfügbarkeitsreplikaten relevant, die für den Verfügbarkeitsmodus mit synchronen Commits (AVAILIBILITY_MODE **=** SYNCHRONOUS_COMMIT) konfiguriert sind. Weiterhin können Fehlerbedingungen nur ein automatisches Failover auslösen, wenn das primäre und das sekundäre Replikat für den automatischen Failovermodus konfiguriert sind (FAILOVER_MODE **=** AUTOMATIC) und das sekundäre Replikat gerade mit dem primären Replikat synchronisiert wird.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
 Die Fehlerbedingungsebenen (1–5) reichen von der Ebene 1 mit den wenigsten Einschränkungen bis zur Ebene 5 mit den meisten Einschränkungen. Jede Bedingungsebene umfasst stets auch sämtliche weniger restriktiven Ebenen. Daher schließt die strengste Bedingungsebene 5 die vier Bedingungsebenen mit weniger Einschränkungen (1-4) ein, Ebene 4 schließt die Ebenen 1-3 ein usw. In der folgenden Tabelle wird die Fehlerbedingung beschrieben, die der jeweiligen Ebene entspricht.  
  
|Ebene|Fehlerbedingung|  
|-----------|-----------------------|  
|1|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst ist ausgefallen.<br /><br /> Das Leasing der Verfügbarkeitsgruppe für die Verbindung mit dem WSFC-Cluster läuft ab, da keine ACK-Meldung von der Serverinstanz empfangen wird. Weitere Informationen finden Sie unter [How It Works: SQL Server Always On Lease Timeout (Funktionsweise: SQL Server Always On-Leasetimeout)](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt keine Verbindung mit dem Cluster her, und der vom Benutzer angegebene HEALTH_CHECK_TIMEOUT-Schwellenwert der Verfügbarkeitsgruppe wurde überschritten.<br /><br /> Das Verfügbarkeitsreplikat weist einen fehlerhaften Status auf.|  
|3|Gibt an, dass ein automatisches Failover bei kritischen internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. verwaisten Spinlocks, schwerwiegenden Schreibzugriffsverletzungen oder zu vielen Sicherungen.<br /><br /> Dies ist das Standardverhalten.|  
|4|Gibt an, dass ein automatisches Failover bei mittelschweren internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. bei dauerhaft unzureichendem Arbeitsspeicher im internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcenpool.|  
|5|Gibt an, dass ein automatisches Failover bei sämtlichen qualifizierten Fehlerbedingungen initiiert werden soll, einschließlich:<br /><br /> Erschöpfung der SQL Engine-Arbeitsthreads.<br /><br /> Erkennung eines unlösbaren Deadlocks.|  
  
> [!NOTE]  
>  Das Fehlen einer Reaktion auf Clientanforderungen durch eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz ist für Verfügbarkeitsgruppen nicht relevant.  
  
 Der FAILURE_CONDITION_LEVEL- und der HEALTH_CHECK_TIMEOUT-Wert definieren eine *flexible Failoverrichtlinie* für eine angegebene Gruppe. Diese flexible Failoverrichtlinie bietet eine präzise Kontrolle der Bedingungen, die ein automatisches Failover verursachen müssen. Weitere Informationen finden Sie unter [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md).  
  
 HEALTH_CHECK_TIMEOUT **=** *milliseconds*  
 Gibt die Wartezeit (in Millisekunden) für die gespeicherte Systemprozedur [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) an, um Informationen über den Serverzustand zurückzugeben, ehe das WSFC-Cluster annimmt, dass die Serverinstanz langsam ist oder nicht reagiert. HEALTH_CHECK_TIMEOUT wird auf Gruppenebene festgelegt, ist aber nur für Verfügbarkeitsreplikate relevant, die für den Verfügbarkeitsmodus für synchrone Commits mit automatischem Failover (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT) konfiguriert sind.  Weiterhin kann ein Integritätsprüfungstimeout nur ein automatisches Failover auslösen, wenn das primäre und das sekundäre Replikat für den automatischen Failovermodus konfiguriert sind (FAILOVER_MODE **=** AUTOMATIC) und das sekundäre Replikat gerade mit dem primären Replikat synchronisiert wird.  
  
 Der standardmäßige HEALTH_CHECK_TIMEOUT-Wert beträgt 30.000 Millisekunden (30 Sekunden). Der minimale Wert beträgt 15.000 Millisekunden (15 Sekunden) und der maximale Wert 4.294.967.295 Millisekunden.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** führt keine Integritätsprüfungen auf Datenbankebene aus.  
  
 DB_FAILOVER  **=** { ON | OFF }  
 Gibt die Antwort an, die akzeptiert wird, wenn eine Datenbank auf dem primären Replikat offline ist. Wenn diese Option auf ON festgelegt ist, löst jeder Status außer ONLINE für eine Datenbank in der Verfügbarkeitsgruppe ein automatisches Failover aus. Wenn diese Option auf OFF festgelegt ist, wird nur die Integrität der Instanz verwendet, um ein automatisches Failover auszulösen.  
 
 Weitere Informationen zu dieser Einstellung finden Sie unter [Integritätserkennung auf Datenbankebene](../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). 

DTC_SUPPORT  **=** { PER_DB | NONE }  
Legt fest, ob verteilte Transaktionen für diese Verfügbarkeitsgruppe aktiviert sind. Verteilte Transaktionen werden ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] nur für Datenbanken für Verfügbarkeitsgruppen unterstützt, und datenbankübergreifende Transaktionen werden erst ab [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2 unterstützt. Mit `PER_DB` wird die Verfügbarkeitsgruppe mit Unterstützung dieser Transaktionen erstellt und die datenbankübergreifenden Transaktionen im Zusammenhang mit Datenbanken in der Verfügbarkeitsgruppe werden auf verteilte Transaktionen höhergestuft. `NONE` verhindert die automatische Höherstufung der datenbankübergreifenden Transaktionen auf verteilte Transaktionen, und die Datenbank wird nicht mit einer stabilen RMID in DTC registriert. Verteilte Transaktionen werden nicht verhindert, wenn die `NONE`-Einstellung verwendet wird, allerdings schlagen das Datenbankfailover und die automatische Wiederherstellung unter bestimmten Umständen fehl. Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
 
> [!NOTE]
> Die Unterstützung zum Ändern der DTC_SUPPORT-Einstellung einer Verfügbarkeitsgruppe wurde in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Service Pack 2 eingeführt. Diese Option kann nicht mit früheren Versionen verwendet werden. Sie müssen die Verfügbarkeitsgruppe mit DROP und CREATE löschen und erneut erstellen, um diese Einstellung in früheren Versionen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] zu ändern.
 
 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT   
 Eingeführt in SQL Server 2017. Dient zum Festlegen einer minimalen Anzahl synchroner sekundärer Replikate, um einen Commit auszuführen, bevor das primäre Replikat einen Commit für die Transaktion ausführt. Garantiert, dass die SQL Server-Transaktion wartet, bis die Transaktionsprotokolle für die minimale Anzahl von sekundären Replikaten aktualisiert werden. Der Standardwert ist 0. Dies bietet das gleiche Verhalten wie SQL Server 2016. Der Mindestwert beträgt 0. Der maximale Wert ist die Anzahl der Replikate minus 1. Diese Option bezieht sich auf Replikate im synchronen Commitmodus. Wenn sich Replikate im synchronen Commitmodus befinden, warten Schreibvorgänge auf dem primären Replikat, bis Schreibvorgänge auf dem zweiten synchronen Replikat an das Transaktionsprotokoll der Replikatsdatenbank übergeben werden. Wenn ein SQL Server, der ein sekundäres synchronisiertes Replikat hostet, nicht mehr reagiert, markiert der SQL Server, der das erste primäre Replikat hostet, dieses sekundäre Replikat als NOT SYNCHRONIZED (nicht synchronisiert). Wenn die nicht reagierende Datenbank wieder online geschaltet wird, befindet sie sich im Zustand „nicht synchronisiert“ und das Replikat wird als fehlerhaft markiert bis das primäre Replikat den Zustand „synchron“ wieder herstellen kann. Diese Einstellung stellt sicher, dass das primäre Replikat wartet, bis die minimale Anzahl der Replikate ein Commit für jede Transaktion ausgeführt hat. Wenn die minimale Anzahl der Replikate nicht verfügbar ist, schlagen Commits auf dem primären Replikat fehl. Für den Clustertyp `EXTERNAL` wird diese Einstellung geändert, wenn die Verfügbarkeitsgruppe der Clusterressource hinzugefügt wird. Weitere Informationen finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](../../linux/sql-server-linux-availability-group-ha.md).
  
 ADD DATABASE *database_name*  
 Gibt eine Liste von Benutzerdatenbanken an, die Sie der Verfügbarkeitsgruppe hinzufügen möchten. Diese Datenbanken müssen sich auf der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befinden, die das aktuelle primäre Replikat hostet. Sie können mehrere Datenbanken für eine Verfügbarkeitsgruppe angeben, aber jede Datenbank kann nur zu einer Verfügbarkeitsgruppe gehören. Informationen über die Datenbanktypen, für die eine Verfügbarkeitsgruppe Unterstützung bietet finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen&#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). In der Spalte **replica_id** in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht können Sie überprüfen, welche lokalen Datenbanken bereits zu einer Verfügbarkeitsgruppe gehören.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
> [!NOTE]  
>  Nachdem Sie eine Verfügbarkeitsgruppe erstellt haben, müssen Sie wiederum eine Verbindung zu jeder Serverinstanz herstellen, die ein sekundäres Replikat hostet, und anschließend jede sekundäre Datenbank vorbereiten und mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 REMOVE DATABASE *database_name*  
 Entfernt die angegebene primäre Datenbank und die entsprechenden sekundären Datenbanken aus der Verfügbarkeitsgruppe. Wird nur für das primäre Replikat unterstützt.  
  
 Informationen zu den empfohlenen Schritten nach dem Entfernen einer Verfügbarkeitsdatenbank aus einer Verfügbarkeitsgruppe finden Sie unter [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 ADD REPLICA ON  
 Gibt eine bis acht SQL Server-Instanzen an, in denen sekundäre Replikate in einer Verfügbarkeitsgruppe gehostet werden sollen.  Jedes Replikat wird von seiner Serverinstanzadresse gefolgt von einer WITH (…)-Klausel angegeben.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
 Sie müssen jedes neue sekundäre Replikat mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie in der Beschreibung der JOIN-Option weiter unten in diesem Abschnitt.  
  
 \<server_instance>  
 Gibt die Adresse der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, die als Host für ein Replikat fungiert. Das Adressformat hängt davon ab, ob die Instanz die Standardinstanz oder eine benannte Instanz ist und ob es eine eigenständige Instanz oder eine Failoverclusterinstanz (FCI) ist. Die Syntax lautet wie folgt:  
  
 { '*system_name*[\\*instance_name*]' | '*FCI_network_name*[\\*instance_name*]' }  
  
 Diese Adresse weist die folgenden Komponenten auf:  
  
 *system_name*  
 Der NetBIOS-Name des Computersystems, auf dem sich eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Zielinstanz befindet. Dieser Computer muss ein WSFC-Knoten sein.  
  
 *FCI_network_name*  
 Ist der Netzwerkname, der verwendet wird, um auf einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failovercluster zuzugreifen. Verwenden Sie diesen Namen, wenn die Serverinstanz als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverpartner beteiligt ist. Wenn SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) in einer FCI-Serverinstanz ausgeführt wird, wird die gesamte '*FCI_network_name*[\\*instance_name*]'-Zeichenfolge zurückgegeben (dabei handelt es sich um den vollständigen Replikatnamen).  
  
 *instance_name*  
 Ist der Name einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, die von *system_name* oder *FCI_network_name* gehostet wird und für die Always On aktiviert ist. Bei einer Standardserverinstanz ist *instance_name* optional. Bei dem Instanznamen wird die Groß-/Kleinschreibung berücksichtigt. In einer eigenständigen Serverinstanz stimmt der Name dieses Werts mit dem Wert überein, der beim Ausführen von SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) zurückgegeben wird.  
  
 \  
 Ist eine Trennzeichen, das nur bei der Angabe von *instance_name* verwendet wird, um dieses Element von *system_name* oder *FCI_network_name* zu trennen.  
  
 Informationen zu den Voraussetzungen für WSFC-Knoten und Serverinstanzen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen&#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 ENDPOINT_URL ='TCP://*system-address*:*port*'  
 Gibt den URL-Pfad des [Datenbankspiegelungsendpunkts](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an, die das Verfügbarkeitsreplikat, das Sie hinzufügen oder ändern, hostet.  
  
 ENDPOINT_URL ist in der ADD REPLICA ON-Klausel erforderlich und in der MODIFY REPLICA ON-Klausel optional.  Weitere Informationen finden Sie unter [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)zu unterstützen.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt eine URL zum Bestimmen einer Endpunkt-URL oder einer URL für das schreibgeschützte Routing an. Die URL-Parameter lauten wie folgt:  
  
 *system-address*  
 Ist eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die das Zielcomputersystem eindeutig identifiziert.  
  
 *port*  
 Ist eine Portnummer, die dem Spiegelungsendpunkt der Serverinstanz (für die ENDPOINT_URL-Option) oder der Portnummer, die von [!INCLUDE[ssDE](../../includes/ssde-md.md)] der Serverinstanz (für die READ_ONLY_ROUTING_URL-Option) verwendet wird, zugeordnet ist.  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 Gibt an, ob das primäre Replikat auf das sekundäre Replikat warten muss, um das Verstärken (Schreiben) der Protokolldatensätze auf einem Datenträger zu bestätigen, bevor das primäre Replikat die Transaktion auf einer bestimmten primären Datenbank ausführen kann. Die Transaktionen auf anderen Datenbanken über dasselbe primäre Replikat können unabhängig einen Commit ausführen.  
  
 SYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat mit der Ausführung von Transaktionen wartet, bis sie auf diesem sekundären Replikat (Modus mit synchronem Commit) verstärkt wurden. Sie können SYNCHRONOUS_COMMIT für bis zu drei Replikate angeben, einschließlich des primären Replikats.  
  
 ASYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat einen Commit für Transaktionen ausführt, ohne zu warten, bis dieses sekundäre Replikat das Protokoll verstärkt (Verfügbarkeitsmodus mit synchronem Commit). Sie können ASYNCHRONOUS_COMMIT für bis zu fünf Verfügbarkeitsreplikate angeben, einschließlich des primären Replikats.  

 CONFIGURATION_ONLY gibt an, dass das primäre Replikat ein synchrones Commit für Metadaten von Verfügbarkeitsgruppen an die Masterdatenbanken auf diesem Replikat durchführt. Das Replikat enthält keine Benutzerdaten. Diese Option:

- Kann auf einer beliebigen Edition von SQL Server, darunter Express Edition gehostet werden.
- Erfordert, dass der Datenbankspiegelungs-Endpunkt des CONFIGURATION_ONLY-Replikats vom Typ `WITNESS` ist.
- Kann nicht geändert werden.
- Ist nicht gültig, wenn `CLUSTER_TYPE = WSFC`. 

   Weitere Informationen finden Sie unter [Configuration only replica (Configuration only-Replikat)](../../linux/sql-server-linux-availability-group-ha.md).
    
 AVAILABILITY_MODE ist in der ADD REPLICA ON-Klausel erforderlich und in der MODIFY REPLICA ON-Klausel optional. Weitere Informationen finden Sie unter [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)ausgetauscht werden.  
  
 FAILOVER_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt den Failovermodus des Verfügbarkeitsreplikats an, das Sie definieren.  
  
 AUTOMATIC  
 Aktiviert das automatische Failover. AUTOMATIC wird nur unterstützt, wenn Sie auch AVAILABILITY_MODE = SYNCHRONOUS_COMMIT angeben. Sie können AUTOMATIC für drei Verfügbarkeitsreplikate angeben, einschließlich des primären Replikats.  
  
> [!NOTE]  
>  Vor SQL Server 2016 war dies, einschließlich des primären Replikats, auf zwei automatische Failoverreplikate beschränkt.
  
> [!NOTE]  
>  SQL Server-Failoverclusterinstanzen (FCIs) unterstützen kein automatisches Failover durch Verfügbarkeitsgruppen. Daher können die Verfügbarkeitsreplikate, die von einer FCI gehostet werden, nur für manuelles Failover konfiguriert werden.  
  
 MANUAL  
 Ermöglicht manuelles Failover oder erzwungenes manuelles Failover (*erzwungenes Failover*) durch den Datenbankadministrator.  
  
 FAILOVER_MODE ist in der ADD REPLICA ON-Klausel erforderlich und in der MODIFY REPLICA ON-Klausel optional. Zwei Typen manuellen Failovers sind vorhanden, manuelles Failover ohne Datenverlust und erzwungenes Failover (mit möglichem Datenverlust), die unter anderen Bedingungen unterstützt werden.  Weitere Informationen finden Sie weiter unten in diesem Thema unter [Failover und Failovermodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt an, wie für das sekundäre Replikat zuerst ein Seeding durchgeführt wird.  
  
 AUTOMATIC  
 Ermöglicht direktes Seeding. Diese Methode führt für das sekundäre Replikat ein Seeding über das Netzwerk aus. Mit dieser Methode ist es nicht mehr erforderlich, eine Kopie der primären Datenbank zu sichern und auf dem Replikat wiederherzustellen.  
  
> [!NOTE]  
>  Um das direkte Seeding zu ermöglichen, müssen Sie die Datenbankerstellung auf jedem sekundären Replikat zulassen, indem Sie den Befehl **ALTER AVAILABILITY GROUP** mit der Option **GRANT CREATE ANY DATABASE** aufrufen.  
  
 MANUAL  
 Gibt das manuelle Seeding an (Standard). Bei dieser Methode müssen Sie eine Sicherungskopie der Datenbank auf dem primären Replikat erstellen und diese manuell auf dem sekundären Replikat wiederherstellen.  
  
 BACKUP_PRIORITY **=** _n_  
 Gibt die Priorität für die Ausführung von Sicherungen auf diesem Replikat in Relation zu den anderen Replikaten in derselben Verfügbarkeitsgruppe an. Der Wert liegt im Bereich von 0 bis 100 und ist eine ganze Zahl. Diese Werte haben die folgenden Bedeutungen:  
  
-   1..100 gibt an, dass das Verfügbarkeitsreplikat zum Ausführen von Sicherungen ausgewählt werden könnte. 1 gibt die niedrigste Priorität und 100 die höchste Priorität an. Wenn BACKUP_PRIORITY = 1, würde das Verfügbarkeitsreplikat nur zum Ausführungen von Sicherungen ausgewählt werden, wenn gerade keine höheren Prioritätsverfügbarkeitsreplikate verfügbar sind.  
  
-   0 gibt an, dass dieses Verfügbarkeitsreplikat nie zum Ausführen von Sicherungen ausgewählt wird. Dies ist zum Beispiel für ein Remoteverfügbarkeitsreplikat hilfreich, für das keine Failover bei Sicherungen auftreten sollen.  
  
 Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
 SECONDARY_ROLE **(** ... **)**  
 Gibt rollenspezifische Einstellungen an, die wirksam werden, wenn dieses Verfügbarkeitsreplikat die sekundäre Rolle (d. h. wenn es gerade ein sekundäres Replikat ist) gerade besitzt. Geben Sie innerhalb der Klammern eine oder beide sekundäre Rollenoptionen an. Wenn Sie beide angeben, verwenden Sie eine durch Trennzeichen getrennte Liste.  
  
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
  
 Weitere Informationen zum Berechnen der schreibgeschützten Routing-URL für ein Verfügbarkeitsreplikat finden Sie unter [Berechnen von read_only_routing_url für Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
> [!NOTE]  
>  Für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollte der Transact-SQL-Listener konfiguriert werden, um einen bestimmten Port zu verwenden. Weitere Informationen finden Sie unter [Konfigurieren eines Servers zur Überwachung eines bestimmten TCP-Ports &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 PRIMARY_ROLE **(** ... **)**  
 Gibt rollenspezifische Einstellungen an, die wirksam werden, wenn dieses Verfügbarkeitsreplikat die primäre Rolle (d. h. wenn es gerade ein primäres Replikat ist) gerade besitzt. Geben Sie innerhalb der Klammern eine oder beide primäre Rollenoptionen an. Wenn Sie beide angeben, verwenden Sie eine durch Trennzeichen getrennte Liste.  
  
 Folgende Optionen stehen für die primäre Rolle zur Verfügung:  
  
 ALLOW_CONNECTIONS **=** { READ_WRITE | ALL }  
 Gibt den Verbindungstyp an, den die Datenbanken eines bestimmten Verfügbarkeitsreplikats, das die primäre Rolle einnimmt (das heißt, als primäres Replikat dient), von Clients akzeptieren können, z. B.:  
  
 READ_WRITE  
 Verbindungen, bei denen die Verbindungseigenschaft für den Anwendungszweck auf **ReadOnly** festgelegt ist, werden nicht zugelassen.  Wenn die Eigenschaft für die Anwendungsabsicht auf **ReadWrite** festgelegt ist oder keine Verbindungseigenschaft für die Anwendungsabsicht festgelegt wurde, wird die Verbindung zugelassen. Weitere Informationen zur Verbindungseigenschaft für die Anwendungsabsicht finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Für die Datenbanken im primären Replikat sind alle Verbindungen zugelassen. Dies ist das Standardverhalten.  
  
 READ_ONLY_ROUTING_LIST **=** { **('** \<server_instance> **'** [ **,** ...*n* ] **)** | NONE }  
 Gibt beim Ausführen unter der sekundären Rolle eine durch Trennzeichen getrennte Liste von Serverinstanzen an, die Verfügbarkeitsreplikate für diese Verfügbarkeitsgruppe hosten, die die folgenden Anforderungen erfüllt:  
  
-   Wird konfiguriert, um alle Verbindungen oder schreibgeschützte Verbindungen (siehe das obige ALLOW_CONNECTIONS-Argument der SECONDARY_ROLE-Option) zuzulassen.  
  
-   Die schreibgeschützte Routing-URL wurde definiert (siehe das obige READ_ONLY_ROUTING_URL-Argument der SECONDARY_ROLE-Option).  
  
 Die READ_ONLY_ROUTING_LIST-Werte lauten wie folgt:  
  
 \<server_instance>  
 Gibt die Adresse der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, die als Host für ein Verfügbarkeitsreplikat fungiert, das ein lesbares sekundäres Replikat ist, wenn es unter der sekundären Rolle ausgeführt wird.  
  
 Verwenden Sie eine durch Trennzeichen getrennte Liste, um alle der Serverinstanzen anzugeben, die ein lesbares sekundäres Replikat hosten könnten. Schreibgeschütztes Routing erfolgt in der Reihenfolge, in der Serverinstanzen in der Liste angegeben werden. Wenn Sie die Hostserverinstanz eines Replikats auf der schreibgeschützten Routingliste des Replikats einschließen, ist es eine empfohlene Vorgehensweise, diese Serverinstanz am Ende der Liste zu platzieren, damit Verbindungen für beabsichtigte Lesevorgänge bei Verfügbarkeit zu einem sekundären Replikat wechseln.  
  
 Beginnend mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie einen Lastenausgleich für Anforderungen von beabsichtigten Lesevorgängen über lesbare sekundäre Replikate durchführen. Dies können Sie angeben, indem Sie die Replikate in geschachtelten Klammern innerhalb der schreibgeschützten Routingliste platzieren. Weitere Informationen und Beispiele finden Sie unter [Configure load-balancing across read-only replicas (Konfigurieren des Lastenausgleichs für mehrere schreibgeschützte Replikate)](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
 Keine  
 Gibt an, dass, wenn dieses Verfügbarkeitsreplikat das primäre Replikat ist, schreibgeschütztes Routing nicht unterstützt wird. Dies ist das Standardverhalten. Wenn dieser Wert zusammen mit MODIFY REPLICA ON verwendet wird, aktiviert er ggf. die vorhandene Liste.  

 READ_WRITE_ROUTING_URL **=** { **('** \<server_instance> **')** }  
 Gilt für: SQL Server (ab SQL Server 2019 (15.x)) 

 Gibt bei Ausführung unter der primären Rolle Serverinstanzen an, die Verfügbarkeitsreplikate für diese Verfügbarkeitsgruppe hosten, die die folgenden Anforderungen erfüllen:
-   Die Replikatspezifikation PRIMARY_ROLE enthält READ_WRITE_ROUTING_URL.
-   Die Verbindungszeichenfolge ist ReadWrite, entweder indem ApplicationIntent als ReadWrite definiert wird oder indem ApplicationIntent nicht festgelegt und somit der Standard (ReadWrite) wirksam wird.

Weitere Informationen finden Sie unter [Umleitung von Lese-/Schreibverbindungen vom sekundären zum primären Replikat (Always On-Verfügbarkeitsgruppen)](../../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).

 SESSION_TIMEOUT **=** _seconds_  
 Gibt den Zeitraum für das Sitzungstimeout in Sekunden an. Wenn Sie die Option nicht angeben, beträgt der Timeoutzeitraum standardmäßig 10 Sekunden. Der Wert muss mindestens 5 Sekunden betragen.  
  
> [!IMPORTANT]  
>  Es wird empfohlen, einen Timeoutzeitraum von 10 Sekunden oder mehr zu wählen.  
  
 Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 MODIFY REPLICA ON  
 Ändert ein beliebiges Replikat der Verfügbarkeitsgruppe. Die Liste der zu ändernden Replikate enthält die Serverinstanzadresse und eine WITH (...)-Klausel für jedes Replikat.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
 REMOVE REPLICA ON  
 Entfernt das angegebene sekundäre Replikat aus der Verfügbarkeitsgruppe. Das aktuelle primäre Replikat kann nicht aus einer Verfügbarkeitsgruppe entfernt werden. Das Replikat empfängt keine Daten mehr, wenn es entfernt wird. Seine sekundären Datenbanken werden aus der Verfügbarkeitsgruppe entfernt und nehmen den Status RESTORING an.  
  
 Wird nur für das primäre Replikat unterstützt.  
  
> [!NOTE]  
>  Wenn Sie ein Replikat im nicht verfügbaren oder fehlerhaften Status entfernen, erkennt es, dass es nicht mehr zur Verfügbarkeitsgruppe gehört, wenn es wieder online ist.  
  
 JOIN  
 Bewirkt, dass die lokale Serverinstanz ein sekundäres Replikat in der angegebenen Verfügbarkeitsgruppe hostet.  
  
 Wird nur für ein sekundäres Replikat unterstützt, das der Verfügbarkeitsgruppe noch nicht hinzugefügt wurde.  
  
 Weitere Informationen finden Sie unter [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)mit einer Always On-Verfügbarkeitsgruppe verknüpft wird.  
  
 FAILOVER  
Initiiert ein manuelles Failover der Verfügbarkeitsgruppe ohne Datenverlust an das sekundäre Replikat, mit dem Sie verbunden sind. Das Replikat, von dem das primäre Replikat gehostet wird, wird das *Failoverziel*.  Das Failoverziel übernimmt die primäre Rolle und stellt seine Kopie jeder Datenbank wieder her und schaltet sie als neue primäre Datenbanken online. Das frühere primäre Replikat geht gleichzeitig in die sekundäre Rolle über, und seine Datenbanken werden sekundäre Datenbanken und werden sofort angehalten. Zwischen diesen Rollen kann möglicherweise durch eine Reihe von Fehlern hin- und hergeschaltet werden.  
  
 Wird nur auf einem sekundären Replikat mit synchronem Commit unterstützt, das derzeit mit dem primären Replikat synchronisiert ist. Hinweis: Damit das sekundäre Replikat synchronisiert werden kann, muss das primäre Replikat ebenfalls im Modus mit synchronem Commit ausgeführt werden.  
  
> [!NOTE]  
>  Ein Failoverbefehl gibt einen Wert zurück, sobald das Failoverziel den Befehl akzeptiert hat. Die Datenbankwiederherstellung tritt jedoch asynchron auf, nachdem die Verfügbarkeitsgruppe aufgehört hat, ein Failover auszuführen.  
  
 Informationen zu Einschränkungen, Voraussetzungen und Empfehlungen in Bezug auf das Ausführen eines geplanten manuellen Failovers finden Sie unter [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
 FORCE_FAILOVER_ALLOW_DATA_LOSS  
 > [!CAUTION]  
>  Das Erzwingen eines Failovers kann zu Datenverlusten führen und ist daher ausschließlich als Notfallwiederherstellungsmethode vorgesehen. Daher empfehlen wir dringend, dass Sie nur Failover erzwingen, wenn das primäre Replikat nicht mehr ausgeführt wird, da Sie bereitwillig Datenverluste riskieren. Außerdem müssen Sie den Dienst sofort für die Verfügbarkeitsgruppe wiederherstellen.  
  
 Wird nur auf einem Replikat unterstützt, dessen Rolle sich im Status SECONDARY oder RESOLVING befindet. Das Replikat, auf dem Sie einen Failoverzielbefehl eingeben, wird als *Failoverziel* bezeichnet.  
  
 Erzwingt ein Failover der Verfügbarkeitsgruppe zum Failoverziel (mit möglichem Datenverlust). Das Failoverziel übernimmt die primäre Rolle und stellt seine Kopie jeder Datenbank wieder her und schaltet sie als neue primäre Datenbanken online. Auf jeglichen verbleibenden sekundären Replikaten wird jede sekundäre Datenbank angehalten, bis sie manuell fortgesetzt wird. Wenn das frühere primäre Replikat verfügbar wird, wechselt es zur sekundären Rolle, und seine Datenbanken werden angehaltene sekundäre Datenbanken.  
  
> [!NOTE]  
>  Ein Failoverbefehl gibt einen Wert zurück, sobald das Failoverziel den Befehl akzeptiert hat. Die Datenbankwiederherstellung tritt jedoch asynchron auf, nachdem die Verfügbarkeitsgruppe aufgehört hat, ein Failover auszuführen.  
  
 Informationen zu den Einschränkungen, Voraussetzungen und Empfehlungen zum Erzwingen eines Failovers sowie den Auswirkungen eines erzwungenen Failovers auf die zuvor primären Datenbanken in der Verfügbarkeitsgruppe finden Sie unter [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
 ADD LISTENER **'** _dns\_name_ **'(** \<add_listener_option> **)**  
 Definiert einen neuen Verfügbarkeitsgruppenlistener für diese Verfügbarkeitsgruppe. Wird nur für das primäre Replikat unterstützt.  
  
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
>  NetBIOS erkennt nur die ersten 15 Zeichen im dns_name. Wenn Sie zwei WSFC-Cluster verwenden, die vom gleichen Active Directory gesteuert werden, und Sie versuchen, Verfügbarkeitsgruppenlistener in beiden Clustern mit Namen mit mehr als 15 Zeichen und einem identischen 15-Zeichen-Präfix zu erstellen, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass die VNN-Ressource nicht online geschaltet werden konnte. Informationen zu Präfix-Benennungsregeln für DNS-Namen finden Sie unter [Zuweisen von Domänennamen](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
 JOIN AVAILABILITY GROUP ON  
 Tritt einer *verteilten Verfügbarkeitsgruppe* bei. Wenn Sie eine verteilte Verfügbarkeitsgruppe erstellen, ist die Verfügbarkeitsgruppe in dem Cluster, in dem sie erstellt wird, die primäre Verfügbarkeitsgruppe. Die Verfügbarkeitsgruppe, die der verteilten Verfügbarkeitsgruppe beitritt, ist die sekundäre Verfügbarkeitsgruppe.  
  
 \<ag_name>  
 Gibt den Namen der Verfügbarkeitsgruppe an, die eine Hälfte der verteilten Verfügbarkeitsgruppe ausmacht.  
  
 LISTENER **='** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt den URL-Pfad für den Listener an, der der Verfügbarkeitsgruppe zugeordnet ist.  
  
 Die LISTENER-Klausel ist erforderlich.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Gibt eine URL für den Listener an, der der Verfügbarkeitsgruppe zugeordnet ist. Die URL-Parameter lauten wie folgt:  
  
 *system-address*  
 Eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die den Listener eindeutig identifiziert.  
  
 *port*  
 Eine Portnummer, die dem Spiegelungsendpunkt der Verfügbarkeitsgruppe zugeordnet ist. Beachten Sie, dass dies nicht der Port des Listeners ist.  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
 Gibt an, ob das primäre Replikat auf die sekundäre Verfügbarkeitsgruppe warten muss, um das Verstärken (Schreiben) der Protokolldatensätze auf einem Datenträger zu bestätigen, bevor das primäre Replikat die Transaktion auf einer bestimmten primären Datenbank ausführen kann.  
  
 SYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat mit der Ausführung von Transaktionen wartet, bis sie auf dieser sekundären Verfügbarkeitsgruppe verstärkt wurden. Sie können SYNCHRONOUS_COMMIT für bis zu zwei Verfügbarkeitsgruppen angeben, einschließlich der primären Verfügbarkeitsgruppe.  
  
 ASYNCHRONOUS_COMMIT  
 Gibt an, dass das primäre Replikat Transaktionen ausführt, ohne zu warten, bis diese sekundäre Verfügbarkeitsgruppe das Protokoll verstärkt. Sie können ASYNCHRONOUS_COMMIT für bis zu zwei Verfügbarkeitsgruppen angeben, einschließlich der primären Verfügbarkeitsgruppe.  
  
 Die AVAILABILITY_MODE-Klausel ist erforderlich.  
  
 FAILOVER_MODE **=** { MANUAL }  
 Gibt den Failovermodus der verteilten Verfügbarkeitsgruppe an.  
  
 MANUAL  
 Ermöglicht ein geplantes manuelles Failover oder ein erzwungenes manuelles Failover (üblicherweise als *erzwungenes Failover* bezeichnet) durch den Datenbankadministrator.  
  
 Das automatische Failover auf die sekundäre Verfügbarkeitsgruppe wird nicht unterstützt.  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Gibt an, wie für die sekundäre Verfügbarkeitsgruppe zuerst ein Seeding durchgeführt wird.  
  
 AUTOMATIC  
 Ermöglicht das automatische Seeding. Diese Methode führt für die sekundäre Verfügbarkeitsgruppe ein Seeding über das Netzwerk aus. Mit dieser Methode ist es nicht mehr erforderlich, eine Kopie der primären Datenbank zu sichern und auf den Replikaten der sekundären Verfügbarkeitsgruppe wiederherzustellen.  
  
 MANUAL  
 Gibt das manuelle Seeding an. Bei dieser Methode müssen Sie eine Sicherungskopie der Datenbank auf dem primären Replikat erstellen und diese manuell auf dem Replikat/den Replikaten der sekundären Verfügbarkeitsgruppe wiederherstellen.  
  
 MODIFY AVAILABILITY GROUP ON  
 Ändert Verfügbarkeitsgruppeneinstellungen einer verteilten Verfügbarkeitsgruppe. Die Liste der zu ändernden Verfügbarkeitsgruppen enthält für jede Verfügbarkeitsgruppe den Namen der Verfügbarkeitsgruppe und eine WITH (…)-Klausel.  
  
> [!IMPORTANT]  
>  Dieser Befehl muss in den Instanzen der primären und der sekundären Verfügbarkeitsgruppe wiederholt werden.  
  
 GRANT CREATE ANY DATABASE  
 Erlaubt der Verfügbarkeitsgruppe die Erstellung von Datenbanken für das primäre Replikat, welches das direkte Seeding unterstützt (SEEDING_MODE = AUTOMATIC). Dieser Parameter muss nach dem Beitritt des sekundären Replikats zur Verfügbarkeitsgruppe auf allen sekundären Replikaten ausgeführt werden, die das direkte Seeding unterstützen.  Erfordert die CREATE ANY DATABASE-Berechtigung.  
  
 DENY CREATE ANY DATABASE  
 Entzieht der Verfügbarkeitsgruppe die Erlaubnis, Datenbanken für das primäre Replikat zu erstellen.  
  
 \<add_listener_option>  
 ADD LISTENER verwendet eine der folgenden Optionen:  
  
 WITH DHCP [ ON { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')** } ]  
 Gibt an, dass der Verfügbarkeitsgruppenlistener das Dynamic Host Configuration-Protokoll (DHCP) verwendet.  Verwenden Sie die ON-Klausel optional, um das Netzwerk zu identifizieren, auf dem dieser Listener erstellt wird. DHCP ist auf ein einzelnes Subnetz beschränkt, das für alle Serverinstanzen verwendet wird, die ein Verfügbarkeitsreplikat in der Verfügbarkeitsgruppe hosten.  
  
> [!IMPORTANT]  
>  DHCP wird in einer Produktionsumgebung nicht empfohlen. Wenn die DHCP-IP-Leasedauer bei einer Downtime abläuft, ist eine Verlängerung erforderlich, um die neue IP-Adresse des DHCP-Netzwerks zu registrieren, die dem DNS-Namen des Listeners zugeordnet ist, was sich auf die Clientkonnektivität auswirkt. DHCP eignet sich jedoch gut zum Einrichten der Entwicklungs- und Testumgebung, um grundlegende Funktionen von Verfügbarkeitsgruppen und die Integration in Ihre Anwendungen zu überprüfen.  
  
 Beispiel:  
  
 `WITH DHCP ON ('10.120.19.0','255.255.254.0')`  
  
 WITH IP **(** { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')**  |  **('** _ipv6\_address_ **')** } [ **,** ..._n_ ] **)** [ **,** PORT **=** _listener\_port_ ]  
 Gibt an, dass, der Listener der Verfügbarkeitsgruppe statt DHCPr eine oder mehrere statische IP-Adressen verwendet. Um eine Verfügbarkeitsgruppe über mehrere Subnetze zu erstellen, erfordert jedes Subnetz in der Listenerkonfiguration eine statische IP-Adresse. Für ein angegebenes Subnetz kann die statische IP-Adresse entweder eine IPv4-Adresse oder eine IPv6-Adresse sein. Wenden Sie sich an Ihren Netzwerkadministrator, um eine statische IP-Adresse für jedes Subnetz zu erhalten, das ein Verfügbarkeitsreplikat für die neue Verfügbarkeitsgruppe hostet.  
  
 Beispiel:  
  
 `WITH IP ( ('10.120.19.155','255.255.254.0') )`  
  
 *ipv4_address*  
 Gibt eine vierteilige IPv4-Adresse für einen Verfügbarkeitsgruppenlistener an. Beispiel: `10.120.19.155`.  
  
 *ipv4_mask*  
 Gibt eine vierteilige IPv4-Maske für einen Verfügbarkeitsgruppenlistener an. Beispiel: `255.255.254.0`.  
  
 *ipv6_address*  
 Gibt eine IPv6-Adresse für einen Verfügbarkeitsgruppenlistener an. Beispiel: `2001::4898:23:1002:20f:1fff:feff:b3a3`.  
  
 PORT **=** *listener_port*  
 Gibt die Portnummer (*listener_port*) an, die von einem Verfügbarkeitsgruppenlistener verwendet wird, der anhand einer WITH IP-Klausel angegeben wird. PORT ist optional.  
  
 Die Standardportnummer 1433 wird unterstützt. Wenn Sie jedoch Sicherheitsbedenken hegen, empfehlen wir die Verwendung einer anderen Portnummer.  
  
 Beispiel: `WITH IP ( ('2001::4898:23:1002:20f:1fff:feff:b3a3') ) , PORT = 7777`  
  
 MODIFY LISTENER **'** _dns\_name_ **'(** \<modify\_listener\_option\> **)**  
 Ändert einen vorhandenen Verfügbarkeitsgruppenlistener für diese Verfügbarkeitsgruppe. Wird nur für das primäre Replikat unterstützt.  
  
 \<modify\_listener\_option\>  
 MODIFY LISTENER verwendet eine der folgenden Optionen:  
  
 ADD IP { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4_mask_ **')** \| <b>('</b>dns\_name *ipv6\_address* __')__ }  
 Fügt dem durch *dns\_name* angegebenen Verfügbarkeitsgruppenlistener die angegebene IP-Adresse hinzu.  
  
 PORT **=** *listener_port*  
 Die Beschreibung dieses Arguments finden Sie weiter oben in diesem Abschnitt.  
  
 RESTART LISTENER **'** _dns\_name_ **'**  
 Startet den Listener, der dem angegebenen DNS-Namen zugeordnet ist, erneut. Wird nur für das primäre Replikat unterstützt.  
  
 REMOVE LISTENER **'** _dns\_name_ **'**  
 Entfernt den Listener, der dem angegebenen DNS-Namen zugeordnet ist. Wird nur für das primäre Replikat unterstützt.  
  
 OFFLINE  
 Schaltet eine Onlineverfügbarkeitsgruppe offline. Es gibt keinen Datenverlust bei Datenbanken mit synchronem Commit.  
  
 Nachdem eine Verfügbarkeitsgruppe offline geschaltet wurde, sind ihre Datenbanken für Clients nicht mehr verfügbar, und Sie können die Verfügbarkeitsgruppe nicht wieder online schalten. Verwenden Sie die OFFLINE-Option daher nur während einer Kreuzclustermigration von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], wenn Sie Verfügbarkeitsgruppenressourcen zu einem neuen WSFC-Cluster migrieren.  
  
 Weitere Informationen finden Sie unter [Offlineschalten einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md).  
  
## <a name="prerequisites-and-restrictions"></a>Voraussetzungen und Einschränkungen  
 Informationen zu Voraussetzungen und Einschränkungen bei Verfügbarkeitsreplikaten sowie zu deren Hostserverinstanzen und -computer finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen (Always On-Verfügbarkeitsgruppen) &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 Informationen zu Einschränkungen bei den AVAILABILITY GROUP-Transact-SQL-Anweisungen finden Sie unter [Übersicht über Transact-SQL-Anweisungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  Erfordert außerdem die ALTER ANY DATABASE-Berechtigung.   
  
## <a name="examples"></a>Beispiele  
  
###  <a name="a-joining-a-secondary-replica-to-an-availability-group"></a><a name="Join_Secondary_Replica"></a> A. Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe  
 Im folgenden Beispiel wird das sekundäre Replikat, mit dem Sie verbunden sind, mit der `AccountsAG`-Verfügbarkeitsgruppe verknüpft.  
  
```SQL  
ALTER AVAILABILITY GROUP AccountsAG JOIN;  
GO  
```  
  
###  <a name="b-forcing-failover-of-an-availability-group"></a><a name="Force_Failover"></a> B. Erzwingen eines Failovers einer Verfügbarkeitsgruppe  
 Im folgenden Beispiel wird ein Failover der `AccountsAG`-Verfügbarkeitsgruppe zum sekundären Replikat erzwungen, mit dem Sie verbunden sind.  
  
```SQL
ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [DROP AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-availability-group-transact-sql.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Problembehandlung für die Always On-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
