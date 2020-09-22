---
description: CREATE PARTITION SCHEME (Transact-SQL)
title: CREATE PARTITION SCHEME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE PARTITION SCHEME
- SCHEME
- PARTITION SCHEME
- CREATE_PARTITION_SCHEME_TSQL
- SCHEME_TSQL
- PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- partitioned indexes [SQL Server], schemes
- partitioned tables [SQL Server], schemes
- CREATE PARTITION SCHEME statement
- partition schemes [SQL Server], creating
- filegroups [SQL Server], partitions
- partitioned indexes [SQL Server], filegroups
- partitioned tables [SQL Server], filegroups
- mapping partitions [SQL Server]
ms.assetid: 5b21c53a-b4f4-4988-89a2-801f512126e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2985351da1e1b0f1c0215df95c3e61440773e9c3
ms.sourcegitcommit: ac9feb0b10847b369b77f3c03f8200c86ee4f4e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90688451"
---
# <a name="create-partition-scheme-transact-sql"></a>CREATE PARTITION SCHEME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Erstellt ein Schema in der aktuellen Datenbank, das die Partitionen einer partitionierten Tabelle oder eines partitionierten Indexes Dateigruppen zuordnet. Die Anzahl und die Domäne der Partitionen einer partitionierten Tabelle oder eines partitionierten Indexes werden in einer Partitionsfunktion bestimmt. Eine Partitionsfunktion muss zunächst in einer [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)-Anweisung erstellt werden, bevor ein Partitionsschema erstellt wird.  

>[!NOTE]
>In Azure SQL-Datenbank werden nur primäre Dateigruppen unterstützt.  

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE PARTITION SCHEME partition_scheme_name  
AS PARTITION partition_function_name  
[ ALL ] TO ( { file_group_name | [ PRIMARY ] } [ ,...n ] )  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *partition_scheme_name*  
 Der Name des Partitionsschemas. Partitionsschemanamen müssen innerhalb der Datenbank eindeutig sein und den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.  
  
 *partition_function_name*  
 Der Name der Partitionsfunktion, die das Partitionsschema verwendet. Von der Partitionsfunktion erstellte Partitionen werden den im Partitionsschema angegebenen Dateigruppen zugeordnet. *partition_function_name* muss in der Datenbank bereits vorhanden sein. Eine einzelne Partition kann nicht sowohl FILESTREAM- als auch nicht-FILESTREAM-Dateigruppen enthalten.  
  
 ALL  
 Legt fest, dass alle Partitionen der in *file_group_name* angegebenen Dateigruppe oder der primären Dateigruppe zugeordnet werden, falls **[** PRIMARY **]** angegeben ist. Wenn ALL angegeben ist, kann nur ein einzelner *file_group_name*-Wert angegeben werden.  
  
 *file_group_name* |  **[** PRIMARY **]** [ **,** _...n_]  
 Gibt die Namen der Dateigruppen an, in denen die durch *partition_function_name* angegebenen Partitionen gespeichert werden. *file_group_name* muss bereits in der Datenbank vorhanden sein.  
  
 Wenn **[** PRIMARY **]** angegeben ist, wird die Partition in der primären Dateigruppe gespeichert. Wenn ALL angegeben ist, kann nur ein einzelner *file_group_name*-Wert angegeben werden. Partitionen werden, beginnend mit der Partition 1, Dateigruppen in der Reihenfolge zugewiesen, in der die Dateigruppen in [ **,** _...n_] aufgelistet sind. Derselbe *file_group_name*-Wert kann mehrmals in [ **,** _...n_] angegeben werden. Falls *n* zum Speichern der in *partition_function_name* angegebenen Anzahl von Partitionen nicht ausreichend ist, wird für CREATE PARTITION SCHEME ein Fehler gemeldet.  
  
 Falls *partition_function_name* weniger Partitionen als Dateigruppen generiert, wird die erste nicht zugewiesene Dateigruppe als NEXT USED markiert, und eine Informationsmeldung mit der NEXT USED-Dateigruppe wird angezeigt. Falls ALL angegeben ist, behält der einzige *file_group_name*-Wert seine NEXT USED-Eigenschaft für diesen *partition_function_name*-Wert bei. Die NEXT USED-Dateigruppe erhält eine zusätzliche Partition, falls eine solche in einer ALTER PARTITION FUNCTION-Anweisung erstellt wird. Verwenden Sie ALTER PARTITION SCHEME, um zusätzliche nicht zugewiesene Dateigruppen zum Speichern neuer Partitionen zu erstellen.  
  
 Wenn Sie die primäre Dateigruppe in *file_group_name* [ 1 **,** _...n_] angeben, muss PRIMARY durch Trennzeichen begrenzt sein, wie z.B. in **[** PRIMARY **]** , da es sich um ein Schlüsselwort handelt.  
  
 Nur PRIMARY wird für [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] unterstützt. Weitere Informationen finden Sie unten im Beispiel E. 
  
## <a name="permissions"></a>Berechtigungen  
 Die folgenden Berechtigungen können für CREATE PARTITION SCHEME verwendet werden:  
  
-   ALTER ANY DATASPACE-Berechtigung. Diese Berechtigung gilt standardmäßig für Mitglieder der festen Serverrolle **sysadmin** und für Mitglieder der festen Datenbankrollen **db_owner** und **db_ddladmin** .  
  
-   CONTROL- oder ALTER-Berechtigung für die Datenbank, in der das Partitionsschema erstellt wird.  
  
-   CONTROL SERVER- oder ALTER ANY DATABASE-Berechtigung für den Server der Datenbank, in der das Partitionsschema erstellt wird.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-partition-scheme-that-maps-each-partition-to-a-different-filegroup"></a>A. Erstellen eines Partitionsschemas, das jede Partition einer anderen Dateigruppe zuordnet  
 Im folgenden Beispiel wird eine Partitionsfunktion zum Partitionieren einer Tabelle oder eines Indexes in vier Partitionen erstellt. Es wird ein Partitionsschema erstellt, das die Dateigruppen zum Speichern der vier Partitionen angibt. In diesem Beispiel wird davon ausgegangen, dass die Dateigruppen bereits in der Datenbank vorhanden sind.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
TO (test1fg, test2fg, test3fg, test4fg);  
```  
  
 Die Partitionen einer Tabelle, die die `myRangePF1`-Partitionsfunktion zum Partitionieren der **col1**-Spalte verwendet, werden wie in der folgenden Tabelle zugewiesen:  
  
||||||  
|-|-|-|-|-|  
|**Dateigruppe**|`test1fg`|`test2fg`|`test3fg`|`test4fg`|  
|**Partition**|1|2|3|4|  
|**Werte**|**col1** <= `1`|**col1** > `1` AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="b-creating-a-partition-scheme-that-maps-multiple-partitions-to-the-same-filegroup"></a>B. Erstellen eines Partitionsschemas, mit dem mehrere Partitionen derselben Dateigruppe zugeordnet werden  
 Verwenden Sie das ALL-Schlüsselwort, wenn alle Partitionen derselben Dateigruppe zugeordnet werden. Falls aber mehrere, jedoch nicht alle Partitionen derselben Dateigruppe zugeordnet werden, muss der Dateigruppenname wiederholt werden, wie im folgenden Beispiel gezeigt.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS2  
AS PARTITION myRangePF2  
TO ( test1fg, test1fg, test1fg, test2fg );  
```  
  
 Die Partitionen einer Tabelle, die die `myRangePF2`-Partitionsfunktion zum Partitionieren der **col1**-Spalte verwendet, werden wie in der folgenden Tabelle zugewiesen:  
  
||||||  
|-|-|-|-|-|  
|**Dateigruppe**|`test1fg`|`test1fg`|`test1fg`|`test2fg`|  
|**Partition**|1|2|3|4|  
|**Werte**|**col1** <= `1`|**col1** > 1 UND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="c-creating-a-partition-scheme-that-maps-all-partitions-to-the-same-filegroup"></a>C. Erstellen eines Partitionsschemas, mit dem alle Partitionen derselben Dateigruppe zugeordnet werden  
 Im folgenden Beispiel wird dieselbe Partitionsfunktion wie in den vorherigen Beispielen erstellt, und ein Partitionsschema wird erstellt, mit dem alle Partitionen derselben Dateigruppe zugeordnet werden.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS3  
AS PARTITION myRangePF3  
ALL TO ( test1fg );  
```  
  
### <a name="d-creating-a-partition-scheme-that-specifies-a-next-used-filegroup"></a>D: Erstellen eines Partitionsschemas, das die Dateigruppe 'NEXT USED' angibt  
 Im folgenden Beispiel wird dieselbe Partitionsfunktion wie in den vorherigen Beispielen erstellt, und ein Partitionsschema wird erstellt, das mehr Dateigruppen auflistet, als Partitionen von der zugehörigen Partitionsfunktionen erstellt werden.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF4 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS4  
AS PARTITION myRangePF4  
TO (test1fg, test2fg, test3fg, test4fg, test5fg)  
```  
  
 Beim Ausführen der Anweisung wird die folgende Meldung zurückgegeben.  
  
Partition scheme 'myRangePS4' has been created successfully. 'test5fg' is marked as the next used filegroup in partition scheme 'myRangePS4'. (Das Partitionsschema „myRangePS4“ wurde erfolgreich erstellt. „test5fg“wird als nächste Dateigruppe im Partitionsschema „myRangePS4“ gekennzeichnet.)  
  
  
 Falls die `myRangePF4`-Partitionsfunktion geändert wird, um eine Partition hinzuzufügen, erhält die `test5fg`-Dateigruppe die neu erstellte Partition.  

### <a name="e-creating-a-partition-schema-only-on-primary---only-primary-is-supported-for-sqldbesa"></a>E. Erstellen eines Partitionsschemas, das ausschließlich für PRIMARY verwendet wird (für [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] wird nur PRIMARY unterstützt)

 Im folgenden Beispiel wird eine Partitionsfunktion zum Partitionieren einer Tabelle oder eines Indexes in vier Partitionen erstellt. Anschließend wird ein Partitionsschema erstellt, das festlegt, dass alle Partitionen in der PRIMARY-Dateigruppe erstellt werden.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
ALL TO ( [PRIMARY] );  
```
   
## <a name="see-also"></a>Weitere Informationen  
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Erstellen partitionierter Tabellen und Indizes](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
