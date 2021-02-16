---
description: MSSQLSERVER_7105
title: MSSQLSERVER_7105
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7105 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: 2f44cf617fad17bb925b73b0f3dc9a3c676ed222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348713"
---
# <a name="mssqlserver_7105"></a>MSSQLSERVER_7105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|7105|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|TXT_PGNOTEXIST|
|Meldungstext|Die Datenbank-ID '%d', Seite '%S_PGID', Slot '%d' für den LOB-Datentypknoten ist nicht vorhanden. Dies ist gewöhnlich auf Transaktionen zurückzuführen, die Daten, für die kein Commit ausgeführt wurde, auf einer Datenseite lesen können. Führen Sie DBCC CHECKTABLE aus.|
||

## <a name="explanation"></a>Erklärung

Bei einer Abfrage wird möglicherweise die Meldung 7105 angezeigt, wenn auf LOB-Daten (Large Object), auf die in einer Zeile einer Datenbankseite verwiesen wird, nicht zugegriffen werden kann.

Da dieser Fehler einen Schweregrad von 22 aufweist, wird die Verbindung vom Server beendet. Diese Fehlermeldung wird auch mit EventID=7105 in die SQL-ERRORLOG-Datei und das Windows-Anwendungsereignisprotokoll geschrieben.

## <a name="possible-causes"></a>Mögliche Ursachen

Dieser Fehler kann aus einem der folgenden Gründe auftreten:

- Auf einer Datenbankseite oder innerhalb der LOB-Seitenstrukturen, auf die die Datenbankseite verweist, liegt ein Problem mit Datenbankbeschädigungen vor.
- Die Abfrage, bei der der Fehler auftritt, verwendet den Abfragehinweis `READ UNCOMMITTED ISOLATION LEVEL` oder `NOLOCK`.
- In der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Engine liegt ein Problem vor, das dazu führt, dass bei der Abfrage dieser Fehler auftritt.

Sehen Sie sich den Abschnitt zur Fehlerbehebung sowie den Abschnitt [Weitere Informationen](#more-information) an, um herauszufinden, was die Ursache für Ihr spezifisches Problem ist, und eine geeignete Lösung zu finden.

## <a name="user-action"></a>Benutzeraktion

1. Wie in der Meldung angegeben, sollten Sie in einem ersten Schritt `DBCC CHECKDB` für die Datenbank oder `DBCC CHECKTABLE` für die Tabelle ausführen, in der das Problem aufgetreten ist.

    - Die Datenbank-ID ist in der Meldung angegeben.
    - Wenn Sie die genaue betroffene Tabelle ermitteln möchten, ohne `DBCC CHECKDB` auszuführen, müssen Sie herausfinden, auf welche Tabellen die Abfrage zugegriffen hat, bei der der Fehler aufgetreten ist. Eine mögliche Methode ist eine Ablaufverfolgung für die Abfrage mit SQL Profiler. In [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] und [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] R2 können Sie die Abfrage jedoch möglicherweise mithilfe der system_health-Sitzung für erweiterte Ereignisse finden. Weitere Informationen zur Verwendung der system_health-Sitzung finden Sie unter dem folgenden Link: [Verwenden der system_health-Sitzung](../extended-events/use-the-system-health-session.md).

    - Wie bei jedem Konsistenzproblem bei Datenbanken können Sie diese Fehler beheben, indem Sie eine Wiederherstellung mit einer als fehlerfrei bekannten Sicherung durchführen, die dieses Problem nicht enthält.

    - Wenn Sie jedoch keine Wiederherstellung aus einer Sicherung durchführen können, befolgen Sie die Empfehlungen für `DBCC CHECKDB` oder `DBCC CHECKTABLE`, um diese Fehler zu beheben. Dies kann möglicherweise zu Datenverlust führen. Weitere Informationen zur Verwendung von CHECKDB und zu den Ursachen für Probleme mit Datenbankbeschädigungen finden Sie im folgenden Artikel: [Beheben von durch DBCC CHECKDB gemeldeten Fehlern der Datenbankkonsistenz](https://support.microsoft.com/kb/2015748).
  
1. Es ist möglich, dass dieser Fehler aufgetreten ist, weil bei der Abfrage, die auf die Tabelle zugreift, eine Isolationsstufe von `READ UNCOMMITTED` oder der Abfragehinweis `NOLOCK` (auch als „Dirty Read“ bezeichnet) verwendet wurde.

   - Wenn bei `DBCC CHECKDB` oder `DBCC CHECKTABLE` keine Fehler im Zusammenhang mit dieser Tabelle und den LOB-Daten angezeigt werden, ist die wahrscheinlichste Ursache die Verwendung eines Dirty Read. Wenn dies bei Ihrer Anwendung der Fall ist, müssen Sie entweder die Verwendung eines Dirty Read vermeiden oder die Abfrage erneut ausführen.
  
   - Wenn Sie feststellen, dass dies die Ursache für den Fehler ist, liegt bei der Datenbank kein tatsächliches Konsistenzproblem vor.

## <a name="more-information"></a>Weitere Informationen

Wenn eine Datenbankbeschädigung die Ursache für dieses Problem ist, sollten `DBCC CHECKDB` und/oder `DBCC CHECKTABLE` Fehler melden. Diese Befehle geben jedoch nicht die Meldung 7105 zurück. Die bei CHECKDB angezeigten Fehler hängen davon ab, was genau an dem Verweis auf LOB-Strukturen oder den LOB-Strukturen selbst beschädigt ist.

- Wenn die Zeile der Datenbankseite nicht ordnungsgemäß auf eine gültige LOB-Seite verweist, werden möglicherweise Fehler wie die folgenden angezeigt:

    > Meldung 8929, Ebene 16, Status 1, Zeile 1  
    Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039828480 (Daten-in-Zeilen-Typ): Es wurden Fehler in Daten mit der ID 131203072 außerhalb von Zeilen gefunden, die sich im Besitz des durch RID = (1:179:1) identifizierten Datensatzes befinden.  
    Meldung 8964, Ebene 16, Status 1, Zeile 1  
    Tabellenfehler: Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039894016 (LOB-Daten-Typ). Auf den Datenknoten außerhalb von Zeilen auf Seite (1:177), Slot 1, Text-ID 131203072 wird nicht verwiesen.  
    Meldung 8965, Ebene 16, Status 1, Zeile 1  
    Tabellenfehler: Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039894016 (LOB-Daten-Typ). Auf den Datenknoten außerhalb von Zeilen auf Seite (255:177), Slot 1, Text-ID 131203072 wird von Seite (1:179), Slot 1 verwiesen, er wurde jedoch im Scan nicht betrachtet.  

- Verschiedene Szenarios für das Problem können zu verschiedenen Fehlerkombinationen führen. In diesem Beispiel:  

    > Die Datenbankseite 1:179, Slot 1 verweist auf eine LOB-Seite, die keine gültige Seite in der Datenbank ist (Seite 255:177). Die Seite (1:177) ist eine gültige LOB-Seite, auf die jedoch nie von einer Datenbankseite verwiesen wurde. In diesem Fall besteht das Problem daher darin, dass die Zeile in Slot 1 auf Seite 1:179 auf Seite 255:177 statt 1:177 verweist.

- Wenn Sie herausfinden möchten, ob `DBCC CHECKDB`-Fehler mit LOB-Seitenproblemen zusammenhängen, ist es wichtig, nach den Begriffen „Daten außerhalb von Zeilen“ und „LOB-Daten-Typ“ zu suchen.

    > Die Meldung 8929 bezieht sich auf einen Fehler im Zusammenhang mit der Datenbankseite, die auf die LOB-Seiten verweist.  
Die Meldung 8964 bezieht sich auf einen Fehler, der angibt, dass keine Datenbankseiten auf eine bestimmte LOB-Seite verweisen.  
Die Meldung 8965 bezieht sich auf einen Fehler, der angibt, dass von einer Datenbankseite auf eine bestimmte LOB-Seite verwiesen wurde, diese jedoch nicht als gültige Seite vorhanden ist.

    In vielen Fällen, in denen diese Arten von Fehlern auftreten, führt die Reparatur dazu, dass die Zeilen, die auf LOB-Daten verweisen, sowie die LOB-Daten selbst gelöscht werden. Der Reparaturalgorithmus versucht, nur LOB-Fragmente zu entfernen, die sich auf die betreffenden Datenbankzeilen auswirken. Dies kann jedoch nicht in allen Fällen garantiert werden, je nachdem, was in der „LOB-Baumstruktur“ beschädigt ist.

- Im hier gezeigten Beispiel sehen die von CHECKTABLE mit `REPAIR_ALLOW_DATA_LOSS` zurückgegeben Meldungen wie folgt aus:

    > Reparaturvorgang: Der Datensatz für die Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039828480 (Daten-in-Zeilen-Typ) auf Seite (1:179), Slot 1 wurde gelöscht. Die Indizes werden neu erstellt.  
    Reparaturvorgang: Eine Spalte mit Daten außerhalb der Zeile mit der ID 131203072 wurde für Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039894016 (Typ LOB-Daten) für Seite (1:177), Slot 1 wurde gelöscht.  
    Meldung 8929, Ebene 16, Status 1, Zeile 1  
    Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039828480 (Daten-in-Zeilen-Typ): Es wurden Fehler in Daten mit der ID 131203072 außerhalb von Zeilen gefunden, die sich im Besitz des durch RID = (1:179:1) identifizierten Datensatzes befinden.  
            Der Fehler wurde behoben.  
    Meldung 8964, Ebene 16, Status 1, Zeile 1  
    Tabellenfehler: Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039894016 (LOB-Daten-Typ). Auf den Datenknoten außerhalb von Zeilen auf Seite (1:177), Slot 1, Text-ID 131203072 wird nicht verwiesen.  
            Der Fehler wurde behoben.  
    Meldung 8965, Ebene 16, Status 1, Zeile 1  
    Tabellenfehler: Objekt-ID 2137058649, Index-ID 0, Partitions-ID 72057594038910976, Zuordnungseinheits-ID 72057594039894016 (LOB-Daten-Typ). Auf den Datenknoten außerhalb von Zeilen auf Seite (255:177), Slot 1, Text-ID 131203072 wird von Seite (1:179), Slot 1 verwiesen, er wurde jedoch im Scan nicht betrachtet.  
            Dieser Fehler konnte nicht behoben werden.

    Die letzte Meldung, derzufolge der Fehler nicht behoben werden konnte, ist irreführend. Der Fehler wurde behoben, da die Datenbankseiten-Zeile, in der auf die ungültige Seite (255:177) verwiesen wurde, gelöscht wurde.