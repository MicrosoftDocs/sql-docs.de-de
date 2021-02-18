---
title: Sicherung über URLs – bewährte Methoden und Problembehandlung
description: Erfahren Sie mehr über bewährte Methoden und Tipps zur Problembehandlung in Bezug auf SQL Server-Sicherungs- und -Wiederherstellungsvorgänge in Azure Blob Storage.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: de676bea-cec7-479d-891a-39ac8b85664f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6620e688dcd8094bbc5b27bfbb540a2e7d3b1585
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348834"
---
# <a name="sql-server-back-up-to-url-best-practices-and-troubleshooting"></a>Bewährte Methoden und Problembehandlung für die SQL Server-Sicherung über URL

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  In diesem Thema werden bewährte Methoden und Tipps zur Problembehandlung beschrieben, die sich auf SQL Server-Sicherungs- und Wiederherstellungsvorgänge im Azure Blob-Dienst beziehen.  
  
 Weitere Informationen zur Verwendung des Azure Blob Storage-Diensts für SQL Server-Sicherungs- oder -Wiederherstellungsvorgänge finden Sie unter:  
  
-   [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
-   [Tutorial: SQL Server-Sicherung und -Wiederherstellung mit dem Azure Blob Storage-Dienst](../../relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="managing-backups"></a><a name="managing-backups-mb1"></a> Verwalten von Sicherungen  
 Die folgende Liste enthält allgemeine Empfehlungen zur Verwaltung von Sicherungen:  
  
-   Für jede Sicherung sollte ein eindeutiger Dateiname verwendet werden, um zu verhindern, dass BLOBs versehentlich überschrieben werden.  
  
-   Beim Erstellen eines Containers wird empfohlen, die Zugriffsebene auf **Privat** festzulegen, sodass der Lese- oder Schreibzugriff auf Blobs im Container nur Benutzern oder Konten mit den erforderlichen Authentifizierungsinformationen gestattet ist.  
  
-   Um Kosten für die Datenübertragung zwischen Regionen zu vermeiden, verwenden Sie für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken auf einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die auf einem virtuellen Azure-Computer ausgeführt wird, ein Speicherkonto, das derselben Region wie der virtuelle Computer angehört. Wenn Sie innerhalb einer Region bleiben, erzielen Sie darüber hinaus optimale Leistung bei Sicherungs- und Wiederherstellungsvorgängen.  
  
-   Eine fehlerhafte Sicherungsaktivität kann dazu führen, dass eine Sicherungsdatei unbrauchbar wird. Es wird empfohlen, regelmäßig nach fehlerhaften Sicherungen zu suchen und die BLOB-Dateien zu löschen. Weitere Informationen hierzu finden Sie unter [Löschen von Sicherungsblobdateien aktiver Lease](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md).  
  
-   Wenn Sie die Sicherung mit der `WITH COMPRESSION`-Option durchführen, können Sie die Speicherkosten und Speichertransaktionskosten minimieren. Darüber hinaus verkürzt die Option die Dauer des Sicherungsvorgangs.  

- Legen Sie die Argumente `MAXTRANSFERSIZE` und `BLOCKSIZE` wie im Artikel [SQL Server-Sicherung über URLs](./sql-server-backup-to-url.md) empfohlen fest.

- SQL Server ist unabhängig von der Art der verwendeten Speicherredundanz. Die Sicherung in Seitenblobs und Blockblobs wird für jede Speicherredundanz unterstützt (LRS\ZRS\GRS\RA-GRS\RA-GZRS\ usw.).
  
## <a name="handling-large-files"></a>Behandlung großer Dateien  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherungsvorgänge verwenden mehrere Threads, um die Datenübertragung an die Azure Blob Storage-Dienste zu optimieren.  Die Leistung hängt jedoch von verschiedenen Faktoren ab wie der Bandbreite des unabhängigen Softwareherstellers (ISV) und der Größe der Datenbank. Wenn Sie beabsichtigen, große Datenbanken oder Dateigruppen von einer lokalen SQL Server-Datenbank zu sichern, sollten Sie zunächst den Durchsatz testen. Die [SLA zum Speicher](https://azure.microsoft.com/support/legal/sla/storage/v1_0/) von Azure bietet maximale Verarbeitungszeiten für Blobs, die Sie als Ausgangspunkt verwenden können.  
  
-   Die Verwendung der im Abschnitt [Verwalten von Sicherungen](#managing-backups-mb1) empfohlenen `WITH COMPRESSION`-Option ist besonders beim Sichern umfangreicher Dateien von Bedeutung.  
  
## <a name="troubleshooting-backup-to-or-restore-from-url"></a>Problembehandlung bei Sicherungs- oder Wiederherstellungsvorgängen über URLs  

Im Folgenden finden Sie einige schnelle Lösungen zur Behandlung von Sicherungs- und Wiederherstellungsfehlern im Azure Blob Storage-Dienst.  
  
Zur Vermeidung von Fehlern, die durch nicht unterstützte Optionen oder Einschränkungen verursacht werden, informieren Sie sich anhand der Liste im Artikel [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage-Dienst](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) über die Möglichkeiten und Grenzen von BACKUP- und RESTORE-Befehlen.  

**Fehler bei der Initialisierung** 

Parallele Sicherungen im selben Blob führen dazu, dass bei einer der Sicherungen die Meldung **Fehler beim Initialisieren** ausgegeben wird. 

Wenn Sie Seitenblobs verwenden, z.B. `BACKUP... TO URL... WITH CREDENTIAL`, verwenden Sie die folgenden Fehlerprotokolle, die Ihnen die Problembehandlung bei Sicherungsfehlern erleichtern:  
  
Legen Sie das Ablaufverfolgungsflag 3051 wie folgt fest, um die Protokollierung in einem bestimmten Fehlerprotokoll zu aktivieren:  
  
`BackupToUrl-\<instname>-\<dbname>-action-\<PID>.log`, wenn `\<action>` einer der folgenden Typen ist:  
  
-   **DB**  
-   **FILELISTONLY**  
-   **LABELONLY**  
-   **HEADERONLY**  
-   **VERIFYONLY**  
  
Ebenso erhalten Sie Informationen, wenn Sie im Windows-Ereignisprotokoll in den Anwendungsprotokollen nach dem Namen `SQLBackupToUrl` suchen.  

**Die Anforderung konnte aufgrund eines E/A-Gerätefehlers nicht ausgeführt werden.**

Berücksichtigen Sie beim Sichern großer Datenbanken COMPRESSION, MAXTRANSFERSIZE, BLOCKSIZE und mehrere URL-Argumente.  Weitere Informationen finden Sie unter [Sichern einer VLDB in Azure Blob Storage](/archive/blogs/sqlcat/backing-up-a-vldb-to-azure-blob-storage).

Der folgende Fehler: 

```console
Msg 3202, Level 16, State 1, Line 1
Write on "https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak" failed: 
1117(The request could not be performed because of an I/O device error.)
Msg 3013, Level 16, State 1, Line 1
BACKUP DATABASE is terminating abnormally.
```

Eine Beispielauflösung: 

```sql  
BACKUP DATABASE TestDb
TO URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak',
URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_1.bak',
URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_2.bak'
WITH COMPRESSION, MAXTRANSFERSIZE = 4194304, BLOCKSIZE = 65536;  
```  

**Dateimarkierung für Gerät ist nicht ausgerichtet.**

Bei der Wiederherstellung von einer komprimierten Sicherung kann eine Fehlermeldung mit etwa folgendem Wortlaut angezeigt werden:  
  
```
SqlException 3284 occurred. Severity: 16 State: 5  
Message Filemark on device 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak' is not aligned.
Reissue the Restore statement with the same block size used to create the backupset: '65536' looks like a possible value.  
```
  
Führen Sie die **RESTORE**-Anweisung erneut aus, und geben Sie dabei **BLOCKSIZE = 65536** an, um diesen Fehler zu beheben.  
  
**Eine fehlerhafte Sicherungsaktivität kann zu Blobs mit aktiven Leases führen.**

Fehler während der Sicherung aufgrund von Blobs mit aktiver Lease: `Failed backup activity can result in blobs with active leases.`  

Wenn die BACKUP-Anweisung erneut ausgeführt wird, kann ein Sicherungsvorgang einen Fehler mit etwa folgendem Wortlaut verursachen:  

```
Backup to URL received an exception from the remote endpoint. Exception Message: 
The remote server returned an error: (412) There is currently a lease on the blob and no lease ID was specified in the request. 
```

Wenn versucht wird, eine RESTORE-Anweisung für eine BLOB-Sicherungsdatei mit aktiver Leasedauer auszuführen, wird eine Fehlermeldung mit etwa folgendem Wortlaut angezeigt:  
  
`Exception Message: The remote server returned an error: (409) Conflict..`  
  
Wenn dieser Fehler auftritt, müssen die BLOB-Dateien gelöscht werden. Weitere Informationen zu diesem Szenario und Lösungsvorschläge finden Sie unter [Löschen von Sicherungsblobdateien mit aktiver Lease](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md).  

**Betriebssystemfehler 50: Die Anforderung wird nicht unterstützt**
 
Beim Sichern einer Datenbank kann Fehler `Operating system error 50(The request is not supported)` aus den folgenden Gründen auftreten: 

   - Das angegebene Speicherkonto ist kein universelles V1/V2-Konto.
   - Das SAS-Token enthält mehr als 128 Zeichen.
   - Das SAS-Token enthielt bei der Erstellung der Anmeldeinformationen ein `?`-Symbol am Anfang des Tokens. Wenn ja, entfernen Sie es.
   - Die aktuelle Verbindung kann vom aktuellen Computer aus über Storage-Explorer oder SQL Server Management Studio (SSMS) keine Verbindung mit dem Speicherkonto herstellen. 
   - Die dem SAS-Token zugewiesene Richtlinie ist abgelaufen. Erstellen Sie mithilfe von Azure Storage-Explorer eine neue Richtlinie. Erstellen Sie dann entweder ein neues SAS-Token mit der Richtlinie, oder ändern Sie die Anmeldeinformationen, und versuchen Sie erneut, eine Sicherung durchzuführen. 

**Authentifizierungsfehler**
  
`WITH CREDENTIAL` ist eine neue Option, die für Sicherungs- oder Wiederherstellungsvorgänge im Azure Blob Storage-Dienst erforderlich ist.

In Zusammenhang mit Anmeldeinformationen können folgende Fehler auftreten: `The credential specified in the **BACKUP** or **RESTORE** command does not exist. `

Um dieses Problem zu vermeiden, können Sie T-SQL-Anweisungen zum Erstellen von Anmeldeinformationen einschließen, falls in der BACKUP-Anweisung keine Anmeldeinformationen enthalten sind. Beachten Sie das folgende Anwendungsbeispiel:  

  
```sql  
IF NOT EXISTS  
(SELECT * FROM sys.credentials   
WHERE credential_identity = 'mycredential')  
CREATE CREDENTIAL <credential name> WITH IDENTITY = 'mystorageaccount'  
, SECRET = '<storage access key>' ;  
```  
  
Die Anmeldeinformationen sind zwar vorhanden, aber das Anmeldekonto zum Ausführen des Sicherungsbefehls verfügt über keine Berechtigung für den Zugriff auf die Anmeldeinformationen. Verwenden Sie ein Anmeldekonto der Rolle **db_backupoperator** mit Berechtigungen des Typs **_Beliebige Anmeldeinformationen ändern_**.  
  
Überprüfen Sie den Namen des Speicherkontos und die Schlüsselwerte. Die in den Anmeldeinformationen gespeicherten Informationen müssen den Eigenschaftswerten Azure-Speicherkontos entsprechen, das Sie für Sicherungs- und Wiederherstellungsvorgänge verwenden.  
  

**Fehler 400: Ungültige Anforderung**

Bei Verwendung von SQL Server 2012 tritt möglicherweise ein Fehler auf, der der folgenden Art von Sicherung ähnelt:

```
Backup to URL received an exception from the remote endpoint. Exception Message: 
The remote server returned an error: (400) Bad Request..
```

Dies wird durch die vom Azure Storage-Konto unterstützte TLS-Version verursacht. Die unterstützte TLS-Version wird geändert, oder die in [KB4017023](https://support.microsoft.com/en-us/topic/kb4017023-sql-server-2012-2014-or-2016-backup-to-microsoft-azure-blob-storage-service-url-isn-t-compatible-for-tls-1-2-e9ef6124-fc05-8128-86bc-f4f4f5ff2b78) aufgeführte Problemumgehung wird verwendet.


## <a name="proxy-errors"></a>Proxyfehler  
 Wenn Sie über Proxyserver auf das Internet zugreifen, können folgende Probleme auftreten:  
  
 **Verbindungsdrosselung durch Proxyserver**  
  
 Proxyserver können über Einstellungen verfügen, die die Anzahl der Verbindungen pro Minute begrenzen. Der URL-Sicherungsprozess ist ein Multithreadprozess und kann diese Begrenzung folglich überschreiten. In diesem Fall wird die Verbindung vom Proxyserver abgebrochen. Um das Problem zu beheben, ändern Sie die Proxyeinstellungen, damit der Proxy von SQL Server nicht verwendet wird. Im Folgenden einige Beispiele für Fehlertypen oder -meldungen, die im Fehlerprotokoll angezeigt werden können:  
  
```console
Write on "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak" failed: Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
```console
A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Error could not be gathered from Remote Endpoint.  
  
Msg 3013, Level 16, State 1, Line 2  
  
BACKUP DATABASE is terminating abnormally.  
```

```console
BackupIoRequest::ReportIoError: write failure on backup device https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak'. Operating system error Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
Wenn Sie die ausführliche Protokollierung mit Ablaufverfolgungsflag 3051 aktivieren, können auch folgende Meldungen in den Protokollen angezeigt werden:  
  
`HTTP status code 502, HTTP Status Message Proxy Error (The number of HTTP requests per minute exceeded the configured limit. Contact your ISA Server administrator.)` 
  
 **Standardproxyeinstellungen wurden nicht abgerufen:**  
  
Teilweise werden die Standardeinstellungen nicht übernommen, wodurch ähnliche Proxyauthentifizierungsfehler wie die untenstehend aufgeführten entstehen:
 
 `A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Backup to URL received an exception from the remote endpoint. Exception Message: The remote server returned an error: (407)* **Proxy Authentication Required.`  
  
Um dieses Problem zu beheben, erstellen Sie mithilfe folgender Schritte eine Konfigurationsdatei, durch die beim URL-Sicherungsprozess die Standardproxyeinstellungen verwendet werden können:  
  
1.  Erstellen Sie eine Konfigurationsdatei mit dem Namen `BackuptoURL.exe.config` und folgendem XML-Inhalt:  
  
    ```xml  
    <?xml version ="1.0"?>  
    <configuration>   
                    <system.net>   
                                    <defaultProxy enabled="true" useDefaultCredentials="true">   
                                                    <proxy usesystemdefault="true" />   
                                    </defaultProxy>   
                    </system.net>  
    </configuration>  
    ```  
  
2.  Speichern Sie die Konfigurationsdatei im Ordner „Binn“ der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz. Beispiel: Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Computer auf Laufwerk C installiert ist, legen Sie die Konfigurationsdatei im Ordner `C:\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\Binn` ab.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiederherstellen von in Microsoft Azure gespeicherten Sicherungen](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)
