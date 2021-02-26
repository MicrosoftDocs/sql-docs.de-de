---
title: SQL Server-Datendateien in Microsoft Azure | Microsoft-Dokumentation
description: In diesem Artikel werden zentrale Konzepte und Überlegungen zur Speicherung von SQL Server-Datendateien in Microsoft Azure Storage sowie einige der Vorteile von Azure Storage vorgestellt.
ms.custom: ''
ms.date: 12/04/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 38ffd9c2-18a5-43d2-b674-e425addec4e4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b59fc811c5bdfa122b306e9522bae97faa37e451
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636713"
---
# <a name="sql-server-data-files-in-microsoft-azure"></a>SQL Server-Datendateien in Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  ![Datendateien in Azure](../../relational-databases/databases/media/data-files-on-azure.png "Datendateien in Azure")  
  
Die SQL Server-Datendateien in Microsoft Azure ermöglichen die native Unterstützung von SQL Server-Datenbankdateien, die als Blobs gespeichert sind. Mit der Funktion können Sie eine Datenbank in SQL Server erstellen, die lokal oder auf einem virtuellen Computer in Microsoft Azure ausgeführt wird. Dabei wird ein dedizierter Speicherort für Ihre Daten in Microsoft Azure Blob Storage bereitgestellt. Dies vereinfacht auch das Verschieben von Datenbanken zwischen Computern. Sie können Datenbanken von einem Computer trennen und einem anderen Computer anfügen. Darüber hinaus bietet sie einen alternativen Speicherort für Datenbank-Sicherungsdateien, da Wiederherstellungen im oder aus dem Microsoft Azure Storage ermöglicht werden. Mit erweiterten Funktionen für das Virtualisieren und Verschieben von Daten sowie für Sicherheit und Verfügbarkeit unterstützt sie verschiedene Hybridlösungen und bietet zusätzlich kostengünstige, einfache Verwaltungsfunktionen für hohe Verfügbarkeit und flexible Skalierung.
 
> [!IMPORTANT]  
>  Das Speichern von Systemdatenbanken in Azure Blob Storage wird nicht empfohlen und nicht unterstützt. 

 In diesem Thema werden zentrale Konzepte und Überlegungen zur Speicherung von SQL Server-Datendateien im Microsoft Azure Storage Service eingeführt.  
  
 Ein praktisches Beispiel für die Verwendung dieses neuen Features finden Sie im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
## <a name="why-use-sql-server-data-files-in-microsoft-azure"></a>Gründe für die Verwendung von SQL Server-Datendateien in Microsoft Azure 

- **Vorteil – Einfache und schnelle Migration:** Diese Funktion vereinfacht den Migrationsprozess, indem jeweils eine Datenbank zwischen Computern in lokalen Umgebungen sowie zwischen lokalen und Cloudumgebungen verschoben wird, ohne dass Änderungen an der Anwendung vorgenommen werden müssen. Auf diese Weise wird eine inkrementelle Migration unterstützt, während Ihre vorhandene lokale Infrastruktur unverändert beibehalten wird. Darüber hinaus vereinfacht der Zugriff auf einen zentralen Datenspeicher die Anwendungslogik, wenn eine Anwendung in einer lokalen Umgebung an mehreren Stellen ausgeführt werden muss. Es kann vorkommen, dass Sie schnell Rechenzentren an geografisch verteilten Standorten einrichten müssen, in denen Daten aus vielen verschiedenen Quellen gesammelt werden. Mit dieser neuen Erweiterung müssen Daten nicht mehr zwischen Speicherorten verschoben werden. Stattdessen können Sie zahlreiche Datenbanken als Microsoft Azure-Blobs speichern und anschließend mithilfe von Transact-SQL-Skripts Datenbanken auf den lokalen oder virtuellen Computern erstellen.

- **Vorteil – Kostengünstig und unbegrenzter Speicher:** Mit dieser Funktion profitieren Sie von unbegrenztem Offsite-Speicher in Microsoft Azure, während Sie gleichzeitig die lokalen Serverressourcen nutzen. Wenn Sie Microsoft Azure als Datenspeicher verwenden, können Sie sich problemlos auf die Anwendungslogik konzentrieren, ohne sich um die aufwendige Hardwareverwaltung kümmern zu müssen. Wenn ein lokaler Serverknoten ausfällt, können Sie einen neuen Knoten einrichten, ohne Daten zu verschieben.

- **Vorteil – Hochverfügbarkeit und Notfallwiederherstellung:** Mithilfe von SQL Server-Datendateien in Microsoft Azure können Lösungen für Hochverfügbarkeit und Notfallwiederherstellung u. U. vereinfacht werden. Stürzt beispielsweise ein virtueller Computer in Microsoft Azure oder eine SQL Server-Instanz ab, können Sie die Datenbanken in einer neuen SQL Server-Instanz neu erstellen, indem Sie noch mal Verknüpfungen zu den Blobs herstellen.

- **Vorteil – Sicherheit:** Durch diese neue Erweiterung können Serverinstanzen von Speicherinstanzen getrennt behandelt werden. Beispielsweise können Sie über eine vollständig verschlüsselte Datenbank verfügen, deren Entschlüsselung ausschließlich auf einer Serverinstanz und nicht auf einer Speicherinstanz stattfindet. Dies bedeutet, dass Sie mit der neuen Erweiterung alle Daten in einer öffentlichen Cloud unter Verwendung von TDE-Zertifikaten (Transparent Data Encryption, transparente Datenverschlüsselung) verschlüsseln können, die physisch von den Daten getrennt sind. Die TDE-Schlüssel können in der Masterdatenbank gespeichert werden, die auf Ihrem physisch sicheren lokalen Computer gespeichert und gesichert wird. Mithilfe dieser lokalen Schlüssel können Sie die Daten verschlüsseln, die sich in Microsoft Azure Storage befinden. Falls Ihre Anmeldeinformationen für das Cloudspeicherkonto ausgespäht werden, sind die Daten trotzdem sicher, da die TDE-Zertifikate immer lokal gespeichert werden.

- **Momentaufnahmesicherung:**  Diese Funktion ermöglicht es Ihnen, Azure-Momentaufnahmen zu verwenden, um nahezu sofortige Sicherungen und schnellere Wiederherstellungen für Datenbankdateien mithilfe des Azure Blob-Storage-Diensts zu nutzen. Sicherungs- und Wiederherstellungsrichtlinien lassen sich dank dieser Funktion vereinfachen. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

## <a name="concepts-and-requirements"></a>Konzepte und Anforderungen  
  
Azure-Datenträger sind mit unternehmensweiten Lösungen für Geschäftskontinuität und Notfallwiederherstellung kompatibel. Wenn Sie Ihre Datenbanken direkt in Blobs oder Azure Premium-Dateien speichern, werden die Daten bezüglich Infrastruktur, Verwaltung und Überwachung nicht automatisch Ihrem virtuellen Computer zugeordnet.

Die Verwendung von Datenbankdateien in Seitenblobs ist komplexer als die Verwendung von Azure-Datenträgern, die einfach und benutzerfreundlich ist.

Grundsätzlich wird die Verwendung von Azure-Datenträgern empfohlen. Eine Ausnahme stellen Szenarios dar, in denen das Kopieren der Daten für Sicherungen oder das Wiederherstellen in einem datenintensiven Vorgang unbedingt vermieden werden sollen. Auch für Hochverfügbarkeit und Notfallwiederherstellungen ist die Verwendung der regulären Sicherung über URLs oder der verwalteten Sicherung in Blob Storage wesentlich nützlicher als Dateimomentaufnahme-Sicherungen, da Sie gleichzeitig Lebenszyklusverwaltung, Unterstützung für mehrere Regionen, vorläufiges Löschen sowie alle anderen Features von Blob Storage für Ihre Sicherungen erhalten.

### <a name="azure-storage-concepts"></a>Azure-Speicherkonzepte  
Bei Verwendung von SQL Server-Datendateien in Azure müssen Sie ein Speicherkonto und einen Container in Azure erstellen. Anschließend müssen Sie SQL Server-Anmeldeinformationen erstellen, die Informationen zur Containerrichtlinie sowie eine SAS (Shared Access Signature, Signatur für gemeinsamen Zugriff) enthalten, die für den Zugriff auf den Container erforderlich ist.  

In [Microsoft Azure](https://azure.microsoft.com) stellt ein [Azure-Speicherkonto](https://azure.microsoft.com/services/storage/) die höchste Namespaceebene für den Zugriff auf Blobs dar. Ein Speicherkonto kann eine unbegrenzte Anzahl von Containern enthalten. Allerdings darf deren Gesamtgröße das Speicherlimit nicht überschreiten. Aktuelle Informationen zu Speichergrößenbeschränkungen finden Sie unter [Azure-Abonnement und Dienstbeschränkungen, Kontingente und Einschränkungen](/azure/azure-subscription-service-limits). Ein Container stellt eine Gruppierung von mehreren [Blobs](/azure/storage/common/storage-introduction#blob-storage) bereit. Alle BLOBs müssen sich in einem Container befinden. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten. Analog dazu kann in einem Container eine unbegrenzte Anzahl von Blobs gespeichert werden. Es gibt zwei Arten von BLOBs, die im Azure-Speicher gespeichert werden können: Blockblobs und Seitenblobs. Diese neue Funktion verwendet Seitenblobs, die effizienter sind, wenn die Bytebereiche in einer Datei häufig geändert werden. Mit dem folgendem URL-Format können Sie auf Blobs zugreifen: `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  

### <a name="azure-billing-considerations"></a>Überlegungen zur Abrechnung in Azure  

 Die Schätzung der für die Nutzung der Azure-Dienste anfallenden Kosten ist ein wichtiger Aspekt des Entscheidungs- und Planungsprozesses. Wenn Sie SQL Server-Datendateien im Azure-Speicher speichern, fallen Kosten für die Speicherung und Transaktionen an. Zusätzlich erfordert die Implementierung von SQL Server-Datendateien in Azure Storage alle 45 bis 60 Sekunden eine implizite Erneuerung der Bloblease. Auf diese Weise entstehen ebenfalls Transaktionskosten pro Datenbankdatei, z. B. MDF- oder LDF-Datei. Die Informationen auf der [Azure-Preisseite](https://azure.microsoft.com/pricing/) sollen Ihnen dabei helfen, die monatlichen Kosten einzuschätzen, die mit der Nutzung von Azure Storage und Azure Virtual Machines verbunden sind.  
  
### <a name="sql-server-concepts"></a>Konzepte von SQL Server  

Die folgenden Voraussetzungen müssen zur Verwendung der neuen Erweiterung erfüllt sein:

- Erstellen einer Richtlinie für einen Container und zusätzliches Generieren eines SAS-Schlüssels (Shared Access Signature)

- Erstellen von SQL Server-Anmeldeinformationen für jeden Container, der von einer Daten- oder Protokolldatei verwendet wird (Name der Anmeldeinformationen muss mit dem Containerpfad übereinstimmen)

- Speichern der Informationen zum Azure Storage-Container, des zugehörigen Richtliniennamens und des SAS-Schlüssels im Anmeldeinformationsspeicher von SQL Server

Im folgenden Beispiel wird vorausgesetzt, dass ein Azure Storage-Container sowie eine Richtlinie mit Lese-, Schreib- und Auflistungsrechten erstellt wurden. Beim Erstellen einer Richtlinie für einen Container wird ein SAS-Schlüssel generiert, der gefahrlos unverschlüsselt im Arbeitsspeicher beibehalten werden kann und von SQL Server für den Zugriff auf die Blobdateien im Container benötigt wird. 

Ersetzen Sie im folgenden Codeausschnitt `'<your SAS key>'` durch den SAS-Schlüssel. Der SAS-Schlüssel sieht etwa folgendermaßen aus: `'sr=c&si=<MYPOLICYNAME>&sig=<THESHAREDACCESSSIGNATURE>'`.

```sql
CREATE CREDENTIAL [https://testdb.blob.core.windows.net/data]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '<your SAS key>'  
  
CREATE DATABASE testdb   
ON  
( NAME = testdb_dat,  
    FILENAME = 'https://testdb.blob.core.windows.net/data/TestData.mdf' )  
 LOG ON  
( NAME = testdb_log,  
    FILENAME =  'https://testdb.blob.core.windows.net/data/TestLog.ldf')  
```  

>[!IMPORTANT]
>Wenn ein Container aktive Verweise auf Datendateien enthält, schlagen Versuche, die entsprechenden SQL Server-Anmeldeinformationen zu löschen, fehl.

Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](/azure/storage/blobs/storage-manage-access-to-resources).  

### <a name="security"></a>Sicherheit  
 Die folgenden Sicherheitsüberlegungen und -anforderungen sollten beim Speichern von SQL Server-Datendateien im Azure-Speicher berücksichtigt werden.

- Beim Erstellen eines Containers für den Azure-BLOB-Speicherdienst sollten Sie den Zugriff auf „Privat“ festlegen. Wenn Sie den Zugriff auf „Privat“ festlegen, können die Container- und BLOB-Daten nur vom Besitzer des Azure-Kontos gelesen werden.

- Wenn Sie SQL Server-Datenbankdateien im Azure-Speicher speichern, müssen Sie eine SAS verwenden, d. h. einen URI, der eingeschränkte Rechte für den Zugriff auf Container, BLOBs, Warteschlangen und Tabellen gewährt. Durch die Verwendung einer SAS ermöglichen Sie SQL Server unter dem Speicherkonto den Zugriff auf Ressourcen, ohne Ihren Schlüssel für das Azure-Speicherkonto offen zu legen.

- Außerdem wird empfohlen, weiterhin die herkömmlichen Sicherheitsvorkehrungen für lokale Datenbanken zu treffen.  
  
### <a name="installation-prerequisites"></a>Voraussetzungen für die Installation  
 Die folgenden Installationsvoraussetzungen gelten beim Speichern von SQL Server-Datendateien in Azure.

- **Lokaler SQL Server:** Dieses Feature ist in SQL Server 2016 und späteren Versionen enthalten. Unter [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) finden Sie eine Anleitung zum Download der neuesten Version von SQL Server.

- SQL Server auf einem virtuellen Azure-Computer: Wenn Sie [SQL Server auf einem virtuellen Azure-Computer installieren](https://azuremarketplace.microsoft.com/marketplace/apps?search=sql%20server&page=1), installieren Sie SQL Server 2016, oder aktualisieren Sie Ihre vorhandene Instanz. Sie können auch einen neuen virtuellen Computer in Azure erstellen, indem Sie ein Plattformimage von SQL Server 2016 verwenden.

  
###  <a name="limitations"></a><a name="bkmk_Limitations"></a> Einschränkungen  
  
- Aufgrund der Leistungsmerkmale von SQL Server-Workloads werden SQL Server-Datendateien als Seitenblobs in Azure Blob Storage implementiert. Andere Blob Storage-Typen wie Blockblobs oder [Azure Data Lake Storage](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) werden nicht unterstützt.

- In der aktuellen Version dieser Funktion wird das Speichern von **FileStream** -Daten im Azure-Speicher nicht unterstützt. Sie können **FileStream**-Daten in einer Datenbank speichern, die auch in Azure Storage gespeicherte Datendateien enthält. Allerdings müssen alle FileStream-Datendateien im lokalen Speicher gespeichert werden.  Da sich FileStream-Daten im lokalen Speicher befinden müssen, können sie nicht mit Azure Storage zwischen Computern verschoben werden. Aus diesem Grund empfehlen wir, weiterhin [herkömmliche Techniken](../../relational-databases/blob/move-a-filestream-enabled-database.md) zum Verschieben von FileStream-Daten zwischen verschiedenen Computern zu verwenden.  
  
- Bei Verwendung der neuen Erweiterung kann derzeit nur eine SQL Server-Instanz (nicht mehrere) auf dieselben Datenbankdateien im Azure-Speicher zugreifen. Wenn ServerA mit einer aktiven Datenbankdatei online ist und ServerB, der auch über eine Datenbank verfügt, die auf dieselbe Datendatei verweist, versehentlich gestartet wird, kann die Datenbank des zweiten Servers nicht gestartet werden. Es wird der Fehlercode „**5120 Die physische Datei „%.\*ls“ kann nicht geöffnet werden.“ angezeigt. Betriebssystemfehler %d: „%ls“** .  
  
- Im Azure-Speicher können ausschließlich MDF-, LDF- und NDF-Dateien mithilfe des Features „SQL Server-Datendateien in Azure“ gespeichert werden.  
  
- Wenn Sie das Feature für SQL Server-Datendateien in Azure nutzen, wird die Georeplikation für Ihr Speicherkonto nicht unterstützt. Wenn für ein Speicherkonto eine Georeplikation ausgeführt wird und ein Geo-Failover auftritt, kann die Datenbank beschädigt werden.  
  
- Nähere Informationen zu Kapazitätsbeschränkungen finden Sie unter [Einführung in Blob Storage](/azure/storage/blobs/storage-blobs-introduction).  
  
- Es ist nicht möglich, In-Memory-OLTP-Daten mithilfe des Features „SQL Server-Datendateien“ in Azure Storage in einem Blobspeicher zu speichern. Das liegt daran, dass In-Memory-OLTP von **FileStream** abhängig ist und das Speichern von **FileStream** -Daten im Azure-Speicher von dem Feature in der derzeitigen Version nicht unterstützt wird.  
  
- Bei Verwendung des Features „SQL Server-Datendateien“ in Azure führt SQL Server alle URL- oder Dateipfadvergleiche mit der in der **Masterdatenbank** festgelegten Sortierung aus.  
  
- **Always On-Verfügbarkeitsgruppen** werden unterstützt, solange der primären Datenbank keine neuen Datenbankdateien hinzugefügt werden. Wenn für einen Datenbankvorgang in der primären Datenbank eine neue Datei erstellt werden muss, deaktivieren Sie zunächst die Verfügbarkeitsgruppen auf dem sekundären Knoten. Führen Sie anschließend den Datenbankvorgang in der primären Datenbank aus, und sichern Sie die Datenbank im primären Knoten. Stellen Sie anschließend die Datenbank auf dem sekundären Knoten wieder her. Aktivieren Sie daraufhin noch mal die Always On-Verfügbarkeitsgruppen auf dem sekundären Knoten. 

   >[!NOTE]
   >Always On-Failoverclusterinstanzen werden bei der Verwendung von SQL Server-Datendateien in Azure nicht unterstützt.
  
- Während des normalen Betriebs verwendet SQL Server temporäre Leases, um Blobs für die Speicherung zu reservieren, wobei jede Bloblease alle 45 bis 60 Sekunden erneuert wird. Stürzt ein Server ab und eine andere SQL Server-Instanz, die für die Verwendung derselben Blobs konfiguriert ist, wird gestartet, wartet die neue Instanz bis zu 60 Sekunden, bis die vorhandene Lease für das Blob abgelaufen ist. Wenn Sie die Datenbank einer anderen Instanz anfügen möchten und nicht 60 Sekunden bis zum Ablauf der Lease warten können, können Sie die Lease für den Blob explizit freigeben.
  
## <a name="tools-and-programming-reference-support"></a>Unterstützung von Tools und Programmierreferenzen  
 In diesem Abschnitt werden die Tools und Referenzbibliotheken für die Programmierung beschrieben, die beim Speichern von SQL Server-Datendateien im Azure-Speicher verwendet werden können.  
  
### <a name="powershell-support"></a>PowerShell-Unterstützung  
 Verwenden Sie zum Speichern von SQL Server-Datendateien im Blob Storage-Dienst PowerShell-Cmdlets. Dabei wird anstatt auf einen Dateipfad auf den URL-Pfad eines Blobspeichers verwiesen. Greifen Sie mit folgendem URL-Format auf Blobs zu: `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  
  
### <a name="sql-server-object-and-performance-counters-support"></a>Unterstützung von SQL Server-Objekten und -Leistungsindikatoren  
 In SQL Server 2014 wurde ein neues SQL Server-Objekt eingeführt, das mit SQL Server-Datendateien im Azure-Speicher verwendet werden kann. Das neue SQL Server-Objekt wird als [SQL Server, HTTP_STORAGE_OBJECT](../../relational-databases/performance-monitor/sql-server-http-storage-object.md) aufgerufen und kann vom Systemmonitor verwendet werden, um Aktivitäten bei der Ausführung von SQL Server mit Azure Storage zu überwachen.  
  
### <a name="sql-server-management-studio-support"></a>Unterstützung von SQL Server Management Studio  
 SQL Server Management Studio unterstützt die Verwendung der Funktion in mehreren Dialogfeldern. So stellt `https://teststorageaccnt.blob.core.windows.net/testcontainer/` beispielsweise den URL-Pfad eines Speichercontainers dar.
 
 als **Pfad** in verschiedenen Dialogfeldern ein, z. B. **Neue Datenbank**, **Datenbank anfügen** und **Datenbank wiederherstellen**. Weitere Informationen finden Sie im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="sql-server-management-objects-smo-support"></a>Unterstützung von SQL Server Management Objects (SMO)  
 Bei Verwendung von SQL Server-Datendateien in Azure werden alle SQL Server Management Objects (SMO) unterstützt. Wenn ein SMO-Objekt einen Dateipfad erfordert, verwenden Sie das BLOB-URL-Format anstelle eines lokalen Dateipfads, beispielsweise `https://teststorageaccnt.blob.core.windows.net/testcontainer/`. Weitere Informationen zu SQL Server Management Objects (SMO) finden Sie unter [SQL Server Management Objects &#40;SMO&#41;-Programmierungshandbuch](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) in der SQL Server-Onlinedokumentation.  
  
### <a name="transact-sql-support"></a>Unterstützung von Transact-SQL  
 Durch die neue Funktion wurde folgende Änderung in der -Oberfläche eingeführt:

- Die neue **int** -Spalte **credential_id** in der **sys.master_files** -Systemsicht. Die Spalte **credential_id** ermöglicht die Rückverweisung von Azure Storage-Datendateien auf `sys.credentials`, um die dafür erstellten Anmeldeinformationen abzurufen. Sie können die Spalte zur Problembehandlung verwenden, etwa wenn Anmeldeinformationen nicht gelöscht werden können, weil sie von einer Datenbankdatei verwendet werden.  
  
##  <a name="troubleshooting-for-sql-server-data-files-in-microsoft-azure"></a><a name="bkmk_Troubleshooting"></a> Problembehandlung für SQL Server-Datendateien in Microsoft Azure  
 Um Fehler aufgrund von nicht unterstützten Funktionen oder Einschränkungen zu vermeiden, sollten Sie sich zunächst unter [Einschränkungen](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md#bkmk_Limitations)informieren.  
  
 In der folgenden Liste sind Fehler aufgeführt, die bei Verwendung von SQL Server-Datendateien mit dem Azure-Speicher auftreten können.  
  
 **Authentifizierungsfehler**  
  
- *Die %.\*ls-Anmeldeinformationen können nicht gelöscht werden, weil sie von einer aktiven Datenbankdatei verwendet werden.*    
    Lösung: Dieser Fehler kann angezeigt werden, wenn Sie versuchen, Anmeldeinformationen zu löschen, die noch von einer aktiven Datenbankdatei im Azure-Speicher verwendet werden. Um die Anmeldeinformationen zu löschen, müssen Sie zuerst das zugeordnete BLOB löschen, das diese Datenbankdatei enthält. Zum Löschen eines Blobs, das über eine aktive Lease verfügt, müssen Sie zunächst die Lease freigeben.  
  
- *Für den Container wurde nicht ordnungsgemäß eine SAS (Shared Access Signature) erstellt.*    
     Lösung: Stellen Sie sicher, dass eine SAS ordnungsgemäß für den Container erstellt wurde. Lesen Sie die Hinweise in Lektion 2 im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature).  
  
- *SQL Server-Anmeldeinformationen wurden nicht ordnungsgemäß erstellt.*    
    Lösung: Vergewissern Sie sich, dass Sie für das Feld **Identität** die Option „Shared Access Signature“ verwendet und ordnungsgemäß einen geheimen Schlüssel erstellt haben. Lesen Sie die Hinweise in Lektion 3 im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#3---database-backup-to-url).  
  
 **Fehler bei BLOB-Leasedauer:**  
  
- Fehler beim Starten von SQL Server, nachdem eine andere Instanz, die dieselben BLOB-Dateien verwendet, abgestürzt ist. Lösung: Während des normalen Betriebs verwendet SQL Server temporäre Leases, um Blobs für die Speicherung zu reservieren, wobei jede Bloblease alle 45 bis 60 Sekunden erneuert wird. Stürzt ein Server ab und eine andere SQL Server-Instanz, die für die Verwendung derselben Blobs konfiguriert ist, wird gestartet, wartet die neue Instanz bis zu 60 Sekunden, bis die vorhandene Lease für das Blob abgelaufen ist. Wenn Sie die Datenbank einer anderen Instanz anfügen möchten und nicht 60 Sekunden bis zum Ablauf der Lease warten können, können Sie die Lease für den Blob explizit freigeben, um Fehler in Anfügevorgängen zu vermeiden.  
  
 **Datenbankfehler**  
  
**Fehler beim Erstellen einer Datenbank.** Lösung: Lesen Sie die Hinweise in Lektion 4 im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#4----restore-database-to-virtual-machine-from-url).  
  
**Fehler beim Ausführen der ALTER-Anweisung.** Lösung: Stellen Sie sicher, dass die ALTER DATABASE-Anweisung ausgeführt wird, während die Datenbank online ist. Wenn Sie die Datendateien in den Azure-Speicher kopieren, erstellen Sie immer ein Seitenblob und kein Blockblob. Andernfalls erzeugt ALTER DATABASE einen Fehler. Lesen Sie die Hinweise in Lektion 7 im [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
**Fehlercode 5120: Die physische Datei „%.\*ls“ kann nicht geöffnet werden. Betriebssystemfehler %d: „%ls“**   

Lösung: Bei Verwendung der neuen Erweiterung kann derzeit nur eine SQL Server-Instanz (nicht mehrere) auf dieselben Datenbankdateien im Azure-Speicher zugreifen. Wenn ServerA mit einer aktiven Datenbankdatei online ist und ServerB, der auch über eine Datenbank verfügt, die auf dieselbe Datendatei verweist, versehentlich gestartet wird, kann die Datenbank des zweiten Servers nicht gestartet werden. Es wird der Fehlercode „*5120 Die physische Datei „%.\*ls“ kann nicht geöffnet werden.“ angezeigt. Betriebssystemfehler %d: „%ls“* .  
  
Um dieses Problem zu beheben, stellen Sie zuerst fest, ob „ServerA“ auf die Datenbankdatei im Azure-Speicher zugreifen muss oder nicht. Ist dies nicht der Fall, entfernen Sie jegliche Verbindungen zwischen ServerA und den Datenbankdateien in Azure Storage. Gehen Sie dazu folgendermaßen vor:  

1.  Legen Sie den Dateipfad von Server A mit der ALTER DATABASE-Anweisung auf einen lokalen Ordner fest.  

2.  Schalten Sie die Datenbank auf Server A offline.  

3.  Kopieren Sie dann die Datenbankdateien aus Azure Storage im lokalen Ordner auf Server A. Dadurch wird sichergestellt, dass „ServerA“ noch lokal eine Kopie der Datenbank hat.  

4.  Schalten Sie die Datenbank online.

**Fehlercode 833: Bei E/A-Anforderungen wurden mehr als 15 Sekunden zum Abschließen des Vorgangs benötigt.** 
   
   Dieser Fehler weist darauf hin, dass das Speichersystem die Anforderungen der SQL Server-Workload nicht erfüllen kann. Senken Sie entweder die E/A-Aktivität auf der Anwendungsschicht, oder steigern Sie die Durchsatzkapazität auf der Speicherschicht. Weitere Informationen finden Sie unter [Fehler 833](../errors-events/mssqlserver-833-database-engine-error.md). Wenn die Leistungsprobleme weiterhin bestehen, sollten Sie in Betracht ziehen, die Dateien in eine andere Speicherebene zu verschieben, z. B. zu Premium oder SSD Ultra. Informationen zu SQL Server auf Azure-VMs finden Sie unter [Azure Storage Premium: Entwurf für hohe Leistung](/azure/virtual-machines/premium-storage-performance).


## <a name="next-steps"></a>Nächste Schritte  
  
[Erstellen einer Datenbank](create-a-database.md)
