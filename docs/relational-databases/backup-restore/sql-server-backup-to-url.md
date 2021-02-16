---
title: SQL Server-Sicherung über URLs | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Konzepte, Anforderungen und Komponenten, die notwendig sind, damit SQL Server Microsoft Azure Blob Storage als Sicherungsziel verwenden kann.
ms.custom: ''
ms.date: 03/25/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 11be89e9-ff2a-4a94-ab5d-27d8edf9167d
author: cawrites
ms.author: chadam
ms.openlocfilehash: e2652320875d3bf22f5a7557dc7c1deba9a93376
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349039"
---
# <a name="sql-server-backup-to-url"></a>SQL Server-Sicherung über URLs

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

In diesem Artikel werden die Konzepte, die Anforderungen und die Komponenten eingeführt, die notwendig sind, um den Microsoft Azure Blob Storage-Dienst als Sicherungsziel zu verwenden. Die Sicherungs- und Wiederherstellungsfunktion sind gleich oder ähnlich wie beim Verwenden von DISK oder TAPE, mit wenigen Unterschieden. Diese Unterschiede sowie einige Codebeispiele werden in diesem Thema erörtert.  
  
## <a name="overview"></a>Übersicht

Es ist wichtig, die jeweiligen Komponenten und Interaktionen zwischen den einzelnen Komponenten zu verstehen, um Daten mit dem Microsoft Azure Blob Storage-Dienst zu sichern oder wiederherzustellen.  
  
 Der erste Schritt in diesem Verfahren besteht im Erstellen eines Azure-Speicherkontos innerhalb Ihres Azure-Abonnements. Dieses Speicherkonto ist ein Administratorkonto, das über vollständige Administratorrechte für alle mit dem Speicherkonto erstellten Container und Objekte verfügt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann entweder den Azure Storage-Kontonamen und den zugehörigen Zugriffsschlüsselwert für die Authentifizierung und das Schreiben und Lesen von Blobs in Microsoft Azure Blob Storage oder ein Shared Access Signature-Token verwenden, das für bestimmte Container generiert wird und Lese- und Schreibberechtigungen erteilt. Weitere Informationen zu Azure-Speicherkonten finden Sie unter [About Azure Storage Accounts (Informationen zu Azure-Speicherkonten)](/azure/storage/common/storage-account-create), und weitere Informationen zu Shared Access Signatures finden Sie unter [Shared Access Signatures, Teil 1: Grundlagen zum SAS-Modell](/azure/storage/common/storage-sas-overview). Diese Authentifizierungsinformationen werden in den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen gespeichert und bei Sicherungs- und Wiederherstellungsvorgängen verwendet.  
  
###  <a name="backup-to-block-blob-vs-page-blob"></a><a name="blockbloborpageblob"></a> Sicherung: Blockblobs vs. Seitenblobs

Es gibt zwei Arten von Blobs, die im Microsoft Azure Blob Storage-Dienst gespeichert werden können: Block- und Seitenblobs. Bei SQL Server 2016 und höher wird das Blockblob bevorzugt.

Wenn der Speicherschlüssel in den Anmeldeinformationen verwendet wird, wird das Seitenblob verwendet.Wenn die SAS verwendet wird, wird das Blockblob verwendet.

Das Sichern mit Blockblobs ist nur in SQL Server 2016 oder neueren Versionen verfügbar. Verwenden Sie Blockblobs statt Seitenblocks zum Sichern, wenn Sie SQL Server 2016 oder höher nutzen.

Die wichtigsten Gründe dafür sind:

- Im Vergleich zum Speicherschlüssel ist SAS ein sicherer Weg, um Blobzugriff zu autorisieren.
- Sie können Sicherungen auf mehreren Blockblobs erstellen, um eine bessere Sicherungs- und Wiederherstellungsleistung sowie eine umfangreichere Datenbanksicherung zu erzielen.
- [Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/) sind kostengünstiger als [Seitenblobs](https://azure.microsoft.com/pricing/details/storage/page-blobs/).
- Kunden, die über einen Proxyserver eine Sicherung in Seitenblobs durchführen müssen, müssen „backuptourl.exe“ verwenden.

Die Sicherung einer großen Datenbank im Blobspeicher unterliegt den Einschränkungen, die unter [Unterschiede bei T-SQL zwischen SQL Server und Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-transact-sql-information#backup) aufgeführt werden.

Wenn die Datenbank zu groß ist, haben Sie die Möglichkeit:

- Sicherungskomprimierung zu verwenden  oder
- Sicherungen auf mehreren Blockblobs auszuführen

#### <a name="support-on-linux-containers-and-azure-arc-enabled-sql-managed-instance"></a>Unterstützung für Linux, Container und SQL Managed Instance mit Azure Arc-Unterstützung

Wenn die SQL Server-Instanz unter Linux gehostet wird, einschließlich:

- Eigenständiges Betriebssystem
- Container
- Azure Arc-fähige SQL Managed Instance-Instanz
- Alle anderen Linux-basierten Umgebungen

Das einzige unterstützte URL-Sicherungsmuster besteht für Blockblobs, wobei die Shared Access Signature verwendet wird.

###  <a name="microsoft-azure-blob-storage-service"></a><a name="Blob"></a> Microsoft Azure Blob Storage-Dienst  

**Speicherkonto:** Das Speicherkonto ist der Ausgangspunkt für alle Speicherdienste. Sie müssen zunächst ein Azure Storage-Konto erstellen, um auf den Azure Blob Storage-Dienst zuzugreifen. Weitere Informationen finden Sie unter [Erstellen eines Speicherkontos](/azure/storage/common/storage-account-create).  
  
**Container:** In einem Container können mehrere BLOBs gruppiert und eine unbegrenzte Anzahl von BLOBs gespeichert werden. Sie müssen zumindest den Stammcontainer erstellen, damit Sie eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherung in den Azure Blob Storage-Dienst schreiben können. Sie können ein Shared Access Signature-Token für einen Container generieren und den Zugriff nur auf Objekte in einem bestimmten Container gewähren.  
  
**BLOB:** Eine Datei eines beliebigen Typs und beliebiger Größe. Es gibt zwei Arten von Blobs, die im Microsoft Azure Blob Storage-Dienst gespeichert werden können: Block- und Seitenblobs. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Je nach der verwendeten Transact-SQL-Syntax können bei der Sicherung beide BLOB-Typen verwendet werden. Blobs sind über das folgende URL-Format adressierbar: https://\<storage account>.blob.core.windows.net/\<container>/\<blob>. Weitere Informationen über den Azure Blob Storage-Dienst finden Sie unter [Verwenden von Blob Storage mit .NET](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/). Weitere Informationen zu Seiten- und Block-BLOBs finden Sie unter [Grundlegendes zu Block- und Seiten-BLOBs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).  
  
![Azure Blob Storage](../../relational-databases/backup-restore/media/backuptocloud-blobarchitecture.gif "Azure Blob Storage")  
  
**Azure-Momentaufnahme:** Eine Momentaufnahme eines Azure-BLOBs zu einem bestimmten Zeitpunkt. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines BLOBs](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob). Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherung unterstützt nun Azure-Momentaufnahmesicherungen von Datenbankdateien, die im Microsoft Azure Blob Storage-Dienst gespeichert werden. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
###  <a name="ssnoversion-components"></a><a name="sqlserver"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten  

**URL:** Eine URL gibt einen URI (Uniform Resource Identifier) für eine eindeutige Sicherungsdatei an. Mit der URL werden Speicherort und Name der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungsdatei angegeben. Die URL muss auf ein tatsächliches BLOB, nicht nur auf einen Container verweisen. Wenn das BLOB nicht vorhanden ist, wird es erstellt. Wird ein vorhandenes BLOB angegeben, erzeugt BACKUP einen Fehler, es sei denn, die WITH FORMAT-Option ist angegeben, um die vorhandene Sicherungsdatei im BLOB zu überschreiben.  
  
 Beispiel für eine URL: http[s]://KONTONAME.blob.core.windows.net/\<CONTAINER>/\<FILENAME.bak>. HTTPS ist zwar nicht erforderlich, aber empfehlenswert.  
  
**Anmeldeinformationen:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen sind ein Objekt zum Speichern von Authentifizierungsinformationen, die für die Verbindung mit einer Ressource außerhalb von SQL Server erforderlich sind. Dabei verwenden die Sicherungs- und Wiederherstellungsprozesse von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Anmeldeinformationen für die Authentifizierung beim Azure Blob Storage-Dienst und den Containern und Blobs. Als Anmeldeinformationen werden entweder der Name und die **Zugriffsschlüsselwerte** des Speicherkontos oder die Container-URL und das zugehörige Shared Access Signature-Token gespeichert. Sobald die Anmeldeinformationen erstellt wurden, bestimmt die Syntax der BACKUP/RESTORE-Anweisungen den Typ des BLOBs und die erforderlichen Anmeldeinformationen.  
  
Ein Beispiel zum Erstellen einer Shared Access Signature finden Sie in den Beispielen zum [Erstellen einer Shared Access Signature](../../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS) weiter unten in diesem Thema. Informationen zum Erstellen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen finden Sie in den Beispielen unter [Erstellen von Anmeldeinformationen](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) weiter unten in diesem Thema.  
  
Weitere allgemeine Informationen über Anmeldeinformationen finden Sie unter [Anmeldeinformationen](../security/authentication-access/credentials-database-engine.md).  
  
Informationen mit weiteren Beispielen zur Verwendung von Anmeldeinformationen finden Sie unter [Erstellen eines Proxys für den SQL Server-Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md).  
  
##  <a name="security"></a><a name="security"></a> Sicherheit  

Die folgenden Sicherheitsüberlegungen und -anforderungen beziehen sich auf die Sicherung oder Wiederherstellung mithilfe des Microsoft Azure Blob Storage-Diensts.  
  
- Es wird empfohlen, dass Sie den Zugriff auf **Privat** festlegen, wenn Sie einen Container für den Azure Blob Storage-Dienst erstellen. Dadurch wird der Zugriff auf Benutzer oder Konten beschränkt, die über die erforderlichen Anmeldeinformationen zur Authentifizierung beim Azure-Konto verfügen.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert, dass entweder der Name und Zugriffsschlüssel zur Authentifizierung beim Azure-Konto oder eine Shared Access Signature und das Zugriffstoken in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen gespeichert werden. Mithilfe dieser Informationen wird im Fall von Sicherungs- und Wiederherstellungsvorgängen die Authentifizierung beim Azure-Konto ausgeführt.  
  
- Das zum Ausgeben von BACKUP- oder RESTORE-Befehlen verwendete Benutzerkonto sollte Mitglied der Datenbankrolle **db_backup operator** sein und über Berechtigungen zum **Ändern beliebiger Anmeldeinformationen** verfügen.   

##  <a name="limitations"></a><a name="limitations"></a> Einschränkungen  
  
- SQL Server schränkt die maximale Sicherungsgröße, die mit einem Seitenblob unterstützt wird, auf 1 TB ein. Die maximal unterstützte Größe für Sicherungen mit Blockblobs ist auf ca. 200 GB (50.000 Blöcke * 4 MB MAXTRANSFERSIZE) beschränkt. Für Blockblobs wird Striping unterstützt, damit erheblich größere Sicherungen unterstützt werden.  
  
    > [!IMPORTANT]  
    >  Obwohl die maximal unterstützte Sicherungsgröße durch einen einzelnen Blockblob bei 200 GB liegt, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in kleinere Blocks schreiben. Dadurch kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Limit von 50.000 Blocks erreichen, bevor die Sicherung vollständig übertragen wurde. Teilen Sie Sicherungen auf, auch wenn diese kleiner als 200 GB sind, um zu vermeiden, dass das Blocklimit erreicht wird. Das gilt insbesondere dann, wenn Sie differenzielle oder nicht komprimierte Sicherungen verwenden.

- Sie können Sicherungs- oder Wiederherstellungsanweisungen ausgeben, indem Sie TSQL, SMO, PowerShell-Cmdlets, SQL Server Management Studio Backup oder den Wiederherstellungs-Assistenten verwenden.   
  
- Das Erstellen von Namen für logische Geräte wird nicht unterstützt. Folglich ist es nicht möglich, eine URL mithilfe von sp_dumpdevice oder über SQL Server Management Studio als Sicherungsmedium hinzuzufügen.  
  
- Das Anfügen an vorhandene Sicherungs-BLOBs wird nicht unterstützt. Sicherungen auf einem vorhandenen BLOB können nur unter Verwendung der Option **WITH FORMAT** überschrieben werden. Allerdings ist das Argument **WITH FORMAT** bei Verwendung von Dateimomentaufnahme-Sicherungen (mit dem Argument **WITH FILE_SNAPSHOT** ) nicht zulässig, um zu vermeiden, dass Dateimomentaufnahmen, die mit der ursprünglichen Dateimomentaufnahme-Sicherung erstellt wurden, verwaist zurückbleiben.  
  
- Um Daten in einem einzigen Sicherungsvorgang in mehrere BLOBs zu sichern, müssen Sie Block-BLOBs und ein SAS-Token anstelle der Speicherkontoschlüssel für die SQL-Anmeldeinformationen verwenden.  
  
- Das Angeben von **BLOCKSIZE** wird für Seiten-Blobs nicht unterstützt. 
  
- Das Angeben von **MAXTRANSFERSIZE** wird für Seiten-Blobs nicht unterstützt. 
  
- Die Angabe von Sicherungssatzoptionen mit **RETAINDAYS** und **EXPIREDATE** wird nicht unterstützt.  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf 259 Zeichen begrenzt. Da BACKUP TO URL 36 Zeichen für die erforderlichen Elemente zur Angabe der URL (https://.blob.core.windows.net//.bak ) beansprucht, verbleiben insgesamt noch 223 Zeichen für Konto-, Container- und Blobnamen.  

- Wenn Ihr Server über einen Proxyserver auf Azure zugreift, müssen Sie das Ablaufverfolgungsflag 1819 verwenden und dann die WinHTTP-Proxykonfiguration mit einer der folgenden Methoden festlegen:
   - Dem [proxycfg.exe](/windows/win32/winhttp/proxycfg-exe--a-proxy-configuration-tool)-Hilfsprogramm unter Windows XP oder Windows Server 2003 und früher. 
   - Dem [netsh.exe](/windows/win32/winsock/netsh-exe)-Hilfsprogramm unter Windows Vista und Windows Server 2008 oder höher. 

- [Unveränderlicher Speicher für Azure Blob Storage](/azure/storage/blobs/storage-blob-immutable-storage) wird nicht unterstützt. Legen Sie die Richtlinie **Unveränderlicher Speicher** auf FALSE fest. 
  
## <a name="supported-arguments--statements"></a>Unterstützte Argumente und Anweisungen

###  <a name="support-for-backuprestore-statements"></a><a name="Support"></a> Unterstützung für BACKUP-/RESTORE-Anweisungen  
  
|BACKUP-/RESTORE-Anweisung|Unterstützt|Ausnahmen|Kommentare|
|-|-|-|-|
|BACKUP|J|BLOCKSIZE und MAXTRANSFERSIZE werden für Block-Blobs unterstützt. Sie werden nicht für Seiten-Blobs unterstützt. | Die Sicherung in einem Block-Blob erfordert eine SAS, die in einer SQL Server-Anmeldeinformation gespeichert ist. Die Sicherung auf einem Seiten-Blob erfordert den Speicherkontoschlüssel, der in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformation gespeichert ist, und erfordert das Argument WITH CREDENTIAL, um festgelegt zu werden.|  
|RESTORE|J||Erfordert die Definition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen sowie die Angabe des Arguments WITH CREDENTIAL, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimem Schlüssel definiert werden.|  
|RESTORE FILELISTONLY|J||Erfordert die Definition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen sowie die Angabe des Arguments WITH CREDENTIAL, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimem Schlüssel definiert werden.|  
|RESTORE HEADERONLY|J||Erfordert die Definition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen sowie die Angabe des Arguments WITH CREDENTIAL, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimem Schlüssel definiert werden.|  
|RESTORE LABELONLY|J||Erfordert die Definition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen sowie die Angabe des Arguments WITH CREDENTIAL, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimem Schlüssel definiert werden.|  
|RESTORE VERIFYONLY|J||Erfordert die Definition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen sowie die Angabe des Arguments WITH CREDENTIAL, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimem Schlüssel definiert werden.|  
|RESTORE REWINDONLY|-|||  
  
 Allgemeine und Syntaxinformationen zu BACKUP-Anweisungen finden Sie unter [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
 Allgemeine und Syntaxinformationen zu RESTORE-Anweisungen finden Sie unter [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="support-for-backup-arguments"></a>Unterstützung für BACKUP-Argumente  

|Argument|Unterstützt|Ausnahme|Kommentare|  
|-|-|-|-|  
|DATABASE|J|||  
|PROTOKOLL|J|||  
||  
|TO (URL)|J|Bei URL wird das Angeben bzw. Erstellen eines logischen Namens im Gegensatz zu DISK und TAPE nicht unterstützt.|Dieses Argument wird verwendet, um den URL-Pfad der Sicherungsdatei anzugeben.|  
|MIRROR TO|J|||  
|**WITH-OPTIONEN:**||||  
|CREDENTIAL|J||WITH CREDENTIAL wird nur unterstützt, wenn die Option BACKUP TO URL für die Sicherung im Azure Blob Storage-Dienst verwendet wird und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen mithilfe des Speicherkontoschlüssels als geheimer Schlüssel definiert werden.|  
|FILE_SNAPSHOT|J|||  
|ENCRYPTION|J||Wird das Argument **WITH ENCRYPTION** angegeben, gewährleistet die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung, dass die gesamte Datenbank vor dem Erstellen der Sicherung mit TDE verschlüsselt wurde, und verschlüsselt die Dateimomentaufnahme-Sicherungsdatei selbst in diesem Fall mit dem für TDE in der Datenbank angegebenen Algorithmus. Die Sicherung schlägt fehl, wenn nicht alle Daten in der gesamten Datenbank verschlüsselt sind (z. B. wenn der Verschlüsselungsvorgang noch nicht abgeschlossen ist).|  
|DIFFERENTIAL|J|||  
|COPY_ONLY|J|||  
|COMPRESSION&#124;NO_COMPRESSION|J|Wird für Dateimomentaufnahme-Sicherungen nicht unterstützt||  
|DESCRIPTION|J|||  
|NAME|J|||  
|EXPIREDATE &#124; RETAINDAYS|-|||  
|NOINIT &#124; INIT|-||Das Anfügen an BLOBs ist nicht möglich. Verwenden Sie zum Überschreiben einer Sicherung das Argument **WITH FORMAT** . Allerdings ist das Argument **WITH FORMAT** bei Verwendung von Dateimomentaufnahme-Sicherungen (mit dem Argument **WITH FILE_SNAPSHOT** ) nicht zulässig, um zu vermeiden, dass Dateimomentaufnahmen, die mit der ursprünglichen Dateimomentaufnahme-Sicherung erstellt wurden, verwaist zurückbleiben.|  
|NOSKIP &#124; SKIP|-|||  
|NOFORMAT &#124; FORMAT|J||Eine Sicherung, die auf ein vorhandenes BLOB geschrieben wird, ist nur erfolgreich, wenn **WITH FORMAT** angegeben wird. Das vorhandene BLOB wird bei Angabe von **WITH FORMAT** überschrieben. Allerdings ist das Argument FORMAT bei Verwendung von Dateimomentaufnahme-Sicherungen (mit dem Argument **WITH FILE_SNAPSHOT** ) nicht zulässig, um zu vermeiden, dass Dateimomentaufnahmen, die mit der ursprünglichen Dateimomentaufnahme-Sicherung erstellt wurden, verwaist zurückbleiben. Allerdings ist das Argument **WITH FORMAT** bei Verwendung von Dateimomentaufnahme-Sicherungen (mit dem Argument **WITH FILE_SNAPSHOT** ) nicht zulässig, um zu vermeiden, dass Dateimomentaufnahmen, die mit der ursprünglichen Dateimomentaufnahme-Sicherung erstellt wurden, verwaist zurückbleiben.|  
|MEDIADESCRIPTION|J|||  
|MEDIANAME|J|||  
|BLOCKSIZE|J|Nicht für Seiten-Blob unterstützt. Unterstützt für Block-Blob.| Empfohlen BLOCKSIZE = 65536 zum Optimieren der Verwendung von 50.000 Blöcken, die in einem Block-Blob zulässig sind. |  
|BUFFERCOUNT|J|||  
|MAXTRANSFERSIZE|J|Nicht für Seiten-Blob unterstützt. Unterstützt für Block-Blob.| Der Standardwert ist 1048576. Der Wert kann in einem Bereich bis zu 4 MB in Schritten von 65.536 Bytes liegen.</br> Empfohlen MAXTRANSFERSIZE = 4194304 zum Optimieren der Verwendung von 50.000 Blöcken, die in einem Block-Blob zulässig sind. |  
|NO_CHECKSUM &#124; CHECKSUM|J|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|J|||  
|STATISTIK|J|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|NORECOVERY &#124; STANDBY|J|||  
|NO_TRUNCATE|J|||  
  
 Weitere Informationen zu BACKUP-Argumenten finden Sie unter [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
### <a name="support-for-restore-arguments"></a>Unterstützung für RESTORE-Argumente  
  
|Argument|Unterstützt|Ausnahmen|Kommentare|  
|-|-|-|-|  
|DATABASE|J|||  
|PROTOKOLL|J|||  
|FROM (URL)|J||Das FROM URL-Argument wird verwendet, um den URL-Pfad der Sicherungsdatei anzugeben.|  
|**WITH Options:**||||  
|CREDENTIAL|J||WITH CREDENTIAL wird nur unterstützt, wenn die Option RESTORE FROM URL für die Wiederherstellung mit dem Azure Blob Storage-Dienst verwendet wird.|  
|PARTIAL|J|||  
|RECOVERY &#124; NORECOVERY &#124; STANDBY|J|||  
|LOADHISTORY|J|||  
|MOVE|J|||  
|REPLACE|J|||  
|RESTART|J|||  
|RESTRICTED_USER|J|||  
|FILE|-|||  
|PASSWORD|J|||  
|MEDIANAME|J|||  
|MEDIAPASSWORD|J|||  
|BLOCKSIZE|J|||  
|BUFFERCOUNT|-|||  
|MAXTRANSFERSIZE|-|||  
|CHECKSUM &#124; NO_CHECKSUM|J|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|J|||  
|FILESTREAM|J|Wird für Momentaufnahmesicherungen nicht unterstützt||  
|STATISTIK|J|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|KEEP_REPLICATION|J|||  
|KEEP_CDC|J|||  
|ENABLE_BROKER &#124; ERROR_BROKER_CONVERSATIONS &#124; NEW_BROKER|J|||  
|STOPAT &#124; STOPATMARK &#124; STOPBEFOREMARK|J|||  
  
 Weitere Informationen zu RESTORE-Argumenten finden Sie unter [RESTORE-Argumente &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
##  <a name="back-up-with-ssms"></a><a name="BackupTaskSSMS"></a> Sichern mit SSMS  
Sie können eine Datenbank über URL mit dem Sicherungstask in SQL Server Management Studio mithilfe einer SQL-Server-Anmeldeinformation schützen.  
  
> [!NOTE]  
>  Sie müssen Sie allerdings Transact-SQL, Powershell oder C# anstelle des Sicherungstasks in SQL Server Management Studio verwenden, um eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung zu erstellen oder einen vorhandenen Mediensatz zu überschreiben.  
  
 Die folgenden Schritte beschreiben die Änderungen, die am Task „Datenbank sichern“ in SQL Server Management Studio vorgenommen wurde, um das Sichern in Azure Storage zu ermöglichen:  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz der SQL Server-Datenbank-Engine her, und erweitern Sie anschließend diese Instanz.

2.  Erweitern Sie **Datenbanken**, klicken Sie mit der rechten Maustaste auf die gewünschte Datenbank, zeigen Sie auf **Tasks**, und klicken Sie anschließend auf **Back Up...** (Sichern).
  
3.  Die Option **URL** ist in der Dropdownliste **Back up to:** (Sichern auf:) auf der Seite **Allgemein** im Abschnitt **Ziel** verfügbar.  Die Option **URL** wird verwendet, um eine Sicherung im Windows Azure-Speicher zu erstellen. Klicken Sie auf **Hinzufügen** . Das Dialogfeld **Sicherungsziel auswählen** wird geöffnet:
    1.  **Azure-Speichercontainer:** Der Name des Microsoft Azure-Speichercontainers, um die Sicherungsdateien zu speichern.  Wählen Sie einen vorhandenen Container aus der Dropdown Liste aus, oder geben Sie ihn manuell ein. 
  
    2.  **Richtlinie für den gemeinsamen Zugriff:** Geben Sie die SAS (Shared Access Signature) für einen manuell eingegebenen Container an.  Dieses Feld ist nicht verfügbar, wenn ein vorhandener Container ausgewählt wurde. 
  
    3.  **Sicherungsdatei:** Name der Sicherungsdatei.
    
    4.  **Neuer Container:** Wird verwendet, um einen vorhandenen Container zu registrieren, für den Sie keine SAS besitzen.  Weitere Informationen finden Sie unter [Herstellen einer Verbindung zu einem Microsoft Azure-Abonnement](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).

> [!NOTE] 
>  **Hinzufügen** unterstützt mehrere Sicherungsdateien und Speichercontainer für einen einzelnen Mediensatz.

Wenn Sie eine **URL** als Ziel auswählen, sind bestimmte Optionen auf der Seite **Medienoptionen** deaktiviert.  Die folgenden Themen enthalten weitere Informationen zum Dialogfeld Datenbank sichern:  
  
 [Datenbank sichern &#40;Seite Allgemein&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)  
  
 [Datenbank sichern &#40;Seite 'Medienoptionen'&#41;](../../relational-databases/backup-restore/back-up-database-media-options-page.md)  
  
 [Datenbank sichern &#40;Seite 'Sicherungsoptionen'&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
 [Erstellen von Anmeldeinformationen – Authentifizieren beim Azure-Speicher](../../relational-databases/backup-restore/create-credential-authenticate-to-azure-storage.md)  
  
##  <a name="back-up-with-maintenance-plan"></a><a name="MaintenanceWiz"></a> Sichern mit Wartungsplan  
 Ähnlich dem zuvor beschriebenen Sicherungstask umfasst der Wartungsplanungs-Assistent in SQL Server Management Studio **URL** als eine der Zieloptionen sowie andere unterstützende Objekte, die erforderlich sind, um Daten in Azure Storage zu sichern, z. B. die SQL-Anmeldeinformationen. Weitere Informationen finden Sie unter der **Definieren von Sicherungstasks** im Abschnitt [Using Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure).  
  
> [!NOTE]  
>  Sie müssen Transact-SQL, Powershell oder C# anstelle des Sicherungstasks im Wartungsplanungs-Assistenten verwenden, um einen Sicherungssatz im Stripesetformat, eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung oder SQL-Anmeldeinformationen unter Verwendung eines Shared Access-Tokens zu erstellen.  
  
##  <a name="restore-with-ssms"></a><a name="RestoreSSMS"></a> Wiederherstellen mit SSMS 
Die Aufgabe „Datenbank wiederherstellen“ enthält als Medium eine **URL** , von der aus wiederhergestellt wird.  In den folgenden Schritten wird die Verwendung der Wiederherstellungsaufgabe zum Durchführen einer Wiederherstellung mit dem Azure Blob Storage-Dienst beschrieben: 
  
1. Klicken Sie mit der rechten Maustaste auf **Datenbanken** , und wählen Sie **Datenbank wiederherstellen** aus. 
  
2. Wählen Sie auf der Seite **Allgemein** im Abschnitt **Quelle** die Option **Gerät** aus.
  
3. Klicken Sie auf die Schaltfläche zum Durchsuchen (...), um das Dialogfeld **Sicherungsmedien auswählen** zu öffnen. 

4. Wählen Sie eine **URL** aus der Dropdownliste **Sicherungsmedientyp** aus.  Klicken Sie auf **Hinzufügen** , um das Dialogfeld **Speicherort der Sicherungsdatei auswählen** zu öffnen.

    1. **Azure-Speichercontainer:** Der vollqualifizierte Name des Microsoft Azure-Speichercontainers, mit dem Sicherungsdateien gespeichert werden.  Wählen Sie einen vorhandenen Container aus der Dropdownliste aus, oder geben Sie manuell einen vollqualifizierten Containernamen ein.
      
    2. **Shared Access Signature (SAS):**  Wird verwendet, um die SAS für den angegebenen Container einzugeben.
      
    3. **Hinzufügen:**  Wird verwendet, um einen vorhandenen Container zu registrieren, für den Sie keine SAS besitzen.  Weitere Informationen finden Sie unter [Herstellen einer Verbindung zu einem Microsoft Azure-Abonnement](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
      
    4. **OK:**    SQL Server stellt mithilfe der angegebenen SQL-Anmeldeinformationen eine Verbindung mit dem Microsoft Azure-Speicher her und öffnet das Dialogfeld **Sicherungsdatei in Microsoft Azure suchen**. Die Sicherungsdateien, die sich im Speichercontainer befinden, werden auf dieser Seite angezeigt. Wählen Sie die Datei aus, die Sie zum Wiederherstellen verwenden möchten, und klicken Sie auf **OK**. Dadurch werden Sie wieder zum Dialogfeld **Sicherungsmedien auswählen** geleitet. Wenn Sie in diesem Dialogfeld auf **OK** klicken, wird das Hauptdialogfeld **Wiederherstellen** aufgerufen, in dem Sie die Wiederherstellung abschließen können. 
  
     [Datenbank wiederherstellen &#40;Seite „Allgemein“&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
     [Datenbank wiederherstellen &#40;Seite Dateien&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
     [Datenbank wiederherstellen &#40;Seite Optionen&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
##  <a name="code-examples"></a><a name="Examples"></a> Codebeispiele  

Dieser Abschnitt enthält die folgenden Beispiele.  
  
- [Erstellen von Anmeldeinformationen](#credential)  
  
- [Sichern einer vollständigen Datenbank](#complete)  

- [Wiederherstellen eines bestimmten Zeitpunkts mithilfe von STOPAT](#PITR)  
  
> [!NOTE]  
> Ein Tutorial zur Verwendung von SQL Server 2016 mit dem Microsoft Azure Blob Storage-Dienst finden Sie unter [Tutorial: Verwenden des Azure Blob Storage-Diensts mit SQL Server 2016-Datenbanken](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="create-a-shared-access-signature"></a><a name="SAS"></a> Erstellen einer Shared Access Signature

In dem folgenden Beispiel werden Shared Access Signatures erstellt, die zum Erstellen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen für einen neu erstellten Container verwendet werden können. Das Skript erstellt eine Shared Access Signature, die einer gespeicherten Zugriffsrichtlinie zugeordnet ist. Weitere Informationen finden Sie unter [Shared Access Signatures, Teil 1: Grundlegendes zum SAS-Modell](/azure/storage/common/storage-sas-overview). Das Skript schreibt auch den T-SQL-Befehl, der zum Erstellen der Anmeldeinformationen unter SQL Server erforderlich ist. 

> [!NOTE]
> Dieses Beispiel erfordert Microsoft Azure PowerShell. Informationen zum Installieren und Verwenden von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/).  
> Diese Skripts wurden mit Azure PowerShell 5.1.15063 überprüft. 

**Shared Access Signature, die einer gespeicherten Zugriffsrichtlinie zugeordnet ist**
  
```Powershell  
# Define global variables for the script  
$prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
$subscriptionName='<your subscription name>'   # the name of subscription name you will use  
$locationName = '<a data center location>'  # the data center region you will use  
$storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
$containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
$policyName = $prefixName + 'policy' # the name of the SAS policy  

# Set a variable for the name of the resource group you will create or use  
$resourceGroupName=$prefixName + 'rg'

# adds an authenticated Azure account for use in the session
Connect-AzAccount

# set the tenant, subscription and environment for use in the rest of
Set-AzContext -SubscriptionName $subscriptionName

# create a new resource group - comment out this line to use an existing resource group  
New-AzResourceGroup -Name $resourceGroupName -Location $locationName

# Create a new ARM storage account - comment out this line to use an existing ARM storage account  
New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   

# Get the access keys for the ARM storage account  
$accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  

# Create a new storage account context using an ARM storage account  
$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].value 

# Creates a new container in blob storage  
$container = New-AzStorageContainer -Context $storageContext -Name $containerName  
$cbc = $container.CloudBlobContainer  

# Sets up a Stored Access Policy and a Shared Access Signature for the new container  
$policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
$sas = New-AzStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName
Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

# Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
Write-Host 'Credential T-SQL'  
$tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
$tSql | clip  
Write-Host $tSql  
```  

Nachdem Sie das Skript erfolgreich ausgeführt haben, kopieren Sie den Befehl `CREATE CREDENTIAL` in ein Abfragetool, stellen eine Verbindung mit einer SQL Server-Instanz her und führen den Befehl zum Erstellen der Anmeldeinformationen mit der Shared Access Signature aus. 

###  <a name="create-a-credential"></a><a name="credential"></a> Erstellen von Anmeldeinformationen

In den folgenden Beispielen werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für die Authentifizierung beim Azure Blob Storage-Dienst erstellt. Führen Sie einen der folgenden Schritte aus:
  
1. **Verwenden von Shared Access Signature**  

   Wenn Sie das oben genannte Skript zum Erstellen der Shared Access Signature ausgeführt haben, kopieren Sie die Anweisung `CREATE CREDENTIAL` in einen mit Ihrer SQL Server-Instanz verbundenen Abfrage-Editor, und führen Sie den Befehl aus. 

   Der folgende T-SQL-Befehl ist ein Beispiel, das die Anmeldeinformationen für die Verwendung einer Shared Access Signature erstellt. 

   ```sql  
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```  
  
2. **Verwenden der Identität und des Zugriffsschlüssels eines Speicherkontos**  
  
   ```sql 
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = '<mycredentialname>')  
   CREATE CREDENTIAL [<mycredentialname>] WITH IDENTITY = '<mystorageaccountname>'  
   ,SECRET = '<mystorageaccountaccesskey>';  
   ```  
  
### <a name="perform-a-full-database-backup"></a><a name="complete"></a> Ausführen einer vollständigen Datenbanksicherung  

In den folgenden Beispielen wird eine vollständige Datenbanksicherung der Datenbank „AdventureWorks2016“ im Microsoft Azure Blob Storage-Dienst durchgeführt. Führen Sie eines der folgenden Verfahren aus:
  
1. **Zur URL unter Verwendung von Shared Access Signature**  
  
   ```sql  
   BACKUP DATABASE AdventureWorks2016   
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak';  
   GO   
   ```  

1. **Zur URL unter Verwendung der Identität und des Zugriffsschlüssels des Speicherkontos**  
  
   ```sql
   BACKUP DATABASE AdventureWorks2016  
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
         WITH CREDENTIAL = '<mycredentialname>'   
        ,COMPRESSION  
        ,STATS = 5;  
   GO
   ```  

###  <a name="restoring-to-a-point-in-time-using-stopat"></a><a name="PITR"></a> Wiederherstellen eines bestimmten Zeitpunkts mithilfe von STOPAT  

Im folgenden Beispiel wird der Zustand der AdventureWorks2016-Beispieldatenbank zu einem bestimmten Zeitpunkt wiederhergestellt und ein Wiederherstellungsvorgang veranschaulicht.  
  
**Von URL unter Verwendung von Shared Access Signature**  
  
   ```sql
   RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.bak'   
   WITH MOVE 'AdventureWorks2016_data' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.mdf'  
   ,MOVE 'AdventureWorks2016_log' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.ldf'  
   ,NORECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO   
  
   RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
   WITH   
   RECOVERY   
   ,STOPAT = 'May 18, 2015 5:35 PM'   
   GO  
   ```  
  
## <a name="see-also"></a>Weitere Informationen  

- [SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)
- [Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Tutorial: Verwenden des Azure Blob Storage-Diensts mit SQL Server 2016-Datenbanken](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).
