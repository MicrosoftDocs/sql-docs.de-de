---
description: MSSQLSERVER_8992
title: MSSQLSERVER_8992 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8992 (Database Engine error)
ms.assetid: 68467e6a-09d8-478f-8bd9-3bb09453ada3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fa54a631a021dc07728490dde490a326901fa4b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194030"
---
# <a name="mssqlserver_8992"></a>MSSQLSERVER_8992
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
|Element|Wert|
|:---|:---|
|Produktname|SQL Server|  
|Ereignis-ID|8992|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DBCC3_CHECK_CATALOG|  
|Meldungstext|Meldung ERROR zum Prüfen des Katalogs, Ebene LEVEL, Status STATE: MESSAGE.|  

> [!NOTE]
> Die Fehlermeldung 8992 verweist auf eine spezifische andere Fehlermeldung (zwischen 3851 und 3858) zur tatsächlichen Inkonsistenz.

## <a name="explanation"></a>Erklärung  
DBCC CHECKCATALOG oder DBCC CHECKDB hat in den Systemmetadatentabellen eine Inkonsistenz für das angegebene Objekt festgestellt. Es handelt sich um eine Inkonsistenz zwischen der aufgezeichneten Objekt-ID und dem in der Fehlermeldung angegebenen Objekt.  
  
Dieser Fehler kann auftreten, wenn eine oder mehrere Systemtabellen manuell auf eine Weise aktualisiert wurden, die in den Systemmetadaten eine Inkonsistenz verursacht. Zum Beispiel hat ein Benutzer möglicherweise manuell ein Objekt aus der Tabelle **sysobjects** gelöscht, ohne die zugeordneten Zeilen in anderen Tabellen, z.B. **sysindexes** und **syscolumns**, zu entfernen.  
  
Dieser Fehler kann beim Ausführen von DBCC CHECKDB für eine Datenbank auftreten, die von SQL Server 2000 auf SQL Server 2005 oder höher aktualisiert wurde. In SQL Server 2000 enthält DBCC CHECKDB die DBCC CHECKCATALOG-Funktion nicht. Deshalb wird der Fehler nicht vor dem Upgrade abgefangen, es sei denn, DBCC CHECKCATALOG wurde ausdrücklich für die Datenbank in SQL Server 2000 ausgeführt.  
  
In Verbindung mit Fehler 8992 werden möglicherweise folgende Fehler angezeigt:  

|Meldungs-ID|Fehlertext|
|:---|:---|
|3851|Eine ungültige Zeile (%ls) wurde in der sys.%ls%ls-Systemtabelle gefunden.|
|3852|Für die Zeile (%ls) in sys.%ls%ls ist keine entsprechende Zeile (%ls) in sys.%ls%ls vorhanden.|
|3853|Für das Attribut (%ls) der Zeile (%ls) in sys.%ls%ls ist keine entsprechende Zeile (%ls) in sys.%ls%ls vorhanden.|
|3854|Für das Attribut (%ls) der Zeile (%ls) in sys.%ls%ls ist eine entsprechende Zeile (%ls) in sys.%ls%ls vorhanden, sie ist jedoch ungültig.|
|3855|Das Attribut (%ls) ist ohne eine Zeile (%ls) in sys.%ls%ls vorhanden.|
|3856|Das Attribut (%ls) ist fälschlicherweise für die Zeile (%ls) in sys.%ls%ls vorhanden.|
|3857|Das Attribut (%ls) ist für die Zeile (%ls) in sys.%ls%ls erforderlich, fehlt jedoch.|
|3858|Das Attribut (%ls) der Zeile (%ls) in sys.%ls%ls weist einen ungültigen Wert auf.|

## <a name="user-action"></a>Benutzeraktion  
  
### <a name="drop-and-re-create-the-specified-object"></a>Löschen und Neuerstellen des angegebenen Objekts  
Wenn möglich, löschen Sie das angegebene Objekt, und erstellen Sie es erneut. Wenn das Objekt z. B. eine gespeicherte Prozedur oder ein benutzerdefinierter Typ ist, behebt die Neuerstellung des Objekts möglicherweise das Problem.  
  
### <a name="restore-from-backup"></a>Sicherungswiederherstellung  
Stellen Sie die Datenbank aus der Sicherung wieder her, wenn das Problem nicht hardwarebezogen ist und eine bekannte intakte Sicherungskopie vorhanden ist. Diese Aktion ist nur anwendbar, wenn die Sicherung den Metadatenfehler nicht enthält.  
  
### <a name="export-the-data-to-a-new-database"></a>Exportieren der Daten in eine neue Datenbank  
Wenn die Metadateninkonsistenz auch in der Sicherung enthalten ist, müssen Sie eine neue Datenbank erstellen und den Inhalt der vorhandenen Datenbank in die neue Datenbank exportieren.  
  
### <a name="dbcc-checkdb-cannot-repair-this-error"></a>Dieser Fehler kann durch DBCC CHECKDB nicht repariert werden  
Dieser Fehler kann nicht repariert werden.  Wenn Sie die Datenbank nicht mithilfe einer Sicherung wiederherstellen können, wenden Sie sich an den Kundenservice und -support von [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
### <a name="do-not-manually-update-system-tables"></a>Systemtabellen dürfen nicht manuell aktualisiert werden  

Nehmen Sie keine manuellen Updates an den Systemtabellen vor. SQL Server unterstützt keine manuellen Änderungen an den Systemdatenbanken. Wenn Sie eine Systemtabelle in SQL Server-Datenbank aktualisieren, werden die folgenden Ereignisse protokolliert:

#### <a name="when-a-system-table-is-manually-updated"></a>Wenn eine Systemtabelle manuell aktualisiert wird

Fehlermeldung 17659: Warnung: Die Systemtabelle mit der ID <id> in der Datenbank mit der ID <id> wurde direkt aktualisiert. Die Cachekohärenz ist möglicherweise verloren gegangen. SQL Server sollte neu gestartet werden.

#### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Beim Starten einer Datenbank mit einer manuell aktualisierten Systemtabelle

Fehlermeldung 3859: Warnung: Der Systemkatalog wurde direkt in der Datenbank mit der ID <id> aktualisiert, zuletzt um date_time.

#### <a name="when-you-execute-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Beim Ausführen des Befehls DBCC_CHECKDB, nachdem eine Systemtabelle manuell aktualisiert wurde

Fehlermeldung 3859: Warnung: Der Systemkatalog wurde direkt in der Datenbank mit der ID <id> aktualisiert, zuletzt um date_time.  

## <a name="see-also"></a>Weitere Informationen

[Systembasistabellen](../system-tables/system-base-tables.md)
  
