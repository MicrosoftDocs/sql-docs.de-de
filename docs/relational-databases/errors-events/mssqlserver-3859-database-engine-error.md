---
description: MSSQLSERVER_3859
title: MSSQLSERVER_3859
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3859 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7fff2944c0fdb3fc6d80326841b9950d3591f12e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348801"
---
# <a name="mssqlserver_3859"></a>MSSQLSERVER_3859
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|3859|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|DBCC_CHECKCAT_DIRECT_UPDATE|
|Meldungstext|Warnung: Der Systemkatalog wurde direkt in der Datenbank mit der ID \%d aktualisiert, zuletzt am %S_DATE.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler deutet darauf hin, dass ein Benutzer Änderungen an Systemtabellen initiiert hat. Das manuelle Aktualisieren von Systemtabellen wird nicht unterstützt. Die Systemtabellen sollten nur von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine aktualisiert werden. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von Benutzern initiierte Änderungen an den Systemtabellen erkennt, wird in den folgenden beiden Szenarios Fehler 3859 ausgelöst:

- Szenario 1

    Ein Ereignis, das dem folgenden ähnelt, wird im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll oder im Anwendungsprotokoll in der Ereignisanzeige protokolliert, wenn Sie eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank starten, in der eine Systemtabelle enthalten ist, die manuell aktualisiert wurde:

    > Protokollname: Application  
    Quelle: MSSQLSERVER, Ereignis-ID: 3859  
    Taskkategorie: Server  
    Ebene: Information  
    Beschreibung: Warnung: Der Systemkatalog wurde direkt in der Datenbank mit der ID \%d aktualisiert, zuletzt am **date_time**.  

- Szenario 2  

    Die folgende Warnmeldung wird zurückgegeben, wenn Sie den Befehl `DBCC_CHECKDB` ausführen, nachdem eine Systemtabelle manuell aktualisiert wurde:

    > DBCC-Ergebnisse für **database_name**.  
    Meldung 8992, Ebene 16, Status 1, Zeile 1  
    Überprüfen Sie die Katalogmeldung 3859, Status 1: Warnung: Der Systemkatalog wurde direkt in der Datenbank mit der ID \%d aktualisiert, zuletzt am **date_time**.  
    Von CHECKDB wurden 0 Zuordnungsfehler und 0 Konsistenzfehler in der Datenbank **db_name** gefunden.  
    Die DBCC-Ausführung wurde abgeschlossen. Falls DBCC Fehlermeldungen ausgegeben hat, wenden Sie sich an den Systemadministrator.

## <a name="user-action"></a>Benutzeraktion

Sie können dieses Problem mit einer der folgenden Methoden beheben:

- Methode 1

    Wenn Sie über eine fehlerfreie Sicherung der Datenbank verfügen, stellen Sie die Datenbank aus dieser wieder her.  
    > [!NOTE]
    > Diese Methode funktioniert nur, wenn die Sicherung keine Inkonsistenzen in den Metadaten aufweist.  

- Methode 2  

    Wenn Sie die Datenbank nicht aus einer Sicherung wiederherstellen können, exportieren Sie die Daten und Objekte in eine neue Datenbank. Übertragen Sie dann den Inhalt der manuell aktualisierten Datenbank in die neue Datenbank. Hinweis: Sie können Inkonsistenzen in den Systemkatalogen nicht mithilfe der REPAIR-Optionen in den DBCC CHECKDB-Befehlen beheben. Da der Befehl keine Metadatenfehler beheben kann, weist er keine der empfohlenen Reparaturstufen auf.

    > [!NOTE]
    > Sie können die Daten in den Systemtabellen über die Systemkatalogsichten abrufen.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen finden Sie unter: [Systembasistabellen](../system-tables/system-base-tables.md).