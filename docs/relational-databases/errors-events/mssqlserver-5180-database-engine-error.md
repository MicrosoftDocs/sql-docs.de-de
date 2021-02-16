---
description: MSSQLSERVER_5180
title: MSSQLSERVER_5180
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5180 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: 8ab3ce2256eadc0952c4ab42f91f74e71372c937
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348747"
---
# <a name="mssqlserver_5180"></a>MSSQLSERVER_5180
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|5180|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|DSK_BAD_FCB_FILEID|
|Meldungstext|FCB (File Control Bank) konnte für die ungültige Datei-ID %d in der '%.*ls'-Datenbank nicht geöffnet werden. Überprüfen Sie den Dateispeicherort. Führen Sie DBCC CHECKDB aus.|
||

## <a name="explanation"></a>Erklärung

Bei einer Abfrage oder einem Vorgang kann der Fehler 5180 auftreten, wenn die [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion_md.md)] auf eine ungültige Datei-ID verweist. Hier ein Beispiel:

> Meldung 5180, Ebene 22, Status 1, Zeile 1  
FCB (File Control Bank) konnte für die ungültige Datei-ID %d in der '%.*ls'-Datenbank nicht geöffnet werden. Überprüfen Sie den Dateispeicherort. Führen Sie DBCC CHECKDB aus.

Da für den Fehler der Schweregrad 22 angegeben ist, wird die Sitzung des Benutzers getrennt. Diese Fehlermeldung wird in das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll und das Windows-Anwendungsereignisprotokoll mit der EventID = 5180 geschrieben.

## <a name="possible-causes"></a>Mögliche Ursachen

Die [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] verweist in vielen verschiedenen Situationen auf eine Datei-ID, meistens beim Verweisen auf eine Seiten-ID (da die Datei-ID der erste Teil der Seiten-ID ist). Wenn die Datei-ID, auf die verwiesen wird, aus irgendeinem Grund < 0 oder keine gültige Datei-ID in einer Datenbank ist (nach den in Systemkatalogsichten aufgelisteten gültigen Datei-IDs wie „sys.database_files“), kann ein 5180-Fehler auftreten.

Eine mögliche Ursache ist, dass eine gespeicherte Datei-ID nicht gültig ist. Da der weitergeleitete Datensatz in einer Zeile auf eine andere Seite verweist, kann beim Zugriff auf diese Seite ein 5180-Fehler auftreten, wenn die Datei-ID ungültig ist. Die Bedingung ist ein Datenbankbeschädigungsfehler auf der Seite mit dem weitergeleiteten Datensatz. (In diesem Beispiel würde `DBCC CHECKDB` die Meldung 8993 ausgeben).

Ein weiteres Beispiel ist eine ungültige Datei-ID als Teil einer Seiten-ID, wie sie im Seitenheader für die Seiten-ID einer nächsten oder vorherigen Seite gespeichert ist. Dieses Feld wird verwendet, um eine Reihe von Seiten zu verknüpfen, wie z. B. in einem gruppierten Index. Wenn die Datei-ID für die vorherige oder nächste Seite ungültig ist, kann ein 5180-Fehler auftreten, wenn die Engine darauf verweisen muss, um zur nächsten oder vorherigen Seite zu gelangen. (In diesem Beispiel gibt `DBCC CHECKDB` die Meldung 8981 aus.)

Wenn das Problem nicht eine ungültige Datei-ID aufgrund einer beschädigten gespeicherten Seiten-ID ist, liegt das Problem möglicherweise bei der [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)].

## <a name="user-action"></a>Benutzeraktion

Wenn dieser Fehler auftritt, sollten Sie `DBCC CHECKDB` wie in der Fehlermeldung aufgeführt für die Datenbank ausführen. Wenn Fehler auftreten, sollten Sie die Daten aus einer als fehlerfrei bekannten Sicherung wiederherstellen. Wenn Sie die Daten aus einer Sicherung nicht wiederherstellen können, bleibt Ihnen als andere Möglichkeit die wie in der Ausgabe empfohlene Verwendung der Reparaturoption von `DBCC CHECKDB`. In den meisten Fällen führt die Reparatur eines solchen Fehlertyps zu einem Datenverlust. Weitere Informationen zur Verwendung und zu den Ursachen für Probleme mit Datenbankbeschädigungen finden Sie im folgenden Artikel: [Beheben von durch DBCC CHECKDB gemeldeten Fehlern der Datenbankkonsistenz](https://support.microsoft.com/kb/2015748)

Wenn `DBCC CHECKDB` keinen Fehler meldet und das Problem weiterhin auftritt, wenden Sie sich an den technischen Support von Microsoft, um Unterstützung zu erhalten. Halten Sie die Abfrage bereit, die den 5180-Fehler verursacht. Im Abschnitt [Weitere Informationen](#more-information) erfahren Sie, wie Sie herausfinden, welche Abfrage den Fehler verursacht hat.

## <a name="more-information"></a>Weitere Informationen

Der File Control Block (FCB) ist eine interne Speicherstruktur, die wichtige Informationen zu der Datei nachverfolgt, die der Datenbank zugeordnet ist. Eine Datei-ID wird verwendet, um einen FCB für eine Datenbank eindeutig zu identifizieren. Wenn die Server-Engine versucht, auf eine ungültige Datei-ID zu verweisen, kann die FCB-Struktur nicht gefunden werden, wodurch die 5180-Fehlerbedingung ausgelöst wird.

Wenn Sie herausfinden möchten, welche Abfrage diesen Fehler verursacht hat, gehen Sie wie folgt vor:

- Überprüfen Sie bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 und höheren Versionen, ob in der system_health-Sitzung eine Aufzeichnung des Fehlers vorhanden ist, die den Abfragetext einschließen sollte. Weitere Informationen zur system_health-Sitzung finden Sie unter folgendem Link: [Unterstützung für SQL Server 2008: Die system_health-Sitzung](https://techcommunity.microsoft.com/t5/sql-server-support/supporting-sql-server-2008-the-system-health-session/ba-p/315509)
- Verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler, und erfassen Sie die `SQL:BatchStarting`- und `RPC:Starting`-Ereignisse sowie Ausnahmeereignisse. Suchen Sie nach der Abfrage, die dem Ausnahmeereignis für 5180 für die Sitzung vorangestellt ist, die dem Ausnahmeereignis zugeordnet ist.
