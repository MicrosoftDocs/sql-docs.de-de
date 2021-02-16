---
description: Erstellen von Primärschlüsseln
title: Erstellen von Primärschlüsseln | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- primary keys [SQL Server], creating
ms.assetid: 85c623ca-4656-4d70-a9db-ee4d897cd214
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: debd09257826c625e6eee4d71a81cdb455683f3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344272"
---
# <a name="create-primary-keys"></a>Erstellen von Primärschlüsseln


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Sie können mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] einen Primärschlüssel in [!INCLUDE[tsql](../../includes/tsql-md.md)]definieren. Beim Erstellen eines Primärschlüssels wird je nach Spezifikation automatisch ein zugehöriger eindeutiger, gruppierter Index oder ein nicht gruppierter Index erstellt.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen

- Eine Tabelle kann nur eine PRIMARY KEY-Einschränkung enthalten.

- Alle Spalten, für die eine PRIMARY KEY-Einschränkung definiert wurde, müssen als NOT NULL definiert sein. Falls weder NULL noch NOT NULL angegeben ist, wird für alle Spalten, auf die eine PRIMARY KEY-Einschränkung angewendet wird, die NULL-Zulässigkeit auf NOT NULL festgelegt.

### <a name="security"></a><a name="Security"></a> Sicherheit

#### <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

Zum Erstellen einer neuen Tabelle mit einem primären Schlüssel sind die CREATE TABLE-Berechtigung für die Datenbank und die ALTER-Berechtigung für das Schema erforderlich, in dem die Tabelle erstellt wird.

Zum Erstellen eines Primärschlüssels für eine vorhandene Tabelle ist die ALTER-Berechtigung für die Tabelle erforderlich.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio

### <a name="to-create-a-primary-key"></a>So erstellen Sie einen Primärschlüssel

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Tabelle, der Sie eine UNIQUE-Einschränkung hinzufügen möchten, und klicken Sie auf **Entwerfen**.
2. Klicken Sie im **Tabellen-Designer** auf den Zeilenselektor für die Datenbankspalte, die Sie als Primärschlüssel definieren möchten. Wenn Sie mehrere Spalten auswählen möchten, halten Sie STRG gedrückt, und klicken Sie auf die Zeilenselektoren für die anderen Spalten.
3. Klicken Sie mit der rechten Maustaste auf den Zeilenselektor für die Spalte, und wählen Sie **Primärschlüssel festlegen** aus.

> [!CAUTION]
> Wenn Sie den Primärschlüssel neu definieren möchten, müssen Sie zunächst alle Beziehungen mit dem vorhandenen Primärschlüssel löschen, da erst dann der neue Primärschlüssel erstellt werden kann. In einer Warnmeldung werden Sie informiert, dass vorhandene Beziehungen bei diesem Prozess automatisch gelöscht werden.

Sie können eine Primärschlüsselspalte am Primärschlüsselsymbol erkennen, das im Zeilenselektor angezeigt wird.

Wenn ein Primärschlüssel aus mehreren Spalten besteht, können Werte in einer Spalte doppelt vorkommen. Die Kombination der Werte in allen Spalten des Primärschlüssels muss jedoch eindeutig sein.

Wenn Sie einen Verbundschlüssel definieren, stimmt die Spaltenreihenfolge im Primärschlüssel mit der Spaltenreihenfolge überein, die in der Tabelle angezeigt wird. Sie können die Spaltenreihenfolge jedoch nach Erstellen des Primärschlüssels ändern. Weitere Informationen finden Sie unter [Ändern von Primärschlüsseln](../../relational-databases/tables/modify-primary-keys.md).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL

### <a name="to-create-a-primary-key-in-an-existing-table"></a>So erstellen Sie einen Primärschlüssel in einer vorhandenen Tabelle

Im folgenden Beispiel wird ein Primärschlüssel für die Spalte `TransactionID` in der AdventureWorks-Datenbank erstellt.

```sql
ALTER TABLE Production.TransactionHistoryArchive
   ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID);
```

### <a name="to-create-a-primary-key-in-a-new-table"></a>So erstellen Sie einen Primärschlüssel in einer neuen Tabelle

Im folgenden Beispiel wird eine Tabelle erstellt, und es wird ein Primärschlüssel für die Spalte `TransactionID` in der AdventureWorks-Datenbank definiert.

```sql
CREATE TABLE Production.TransactionHistoryArchive1
   (
      TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
   )
;
```

### <a name="to-create-a-primary-key-with-clustered-index-in-a-new-table"></a>Erstellen eines Primärschlüssels mit einem gruppierten Index in einer neuen Tabelle

Im folgenden Beispiel wird eine Tabelle erstellt, und es wird ein Primärschlüssel für die Spalte `CustomerID` sowie ein gruppierter Index für `TransactionID` in der AdventureWorks-Datenbank definiert.

```sql
-- Create table to add the clustered index
CREATE TABLE Production.TransactionHistoryArchive1
   (
      CustomerID uniqueidentifier DEFAULT NEWSEQUENTIALID()
      , TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive1_CustomerID PRIMARY KEY NONCLUSTERED (CustomerID)
   )
;

-- Now add the clustered index
CREATE CLUSTERED INDEX CIX_TransactionID ON Production.TransactionHistoryArchive1 (TransactionID);
```

## <a name="see-also"></a>Siehe auch

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 
- [table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)
