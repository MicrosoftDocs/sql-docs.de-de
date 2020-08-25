---
description: Erstellen einer speicheroptimierten temporalen Tabelle mit Systemversionsverwaltung
title: Erstellen einer speicheroptimierten temporalen Tabelle mit Systemversionsverwaltung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 1c1fc682-bf5b-4096-a0ff-3235d71c205a
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83a7cdd151b958a7840696c09af12dcf2b64d319
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88645849"
---
# <a name="creating-a-memory-optimized-system-versioned-temporal-table"></a>Erstellen einer speicheroptimierten temporalen Tabelle mit Systemversionsverwaltung


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Wie eine datenträgerbasierte Verlaufstabelle können Sie auch eine speicheroptimierte temporale Tabelle auf verschiedene Weisen erstellen.

> [!NOTE]
> Um speicheroptimierte Tabellen zu erstellen, müssen Sie zuerst [die speicheroptimierte Dateigruppe](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)erstellen.

Das Erstellen einer temporalen Tabelle mit einer Standardverlaufstabelle ist eine praktische Möglichkeit, wenn Sie die Benennung steuern möchten, die Verlaufstabelle aber trotzdem mit der Standardkonfiguration vom System erstellt werden soll. Im nachfolgenden Beispiel wird eine neue speicheroptimierte temporale Tabelle mit Systemversionsverwaltung erstellt, die mit einer neuen datenträgerbasierten Verlaufstabelle verknüpft wird.

```sql
CREATE SCHEMA History
GO
CREATE TABLE dbo.Department
   (  
      DepartmentNumber char(10) NOT NULL PRIMARY KEY NONCLUSTERED,
      DepartmentName varchar(50) NOT NULL,
      ManagerID int NULL,
      ParentDepartmentNumber char(10) NULL,
      SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
      SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
      PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
   )
WITH
   (
       MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA,
          SYSTEM_VERSIONING = ON ( HISTORY_TABLE = History.DepartmentHistory )
    );
```

Das Erstellen einer temporalen, mit einer vorhandenen Verlaufstabelle verknüpfte Tabelle, ist praktisch, wenn Sie die Systemversionsverwaltung mittels einer vorhandenen Tabelle hinzufügen möchten, beispielsweise, wenn Sie eine benutzerdefinierte temporale Lösung auf die integrierte Unterstützung migrieren möchten. Im nachfolgenden Beispiel wird eine neue temporale Tabelle erstellt, die mit einer vorhandenen Verlaufstabelle verknüpft ist.

```sql
--Existing table
CREATE TABLE Department_History
   (
      DepartmentNumber char(10) NOT NULL,
      DepartmentName varchar(50) NOT NULL,
      ManagerID int NULL,
      ParentDepartmentNumber char(10) NULL,
      SysStartTime datetime2 NOT NULL, SysEndTime datetime2 NOT NULL
   )
;
--Temporal table
CREATE TABLE Department
   (
      DepartmentNumber char(10) NOT NULL PRIMARY KEY NONCLUSTERED,
      DepartmentName varchar(50) NOT NULL,
      ManagerID INT NULL,
      ParentDepartmentNumber char(10) NULL,
      SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
      SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
      PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
   )
WITH
   (
      SYSTEM_VERSIONING = ON
         (  
            HISTORY_TABLE = dbo.Department_History
            , DATA_CONSISTENCY_CHECK = ON
         )  
      , MEMORY_OPTIMIZED = ON
      , DURABILITY = SCHEMA_AND_DATA
   )
;
```

## <a name="see"></a>Siehe

- [Temporale Tabellen mit Systemversionsverwaltung und speicheroptimierten Tabellen](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Verwenden von speicheroptimierten temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)
- [Überwachung von speicheroptimierten temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [Überlegungen zur Systemleistung bei Verwendung von speicheroptimierten temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/memory-optimized-system-versioned-temporal-tables-performance.md)
- [Temporale Tabellen](../../relational-databases/tables/temporal-tables.md)
- [Systemkonsistenzprüfungen von temporalen Tabellen](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Verwalten der Beibehaltung von Verlaufsdaten in temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Metadatenansichten und Funktionen für temporale Tabellen](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
