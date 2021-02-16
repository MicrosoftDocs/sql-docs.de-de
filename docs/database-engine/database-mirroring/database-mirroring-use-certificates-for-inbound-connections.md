---
title: Verwenden von Zertifikaten für eingehende Verbindungen
description: In diesem Thema werden die Schritte beschrieben, um Serverinstanzen so zu konfigurieren, dass bei der Datenbankspiegelung Zertifikate zur Authentifizierung von eingehenden Verbindungen verwendet werden können.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- inbound connections
- database mirroring [SQL Server], security
ms.assetid: 5d48bb98-61f0-4b99-8f1a-b53f831d63d0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c1ff8414ef765459bea59af5d158a0d4bdcd6cc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349218"
---
# <a name="database-mirroring---use-certificates-for-inbound-connections"></a>Datenbankspiegelung: Verwenden von Zertifikaten für eingehende Verbindungen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema werden die Schritte beschrieben, um Serverinstanzen so zu konfigurieren, dass bei der Datenbankspiegelung Zertifikate zur Authentifizierung von eingehenden Verbindungen verwendet werden können. Bevor Sie eingehende Verbindungen einrichten können, müssen Sie ausgehende Verbindungen für jede Serverinstanz konfigurieren. Weitere Informationen finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 Der Vorgang zum Konfigurieren der eingehenden Verbindungen umfasst die folgenden allgemeinen Schritte:  
  
1.  Erstellen eines Anmeldenamens für das andere System.  
  
2.  Erstellen Sie einen Benutzer für den Anmeldenamen.  
  
3.  Abrufen des Zertifikats für den Spiegelungsendpunkt der anderen Serverinstanz  
  
4.  Ordnen Sie das Zertifikat dem in Schritt 2 erstellten Benutzer zu.  
  
5.  Erteilen der CONNECT-Berechtigung für den Anmeldenamen für den entsprechenden Spiegelungsendpunkt  
  
 Falls ein Zeuge vorhanden ist, müssen Sie auch eingehende Verbindungen für diesen einrichten. Hierfür ist es erforderlich, Anmeldenamen, Benutzer und Zertifikate für den Zeugen auf beiden Partnern (und umgekehrt) einzurichten.  
  
 Die folgende Prozedur beschreibt diese Schritte detailliert. Für jeden Schritt enthält die Prozedur ein Beispiel für das Konfigurieren einer Serverinstanz auf einem System namens HOST_A. Im zugehörigen Beispielabschnitt werden dieselben Schritte für eine andere Serverinstanz auf einem System namens HOST_B beschrieben.  
  
### <a name="to-configure-server-instances-for-inbound-mirroring-connections-on-host_a"></a>So konfigurieren Sie Serverinstanzen für eingehende Spiegelungsverbindungen (auf HOST_A)  
  
1.  Erstellen eines Anmeldenamens für das andere System  
  
     Das folgende Beispiel erstellt einen Anmeldenamen für das System HOST_B in der **master** -Datenbank der Serverinstanz auf HOST_A. In diesem Beispiel heißt der Anmeldename `HOST_B_login`. Ersetzen Sie das Beispielkennwort durch ein Kennwort Ihrer Wahl.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login   
       WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
     Um die Anmeldenamen für diese Serverinstanz anzuzeigen, können Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung verwenden:  
  
    ```  
    SELECT * FROM sys.server_principals  
    ```  
  
     Weitere Informationen finden Sie unter [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
2.  Erstellen Sie einen Benutzer für den Anmeldenamen.  
  
     Das folgende Beispiel erstellt einen Benutzer namens `HOST_B_user` für den im vorherigen Schritt erstellten Anmeldenamen.  
  
    ```  
    USE master;  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md).  
  
     Um die Benutzer für diese Serverinstanz anzuzeigen, können Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung verwenden:  
  
    ```  
    SELECT * FROM sys.sysusers;  
    ```  
  
     Weitere Informationen finden Sie unter [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md).  
  
3.  Abrufen des Zertifikats für den Spiegelungsendpunkt der anderen Serverinstanz  
  
     Wenn dies noch nicht beim Konfigurieren ausgehender Verbindungen geschehen ist, rufen Sie eine Kopie des Zertifikats für den Spiegelungsendpunkt der Remoteserverinstanz ab. Sichern Sie zu diesem Zweck das Zertifikat für diese Serverinstanz wie unter [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md) beschrieben. Verwenden Sie zum Kopieren eines Zertifikats zu einem anderen System eine sichere Kopiermethode. Seien Sie äußerst vorsichtig, um Ihre Zertifikate zu schützen.  
  
     Weitere Informationen finden Sie unter [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
4.  Ordnen Sie das Zertifikat dem in Schritt 2 erstellten Benutzer zu.  
  
     Das folgende Beispiel ordnet das Zertifikat von HOST_B seinem Benutzer auf HOST_A zu.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Um die Zertifikate für diese Serverinstanz anzuzeigen, verwenden Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung:  
  
    ```  
    SELECT * FROM sys.certificates  
    ```  
  
     Weitere Informationen finden Sie unter [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
5.  Erteilen Sie die CONNECT-Berechtigung für den Anmeldenamen für den Remotespiegelungsendpunkt.  
  
     Um beispielsweise auf HOST_A die Berechtigung für die Remoteserverinstanz auf HOST_B zum Herstellen der Verbindung mit ihrem lokalen Anmeldenamen zu erteilen, d.h., um eine Verbindung mit `HOST_B_login` herzustellen, verwenden Sie die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen:  
  
    ```  
    USE master;  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [GRANT (Endpunktberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 Hiermit ist das Einrichten der Zertifikatsauthentifizierung für HOST_B zur Anmeldung bei HOST_A abgeschlossen.  
  
 Jetzt müssen die entsprechenden Schritte für eingehende Verbindungen für HOST_A auf HOST_B ausgeführt werden. Diese Schritte werden im Abschnitt zu eingehenden Verbindungen des folgenden Beispiels erläutert.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird das Konfigurieren von HOST_B für eingehende Verbindungen erläutert.  
  
> [!NOTE]  
>  In diesem Beispiel wird eine Zertifikatsdatei mit dem Zertifikat von HOST_A verwendet, das durch einen Codeausschnitt in [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md) erstellt wird.  
  
```  
USE master;  
--On HOST_B, create a login for HOST_A.  
CREATE LOGIN HOST_A_login WITH PASSWORD = 'AStrongPassword!@#';  
GO  
--Create a user, HOST_A_user, for that login.  
CREATE USER HOST_A_user FOR LOGIN HOST_A_login  
GO  
--Obtain HOST_A certificate. (See the note   
--   preceding this example.)  
--Asscociate this certificate with the user, HOST_A_user.  
CREATE CERTIFICATE HOST_A_cert  
   AUTHORIZATION HOST_A_user  
   FROM FILE = 'C:\HOST_A_cert.cer';  
GO  
--Grant CONNECT permission for the server instance on HOST_A.  
GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO HOST_A_login  
GO  
```  
  
 Wenn Sie die Ausführung im Modus für hohe Sicherheit mit automatischem Failover planen, müssen Sie die gleichen Setupschritte zum Konfigurieren des Zeugen für aus- und eingehende Verbindungen ausführen.  
  
 Weitere Informationen zum Erstellen einer Spiegeldatenbank, einschließlich eines Transact-SQL-Beispiels, finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Ein Transact-SQL-Beispiel zum Einrichten einer Sitzung im Hochleistungsmodus finden Sie unter [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>.NET Framework-Sicherheit  
 Verwenden Sie zum Kopieren eines Zertifikats zu einem anderen System eine sichere Kopiermethode. Seien Sie äußerst vorsichtig, um Ihre Zertifikate zu schützen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [GRANT (Endpunktberechtigungen) (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Einrichten einer verschlüsselten Spiegeldatenbank](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Problembehandlung für die Datenbankspiegelungskonfiguration &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
