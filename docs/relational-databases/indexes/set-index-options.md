---
description: Festlegen von Indexoptionen
title: Festlegen von Indexoptionen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab6e8a4195ec78c3ae9593991d7c376464925cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353618"
---
# <a name="set-index-options"></a>Festlegen von Indexoptionen

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In diesem Thema wird beschrieben, wie die Eigenschaften eines Indexes in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]geändert werden.

 **In diesem Artikel**

- **Vorbereitungen:**

   [Einschränkungen](#Restrictions)

   [Security](#Security)

- **So ändern Sie die Eigenschaften eines Indexes mithilfe von:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen

- Die folgenden Optionen werden sofort auf den Index angewendet, indem die SET-Klausel in der ALTER INDEX-Anweisung verwendet wird: ALLOW_PAGE_LOCKS, ALLOW_ROW_LOCKS, OPTIMIZE_FOR_SEQUENTIAL_KEY, IGNORE_DUP_KEY und STATISTICS_NORECOMPUTE.
- Die folgenden Optionen können festgelegt werden, wenn Sie mit ALTER INDEX REBUILD oder CREATE INDEX WITH DROP_EXISTING einen Index neu erstellen: PAD_INDEX, FILLFACTOR, SORT_IN_TEMPDB, IGNORE_DUP_KEY, STATISTICS_NORECOMPUTE, ONLINE, ALLOW_ROW_LOCKS, ALLOW_PAGE_LOCKS, MAXDOP und DROP_EXISTING (nur CREATE INDEX).

### <a name="security"></a><a name="Security"></a> Sicherheit

#### <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

Erfordert die ALTER-Berechtigung in der Tabelle oder Sicht.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>So ändern Sie die Eigenschaften eines Indexes im Tabellen-Designer

1. Klicken Sie im Objekt-Explorer auf das Pluszeichen, um die Datenbank mit der Tabelle zu erweitern, in der Sie die Eigenschaften eines Indexes ändern möchten.
2. Klicken Sie auf das Pluszeichen, um den Ordner **Tabellen** zu erweitern.
3. Klicken Sie mit der rechten Maustaste auf die Tabelle, in der Sie die Eigenschaften eines Indexes ändern möchten, und wählen Sie **Entwurf** aus.
4. Klicken Sie im Menü **Tabellen-Designer** auf **Indizes/Schlüssel**.
5. Wählen Sie den Index aus, den Sie ändern möchten. Seine Eigenschaften werden im Hauptraster angezeigt.
6. Ändern Sie die Einstellungen beliebiger oder aller Eigenschaften, um den Index anzupassen.
7. Klicken Sie auf **Schließen**.
8. Klicken Sie im Menü **Datei** auf **Save**_Tabellenname_.

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>So ändern Sie die Eigenschaften eines Indexes in Objekt-Explorer

1. Klicken Sie im Objekt-Explorer auf das Pluszeichen, um die Datenbank mit der Tabelle zu erweitern, in der Sie die Eigenschaften eines Indexes ändern möchten.
2. Klicken Sie auf das Pluszeichen, um den Ordner **Tabellen** zu erweitern.
3. Klicken Sie auf das Pluszeichen, um die Tabelle zu erweitern, in der Sie die Eigenschaften eines Indexes ändern möchten.
4. Klicken Sie auf das Pluszeichen, um den Ordner **Indizes** zu erweitern.
5. Klicken Sie mit der rechten Maustaste auf den Index, dessen Eigenschaften Sie ändern möchten, und wählen Sie **Eigenschaften** aus.
6. Wählen Sie unter **Seite auswählen** die Option **Optionen** aus.
7. Ändern Sie die Einstellungen beliebiger oder aller Eigenschaften, um den Index anzupassen.
8. Zum Hinzufügen, Entfernen oder Ändern der Position einer Indexspalte wählen Sie auf der Seite **Allgemein** im Dialogfeld **Index Properties –** _Indexname_ (Indexeigenschaften – Indexname) aus. Weitere Informationen finden Sie unter [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>So sehen Sie die Eigenschaften aller Indizes in einer Tabelle

Das folgende Beispiel zeigt die Eigenschaften aller Indizes in einer Tabelle in der AdventureWorks-Datenbank.

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>So stellen Sie die Eigenschaften eines Indexes ein

Das folgende Beispiel legt die Eigenschaften von Indizes in der AdventureWorks-Datenbank fest.

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

Weitere Informationen finden Sie unter [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).
