---
title: 'Datenbankspiegelung: Verwenden von Zertifikaten für ausgehende Verbindungen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Serverinstanzen zur Verwendung von Zertifikaten konfigurieren, um eingehende Verbindungen für die Datenbankspiegelung zu authentifizieren, nachdem Sie ausgehende Verbindungen festgelegt haben.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- outbound connections [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 464c9096-10d6-4c5e-8bb1-19acba27ad9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a53b3c3c871b62ac8824d170f71035d4359fa09f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349188"
---
# <a name="database-mirroring---use-certificates-for-outbound-connections"></a>Datenbankspiegelung: Verwenden von Zertifikaten für ausgehende Verbindungen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema werden die Schritte beschrieben, um Serverinstanzen so zu konfigurieren, dass bei der Datenbankspiegelung Zertifikate zur Authentifizierung von ausgehenden Verbindungen verwendet werden können. Die ausgehende Verbindung muss konfiguriert werden, bevor eingehende Verbindungen eingerichtet werden können.  
  
> [!NOTE]  
>  Alle Spiegelungsverbindungen auf einer Serverinstanz verwenden einen gemeinsamen Datenbankspiegelungsendpunkt. Sie müssen beim Erstellen dieses Endpunktes die Authentifizierungsmethode der Serverinstanz angeben.  
  
 Der Vorgang zum Konfigurieren der ausgehenden Verbindungen umfasst die folgenden allgemeinen Schritte:  
  
1.  Erstellen eines Datenbankhauptschlüssels in der **master** -Datenbank  
  
2.  Erstellen eines verschlüsselten Zertifikats für die Serverinstanz in der **master** -Datenbank  
  
3.  Erstellen eines Endpunktes für die Serverinstanz mithilfe ihres Zertifikats  
  
4.  Sichern des Zertifikats in einer Datei und sicheres Kopieren dieses Zertifikats zum anderen System bzw. zu den anderen Systemen  
  
 Diese Schritte müssen Sie für jeden Partner und ggf. den Zeugen ausführen.  
  
 Die folgende Prozedur beschreibt diese Schritte detailliert. Für jeden Schritt enthält die Prozedur ein Beispiel für das Konfigurieren einer Serverinstanz auf einem System namens HOST_A. Im zugehörigen Beispielabschnitt werden dieselben Schritte für eine andere Serverinstanz auf einem System namens HOST_B beschrieben.  
  
## <a name="procedure"></a>Verfahren  
  
#### <a name="to-configure-server-instances-for-outbound-mirroring-connections-on-host_a"></a>So konfigurieren Sie Serverinstanzen für ausgehende Spiegelungsverbindungen (auf HOST_A)  
  
1.  Erstellen Sie in der **master** -Datenbank den Datenbankhauptschlüssel, sofern noch keiner vorhanden ist. Zum Anzeigen der für eine Datenbank vorhandenen Schlüssel verwenden Sie die Katalogsicht [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) .  
  
     Zum Erstellen des Datenbankhauptschlüssels verwenden Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehl:  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
     Verwenden Sie ein eindeutiges und aussagekräftiges Kennwort, und hinterlegen Sie es an einem sicheren Ort.  
  
     Weitere Informationen finden Sie unter [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) und [Erstellen eines Datenbank-Hauptschlüssels](../../relational-databases/security/encryption/create-a-database-master-key.md).  
  
2.  Erstellen Sie in der **master** -Datenbank ein verschlüsseltes Zertifikat für die Serverinstanz, das für deren ausgehende Verbindungen zur Datenbankspiegelung verwendet werden soll.  
  
     Um z. B. ein Zertifikat für das HOST_A-System zu erstellen:  
  
    > [!IMPORTANT]  
    >  Wenn Sie beabsichtigen, das Zertifikat länger als ein Jahr zu verwenden, geben Sie das Ablaufdatum in UTC-Zeit mit der EXPIRY_DATE-Option in der CREATE CERTIFICATE-Anweisung an. Es wird auch empfohlen, dass Sie SQL Server Management Studio verwenden, um eine richtlinienbasierte Verwaltungsregel zu erstellen, damit Sie benachrichtigt werden, wann die Zertifikate ablaufen. Erstellen Sie in der Richtlinienverwaltung mithilfe des Dialogfelds **Neue Bedingung erstellen** diese Regel im Feld **\@ExpirationDate** des Facets **Zertifikat**. Weitere Informationen finden Sie unter [Verwalten von Servern mit der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) und [Sichern von SQL Server](../../relational-databases/security/securing-sql-server.md).  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate for database mirroring',   
       EXPIRY_DATE = '11/30/2013';  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Zum Anzeigen der Zertifikate in der **master** -Datenbank können Sie die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen verwenden:  
  
    ```  
    USE master;  
    SELECT * FROM sys.certificates;  
    ```  
  
     Weitere Informationen finden Sie unter [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
3.  Stellen Sie sicher, dass der Datenbank-Spiegelungsendpunkt auf jeder Serverinstanz vorhanden ist.  
  
     Wenn für die Serverinstanz bereits ein Datenbank-Spiegelungsendpunkt vorhanden ist, sollten Sie diesen Endpunkt für alle anderen Sitzungen verwenden, die Sie auf der Serverinstanz herstellen. Um festzustellen, ob auf einer Serverinstanz bereits ein Datenbank-Spiegelungsendpunkt vorhanden ist und um dessen Konfiguration anzuzeigen, verwenden Sie folgende Anweisung:  
  
    ```  
    SELECT name, role_desc, state_desc, connection_auth_desc, encryption_algorithm_desc   
       FROM sys.database_mirroring_endpoints;  
    ```  
  
     Wenn kein Endpunkt vorhanden ist, erstellen Sie einen Endpunkt, der dieses Zertifikat für ausgehende Verbindungen und für die Anmeldeinformationen des Zertifikats zur Überprüfung auf dem anderen System verwendet. Dies ist ein serverweiter Endpunkt, der von allen Spiegelungssitzungen verwendet wird, an denen die Serverinstanz beteiligt ist.  
  
     So erstellen Sie beispielsweise einen Spiegelungsendpunkt für die Beispiel-Serverinstanz auf HOST_A:  
  
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
  
     Weitere Informationen finden Sie unter [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
4.  Sichern Sie das Zertifikat, und kopieren Sie es zum anderen System bzw. zu den anderen Systemen. Das ist erforderlich, um eingehende Verbindungen auf dem anderen System zu konfigurieren.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
     Weitere Informationen finden Sie unter [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
     Kopieren Sie dieses Zertifikat mit einer sicheren Methode Ihrer Wahl. Seien Sie äußerst vorsichtig, um Ihre Zertifikate zu schützen.  
  
 Mit dem Beispielcode in den vorherigen Schritten werden ausgehende Verbindungen auf HOST_A konfiguriert.  
  
 Jetzt müssen die entsprechenden Schritte für ausgehende Verbindungen für HOST_B ausgeführt werden. Diese Schritte werden im folgenden Beispielabschnitt veranschaulicht.  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel erläutert das Konfigurieren von HOST_B für ausgehende Verbindungen.  
  
```  
USE master;  
--Create the database Master Key, if needed.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
GO  
-- Make a certifcate on HOST_B server instance.  
CREATE CERTIFICATE HOST_B_cert   
   WITH SUBJECT = 'HOST_B certificate for database mirroring',   
   EXPIRY_DATE = '11/30/2013';  
GO  
--Create a mirroring endpoint for the server instance on HOST_B.  
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
--Backup HOST_B certificate.  
BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
GO   
--Using any secure copy method, copy C:\HOST_B_cert.cer to HOST_A.  
```  
  
 Kopieren Sie das Zertifikat zum anderen System. Verwenden Sie dazu eine sichere Methode Ihrer Wahl. Seien Sie äußerst vorsichtig, um Ihre Zertifikate zu schützen.  
  
> [!IMPORTANT]  
>  Nach dem Einrichten von ausgehenden Verbindungen müssen Sie eingehende Verbindungen auf jeder Serverinstanz für die andere Serverinstanz bzw. die anderen Serverinstanzen konfigurieren. Weitere Informationen finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 Weitere Informationen zum Erstellen einer Spiegeldatenbank, einschließlich eines Transact-SQL-Beispiels, finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Ein Transact-SQL-Beispiel zum Einrichten einer Sitzung im Modus für hohe Leistung finden Sie unter [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>.NET Framework-Sicherheit  
 Sofern Sie nicht garantieren können, dass Ihr Netzwerk sicher ist, wird das Verschlüsseln bei Verbindungen zur Datenbankspiegelung empfohlen.  
  
 Verwenden Sie zum Kopieren eines Zertifikats zu einem anderen System eine sichere Kopiermethode.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [Vorbereiten einer Spiegeldatenbank auf die Spiegelung (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Problembehandlung für die Datenbankspiegelungskonfiguration (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Einrichten einer verschlüsselten Spiegeldatenbank](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
