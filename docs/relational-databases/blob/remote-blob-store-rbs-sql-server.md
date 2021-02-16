---
title: Remote Blob Store (RBS) (SQL Server) | Microsoft-Dokumentation
description: SQL Server Remote Blob Store (RBS) speichert Binary Large Objects (Blobs) im Speicher anstatt auf dem Hauptdatenbankserver. Hier erfahren Sie mehr über diese Add-On-Komponente.
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 89a79b07977d8fec2e2dd6f2a8b9acd9cea0ba5e
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100343158"
---
# <a name="remote-blob-store-rbs-sql-server"></a>Remote Blob Store (RBS) (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Remote BLOB-Speicher (RBS) ist eine optionale Add-On-Komponente, mit der Datenbankadministratoren Binary Large Objects in Speicherlösungen statt direkt auf dem Hauptdatenbankserver speichern können.  
  
 RBS ist nicht auf den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedien enthalten. Er wird auch nicht über das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setupprogram installiert. Suchen Sie auf dem Installationsmedium nach „RBS.msi“, um die Setupdatei zu finden.

 Wenn Sie nicht über ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationsmedium verfügen, können Sie RBS über einen der folgenden Links herunterladen:

| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version | RBS-Downloadort |
|:---|:---|
| [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] | [Downloadseite für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] RBS](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>Gründe für RBS  
  
### <a name="optimized-database-storage-and-performance"></a>Optimierte Datenbankspeicher und optimierte Datenbankleistung  
 Durch das Speichern von BLOBs in der Datenbank können große Mengen an Dateispeicherplatz sowie teure Serverressourcen belegt werden. RBS überträgt BLOBS an eine dedizierte Speicherlösung Ihrer Wahl und speichert Verweise auf diese BLOBS in der Datenbank. Dadurch werden Serverspeicher für strukturierte Daten und Serverressourcen für Datenbankoperationen freigegeben.  
  
### <a name="efficient-blob-management"></a>Effiziente Verwaltung von BLOBs  
 Mehrere RBS-Funktionen unterstützen die Verwaltung gespeicherter BLOBs:  
  
-   BLOBs werden mithilfe von ACID-Transaktionen (atomic, consistent, isolatable, durable; Unteilbarkeit, Konsistenz, Isolation, Dauerhaftigkeit) verwaltet.  
  
-   BLOBs sind in Auflistungen unterteilt.  
  
-   Automatische Speicherbereinigung, Konsistenzüberprüfung und andere Wartungsfunktionen sind enthalten.  
  
### <a name="standardized-api"></a>Standardisierte API  
 RBS definiert einen Satz von APIs, die ein standardisiertes Programmiermodell für Anwendungen bereitstellen, um auf BLOB-Speicher zuzugreifen und zu ändern. Jeder BLOB-Speicher kann seine eigene Anbieterbibliothek angeben, die in die RBS-Clientbibliothek eingebunden wird und angibt, wie BLOBs gespeichert werden und auf sie zugegriffen wird.  
  
 Eine Reihe von Speicherlösungen von Drittanbietern haben RBS-Anbieter entwickelt, die diesen Standard-APIs entsprechen und BLOB-Speicherung auf verschiedenen Speicherplattformen unterstützen.  
  
## <a name="rbs-requirements"></a>RSB-Anforderungen  
 - RSB erfordert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise für den Hauptdatenbankserver, in dem die BLOB-Metadaten gespeichert werden.  Wenn Sie jedoch den bereitgestellten FILESTREAM-Anbieter verwenden, können Sie die BLOBs selbst auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard speichern. RBS erfordert mindestens ODBC-Treiberversion 11 für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]und ODBC-Treiber, Version 13 für [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] , um eine Verbindung mit [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]herzustellen. Treiber stehen unter [Herunterladen von ODBC-Treibern für SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md)zur Verfügung.    
  
 RBS beinhaltet einen FILESTREAM-Anbieter, mit dem Sie BLOBs mithilfe von RBS auf einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]speichern können. Wenn Sie BLOBs mithilfe von RBS in einer anderen Speicherlösung speichern möchten, müssen Sie einen für diese Speicherlösung entwickelten RSB-Anbieter eines Drittanbieters verwenden oder einen benutzerdefinierten RBS-Anbieter mithilfe der RBS-API entwickeln. Ein Beispielanbieter, der BLOBs im NTFS-Dateisystem speichert, steht als Lernressource auf [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190)zur Verfügung.  
  
## <a name="rbs-security"></a>RSB-Sicherheit  
 Der SQL Remote Blob Storage-Teamblog ist eine gute Informationsquelle für diese Funktion. Das RBS-Sicherheitsmodell wird in dem Beitrag unter [RBS Security Model](/archive/blogs/sqlrbs/rbs-security-model)(RBS-Sicherheitsmodell) beschrieben.  
  
### <a name="custom-providers"></a>Benutzerdefinierte Anbieter  
 Wenn Sie einen benutzerdefinierten Anbieter zum Speichern von Blobs außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwenden, stellen Sie sicher, dass die gespeicherten Blobs anhand von Berechtigungen und Verschlüsselungsoptionen geschützt sind, die für das vom benutzerdefinierten Anbieter verwendete Speichermedium geeignet sind.  
  
### <a name="credential-store-symmetric-key"></a>Symmetrischer Schlüssel des Anmeldeinformationsspeichers  
 Wenn für einen Anbieter das Setup und die Verwendung eines Schlüssels im Anmeldeinformationsspeicher gespeichert werden muss, verwendet RBS einen symmetrischen Schlüssel um die Anbieterschlüssel zu verschlüsseln, die der Client möglicherweise benötigt, um die Autorisierung für den Blobspeicher des Anbieters zu erhalten.  
  
-   RBS 2016 verwendet einen mit **AES_128** erstellten symmetrischen Schlüssel. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] lässt die Erstellung neuer **TRIPLE_DES** -Schlüssel nur zu Zwecken der Abwärtskompatibilität zu. Weitere Informationen finden Sie unter [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
-   RBS 2014 und frühere Versionen verwenden einen Anmeldeinformationsspeicher mit geheimen Schlüsseln, die mithilfe des veralteten symmetrischen **TRIPLE_DES** -Schlüsselalgorithmus verschlüsselt werden. Wenn Sie derzeit **TRIPLE_DES** verwenden, empfiehlt [!INCLUDE[msCoName](../../includes/msconame-md.md)], dass Sie ihre Sicherheit erhöhen, indem Sie folgende Schritte in diesem Thema ausführen, um beim Rotieren des Schlüssels auf eine sicherere Verschlüsselungsmethode wechseln.  
  
 Sie können die Eigenschaften der symmetrischen Schlüssel für den RBS-Anmeldeinformationsspeicher bestimmen, indem Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung in der RBS-Datenbank ausführen:   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` Wenn die Ausgabe von dieser Anweisung zeigt, dass **TRIPLE_DES** weiterhin verwendet wird, sollten Sie diesen Schlüssel rotieren.  
  
### <a name="rotating-the-symmetric-key"></a>Rotieren des symmetrischen Schlüssels  
 Wenn Sie RBS verwenden, sollten Sie in regelmäßigen Abständen den symmetrischen Schlüssel des Anmeldeinformationsspeichers drehen. Dies ist eine allgemeine bewährte Sicherheitsmethode, um organisationsweite Sicherheitsrichtlinien zu erfüllen.  Eine Möglichkeit, den symmetrischen Schlüssel des RBS-Anmeldeinformationsspeichers zu rotieren, ist die Verwendung des [unten aufgeführten Skripts](#Key_rotation) in der RBS-Datenbank.  Mit diesem Skript können Sie auch zu stärkeren Verschlüsselungsstärkeeigenschaften migrieren, z.B. Algorithmus oder Schlüssellänge. Sichern Sie Ihre Datenbank vor der Schlüsselrotation.  Am Ende des Skripts finden Sie einige Überprüfungsschritte.  
Wenn Ihre Sicherheitsrichtlinien verschiedene Schlüsseleigenschaften (z.B. Algorithmus oder Schlüssellänge) aus den angebotenen benötigen, kann das Skript als Vorlage verwendet werden. Ändern Sie die Schlüsseleigenschaften an zwei Stellen: 1) die Erstellung des temporären Schlüssels, 2) die Erstellung des permanenten Schlüssels.  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> RSB-Ressourcen  
  
 **RSB-Beispiele**  
 In den auf [CodePlex](https://go.microsoft.com/fwlink/?LinkId=210190) verfügbaren RSB-Beispielen wird veranschaulicht, wie Sie eine RBS-Anwendung entwickeln und einen benutzerdefinierten RBS-Anbieter installieren.  
  
 **RBS-Blog**  
 Der [RBS-Blog](/archive/blogs/sqlrbs/) bietet zusätzliche Informationen, durch die Sie RBS besser verstehen, bereitstellen und verwalten können.  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> Skript zur Schlüsselrotation  
 In diesem Beispiel wird eine gespeicherte Prozedur namens `sp_rotate_rbs_symmetric_credential_key` erstellt, um den aktuell verwendeten symmetrischen Schlüssel des RBS-Anmeldeinformationsspeichers  
durch einen Schlüssel Ihrer Wahl zu ersetzen.  Sie sollten dies tun, wenn eine Sicherheitsrichtlinie   
die regelmäßige Rotation von Schlüsseln erfordert oder der Algorithmus besondere Anforderungen hat.  
 In dieser gespeicherten Prozedur wird ein symmetrischer Schlüssel, der **AES_256** verwendet, den aktuellen ersetzten.  Aufgrund  
des Austauschs des symmetrischen Schlüssels müssen geheime Schlüssel mit dem neuen Schlüssel erneut verschlüsselt werden.  Diese gespeicherte   
Prozedur wird auch die geheimen Schlüssel erneut verschlüsseln.  Die Datenbank sollte vor der Schlüsselrotation gesichert werden.  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 Nun können Sie die gespeicherte `sp_rotate_rbs_symmetric_credential_key` -Prozedur verwenden, um den symmetrischen Schlüssel des RBS-Anmeldeinformationsspeichers zu rotieren. Die geheimen Schlüssel bleiben dabei vor und nach der Schlüsselrotation dieselben.  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Remoteblobspeicher und Always On-Verfügbarkeitsgruppen (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
