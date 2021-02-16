---
title: 'Beispiel: Datenbankspiegelung mit Zertifikaten (T-SQL)'
description: In diesem Artikel finden Sie eine Beispielkonfiguration für die SQL Server-Datenbankspiegelung mit der zertifikatbasierten Authentifizierung mithilfe von Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: df489ecd-deee-465c-a26a-6d1bef6d7b66
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 12459a76f612ba2e9acc915acf5189792512b2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342343"
---
# <a name="example-setting-up-database-mirroring-using-certificates-transact-sql"></a>Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Beispiel werden sämtliche Schritte erläutert, die für das Erstellen einer Datenbank-Spiegelungssitzung mithilfe der zertifikatbasierten Authentifizierung erforderlich sind. In den Beispielen in diesem Thema wird [!INCLUDE[tsql](../../includes/tsql-md.md)]verwendet. Sofern Sie nicht garantieren können, dass Ihr Netzwerk sicher ist, wird das Verschlüsseln bei Verbindungen zur Datenbankspiegelung empfohlen.  
  
 Verwenden Sie zum Kopieren eines Zertifikats zu einem anderen System eine sichere Kopiermethode. Seien Sie äußerst vorsichtig, um Ihre Zertifikate zu schützen.  
  
##  <a name="example"></a><a name="ExampleH2"></a> Beispiel  
 Das folgende Beispiel zeigt, welche Aktionen für einen der Partner ausgeführt werden müssen, der auf HOST_A gespeichert ist. In diesem Beispiel sind die beiden Partner die Standardserverinstanzen auf drei Computersystemen. Die beiden Serverinstanzen werden in nicht vertrauenswürdigen Windows-Domänen ausgeführt, daher ist zertifikatbasierte Authentifizierung erforderlich.  
  
 HOST_A übernimmt die anfängliche Prinzipalrolle, während die Spiegelrolle von HOST_B übernommen wird.  
  
 Das Einrichten der Datenbankspiegelung mithilfe von Zertifikaten umfasst vier allgemeine Phasen, von den drei Phasen, nämlich 1, 2 und 4, in diesem Beispiel demonstriert werden. Diese Phasen sind im Folgenden aufgeführt:  
  
1.  [Konfigurieren ausgehender Verbindungen](#ConfiguringOutboundConnections)  
  
     Im Beispiel werden die Schritte für folgende Aufgaben veranschaulicht.  
  
    1.  Konfigurieren von Host_A für ausgehende Verbindungen  
  
    2.  Konfigurieren von Host_B für ausgehende Verbindungen  
  
     Informationen zu dieser Phase der Einrichtung der Datenbankspiegelung finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)verwendet.  
  
2.  [Konfigurieren eingehender Verbindungen](#ConfigureInboundConnections)  
  
     Im Beispiel werden die Schritte für folgende Aufgaben veranschaulicht.  
  
    1.  Konfigurieren von Host_A für eingehende Verbindungen  
  
    2.  Konfigurieren von Host_B für eingehende Verbindungen  
  
     Informationen zu dieser Phase der Einrichtung der Datenbankspiegelung finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)verwendet.  
  
3.  Erstellen der Spiegeldatenbank  
  
     Weitere Informationen zum Erstellen einer Spiegeldatenbank finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)verwendet.  
  
4.  [Konfigurieren der Spiegelungspartner](#ConfigureMirroringPartners)  
  
###  <a name="configuring-outbound-connections"></a><a name="ConfiguringOutboundConnections"></a> Konfigurieren ausgehender Verbindungen  
 **So konfigurieren Sie Host_A für ausgehende Verbindungen**  
  
1.  Erstellen Sie in der master-Datenbank den Datenbankhauptschlüssel, sofern erforderlich.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
2.  Erstellen Sie ein Zertifikat für diese Serverinstanz.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate';  
    GO  
    ```  
  
3.  Erstellen Sie einen Spiegelungsendpunkt für die Serverinstanz, die das Zertifikat verwendet.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Sichern Sie das Zertifikat von HOST_A, und kopieren Sie es auf das andere System, HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
5.  Kopieren Sie C:\HOST_A_cert.cer mithilfe einer sicheren Kopiermethode auf HOST_B.  
  
 **So konfigurieren Sie Host_B für ausgehende Verbindungen**  
  
1.  Erstellen Sie in der master-Datenbank den Datenbankhauptschlüssel, sofern erforderlich.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
    GO  
    ```  
  
2.  Erstellen Sie ein Zertifikat auf der Serverinstanz HOST_B.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert   
       WITH SUBJECT = 'HOST_B certificate for database mirroring';  
    GO  
    ```  
  
3.  Erstellen Sie einen Spiegelungsendpunkt für die Serverinstanz auf HOST_B.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_B_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Sichern Sie das Zertifikat von HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
    GO   
    ```  
  
5.  Kopieren Sie C:\HOST_B_cert.cer mithilfe einer sicheren Kopiermethode auf HOST_A.  
  
 Weitere Informationen finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 [&#91;Anfang des Beispiels&#93;](#ExampleH2)  
  
###  <a name="configuring-inbound-connections"></a><a name="ConfigureInboundConnections"></a> Konfigurieren eingehender Verbindungen  
 **So konfigurieren Sie Host_A für eingehende Verbindungen**  
  
1.  Erstellen Sie auf HOST_A einen Anmeldenamen für HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
2.  -- Erstellen Sie einen Benutzer für diesen Anmeldenamen.  
  
    ```  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
3.  -- Ordnen Sie das Zertifikat dem Benutzer zu.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
4.  Erteilen Sie die CONNECT-Berechtigung für den Anmeldenamen für den Remotespiegelungsendpunkt.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
 **So konfigurieren Sie Host_B für eingehende Verbindungen**  
  
1.  Erstellen Sie auf HOST_B einen Anmeldenamen für HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_A_login WITH PASSWORD = '=Sample#2_Strong_Password2';  
    GO  
    ```  
  
2.  Erstellen Sie einen Benutzer für den Anmeldenamen.  
  
    ```  
    CREATE USER HOST_A_user FOR LOGIN HOST_A_login;  
    GO  
    ```  
  
3.  Ordnen Sie das Zertifikat dem Benutzer zu.  
  
    ```  
    CREATE CERTIFICATE HOST_A_cert  
       AUTHORIZATION HOST_A_user  
       FROM FILE = 'C:\HOST_A_cert.cer'  
    GO  
    ```  
  
4.  Erteilen Sie die CONNECT-Berechtigung für den Anmeldenamen für den Remotespiegelungsendpunkt.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_A_login];  
    GO  
    ```  
  
> [!IMPORTANT]  
>  Wenn Sie die Ausführung im Modus für hohe Sicherheit mit automatischem Failover planen, müssen Sie die gleichen Setupschritte zum Konfigurieren des Zeugen für aus- und eingehende Verbindungen ausführen. Für das Setup der eingehenden Verbindungen bei Verwendung eines Zeugen ist es erforderlich, Anmeldungen und Benutzer für den Zeugen auf beiden Partnern und für beide Partner auf dem Zeugen einzurichten.  
  
 Weitere Informationen finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 [&#91;Anfang des Beispiels&#93;](#ExampleH2)  
  
### <a name="creating-the-mirror-database"></a>Erstellen der Spiegeldatenbank  
 Weitere Informationen zum Erstellen einer Spiegeldatenbank finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)verwendet.  
  
###  <a name="configuring-the-mirroring-partners"></a><a name="ConfigureMirroringPartners"></a> Konfigurieren der Spiegelungspartner  
  
1.  Legen Sie für die Spiegelserverinstanz auf HOST_B die Serverinstanz auf HOST_A als Partner fest (hierdurch wird sie zur ersten Prinzipalserverinstanz): Ersetzen Sie `TCP://HOST_A.Mydomain.Corp.Adventure-Works``.com:7024`durch eine gültige Netzwerkadresse. Weitere Informationen finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
    ```  
    --At HOST_B, set server instance on HOST_A as partner (principal server):  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_A.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
2.  Legen Sie für die Prinzipalserverinstanz auf HOST_A die Serverinstanz auf HOST_B als Partner fest (hierdurch wird sie zur ersten Spiegelserverinstanz): Ersetzen Sie `TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024`durch eine gültige Netzwerkadresse.  
  
    ```  
    --At HOST_A, set server instance on HOST_B as partner (mirror server).  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
3.  Dieses Beispiel geht davon aus, dass die Sitzung im Modus für hohe Verfügbarkeit ausgeführt wird. Wenn Sie diese Sitzung für den Modus für hohe Verfügbarkeit konfigurieren möchten, legen Sie die Transaktionssicherheit für die Prinzipalserverinstanz (auf HOST_A) auf OFF fest.  
  
    ```  
    --Change to high-performance mode by turning off transacton safety.  
    ALTER DATABASE AdventureWorks   
        SET PARTNER SAFETY OFF  
    GO  
    ```  
  
    > [!NOTE]  
    >  Wenn die Ausführung im Modus für hohe Sicherheit mit automatischem Failover geplant ist, sollte die Transaktionssicherheit auf FULL (Standardeinstellung) festgelegt bleiben und der Zeuge so schnell wie möglich nach Ausführung der zweiten SET PARTNER **'** _partner_server_ **'** -Anweisung hinzugefügt werden. Beachten Sie, dass der Zeuge zuerst für aus- und eingehende Verbindungen konfiguriert werden muss.  
  
 [&#91;Anfang des Beispiels&#93;](#ExampleH2)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Verwaltung von Anmeldenamen und Aufträgen nach einem Rollenwechsel &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
-   [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md) (SQL Server)  
  
-   [Problembehandlung für die Datenbankspiegelungskonfiguration &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Angeben einer Servernetzwerkadresse (Datenbankspiegelung)](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
