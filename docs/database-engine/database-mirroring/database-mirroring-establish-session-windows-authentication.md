---
title: Konfigurieren der Datenbankspiegelung
description: Hier finden Sie Schritte zum Konfigurieren einer Datenbankspiegelungsbeziehung zwischen einem Prinzipal und einem Spiegel mithilfe der Windows-Authentifizierung.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: baa9d359af3b4003cdeb29d79e6b8a699519a99d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353154"
---
# <a name="configure-database-mirroring"></a>Konfigurieren der Datenbankspiegelung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 Nach der Vorbereitung der Spiegeldatenbank ( [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)) können Sie eine Sitzung zur Datenbankspiegelung beginnen. Die Prinzipal-, Spiegel- und Zeugenserverinstanzen müssen separate Serverinstanzen sein, die auf getrennten Hostsystemen ausgeführt werden.  
  
> [!IMPORTANT]
>  Die Konfiguration der Datenbankspiegelung sollte außerhalb der Spitzenbetriebszeiten durchgeführt werden, da sich die Konfiguration der Spiegelung auf die Leistung auswirken kann.  
> 
> [!NOTE]
>  Eine Serverinstanz kann an mehreren gleichzeitigen Datenbank-Spiegelungssitzungen mit den gleichen oder anderen Partnern teilnehmen. Eine Serverinstanz kann bei manchen Sitzungen als Partner und bei anderen Sitzungen als Zeuge dienen. Auf der Spiegelserverinstanz muss dieselbe Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wie auf der Prinzipalserverinstanz ausgeführt werden. Die Datenbankspiegelung ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Darüber hinaus wird die Ausführung auf vergleichbaren Systemen empfohlen, die identische Arbeitsauslastungen bewältigen können.  
  
### <a name="to-establish-a-database-mirroring-session"></a>So richten Sie eine Datenbank-Spiegelungssitzung ein  
  
1.  Erstellen Sie die Spiegeldatenbank. Weitere Informationen finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)verwendet.  
  
2.  Richten Sie die Sicherheit auf jeder Serverinstanz ein.  
  
     Jede Serverinstanz in einer Datenbank-Spiegelungssitzung benötigt einen eigenen Datenbank-Spiegelungsendpunkt. Falls der Endpunkt nicht vorhanden ist, müssen Sie ihn erstellen.  
  
    > [!NOTE]  
    >  Der für die Datenbankspiegelung von einer Serverinstanz verwendete Authentifizierungstyp ist eine Eigenschaft des Endpunkts der Datenbankspiegelung. Für die Datenbankspiegelung sind zwei Arten von Transportsicherheit verfügbar: die Windows-Authentifizierung oder die zertifikatbasierte Authentifizierung. Weitere Informationen finden Sie unter [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Stellen Sie sicher, dass auf allen Partnerservern ein Endpunkt für die Datenbankspiegelung vorhanden ist. Unabhängig von der Anzahl der zu unterstützenden Spiegelungssitzungen darf die Serverinstanz nur einen Endpunkt für die Datenbankspiegelung enthalten. Wenn Sie diese Serverinstanz in Sitzungen zur Datenbankspiegelung ausschließlich für Partner verwenden möchten, können Sie dem Endpunkt die Rolle Partner zuweisen (ROLE **=** PARTNER). Wenn Sie auch den Server in anderen Datenbank-Spiegelungssitzungen für Zeugen verwenden möchten, weisen Sie dem Endpunkt die Rolle ALL zu.  
  
     Zum Ausführen der SET PARTNER-Anweisung muss die Option STATE der Endpunkte beider Partner auf STARTED festgelegt sein.  
  
     Um zu erfahren, ob eine Serverinstanz einen Endpunkt für die Datenbankspiegelung besitzt, und um Informationen zu Rolle und Status der Serverinstanz zu erhalten, führen Sie auf der Instanz die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung aus:  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Ein bereits verwendeter Datenbankspiegelungs-Endpunkt darf nicht neu konfiguriert werden. Wenn ein Endpunkt für die Datenbankspiegelung vorhanden ist und bereits verwendet wird, empfiehlt es sich, diesen Endpunkt auf der Serverinstanz für jede Sitzung zu verwenden. Wird ein bereits verwendeter Endpunkt gelöscht, kann dies zum erneuten Starten des Endpunkts führen, wodurch die Verbindungen der vorhandenen Sitzungen gestört werden, was von den anderen Serverinstanzen als Fehler interpretiert werden kann. Dies ist vor allem im Modus für hohe Sicherheit mit automatischem Failover wichtig, bei dem das Neukonfigurieren des Endpunkts auf einem Partner zur Ausführung eines Failovers führen könnte. Wenn für eine Sitzung zudem ein Zeuge festgelegt wurde, kann das Entfernen des Endpunkts für die Datenbankspiegelung dazu führen, dass der Prinzipalserver der Sitzung das Quorum verliert. In diesem Fall wird die Datenbank offline gebracht und die Verbindung mit den Benutzern getrennt. Weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Wenn einem der Partner ein Endpunkt fehlt, finden Sie eine Hilfestellung unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
3.  Wenn Serverinstanzen unter unterschiedlichen Domänenbenutzerkonten ausgeführt werden, erfordert jede Instanz eine Anmeldung bei der **master** -Datenbank der anderen Instanzen. Falls die Anmeldung nicht vorhanden ist, müssen Sie sie erstellen. Weitere Informationen finden Sie unter [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
4.  Um den Prinzipalserver als Partner auf der Spiegelungsdatenbank festzulegen, stellen Sie eine Verbindung mit dem Spiegelungsserver her, und geben Sie die folgende Anweisung aus:  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     Dabei ist _\<database\_name\>_ der Name der zu spiegelnden Datenbank (dieser Name ist auf beiden Partnern gleich) und _\<server\_network\_address\>_ die Servernetzwerkadresse des Prinzipalservers.  
  
     Die Syntax für eine Server-Netzwerkadresse lautet folgendermaßen:  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     Dabei ist _\<system-address>_ eine Zeichenfolge, die das Zielcomputersystem eindeutig identifiziert, und _\<port>_ ist die vom Spiegelungsendpunkt der Partnerserverinstanz verwendete Portnummer. Weitere Informationen finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Auf der Spiegelserverinstanz wird z. B. mit der folgenden ALTER DATABASE-Anweisung der Partner als ursprüngliche Prinzipalserverinstanz festgelegt. Der Datenbankname lautet **AdventureWorks**, die Systemadresse ist „DBSERVER1“ (der Name des Partnersystems) und der vom Endpunkt für die Datenbankspiegelung des Partners verwendete Port ist 7022:  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     Diese Anweisung bereitet den Spiegelserver darauf vor, eine Sitzung einzurichten, wenn er durch den Prinzipalserver kontaktiert wird.  
  
5.  Um den Spiegelserver als Partner auf der Prinzipaldatenbank festzulegen, stellen Sie eine Verbindung mit dem Prinzipalserver her, und geben Sie die folgende Anweisung aus:  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     Weitere Informationen finden Sie unter Schritt 4.  
  
     Auf der Prinzipalserverinstanz wird z. B. mit der folgenden ALTER DATABASE-Anweisung der Partner als ursprüngliche Spiegelserverinstanz festgelegt. Der Datenbankname lautet **AdventureWorks**, die Systemadresse ist „DBSERVER2“ (der Name des Partnersystems) und der vom Endpunkt für die Datenbankspiegelung des Partners verwendete Port ist 7025:  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     Durch Eingeben dieser Anweisung auf dem Prinzipalserver beginnt die Datenbankspiegelungssitzung.  
  
6.  Standardmäßig ist für eine Sitzung die Transaktionssicherheitsstufe FULL festgelegt (SAFETY ist auf FULL festgelegt), wodurch die Sitzung im synchronen Modus für hohe Sicherheit ohne automatisches Failover gestartet wird. Sie können die Sitzung wie folgt umkonfigurieren, sodass sie entweder im Modus für hohe Sicherheit mit automatischem Failover oder im asynchronen Modus für hohe Leistung ausgeführt wird:  
  
    -   **Modus für hohe Sicherheit mit automatischem Failover**  
  
         Wenn die Sitzung im Modus für hohe Sicherheit automatisches Failover unterstützen soll, fügen Sie eine Zeugenserverinstanz hinzu. Weitere Informationen finden Sie unter [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
    -   **Modus mit hoher Leistung**  
  
         Wenn Sie kein automatisches Failover wünschen oder wenn Ihnen Leistung wichtiger als hohe Verfügbarkeit ist, können Sie alternativ die Transaktionssicherheit deaktivieren. Weitere Informationen finden Sie unter [Ändern der Transaktionssicherheit in einer Datenbank-Spiegelungssitzung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  Im Modus für hohe Leistung sollte WITNESS auf OFF festgelegt sein. Weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
## <a name="example"></a>Beispiel  
  
> [!NOTE]  
>  Im folgenden Beispiel wird für eine vorhandene Spiegeldatenbank eine Datenbank-Spiegelungssitzung zwischen Partnern eingerichtet. Weitere Informationen zum Erstellen einer Spiegeldatenbank finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Das Beispiel zeigt die grundlegenden Schritte zum Erstellen einer Datenbankspiegelungssitzung ohne einen Zeugen. Die zwei Partner sind die Standardserverinstanzen auf zwei Computersystemen (PARTNERHOST1 und PARTNERHOST5). Beide Partnerinstanzen führen dasselbe Windows-Domänenbenutzerkonto aus (MYDOMAIN\dbousername).  
  
1.  Auf der Prinzipalserverinstanz (Standardinstanz auf PARTNERHOST1) erstellen Sie einen Endpunkt, der mithilfe von Port 7022 alle Rollen unterstützt:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  Ein Beispiel zum Einrichten einer Anmeldung finden Sie unter [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
2.  Auf der Spiegelserverinstanz (Standardinstanz auf PARTNERHOST5) erstellen Sie einen Endpunkt, der mithilfe von Port 7022 alle Rollen unterstützt:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  Auf der Prinzipalserverinstanz (auf PARTNERHOST1) sichern Sie die Datenbank:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  Stellen Sie auf der Spiegelserverinstanz (auf `PARTNERHOST5`) die Datenbank wieder her:  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  Nach dem Erstellen der vollständigen Datenbanksicherung müssen Sie eine Protokollsicherung für die Prinzipaldatenbank erstellen. Mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung wird beispielsweise das Protokoll in derselben Datei gesichert, die auch bei der vorhergehenden vollständigen Datenbanksicherung verwendet wurde:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Sie können erst mit der Spiegelung beginnen, nachdem Sie die erforderliche Protokollsicherung (und alle nachfolgenden Protokollsicherungen) angewendet haben.  
  
     Mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung wird z. B. das erste Protokoll aus C:\AdventureWorks.bak wiederhergestellt:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Legen Sie die Serverinstanz für die Spiegelserverinstanz auf PARTNERHOST1 als Partner fest (wodurch sie zum ersten Prinzipalserver wird):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  Eine Datenbankspiegelungssitzung wird standardmäßig im synchronen Modus ausgeführt, wozu die Transaktionssicherheitsstufe FULL verwendet wird (d. h., SAFETY auf FULL festgelegt sein muss). Um eine Sitzung im asynchronen Modus für hohe Leistung auszuführen, legen Sie SAFETY auf OFF fest. Weitere Informationen finden Sie unter [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
8.  Legen Sie auf der Prinzipalserverinstanz die Serverinstanz auf `PARTNERHOST5` als Partner fest (wodurch sie zum ersten Spiegelserver wird):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. Optional oder wenn Sie den Modus für hohe Sicherheit mit automatischem Failover verwenden möchten, richten Sie die Zeugenserverinstanz ein. Weitere Informationen finden Sie unter [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
> [!NOTE]  
>  Ein vollständiges Beispiel für das Einrichten der Sicherheitskomponenten, das Vorbereiten der Spiegeldatenbank, das Einrichten der Partner und das Hinzufügen eines Zeugen finden Sie unter [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Vorbereiten einer Spiegeldatenbank auf die Spiegelung (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Datenbankspiegelung und Protokollversand (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Datenbankspiegelung und Replikation (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Angeben einer Servernetzwerkadresse (Datenbankspiegelung)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

