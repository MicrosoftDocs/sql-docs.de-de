---
description: MSSQLSERVER_5009
title: MSSQLSERVER_5009
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5009 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 37a21597feb9372419915151147949539d321116
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348763"
---
# <a name="mssqlserver_5009"></a>MSSQLSERVER_5009
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|5009|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|ALT_BADDISKS|
|Meldungstext|One or more files listed in the statement could not be found or could not be initialized (Mindestens eine in der Anweisung aufgeführte Datei wurde nicht gefunden oder konnte nicht initialisiert werden)|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler deutet darauf hin, dass Sie einen Dateinamen oder eine Datei-ID im Befehl ALTER DATABASE oder DBCC SHRINK* angegeben haben, die nicht gefunden werden konnte.

Als Beispiel dient das folgende Szenario:

- Sie verfügen über eine Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank, die ein vollständiges oder massenprotokolliertes Wiederherstellungsmodell verwendet.
- Sie fügen der Datenbank eine neue Datendatei mit dem Namen *db_file1* hinzu.
- Sie legen den Dateityp für die Datei `db_file1` auf „data“ fest.
- Sie stellen fest, dass Sie den falschen Dateityp festgelegt haben.
- Sie entfernen die `db_file1`-Datei und sichern das Transaktionsprotokoll für diese Datenbank.
- Sie fügen derselben Datenbank eine neue Protokolldatei mit dem Namen *db_file1* hinzu.
- Sie versuchen, die Protokolldatei mit dem Namen *db_file1* mithilfe der Anweisung ALTER DATABASE oder mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio zu entfernen.

In diesem Szenario erhalten Sie eine Fehlermeldung, die wie folgt aussieht:

> Msg 5009, Level 16, State 9, Line 1 One or more files listed in the statement could not be found or could not be initialized. (Meldung 5009, Ebene 16, Status 9, Zeile 1: Mindestens eine in der Anweisung aufgeführte Datei wurde nicht gefunden oder konnte nicht initialisiert werden.)

## <a name="possible-causes"></a>Mögliche Ursachen

Dieses Problem tritt auf, wenn der logische Name der Datei, die Sie entfernen möchten, in den Systemkatalogtabellen nicht eindeutig ist. Dies ist beispielsweise dann der Fall, wenn die Datei erst in der Datenbank enthalten war, aber dann gelöscht wurde.

Wenn Sie versuchen, eine Datei mit demselben logischen Namen zu entfernen, versucht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die gelöschte logische Datei zu entfernen. Dadurch wird eine Fehlermeldung ausgelöst.

## <a name="user-action"></a>Benutzeraktion

Führen Sie die folgenden Schritte aus, um dieses Problem zu umgehen.

> [!NOTE]
> Wenn Sie wie nachfolgend beschrieben vorgehen, können die Werte der Datei-ID wiederverwendet werden.

1. Verwenden Sie die Anweisung ALTER DATABASE, um eine neue logische Datei mit einem anderen Namen, aber mit demselben Datentyp zu erstellen. Nennen Sie die logische Datei z. B. wie im folgenden Beispiel gezeigt `different_remove_file_name` anstelle von `db_file1`:

    ```sql
    ALTER DATABASE [DBNAME] ADD FILE ( NAME = N'different_remove_file_name',
    FILENAME = N'D:\MSSQL.1\MSSQL\DATA\db_file1.ndf', SIZE = 1MB, MAXSIZE = 1MB)
    ```

    > [!NOTE]
    > Sie können Dateinamen und -pfad beliebig festlegen.

1. Verwenden Sie die ALTER DATABASE-Anweisung, um die logische Datei, die Sie in Schritt 1 erstellt haben, wie im folgenden Beispiel zu entfernen:

    ```sql
    ALTER DATABASE [DBNAME] REMOVE FILE [different_remove_file_name]
    ```

1. Erstellen Sie eine Transaktionsprotokollsicherung der Datenbank.
1. Versuchen Sie noch einmal, die logische Datei mit dem Namen *db_file1* zu entfernen.
