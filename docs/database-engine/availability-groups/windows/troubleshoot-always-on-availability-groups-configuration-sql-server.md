---
title: Häufige Probleme mit Verfügbarkeitsgruppen und Lösungen
description: Beheben Sie typische Probleme mit der Konfiguration von Serverinstanzen für Always On-Verfügbarkeitsgruppen in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b7c6a5df1bcef7e8438c01910233c2c3a38d070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337932"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema enthält Informationen, um Sie beim Beheben typischer Probleme beim Konfigurieren von Serverinstanzen für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]zu unterstützen. Typische Konfigurationsprobleme: [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ist deaktiviert, Konten werden falsch konfiguriert, der Datenbankspiegelungs-Endpunkt ist nicht vorhanden, auf den Endpunkt kann nicht zugegriffen werden (SQL Server-Fehler 1418), Netzwerkzugriff ist nicht vorhanden und der Befehl zum Verknüpfen der Datenbank schlägt fehl (SQL Server-Fehler 35250).  
  
> [!NOTE]  
>  Stellen Sie sicher, dass die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Voraussetzungen erfüllt sind. Weitere Informationen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)zu unterstützen.  
  
 **In diesem Thema:**  
  
|`Section`|BESCHREIBUNG|  
|-------------|-----------------|  
|[AlwaysOn-Verfügbarkeitsgruppen ist nicht aktiviert](#IsHadrEnabled)|Wenn eine Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nicht für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]aktiviert ist, wird von der Instanz die Verfügbarkeitsgruppenerstellung nicht unterstützt. Es können außerdem keine Verfügbarkeitsreplikate gehostet werden.|  
|[Konten](#Accounts)|Erläutert die Anforderungen für eine ordnungsgemäße Konfiguration der Konten, unter denen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ausgeführt wird.|  
|[Endpunkte](#Endpoints)|Erläutert, wie Probleme mit dem Datenbankspiegelungs-Endpunkt einer Serverinstanz diagnostiziert werden.|  
|[Systemname](#SystemName)|Fasst die Alternativen zum Angeben des Systemnamens einer Serverinstanz in einer Endpunkt-URL zusammen.|  
|[Netzwerkzugriff](#NetworkAccess)|Dokumentiert die Anforderung, dass jeder Serverinstanz, die ein Verfügbarkeitsreplikat hostet, der Zugriff auf den Port aller anderen Serverinstanzen über TCP ermöglicht werden muss.|  
|[Endpunktzugriff (SQL Server-Fehler 1418)](#Msg1418)|Enthält Informationen zu dieser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Fehlermeldung.|  
|[Fehler beim Verknüpfen der Datenbank (SQL Server-Fehler 35250)](#JoinDbFails)|Erläutert die möglichen Ursachen und die Lösung des Fehlers beim Verknüpfen von sekundären Datenbanken mit einer Verfügbarkeitsgruppe aufgrund einer inaktiven Verbindung mit dem primären Replikat.|  
|[Schreibgeschütztes Routing funktioniert nicht ordnungsgemäß](#ROR)||  
|[Verwandte Aufgaben](#RelatedTasks)|Enthält eine Liste aufgabenbezogener Themen in der [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] -Onlinedokumentation, die besonders relevant für die Problembehandlung an einer Verfügbarkeitsgruppenkonfiguration sind.|  
|[Verwandte Inhalte](#RelatedContent)|Enthält eine Liste von relevanten Ressourcen außerhalb der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation.|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> AlwaysOn-Verfügbarkeitsgruppen ist nicht aktiviert  
 Die Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] muss auf allen Instanzen von [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]aktiviert werden. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
##  <a name="accounts"></a><a name="Accounts"></a> Konten  
 Die Konten, unter denen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ausgeführt wird, müssen ordnungsgemäß konfiguriert sein.  
  
1.  Verfügen die Konten über die richtigen Berechtigungen?  
  
    1.  Falls die Partner unter demselben Domänenbenutzerkonto ausgeführt werden, sind die richtigen Benutzeranmeldenamen automatisch in beiden **master** -Datenbanken vorhanden. Dadurch wird die Sicherheitskonfiguration der Datenbank vereinfacht und wird somit empfohlen.  
  
    2.  Wenn zwei Serverinstanzen als unterschiedliche Konten ausgeführt werden, muss die Anmeldung für die Konten in **master** auf der Remoteserverinstanz erstellt werden, und dieser Anmeldung müssen CONNECT-Berechtigungen gewährt werden, damit eine Verbindung mit dem Datenbankspiegelungs-Endpunkt dieser Serverinstanz hergestellt werden kann. Weitere Informationen finden Sie unter [Einrichten von Anmeldekonten für die Datenbankspiegelung oder Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
2.  Wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] als integriertes Konto, z. B. Lokales System, Lokaler Dienst oder Netzwerkdienst, oder als Nicht-Domänenkonto ausgeführt wird, müssen Sie Zertifikate zur Endpunktauthentifizierung verwenden. Wenn die Dienstkonten Domänenkonten in derselben Domäne verwenden, können Sie CONNECT-Zugriff für jedes Dienstkonto an allen Replikatspeicherorten gewähren oder Zertifikate verwenden. Weitere Informationen finden Sie unter [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a>Endpoints  
 Endpunkte müssen ordnungsgemäß konfiguriert sein.  
  
1.  Stellen Sie sicher, dass jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die ein Verfügbarkeitsreplikat hosten wird (jeder *Replikatspeicherort*) einen Datenbankspiegelungs-Endpunkt besitzt. Um festzustellen, ob auf einer bestimmten Serverinstanz bereits ein Datenbankspiegelungs-Endpunkt vorhanden ist, verwenden Sie die Katalogsicht [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) . Weitere Informationen finden Sie unter [Create a Database Mirroring Endpoint for Windows Authentication (Transact-SQL) (Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung (Transact-SQL))](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) oder [Allow a Database Mirroring Endpoint to Use Certificates for Outbound Connections (Transact-SQL) (Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt (Transact-SQL))](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  Überprüfen Sie, ob die Portnummern richtig sind.  
  
     Verwenden Sie die folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung, um den Port zu identifizieren, der derzeit dem Endpunkt der Datenbankspiegelung einer Serverinstanz zugeordnet ist:  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  Bei Problemen mit der Einrichtung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , die schwer zu erklären sind, empfiehlt es sich, jede Serverinstanz zu prüfen, um zu ermitteln, ob von der Serverinstanz die richtigen Ports überwacht werden.  
  
4.  Stellen Sie sicher, dass die Endpunkte gestartet wurden (STATE=STARTED). Verwenden Sie auf jeder Serverinstanz folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Weitere Informationen zur **state_desc**-Spalte finden Sie unter [database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Verwenden Sie die folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung, um einen Endpunkt zu starten:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Stellen Sie sicher, dass der Anmeldename auf dem anderen Server über CONNECT-Berechtigungen verfügt. Führen Sie auf jeder Serverinstanz die folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung aus, um zu ermitteln, wer über CONNECT-Berechtigungen für einen Endpunkt verfügt:  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 Für den Systemnamen einer Serverinstanz in einer Endpunkt-URL können Sie jeden beliebigen Namen verwenden, der das System eindeutig bezeichnet. Die Serveradresse kann ein Systemname sein (wenn sich die Systeme in derselben Domäne befinden), ein vollqualifizierter Domänenname oder eine IP-Adresse (vorzugsweise eine statische IP-Adresse). Bei Verwendung des vollqualifizierten Domänennamens ist eine problemfreie Funktionsweise sichergestellt. Weitere Informationen finden Sie unter [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)zu unterstützen.  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Jeder Serverinstanz, die ein Verfügbarkeitsreplikat hostet, muss der Zugriff auf den Port aller anderen Serverinstanzen über TCP ermöglicht werden. Dies ist insbesondere von Bedeutung, wenn sich die Serverinstanzen in unterschiedlichen Domänen befinden, die sich nicht vertrauen (nicht vertrauenswürdige Domänen).  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> Endpunktzugriff (SQL Server-Fehler 1418)  
 In dieser Meldung in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wird angegeben, dass die in der Endpunkt-URL angegebene Servernetzwerkadresse nicht erreicht werden kann oder nicht vorhanden ist. Es wird vorgeschlagen, den Namen der Netzwerkadresse zu überprüfen und den Befehl erneut auszugeben.  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> Fehler beim Verknüpfen der Datenbank (SQL Server-Fehler 35250)  
 Dieser Abschnitt erläutert die möglichen Ursachen und die Lösung des Fehlers beim Verknüpfen von sekundären Datenbanken mit einer Verfügbarkeitsgruppe aufgrund einer inaktiven Verbindung mit dem primären Replikat.  
  
 **Lösung:**  
  
1.  Überprüfen Sie die Firewalleinstellung, um zu ermitteln, ob die Endpunktportkommunikation zwischen den Serverinstanzen, auf denen das primäre Replikat gehostet wird, und dem sekundären Replikat (standardmäßig Port 5022) möglich ist.  
  
2.  Überprüfen Sie, ob das Netzwerkdienstkonto über Verbindungsberechtigung für den Endpunkt verfügt.  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> Schreibgeschütztes Routing funktioniert nicht ordnungsgemäß  
 Überprüfen Sie die folgenden Konfigurationswerteinstellungen und korrigieren Sie sie gegebenenfalls.  
  
|Am...|Aktion|Kommentare|Link|  
|---------|------------|--------------|----------|  
|dem aktuellen primären Replikat|Stellen Sie sicher, dass der Listener der Verfügbarkeitsgruppe online ist.|**So überprüfen Sie, ob der Listener online ist:**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **So starten Sie einen Offlinelistener neu:**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|dem aktuellen primären Replikat|Stellen Sie sicher, dass READ_ONLY_ROUTING_LIST nur Serverinstanzen enthält, die ein lesbares sekundäres Replikat hosten.|**So identifizieren Sie lesbare sekundäre Replikate:** sys.availability_replicas (Spalte **econdary_role_allow_connections_desc** )<br /><br /> **So zeigen Sie eine schreibgeschützte Routingliste an:** sys.availability_read_only_routing_lists<br /><br /> **So ändern Sie eine schreibgeschützte Routingliste:** ALTER AVAILABILITY GROUP|[sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|jedem Replikat in read_only_routing_list|Stellen Sie sicher, dass die Windows-Firewall den Port READ_ONLY_ROUTING_URL nicht blockiert.|-|[Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|jedem Replikat in read_only_routing_list|Überprüfen Sie in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager die folgenden Aspekte:<br /><br /> SQL Server-Remoteverbindung ist aktiviert.<br /><br /> TCP/IP ist aktiviert.<br /><br /> Die IP-Adressen sind ordnungsgemäß konfiguriert.|-|[Anzeigen oder Ändern von Servereigenschaften &#40;SQL Server&#41;](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [Konfigurieren eines Servers zur Überwachung eines bestimmten TCP-Ports &#40;SQL Server-Konfigurations-Manager&#41;](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|jedem Replikat in read_only_routing_list|Stellen Sie sicher, dass die READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*Systemadresse*<strong>:</strong>*Port*) den richtigen vollqualifizierten Domänennamen (FQDN) und die richtige Portnummer enthält.|-|[Berechnen von „read_only_routing_url“ für AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Clientsystem|Überprüfen Sie, ob der Clienttreiber schreibgeschütztes Routing unterstützt.|-|[AlwaysOn-Clientkonnektivität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [Verwaltung von Anmeldungen und Aufträgen für die Datenbanken einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Anzeigen von Ereignissen und Protokollen für einen Failovercluster](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog-Failovercluster-Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [SQL Server Always On Team Blog: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Client-Netzwerkkonfiguration](../../../database-engine/configure-windows/client-network-configuration.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
