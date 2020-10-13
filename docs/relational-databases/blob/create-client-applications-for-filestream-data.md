---
title: Erstellen von Clientanwendungen für FILESTREAM-Daten | Microsoft Dokumentation
description: Hier erfahren Sie, wie Sie Win32-APIs zum Erstellen von Clientanwendungen verwenden, die auf FILESTREAM-Daten zugreifen. Hier finden Sie die verfügbaren Funktionen, die erforderlichen Schritte, Beispiele und bewährte Methoden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], Win32
ms.assetid: 8a02aff6-e54c-40c6-a066-2083e9b090aa
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5de9ab4b9a0e89197173dcee5f0e6f7ffb1b7e75
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809942"
---
# <a name="create-client-applications-for-filestream-data"></a>Erstellen von Clientanwendungen für FILESTREAM-Daten
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Sie können Win32 APIs zum Lesen und Schreiben von Daten in einem FILESTREAM BLOB verwenden. Hierfür sind die folgenden Schritte erforderlich:  
  
-   Lesen des FILESTREAM-Dateipfads.  
  
-   Lesen des aktuellen Transaktionskontexts.  
  
-   Abrufen eines Win32-Handles und Lesen und Schreiben von Daten im FILESTREAM BLOB mit diesem Handle.  
  
> [!NOTE]  
>  Für die Beispiele in diesem Thema sind die FILESTREAM-aktivierte Datenbank und die Tabelle erforderlich, die unter [Erstellen einer FILESTREAM-aktivierten Datenbank](../../relational-databases/blob/create-a-filestream-enabled-database.md) und [Erstellen einer Tabelle zum Speichern von FILESTREAM-Daten](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md)erstellt werden.  
  
##  <a name="functions-for-working-with-filestream-data"></a><a name="func"></a> Funktionen zum Arbeiten mit FILESTREAM-Daten  
 Wenn Sie FILESTREAM zum Speichern von Binary Large Object (BLOB)-Daten verwenden, können Sie die Dateien mit Win32-APIs bearbeiten. Zum Arbeiten mit FILESTREAM BLOB-Daten in Win32-Anwendungen stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die folgenden Funktionen und die folgende API bereit:  
  
-   [PathName](../../relational-databases/system-functions/pathname-transact-sql.md) gibt einen Pfad als Token an ein BLOB zurück. Eine Anwendung verwendet dieses Token, um ein Win32-Handle zu erhalten und mit BLOB-Daten zu arbeiten.  
  
     Wenn die Datenbank, die FILESTREAM-Daten enthält, zu einer AlwaysOn-Verfügbarkeitsgruppe gehört, dann gibt die PathName-Funktion den Namen eines virtuellen Netzwerks (VNN) statt eines Computernamens zurück.  
  
-   [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md) gibt ein Token zurück, das die aktuelle Transaktion einer Sitzung darstellt. Anwendungen verwenden dieses Token, um FILESTREAM-Dateisystem-Streamingvorgänge an die Transaktion zu binden.  
  
-   Die [OpenSqlFilestream-API](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) bezieht ein Win32-Dateihandle. Die Anwendung verwendet das Handle zum Streamen der FILESTREAM-Daten und kann das Handle dann an die folgenden Win32-APIs übergeben: [ReadFile](https://go.microsoft.com/fwlink/?LinkId=86422), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [TransmitFile](/windows/win32/api/mswsock/nf-mswsock-transmitfile), [SetFilePointer](/windows/win32/api/fileapi/nf-fileapi-setfilepointer), [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile) oder [FlushFileBuffers](/windows/win32/api/fileapi/nf-fileapi-flushfilebuffers). Wenn die Anwendung mit dem Handle irgendeine andere API anruft, wird ein ERROR_ACCESS_DENIED-Fehler zurückgegeben. Die Anwendung sollte das Handle mit [CloseHandle](/windows/win32/api/handleapi/nf-handleapi-closehandle)schließen.  
  
 Alle Zugriffe auf FILESTREAM-Datencontainer müssen in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Transaktion erfolgen. [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen können auch in der gleichen Transaktion ausgeführt werden, damit die SQL-Daten mit den FILESTREAM-Daten konsistent bleiben.  
  
##  <a name="steps-for-accessing-filestream-data"></a><a name="steps"></a> Schritte zum Zugreifen auf FILESTREAM-Daten  
  
###  <a name="reading-the-filestream-file-path"></a><a name="path"></a> Lesen des FILESTREAM-Dateipfads  
 Jeder Zelle in einer FILESTREAM-Tabelle ist ein Dateipfad zugeordnet. Um den Pfad zu lesen, verwenden Sie die **PathName** -Eigenschaft einer **varbinary(max)** -Spalte in einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung. Das folgende Beispiel zeigt, wie der Dateipfad einer **varbinary(max)** -Spalte gelesen wird.  
  
 [!code-sql[FILESTREAM#FS_PathName](../../relational-databases/blob/codesnippet/tsql/create-client-applicatio_1.sql)]  
  
###  <a name="reading-the-transaction-context"></a><a name="trx"></a> Lesen des Transaktionskontexts  
 Verwenden Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md), um den aktuellen Transaktionskontext abzurufen. Das folgende Beispiel zeigt, wie eine Transaktion gestartet und der aktuelle Transaktionskontext gelesen wird.  
  
 [!code-sql[FILESTREAM#FS_GET_TRANSACTION_CONTEXT](../../relational-databases/blob/codesnippet/tsql/create-client-applicatio_2.sql)]  
  
###  <a name="obtaining-a-win32-file-handle"></a><a name="handle"></a> Abrufen eines Win32-Dateihandles  
 Um ein Win32-Dateihandle abzurufen, rufen Sie die OpenSqlFilestream-API auf. Diese API wird aus der Datei sqlncli.dll exportiert. Das zurückgegebene Handle kann an eine der folgenden Win32-APIs übergeben werden: [ReadFile](https://go.microsoft.com/fwlink/?LinkId=86422), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [TransmitFile](/windows/win32/api/mswsock/nf-mswsock-transmitfile), [SetFilePointer](/windows/win32/api/fileapi/nf-fileapi-setfilepointer), [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile) oder [FlushFileBuffers](/windows/win32/api/fileapi/nf-fileapi-flushfilebuffers). Die folgenden Beispiele veranschaulichen, wie Sie ein Win32-Handle abrufen und zum Lesen und Schreiben von FILESTREAM BLOB-Daten verwenden.  
  
 [!code-cs[FILESTREAM#FS_CS_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/csharp/create-client-applicatio_3.cs)]  
  
 [!code-vb[FILESTREAM#FS_VB_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/visualbasic/create-client-applicatio_4.vb)]  
  
 [!code-cpp[FILESTREAM#FS_CPP_WriteBLOB](../../relational-databases/blob/codesnippet/cpp/create-client-applicatio_5.cpp)]  
  
##  <a name="best-practices-for-application-design-and-implementation"></a><a name="best"></a> Bewährte Methoden für Anwendungsentwurf und Implementierung  
  
-   Wenn Sie Anwendungen entwerfen und implementieren, die FILESTREAM verwenden, beachten Sie die folgenden Richtlinien:  
  
-   Verwenden Sie NULL statt 0x, um eine nicht initialisierte FILESTREAM-Spalte darzustellen. Der 0x-Wert bewirkt, dass eine Datei erstellt wird. Bei NULL ist dies nicht der Fall.  
  
-   Vermeiden Sie Einfüge- und Löschvorgänge in Tabellen, die FILESTREAM-Spalten ungleich NULL enthalten. Bei Einfüge- und Löschvorgängen können die FILESTREAM-Tabellen geändert werden, die für Garbage Collection verwendet werden. Dies kann dazu führen, dass die Leistung einer Anwendung mit der Zeit nachlässt.  
  
-   Verwenden Sie in Anwendungen, die Replikation verwenden, NEWSEQUENTIALID() statt NEWID(). NEWSEQUENTIALID() erzielt in diesen Anwendungen bei der GUID-Generierung eine bessere Leistung als NEWID().  
  
-   Die FILESTREAM-API ist für Win32-Streamingzugriff auf Daten vorgesehen. Vermeiden Sie die Verwendung von [!INCLUDE[tsql](../../includes/tsql-md.md)] zum Lesen oder Schreiben von FILESTREAM-BLOBs (Binary Large Objects), die größer als 2 MB sind. Wenn Sie BLOB-Daten aus [!INCLUDE[tsql](../../includes/tsql-md.md)]lesen oder schreiben müssen, stellen Sie sicher, dass alle BLOB-Daten verarbeitet werden, bevor Sie versuchen, den FILESTREAM-BLOB über Win32 zu öffnen. Werden nicht alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Daten verarbeitet, schlagen möglicherweise alle folgenden FILESTREAM-Vorgänge zum Öffnen oder Schließen fehl.  
  
-   Vermeiden Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die FILESTREAM-BLOBs aktualisieren oder diesen Daten anfügen bzw. voranstellen. Dies bewirkt, dass die BLOB-Daten in die Datenbank tempdb und anschließend zurück in eine neue physische Datei gespoolt werden.  
  
-   Vermeiden Sie, kleine BLOB-Updates an ein FILESTREAM-BLOB anzufügen. Jeder Anfügevorgang bewirkt, dass die zugrunde liegenden FILESTREAM-Dateien kopiert werden. Wenn eine Anwendung kleine BLOBs anfügen muss, schreiben Sie die BLOBs in eine **varbinary(max)** -Spalte, und führen Sie dann einen einzelnen Schreibvorgang für den FILESTREAM-BLOB durch, wenn die Anzahl der BLOBs eine vorher festgelegte Beschränkung erreicht.  
  
-   Vermeiden Sie, die Datenlänge von vielen BLOB-Dateien in einer Anwendung abzurufen. Dies ist ein zeitaufwendiger Vorgang, da die Größe nicht in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]gespeichert wird. Wenn Sie die Länge einer BLOB-Datei festlegen müssen, verwenden Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)] -DATALENGTH()-Funktion, um die Größe des BLOB zu bestimmen, wenn dieses geschlossen ist. DATALENGTH() öffnet die BLOB-Datei nicht, um deren Größe zu bestimmen.  
  
-   Wenn eine Anwendung das SMB (Server Message Block)-Protokoll verwendet, sollten FILESTREAM-BLOB-Daten in einem Vielfachen von 60 KB gelesen werden, um die Leistung zu optimieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vermeiden von Konflikten mit Datenbankvorgängen in FILESTREAM-Anwendungen](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)   
 [ZUgreifen auf FILESTREAM-Daten mit OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)   
 [Binary Large Object &#40;Blob&#41; Daten &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [Vornehmen von Teilupdates an FILESTREAM-Daten](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)  
  
