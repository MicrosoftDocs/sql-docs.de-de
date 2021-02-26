---
title: Anzeigen von Informationen zum Daten- und Protokollspeicherplatz einer Datenbank
description: Hier erfahren Sie, wie Sie mithilfe von SQL Server Management Studio order Transact-SQL Informationen zum Daten- und Protokollspeicherplatz einer Datenbank in SQL Server anzeigen.
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 648e33d685e7c97a7e900fd752402ed135fb9ecb
ms.sourcegitcommit: c821c2bdc383a84e45bbdc95ff6fbabf4f54901c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560942"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>Anzeigen von Informationen zum Daten- und Protokollspeicherplatz einer Datenbank
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In diesem Thema wird beschrieben, wie die Informationen zum Daten- und Protokollspeicherplatz einer Datenbank in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]angezeigt werden.  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Berechtigung zum Ausführen von **sp_spaceused** wird der **public** -Rolle erteilt. Nur Mitglieder der festen Datenbankrolle **db_owner** können den Parameter **\@updateusage** angeben.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>So zeigen Sie Informationen zum Daten- und Protokollspeicherplatz einer Datenbank an  
  
1. Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2. Erweitern Sie **Datenbanken**.  
  
3. Klicken Sie mit der rechten Maustaste auf eine Datenbank, zeigen Sie auf **Berichte**, zeigen Sie auf **Standardberichte**, und klicken Sie dann auf **Datenträgerverwendung**.  

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL

#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>So zeigen Sie Informationen zum Daten- und Protokollspeicherplatz einer Datenbank mit sp_spaceused an
  
1. Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird die gespeicherte Systemprozedur [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) verwendet, um Speicherplatzinformationen für die gesamte Datenbanktabelle und ihre Indizes zu melden.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused;  
GO  
```  

#### <a name="to-display-data-space-used-by-object-and-allocation-unit-for-a-database"></a>So zeigen Sie den vom Objekt und der Zuordnungseinheit genutzten Datenspeicher für eine Datenbank an
  
1. Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel werden [Katalogsichten für Objekte](../system-catalog-views/object-catalog-views-transact-sql.md) abgefragt, um die Auslastung des Speicherplatzes auf dem Datenträger pro Tabelle und innerhalb jeder Tabelle pro [Zuordnungseinheit](../pages-and-extents-architecture-guide.md#IAM) zu melden.  
  
```sql  
SELECT
  t.object_id,
  OBJECT_NAME(t.object_id) ObjectName,
  sum(u.total_pages) * 8 Total_Reserved_kb,
  sum(u.used_pages) * 8 Used_Space_kb,
  u.type_desc,
  max(p.rows) RowsCount
FROM
  sys.allocation_units u
  join sys.partitions p on u.container_id = p.hobt_id
  join sys.tables t on p.object_id = t.object_id
GROUP BY
  t.object_id,
  OBJECT_NAME(t.object_id),
  u.type_desc
ORDER BY
  Used_Space_kb desc,
  ObjectName
```  

#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>So zeigen Sie Daten und Protokollspeicherplatzinformationen für eine Datenbank durch das Abfragen von sys.database_files an  
  
1. Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird die Katalogsicht [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) abgefragt, um bestimmte Informationen zu den Daten- und Protokolldateien in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank zurückzugeben.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen

 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [Hinzufügen von Daten- oder Protokolldateien zu einer Datenbank](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [Löschen von Daten- oder Protokolldateien aus einer Datenbank](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
