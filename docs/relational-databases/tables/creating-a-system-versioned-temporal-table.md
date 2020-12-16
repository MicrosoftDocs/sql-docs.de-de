---
description: Erstellen einer temporalen Tabelle mit Systemversionsverwaltung
title: Erstellen einer temporalen Tabelle mit Systemversionsverwaltung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 21e6d74f-711f-40e6-a8b7-85f832c5d4b3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b795f2fc65cade53dac533795d41ae8013e90cb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439391"
---
# <a name="creating-a-system-versioned-temporal-table"></a>Erstellen einer temporalen Tabelle mit Systemversionsverwaltung


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Es gibt drei Methoden zum Erstellen einer temporalen Tabelle mit Systemversionsverwaltung, die sich darin unterscheiden, wie die Verlaufstabelle angegeben wird:

- Temporale Tabelle mit einer anonymen Verlaufstabelle: Sie geben das Schema der aktuellen Tabelle an und lassen eine entsprechende Verlaufstabelle mit einem automatisch generierten Namen vom System erstellen.
- Temporale Tabelle mit einer Standardverlaufstabelle: Sie geben den Namen des Verlaufstabellenschemas und der Tabelle an und lassen vom System eine Verlaufstabelle in diesem Schema erstellen.
- Temporale Tabelle mit einer vorab erstellten, benutzerdefinierten Verlaufstabelle: Sie erstellen eine Verlaufstabelle, die Ihren Anforderungen am besten entspricht, und verweisen dann beim Erstellen der temporalen Tabelle auf diese Tabelle.

## <a name="creating-a-temporal-table-with-an-anonymous-history-table"></a>Erstellen einer temporalen Tabelle mit einer anonymen Verlaufstabelle

Das Erstellen einer temporalen Tabelle mit einer „anonymen“ Verlaufstabelle ist eine praktische Möglichkeit für das schnelle Erstellen von Objekten insbesondere in Prototyp- und Testumgebungen. Es ist auch die einfachste Möglichkeit, eine temporale Tabelle zu erstellen, da keine Parameter in der **SYSTEM_VERSIONING**-Klausel erforderlich sind. Im folgenden Beispiel wird eine neue Tabelle mit aktivierter Systemversionsverwaltung erstellt, ohne den Namen der Verlaufstabelle zu definieren.

```sql
CREATE TABLE Department
(
    DeptID INT NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

### <a name="important-remarks"></a>Wichtige Hinweise

- Für eine temporale Tabelle mit Systemversionsverwaltung muss ein Primärschlüssel und genau eine **PERIOD FOR SYSTEM_TIME** mit zwei „datetime2“-Spalten definiert sein, die als **GENERATED ALWAYS AS ROW START / END** deklariert sind.
- Die **PERIOD** -Spalten dürfen keine NULL-Werte zulassen, auch wenn keine Angabe zur NULL-Zulässigkeit gemacht wurde. Wenn für **PERIOD**-Spalten explizit angegeben ist, dass NULL-Werte zulässig sind, tritt bei der Anweisung **CREATE TABLE** ein Fehler auf.
- Das Schema der Verlaufstabelle muss im Hinblick auf Spaltenanzahl, Spaltennamen, Sortierung und Datentypen stets an die aktuelle oder temporale Tabelle angepasst sein.
- Eine anonyme Verlaufstabelle wird automatisch im gleichen Schema wie die aktuelle oder temporale Tabelle erstellt.
- Der Name der anonymen Verlaufstabelle weist das folgende Format auf: *MSSQL_TemporalHistoryFor_<current_temporal_table_object_id>_[suffix]* . Das Suffix ist optional und wird nur hinzugefügt, wenn der erste Teil des Tabellennamens nicht eindeutig ist.
- Die Verlaufstabelle wird als Rowstoretabelle erstellt. Sofern möglich, wird die PAGE-Komprimierung angewendet; andernfalls wird die Verlaufstabelle nicht komprimiert. Einige Tabellenkonfigurationen, z. B. SPARSE-Spalten, lassen beispielsweise keine Komprimierung zu.
- Für die Verlaufstabelle wird ein gruppierter Standardindex mit einem automatisch generierten Namen im Format *IX_<history_table_name>* erstellt. Der gruppierte Index enthält die **PERIOD** -Spalten (Ende, Anfang).
- Informationen zum Erstellen der aktuellen Tabelle als speicheroptimierte Tabelle finden Sie unter [Temporale Tabellen mit Systemversionsverwaltung und speicheroptimierten Tabellen](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md).

## <a name="creating-a-temporal-table-with-a-default-history-table"></a>Erstellen einer temporalen Tabelle mit einer Standardverlaufstabelle

Das Erstellen einer temporalen Tabelle mit einer Standardverlaufstabelle ist eine praktische Möglichkeit, wenn Sie die Benennung steuern möchten, die Verlaufstabelle aber trotzdem mit der Standardkonfiguration vom System erstellt werden soll. Im folgenden Beispiel wird eine neue Tabelle mit aktivierter Systemversionsverwaltung erstellt, wobei der Name der Verlaufstabelle explizit definiert wird.

```sql
CREATE TABLE Department
(
    DeptID INT NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory));
```

### <a name="important-remarks"></a>Wichtige Hinweise

Die Verlaufstabelle wird unter Anwendung der gleichen Regeln erstellt, die für das Erstellen einer „anonymen“ Verlaufstabelle gelten. Allerdings gelten die folgenden Regeln speziell für die benannte Verlaufstabelle.

- Der Schemaname ist für den Parameter **HISTORY_TABLE** obligatorisch.
- Wenn das angegebene Schema nicht vorhanden ist, tritt bei der Anweisung **CREATE TABLE** ein Fehler auf.
- Wenn die im Parameter **HISTORY_TABLE** angegebene Tabelle bereits vorhanden ist, wird sie mit der neu erstellten temporalen Tabelle auf [Schemakonsistenz und temporale Datenkonsistenz](./temporal-tables.md)verglichen. Wenn Sie eine ungültige Verlaufstabelle angeben, tritt bei der Anweisung **CREATE TABLE** ein Fehler auf.

## <a name="creating-a-temporal-table-with-a-user-defined-history-table"></a>Erstellen einer temporalen Tabelle mit einer benutzerdefinierten Verlaufstabelle

Das Erstellen einer temporalen Tabelle mit einer benutzerdefinierten Verlaufstabelle ist eine praktische Möglichkeit, wenn der Benutzer eine Verlaufstabelle mit bestimmten Speicheroptionen und zusätzlichen Indizes festlegen möchte. Im folgenden Beispiel wird eine benutzerdefinierte Verlaufstabelle mit einem Schema erstellt, das auf die zu erstellende temporale Tabelle abgestimmt ist. Für diese benutzerdefinierte Verlaufstabelle werden ein gruppierter Columnstore-Index und ein weiterer nicht gruppierter Rowstore-Index (B-Struktur) für Punktsuchen erstellt. Nach Erstellung der benutzerdefinierten Verlaufstabelle wird die temporale Tabelle mit Systemversionsverwaltung unter Angabe der benutzerdefinierten Verlaufstabelle als Standardverlaufstabelle erstellt.

```sql
CREATE TABLE DepartmentHistory
(
    DeptID INT NOT NULL
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 NOT NULL
  , SysEndTime DATETIME2 NOT NULL
);
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_DepartmentHistory
    ON DepartmentHistory;
CREATE NONCLUSTERED INDEX IX_DepartmentHistory_ID_PERIOD_COLUMNS
    ON DepartmentHistory (SysEndTime, SysStartTime, DeptID);
GO

CREATE TABLE Department
(
    DeptID int NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory));
```

### <a name="important-remarks"></a>Wichtige Hinweise

- Wenn Sie analytische Abfragen für die historischen Daten ausführen möchten, die Aggregate oder Fensterfunktionen einsetzen, ist es zum Zwecke der Leistung bei Komprimierung und Abfrage sehr zu empfehlen, einen gruppierten Columnstore als primären Index zu erstellen.
- Wenn der primäre Anwendungsfall die Datenüberwachung ist (d.h. wenn Sie Verlaufsänderungen für eine bestimmte Zeile in der aktuellen Tabelle suchen möchten), empfiehlt sich die Erstellung einer Rowstoreverlaufstabelle mit einem gruppierten Index.
- Die Verlaufstabelle kann keine Primärschlüssel, Fremdschlüssel, eindeutige Indizes, Tabelleneinschränkungen oder Trigger enthalten. Sie kann nicht zur Erfassung von Änderungsdaten, zur Änderungsnachverfolgung oder zur Transaktions- oder Mergereplikation konfiguriert werden.

## <a name="alter-non-temporal-table-to-be-a-system-versioned-temporal-table"></a>Ändern einer nicht temporalen Tabelle in eine temporale Tabelle mit Systemversionsverwaltung

Wenn Sie die Systemversionsverwaltung für eine vorhandene Tabelle aktivieren müssen, z. B., wenn Sie eine benutzerdefinierte temporale Lösung zu integrierter Unterstützung migrieren möchten.
Angenommen, Sie verfügen über eine Gruppe von Tabellen, bei denen die Versionsverwaltung mit Triggern implementiert ist. Die Verwendung temporaler Systemversionsverwaltung ist weniger komplex und bietet zusätzliche Vorteile, z. B.:

- Unveränderlichen Verlauf
- Neue Syntax für „Zeitreiseabfragen“
- Eine bessere DML-Leistung
- Minimale Wartungskosten

 Ziehen Sie beim Konvertieren einer vorhandenen Tabelle die Verwendung der Klausel **HIDDEN** in Betracht, um die neuen **PERIOD**-Spalten (die datetime2-Spalten **SysStartTime** und **SysEndTime**) auszublenden und so Auswirkungen auf vorhandene Anwendungen zu vermeiden, die Spaltennamen nicht explizit angeben (z. B. SELECT* oder INSERT ohne Spaltenliste) und keine neuen Spalten verarbeiten können.

### <a name="adding-versioning-to-non-temporal-tables"></a>Hinzufügen der Versionsverwaltung zu nicht temporalen Tabellen

Wenn Sie das Nachverfolgen von Änderungen für eine nicht temporale Tabelle mit den Daten starten möchten, müssen Sie die **PERIOD** -Definition hinzufügen und optional einen Namen für die leere Verlaufstabelle angeben, die SQL Server für Sie erstellt:

```sql
CREATE SCHEMA History;
GO

ALTER TABLE InsurancePolicy
    ADD
        SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN
            CONSTRAINT DF_SysStart DEFAULT SYSUTCDATETIME()
      , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN
            CONSTRAINT DF_SysEnd DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.9999999'),
        PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);
GO

ALTER TABLE InsurancePolicy
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.InsurancePolicy));
```

> [!IMPORTANT]
> Die Genauigkeit für DATETIME2 muss mit der Genauigkeit für die zugrunde liegende Tabelle übereinstimmen (siehe folgende Anmerkungen).

#### <a name="important-remarks"></a>Wichtige Hinweise

- Das Hinzufügen von Spalten, die keine NULL-Werte zulassen, mit Standardwerten zu einer vorhandenen Tabelle mit Daten ist in allen Editionen außer SQL Server Enterprise Edition ein Datengrößenvorgang (in SQL Server Enterprise Edition wäre es ein Metadatenvorgang). Bei einer vorhandenen großen Verlaufstabelle mit Daten in SQL Server Standard Edition kann das Hinzufügen einer Nicht-NULL-Spalte ein aufwendiger Vorgang sein.
- Einschränkungen für Spalten des Zeitraumstarts und -endes müssen sorgfältig gewählt werden:
  - Der Standardwert für die Startspalte gibt an, ab welchem Zeitpunkt vorhandene Zeilen als gültig betrachtet werden. Es kann kein Zeitpunkt in der Zukunft als datetime-Wert angegeben werden.
  - Die Endzeit muss als maximaler Wert für eine bestimmte datetime2-Genauigkeit angegeben werden, z. B. `9999-12-31 23:59:59` oder `9999-12-31 23:59:59.9999999`.
- Durch das Hinzufügen eines Zeitraums wird für die aktuelle Tabelle eine Datenkonsistenzprüfung durchgeführt, um sicherzustellen, dass die Standardwerte für Zeitraumspalten gültig sind.
- Wenn bei der Aktivierung von **SYSTEM_VERSIONING** eine vorhandene Verlaufstabelle angegeben wird, erfolgt eine Datenkonsistenzprüfung der aktuellen Tabelle und der Verlaufstabelle. Diese kann übersprungen werden, indem Sie **DATA_CONSISTENCY_CHECK = OFF** als zusätzlichen Parameter angeben.

### <a name="migrate-existing-tables-to-built-in-support"></a>Migrieren von vorhandenen Tabellen zu integrierter Unterstützung

Dieses Beispiel zeigt, wie eine vorhandene Lösung basierend auf Triggern zu integrierter temporaler Unterstützung migriert wird. In diesem Beispiel wird angenommen, dass in der aktuellen benutzerdefinierten Lösung die aktuellen und die historischen Daten auf zwei getrennte Benutzertabellen aufgeteilt sind (**ProjectTaskCurrent** und **ProjectTaskHistory**). Wenn Ihre vorhandene Lösung sowohl die aktuellen als auch die historischen Zeilen in einer einzigen Tabelle speichert, sollten Sie die Daten auf zwei Tabellen aufteilen, bevor Sie die in diesem Beispiel gezeigten Migrationsschritte ausführen:

```sql
/*Drop trigger on future temporal table*/
DROP TRIGGER ProjectCurrent_OnUpdateDelete;
/*Make sure that future period columns are non-nullable*/
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidFrom] datetime2 NOT NULL;
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidTo] datetime2 NOT NULL;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidFrom] datetime2 NOT NULL;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidTo] datetime2 NOT NULL;
ALTER TABLE ProjectTaskCurrent
    ADD PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])
ALTER TABLE ProjectTaskCurrent
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON));
```

#### <a name="important-remarks"></a>Wichtige Hinweise

- Durch das Verweisen auf vorhandene Spalten in der **PERIOD** -Definition wird „generated_always_type“ für diese Spalten implizit zu **AS_ROW_START** und **AS_ROW_END** geändert.
- Durch das Hinzufügen von **PERIOD** wird für die aktuelle Tabelle eine Datenkonsistenzprüfung durchgeführt, um sicherzustellen, dass die vorhandenen Werte für Zeitraumspalten gültig sind.
- Es wird dringend empfohlen, **SYSTEM_VERSIONING** mit **DATA_CONSISTENCY_CHECK = ON** festzulegen, um Datenkonsistenzprüfungen für die vorhandenen Daten zu erzwingen.
- Wenn ausgeblendete Spalten bevorzugt werden, verwenden Sie den Befehl `ALTER TABLE [tableName] ALTER COLUMN [columnName] ADD HIDDEN;`.

## <a name="next-steps"></a>Nächste Schritte

- [Temporale Tabellen](../../relational-databases/tables/temporal-tables.md)
 [Erste Schritte mit temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Verwalten der Beibehaltung von Verlaufsdaten in temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Temporale Tabellen mit Systemversionsverwaltung und speicheroptimierten Tabellen](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)
- [Ändern von Daten in einer temporalen Tabelle mit Systemversionsverwaltung](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [Abfragen von Daten in einer temporalen Tabelle mit Systemversionsverwaltung](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [Ändern vom Schema einer versionsverwalteten temporalen Tabelle](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
- [Beenden der Versionsverwaltung auf einer versionsverwalteten temporalen Tabelle](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)