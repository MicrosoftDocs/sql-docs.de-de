---
description: MSSQLSERVER_3023
title: MSSQLSERVER_3023
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3023 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 213e761e37355f9e38276de9fab07c934a2cdaf8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348815"
---
# <a name="mssqlserver_3023"></a>MSSQLSERVER_3023
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|3023|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|DB_IN_USE_DUMP|
|Meldungstext|Sicherungs- und Dateibearbeitungsvorgänge (z. B. ALTER DATABASE ADD FILE) in einer Datenbank müssen serialisiert werden. Wiederholen Sie die Anweisung nach Abschluss des aktuellen Sicherungs- oder Dateibearbeitungsvorgangs.|
||

## <a name="explanation"></a>Erklärung

Sie versuchen, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Befehl zum Sichern, Verkleinern oder Ändern einer Datenbank auszuführen, und es werden folgende Meldungen angezeigt:

> Meldung 3023, Ebene 16, Status 2, Zeile 1  
Sicherungs- und Dateibearbeitungsvorgänge (z. B. ALTER DATABASE ADD FILE) in einer Datenbank müssen serialisiert werden. Wiederholen Sie die Anweisung nach Abschluss des aktuellen Sicherungs- oder Dateibearbeitungsvorgangs.

> Meldung 3013, Ebene 16, Status 1, Zeile 1  
BACKUP DATABASE wird fehlerbedingt beendet.

Außerdem enthält das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll Meldungen wie die folgenden:

> \<Datetime> Sicherungsfehler: 3041, Schweregrad: 16, Status: 1.  
\<Datetime> BACKUP-Fehler beim Abschließen des BACKUP DATABASE MyDatabase WITH DIFFERENTIAL-Befehls. Ausführliche Informationen finden Sie im Sicherungsanwendungsprotokoll.

Sie werden möglicherweise auch bemerken, dass diese Befehle auf `wait_type = LCK_M_U` und `wait_resource = DATABASE: <id> [BULKOP_BACKUP_DB]` stoßen, wenn der Status dieser Befehle von den verschiedenen dynamischen Verwaltungssichten (DMVs) aus angezeigt wird, z. B. von `sys.dm_exec_requests` oder `sys.dm_os_waiting_tasks`.

## <a name="possible-causes"></a>Mögliche Ursachen

Es gibt mehrere Regeln zu den zulässigen und nicht zulässigen Datenbankvorgängen während eines über eine gesamte Datenbank ausgeführten Vorgangs. Einige Beispiele:

- Es kann immer nur eine Datensicherung durchgeführt werden (während einer vollständigen Datenbanksicherung können nicht gleichzeitig differenzielle oder inkrementelle Sicherungen durchgeführt werden).
- Es kann immer nur eine Protokollsicherung durchgeführt werden (eine Protokollsicherung ist während einer vollständigen Datenbanksicherung zulässig).
- Es ist nicht möglich, Dateien während einer Sicherung zu einer Datenbank hinzuzufügen oder daraus zu löschen.
- Dateien können während Datenbanksicherungen nicht verkleinert werden.
- Änderungen am Wiederherstellungsmodell sind während Sicherungsvorgängen nur eingeschränkt zulässig.

Wenn einer dieser in Konflikt stehenden Vorgänge ausgeführt wird, stoßen die Befehle auf die im Abschnitt „Erklärung“ genannten Sperrwartevorgänge, und Ihnen werden die Meldungen 3023 und 3041 angezeigt.

## <a name="user-action"></a>Benutzeraktion

Überprüfen Sie die Zeitpläne der verschiedenen Datenbank-Wartungsaktivitäten, und passen Sie die Zeitpläne so an, dass diese Vorgänge oder Befehle nicht miteinander in Konflikt stehen.

## <a name="more-information"></a>Weitere Informationen

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zeichnet die Startzeit und die Endzeit der Sicherung in der `msdb`-Datenbank auf. Sie können den Sicherungsverlauf überprüfen, um zu bestimmen, ob während des Versuchs einer inkrementellen Sicherung eine vollständige Datenbanksicherung durchgeführt wurde und zu dem Fehler führte. Sie können die folgende Abfrage als Unterstützung bei diesem Prozess verwenden:

```sql
select database_name, type, backup_start_date, backup_finish_date
from msdb.dbo.backupset
order by database_name, type, backup_start_date, backup_finish_date
go
```

Sie können auch das **User Error Message**-Ereignis in der SQL-Ablaufverfolgung oder das **error_reported**-Ereignis aus „Erweiterte Ereignisse“ verwenden, um die Berichterstattung der Meldung 3023 zu der Anwendung zurückzuverfolgen, von der die Sicherung oder andere Wartungsbefehle ausgingen.
