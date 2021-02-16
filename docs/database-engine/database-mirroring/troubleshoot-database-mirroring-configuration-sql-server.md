---
title: Häufige Probleme beim Konfigurieren der Datenbankspiegelung
description: Dieser Artikel enthält Informationen, die Ihnen die Problembehandlung beim Einrichten einer Datenbankspiegelungssitzung erleichtern.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4f8b18819666c7cec7c0f50b074a5a9a7ed5a804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352218"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>Problembehandlung für die Datenbankspiegelungskonfiguration (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dieses Thema enthält Informationen, die Ihnen die Problembehandlung beim Einrichten einer Datenbank-Spiegelungssitzung erleichtern.  
  
> [!NOTE]  
>  Stellen Sie sicher, dass alle [Voraussetzungen für die Datenbankspiegelung](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md)erfüllt werden.  
  
|Problem|Zusammenfassung|  
|-----------|-------------|  
|Fehlermeldung 1418|In dieser Meldung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird angegeben, dass die Servernetzwerkadresse nicht erreicht werden kann oder nicht vorhanden ist. Es wird vorgeschlagen, den Namen der Netzwerkadresse zu überprüfen und den Befehl erneut auszugeben. |  
|[Konten](#Accounts)|Erläutert die Anforderungen für eine ordnungsgemäße Konfiguration der Konten, unter denen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.|  
|[Endpunkte](#Endpoints)|Erläutert die Anforderungen für eine ordnungsgemäße Konfiguration des Endpunkts der Datenbankspiegelung für jede Serverinstanz.|  
|[SystemAddress](#SystemAddress)|Gibt einen Überblick über die verschiedenen Methoden, die zum Angeben des Systemnames einer Serverinstanz in einer Datenbank-Spiegelungskonfiguration zur Verfügung stehen.|  
|[Netzwerkzugriff](#NetworkAccess)|Dokumentiert die Anforderung, dass jeder Serverinstanz der Zugriff auf die Ports der anderen Serverinstanz bzw. Serverinstanzen über TCP ermöglicht werden muss.|  
|[Vorbereitung der Spiegeldatenbank](#MirrorDbPrep)|Gibt einen Überblick über die Anforderungen zum Vorbereiten der Spiegeldatenbank, sodass die Spiegelung beginnen kann.|  
|[Fehler bei einem Dateierstellungsvorgang](#FailedCreateFileOp)|Beschreibt, wie auf Fehler bei einem Dateierstellungsvorgang zu reagieren ist.|  
|[Starten der Spiegelung mit Transact-SQL](#StartDbm)|Beschreibt, welche Reihenfolge für ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** -Anweisungen eingehalten werden muss.|  
|[Datenbankübergreifende Transaktionen](#CrossDbTxns)|Ein automatisches Failover kann zu einer automatischen und möglicherweise falschen Auflösung von unsicheren Transaktionen führen. Aus diesem Grund werden datenbankübergreifende Transaktionen von der Datenbankspiegelung nicht unterstützt.|  
  
##  <a name="accounts"></a><a name="Accounts"></a> Konten  
 Die Konten, unter denen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, müssen ordnungsgemäß konfiguriert sein.  
  
1.  Verfügen die Konten über die richtigen Berechtigungen?  
  
    1.  Das Ausführen der Konten in den gleichen Domänenkonten reduziert das Risiko einer Fehlkonfiguration.  
  
    2.  Wenn die Konten in unterschiedlichen Domänen ausgeführt werden oder es sich nicht um Domänenkonten handelt, muss der Anmeldename eines Kontos in der **master** -Datenbank des anderen Computers erstellt werden, und diesem Anmeldenamen müssen CONNECT-Berechtigungen für den Endpunkt erteilt werden. Weitere Informationen finden Sie unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md). Dies gilt auch für das Netzwerkdienstkonto.  
  
2.  Wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Dienst unter dem lokalen Systemkonto ausgeführt, müssen Sie Zertifikate für die Authentifizierung verwenden. Weitere Informationen finden Sie unter [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a>Endpoints  
 Endpunkte müssen ordnungsgemäß konfiguriert sein.  
  
1.  Stellen Sie sicher, dass jede Serverinstanz (Prinzipalserver, Spiegelserver und ggf. Zeuge) über einen Endpunkt der Datenbankspiegelung verfügt. Weitere Informationen finden Sie unter [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) und, je nach Form der Authentifizierung, entweder unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) oder [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Überprüfen Sie, ob die Portnummern richtig sind.  
  
     Verwenden Sie die [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) -Katalogsicht und die [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) -Katalogsicht, um den Port zu identifizieren, der dem Endpunkt der Datenbankspiegelung einer Serverinstanz derzeit zugeordnet ist.  
  
3.  Bei Problemen mit der Einrichtung der Datenbankspiegelung, die schwer zu erklären sind, empfiehlt es sich, jede Serverinstanz zu prüfen, um zu ermitteln, ob die richtigen Ports von der Serverinstanz überwacht werden.   
  
4.  Stellen Sie sicher, dass die Endpunkte gestartet wurden (STATE=STARTED). Verwenden Sie auf jeder Serverinstanz folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung.  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Weitere Informationen zur **state_desc**-Spalte finden Sie unter [database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Verwenden Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, um einen Endpunkt zu starten.  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Überprüfen Sie, ob die Rolle richtig ist. Verwenden Sie auf jeder Serverinstanz folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung.  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
6.  Um sich von der anderen Serverinstanz unter dem Dienstkonto anzumelden, ist die CONNECT-Berechtigung erforderlich. Stellen Sie sicher, dass der Anmeldename auf dem anderen Server über CONNECT-Berechtigungen verfügt. Führen Sie auf jeder Serverinstanz die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung aus, um herauszufinden, wer über CONNECT-Berechtigungen für einen Endpunkt verfügt.  
  
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
  
##  <a name="system-address"></a><a name="SystemAddress"></a> Systemadresse  
 Für den Systemnamen einer Serverinstanz in einer Datenbank-Spiegelungskonfiguration können Sie jeden beliebigen Namen verwenden, der das System eindeutig bezeichnet. Die Serveradresse kann ein Systemname sein (wenn sich die Systeme in derselben Domäne befinden), ein vollqualifizierter Domänenname oder eine IP-Adresse (vorzugsweise eine statische IP-Adresse). Bei Verwendung des vollqualifizierten Domänennamens ist eine problemfreie Funktionsweise sichergestellt. Weitere Informationen finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Jede Serverinstanz muss über TCP auf die Ports der anderen Serverinstanz bzw. der anderen Serverinstanzen zugreifen können. Dies ist insbesondere von Bedeutung, wenn sich die Serverinstanzen in unterschiedlichen Domänen befinden, die sich nicht vertrauen (nicht vertrauenswürdige Domänen). Damit wird ein Großteil der Kommunikation zwischen den Serverinstanzen eingeschränkt.  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 Überprüfen Sie, ob die Spiegeldatenbank für die Spiegelung vorbereitet ist, unabhängig davon, ob Sie die Spiegelung zum ersten Mal starten oder nach dem Entfernen der Spiegelung erneut starten.  
  
 Stellen Sie bei der Erstellung der Spiegeldatenbank auf dem Spiegelserver sicher, dass Sie die Sicherung der Prinzipaldatenbank wiederherstellen, wobei Sie mit WITH NORECOVERY denselben Datenbanknamen angeben. Darüber hinaus müssen alle Protokollsicherungen, die nach dieser Sicherung erstellt wurden, ebenfalls mithilfe von WITH NORECOVERY angewendet werden.  
  
 Außerdem sollte der Dateipfad (einschließlich des Laufwerkbuchstabens) der Spiegeldatenbank nach Möglichkeit mit dem Pfad der Prinzipaldatenbank übereinstimmen. Wenn sich die Pfade unterscheiden müssen, weil sich beispielsweise die Prinzipaldatenbank auf Laufwerk 'F': befindet, auf dem Spiegelsystem jedoch kein Laufwerk F: vorhanden ist, müssen Sie die MOVE-Option in die RESTORE-Anweisung einschließen.  
  
> [!IMPORTANT]  
>  Falls Sie die Datenbankdateien bei der Erstellung der Spiegeldatenbank verschieben, können Sie der Datenbank später u. U. keine Dateien hinzufügen, ohne dass die Spiegelung unterbrochen wird.  
  
 Wurde die Datenbankspiegelung angehalten, müssen alle nachfolgenden Protokollsicherungen in der Prinizipaldatenbank auf die Spiegeldatenbank angewendet werden, ehe die Spiegelung erneut gestartet werden kann.  
  
 Weitere Informationen finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)verwendet.  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 Damit eine Datei ohne Auswirkung auf eine Spiegelungssitzung hinzugefügt werden kann, muss der Pfad der Datei auf beiden Servern vorhanden sein. Wenn Sie die Datenbankdateien bei der Erstellung der Spiegeldatenbank verschieben, kann bei einem später durchgeführten Vorgang zum Hinzufügen einer Datei in der Spiegeldatenbank ein Fehler auftreten oder die Spiegelung wird möglicherweise angehalten.  
  
 So beheben Sie das Problem:  
  
1.  Der Datenbankbesitzer muss die Spiegelsitzung entfernen und eine vollständige Sicherung der Dateigruppe wiederherstellen, die die hinzugefügte Datei enthält.  
  
2.  Anschließend muss der Besitzer das Protokoll mit dem Vorgang zum Hinzufügen der Datei auf dem Prinzipalserver sichern und die Protokollsicherung auf der Spiegeldatenbank mithilfe der Optionen WITH NORECOVERY und WITH MOVE manuell wiederherstellen. Dadurch wird der angegebene Dateipfad auf dem Spiegelserver erstellt, und die neue Datei wird an diesem Speicherort wiederhergestellt.  
  
3.  Um die Datenbank für eine neue Spiegelsitzung vorzubereiten, muss der Besitzer außerdem mit der Option WITH NO RECOVERY alle anderen ausstehenden Protokollsicherungen vom Prinzipalserver wiederherstellen.  
  
 Weitere Informationen finden Sie unter [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md), [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md), [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md), or [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> Starten der Spiegelung mit Transact-SQL  
 Die Reihenfolge, in der ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** -Anweisungen ausgegeben werden, ist von großer Bedeutung.  
  
1.  Die erste Anweisung muss auf dem Spiegelserver ausgeführt werden. Wird diese Anweisung ausgegeben, versucht der Spiegelserver nicht, Kontakt zu anderen Serverinstanzen aufzunehmen. Stattdessen weist der Spiegelserver die Datenbank an, so lange zu warten, bis der Prinzipalserver Kontakt mit dem Spiegelserver aufgenommen hat.  
  
2.  Die zweite ALTER DATABASE-Anweisung muss auf dem Prinzipalserver ausgeführt werden. Diese Anweisung veranlasst den Prinzipalserver, eine Verbindung mit dem Spiegelserver herzustellen. Nachdem diese Verbindung erstellt wurde, versucht der Spiegelserver, den Prinzipalserver über eine andere Verbindung zu kontaktieren.  
  
 Weitere Informationen zu dieser Einstellung finden Sie unter [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
> [!NOTE]  
>  Informationen zum Verwenden von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], um Spiegelung zu starten, finden Sie unter [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> Datenbankübergreifende Transaktionen  
 Wenn eine Datenbank im Modus für hohe Sicherheit mit automatischem Failover gespiegelt wird, kann ein automatisches Failover zu einer automatischen und möglicherweise falschen Auflösung von unsicheren Transaktionen führen. Falls auf einer der Datenbanken ein automatisches Failover stattfindet, während für eine datenbankübergreifende Transaktion gerade ein Commit ausgeführt wird, können zwischen den Datenbanken logische Inkonsistenzen auftreten.  
  
 Zu den Typen datenbankübergreifender Transaktionen, auf die sich ein automatisches Failover auswirken kann, zählen:  
  
-   Eine Transaktion, die mehrere Datenbanken in derselben Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisiert.  
  
-   Transaktionen, die [!INCLUDE[msCoName](../../includes/msconame-md.md)] DTC (Microsoft Distributed Transaction Coordinator) verwenden.  
  
 Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


