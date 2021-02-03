---
description: TRUNCATE TABLE (Transact-SQL)
title: TRUNCATE TABLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRUNCATE
- TRUNCATE TABLE
- TRUNCATE_TSQL
- TRUNCATE_TABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server], TRUNCATE TABLE statement
- table truncating [SQL Server]
- removing rows
- TRUNCATE TABLE statement
- deleting rows
- dropping rows
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0da5fd480fcb5be450b1f45a9da63d0b0b7177f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233082"
---
# <a name="truncate-table-transact-sql"></a>TRUNCATE TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Entfernt alle Zeilen oder angegebenen Partitionen einer Tabelle, ohne die einzelnen Löschungen zu protokollieren. TRUNCATE TABLE entspricht DELETE ohne WHERE-Klausel. TRUNCATE TABLE ist jedoch schneller und verwendet weniger Systemressourcen und Ressourcen für die Transaktionsprotokollierung.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
TRUNCATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( PARTITIONS ( { <partition_number_expression> | <range> }   
    [ , ...n ] ) ) ]  
[ ; ]  
  
<range> ::=  
<partition_number_expression> TO <partition_number_expression>  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
TRUNCATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *database_name*  
 Der Name der Datenbank.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Tabelle gehört.  
  
 *table_name*  
 Der Name der Tabelle, die gekürzt werden soll oder aus der alle Zeilen entfernt werden. *table_name* muss ein Literal sein. *table_name* kann nicht die **OBJECT_ID()** -Funktion oder eine Variable sein.  
  
 WITH ( PARTITIONS ( { \<*partition_number_expression*> | \<*range*> } [ , ...n ] ) )    
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level))
  
 Gibt die Partitionen an, die abgeschnitten oder aus denen alle Zeilen entfernt werden sollen. Wenn die Tabelle nicht partitioniert ist, erzeugt das Argument `WITH PARTITIONS` einen Fehler. Wenn die `WITH PARTITIONS`-Klausel nicht bereitgestellt wird, wird die gesamte Tabelle abgeschnitten.  
  
 *\<partition_number_expression>* kann wie folgt angegeben werden: 
  
-   Geben Sie die Nummer einer Partition an, beispielsweise: `WITH (PARTITIONS (2))`  
  
-   Geben Sie die Partitionsnummern mehrerer einzelner Partitionen durch Kommas getrennt an, beispielsweise: `WITH (PARTITIONS (1, 5))`  
  
-   Geben Sie sowohl Bereiche als auch einzelne Partitionen an, beispielsweise: `WITH (PARTITIONS (2, 4, 6 TO 8))`  
  
-   Für *\<range>* können durch das Wort **TO** getrennte Partitionsnummern angegeben werden, z. B. `WITH (PARTITIONS (6 TO 8))`.  
  
 Um eine partitionierte Tabelle abzuschneiden, müssen die Tabelle und Indizes ausgerichtet werden (partitioniert auf dieselbe Partitionsfunktion).  
  
## <a name="remarks"></a>Bemerkungen  
 `TRUNCATE TABLE` bietet im Vergleich zur DELETE-Anweisung die folgenden Vorteile:  
  
-   Es wird weniger Speicherplatz für die Transaktionsprotokolle verwendet.  
  
     Die DELETE-Anweisung entfernt jede Zeile einzeln und protokolliert jede Löschung einzeln im Transaktionsprotokoll. Beim Entfernen der Daten mit `TRUNCATE TABLE` werden die zur Speicherung der Tabellendaten verwendeten Datenseiten freigegeben, und nur die Freigaben der Datenseiten werden im Transaktionsprotokoll aufgezeichnet.  
  
-   In der Regel werden weniger Sperren verwendet.  
  
     Wenn die DELETE-Anweisung mithilfe einer Zeilensperre ausgeführt wird, wird jede Zeile in der Tabelle zum Löschen gesperrt. Durch `TRUNCATE TABLE` werden immer die Tabelle (einschließlich einer Schemasperre (SCH-M)) und die Seite, aber nicht die einzelnen Zeilen gesperrt.  
  
-   Nullseiten verbleiben ausnahmslos in der Tabelle.  
  
     Nach dem Ausführen einer DELETE-Anweisung kann die Tabelle weiterhin leere Seiten enthalten. Die Zuordnung von leeren Seiten in einem Heap kann z. B. nur aufgehoben werden, wenn mindestens eine exklusive (LCK_M_X) Tabellensperre vorhanden ist. Wenn der Löschvorgang keine Tabellensperre verwendet, enthält die Tabelle (der Heap) viele leere Seiten. In Bezug auf Indizes können durch den Löschvorgang leere Seiten verbleiben, obwohl die Zuordnung dieser Seiten durch einen Cleanupprozess im Hintergrund schnell aufgehoben wird.  
  
 `TRUNCATE TABLE` entfernt alle Zeilen aus einer Tabelle, die Tabellenstruktur (Spalten, Einschränkungen, Indizes usw.) dagegen bleibt erhalten. Verwenden Sie die `DROP TABLE`-Anweisung, um neben den dazugehörigen Daten auch die Tabellendefinition zu entfernen.  
  
 Wenn die Tabelle eine Identitätsspalte enthält, wird der Zähler für diese Spalte auf den Ausgangswert zurückgesetzt, der für die Spalte definiert ist. Wenn kein Ausgangswert definiert wurde, wird der Standardwert 1 verwendet. Falls Sie den Wert des Identitätszählers erhalten möchten, verwenden Sie stattdessen DELETE.  
 
 > [!NOTE]
 > Für einen `TRUNCATE TABLE`-Vorgang kann ein Rollback ausgeführt werden.
  
## <a name="restrictions"></a>Beschränkungen  
 `TRUNCATE TABLE` kann nicht in Tabellen verwendet werden, für die Folgendes gilt:  
  
-   Auf die Tabelle wird mit einer FOREIGN KEY-Einschränkung verwiesen. Sie können eine Tabelle abschneiden, die über einen Fremdschlüssel verfügt, der auf sich selbst verweist. 
  
-   Die Tabelle ist an einer indizierten Sicht beteiligt.  
  
-   Die Veröffentlichung wird mithilfe einer Transaktionsreplikation oder Mergereplikation vorgenommen.  

-   Es handelt sich um eine temporale Tabelle.

-   Auf die Tabelle wird durch eine EDGE-Einschränkung verwiesen.  
  
 Verwenden Sie für Tabellen, die ein oder mehrere dieser Eigenschaften aufweisen, stattdessen die DELETE-Anweisung.  
  
 TRUNCATE TABLE kann keinen Trigger aktivieren, da der Vorgang keine einzelnen Zeilenlöschungen protokolliert. Weitere Informationen finden Sie unter [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md). 
 
 In [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[sspdw](../../includes/sspdw-md.md)]:

- `TRUNCATE TABLE` ist in der EXPLAIN-Anweisung nicht zulässig.

- `TRUNCATE TABLE` kann nicht innerhalb einer Transaktion ausgeführt werden.
  
## <a name="truncating-large-tables"></a>Abschneiden von großen Tabellen  
 In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Tabellen gelöscht oder abgeschnitten werden, die mehr als 128 Blöcke enthalten, ohne simultane Sperren für alle Blöcke aufrechtzuerhalten, die für den Löschvorgang erforderlich sind.  
  
## <a name="permissions"></a>Berechtigungen  
 Die mindestens erforderliche Berechtigung ist `ALTER` für *table_name*. `TRUNCATE TABLE`-Berechtigungen werden standardmäßig dem Tabellenbesitzer, den Mitgliedern der festen Serverrolle `sysadmin` sowie den Mitgliedern der festen Datenbankrollen `db_owner` und `db_ddladmin` erteilt. Diese Berechtigungen sind nicht übertragbar. Sie können jedoch die `TRUNCATE TABLE`-Anweisung innerhalb eines Moduls einbinden, z. B. eine gespeicherte Prozedur, und mit der `EXECUTE AS`-Klausel für das Modul die passenden Berechtigungen erteilen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-truncate-a-table"></a>A. Abschneiden einer Tabelle  
 Im folgenden Beispiel werden alle Daten aus der `JobCandidate`-Tabelle entfernt. `SELECT`-Anweisungen werden vor und nach der `TRUNCATE TABLE`-Anweisung eingeschlossen, um die Ergebnisse zu vergleichen.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT COUNT(*) AS BeforeTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
TRUNCATE TABLE HumanResources.JobCandidate;  
GO  
SELECT COUNT(*) AS AfterTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
```  
  
### <a name="b-truncate-table-partitions"></a>B. Tabellenpartition abschneiden  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level))
  
 Im folgende Beispiel werden die angegebenen Partitionen einer partitionierten Tabelle abgeschnitten. Mit der Syntax `WITH (PARTITIONS (2, 4, 6 TO 8))` werden die Partitionen 2, 4, 6, 7 und 8 abgeschnitten.  
  
```sql  
TRUNCATE TABLE PartitionTable1   
WITH (PARTITIONS (2, 4, 6 TO 8));  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [IDENTITY &#40;Property&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
