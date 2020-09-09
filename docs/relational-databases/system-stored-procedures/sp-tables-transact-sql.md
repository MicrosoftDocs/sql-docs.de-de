---
description: sp_tables (Transact-SQL)
title: sp_tables (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 03e8a9ddaa10ad154f26abc8baf5e005209e1494
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89547622"
---
# <a name="sp_tables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Liste von Objekten zurück, die in der aktuellen Umgebung abgefragt werden können. Dies bedeutet jede Tabelle oder Sicht, mit Ausnahme von Synonymobjekten.  
  
> [!NOTE]  
>  Um den Namen des Basis Objekts eines Synonyms zu ermitteln, Fragen Sie die [sys. Synonyms](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) -Katalog Sicht ab.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Argumente  
`[ @table_name = ] 'name'` Die Tabelle, die zum Zurückgeben von Katalog Informationen verwendet wird. *Name ist vom Datentyp* **nvarchar (384)** und hat den Standardwert NULL. Mustervergleiche mit Platzhalterzeichen werden unterstützt.  
  
`[ @table_owner = ] 'owner'` Der Tabellen Besitzer der Tabelle, mit der Katalog Informationen zurückgegeben werden. *Owner* ist vom Datentyp **nvarchar (384)** und hat den Standardwert NULL. Mustervergleiche mit Platzhalterzeichen werden unterstützt. Wenn der Besitzer nicht angegeben ist, werden die Standardregeln für die Sichtbarkeit von Tabellen des zugrunde liegenden DBMS angewendet.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden die Spalten einer Tabelle zurückgegeben, wenn der aktuelle Benutzer diese Tabelle mit dem angegebenen Namen besitzt. Falls der Besitzer nicht angegeben ist und der aktuelle Benutzer keine Tabelle mit dem angegebenen Namen besitzt, wird nach einer Tabelle mit dem angegebenen Namen gesucht, deren Besitzer der Datenbankbesitzer ist. Wenn eine solche Tabelle vorhanden ist, werden die Spalten dieser Tabelle zurückgegeben.  
  
`[ @table_qualifier = ] 'qualifier'` Der Name des Tabellen Qualifizierers. *qualifier* ist vom Datentyp **sysname**und hat den Standardwert NULL. Verschiedene DBMS-Produkte unterstützen eine dreiteilige Benennung für Tabellen (_Qualifizierer_)**.** _Besitzer_**.** _Name_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Spalte den Datenbanknamen dar. Bei anderen Produkten stellt sie den Servernamen der Datenbankumgebung für die Tabelle dar.  
  
``[ , [ @table_type = ] "'type', 'type'" ]`` Eine durch Kommas getrennte Liste von Werten, die Informationen zu allen Tabellen der angegebenen Tabellentypen enthält. Dazu zählen **Table**, **systemtable**und **View**. *Type ist vom Datentyp* **varchar (100)** und hat den Standardwert NULL.  
  
> [!NOTE]  
>  Jeder Tabellentyp muss in einfache Anführungszeichen und der gesamte Parameter in doppelte Anführungszeichen eingeschlossen werden. Für Tabellentypen müssen Großbuchstaben verwendet werden. Wenn SET QUOTED_IDENTIFIER auf ON festgelegt ist, müssen alle einfachen Anführungszeichen in doppelte Anführungszeichen geändert werden, und der gesamte Parameter muss in einfache Anführungszeichen eingeschlossen werden.  
  
`[ @fUsePattern = ] 'fUsePattern'` Bestimmt, ob die Zeichen Unterstrich (_), Prozentzeichen (%) und eckige Klammern ([oder]) als Platzhalter Zeichen interpretiert werden. Gültige Werte sind 0 (Mustervergleich ist deaktiviert) und 1 (Mustervergleich ist aktiviert). *fUsePattern* ist vom Datentyp **bit**. Der Standardwert ist 1.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 Keine  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Tabellen qualifizierername. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Spalte den Datenbanknamen dar. Dieses Feld kann den Wert NULL annehmen.|  
|**TABLE_OWNER**|**sysname**|Der Name des Tabellen Besitzers. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Spalte den Namen des Daten Bank Benutzers dar, der die Tabelle erstellt hat. Dieses Feld gibt immer einen Wert zurück.|  
|**TABLE_NAME**|**sysname**|Tabellenname. Dieses Feld gibt immer einen Wert zurück.|  
|**TABLE_TYPE**|**varchar(32)**|Tabelle, Systemtabelle oder Sicht.|  
|**HINWEISE**|**varchar (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt für diese Spalte keinen Wert zurück.|  
  
## <a name="remarks"></a>Hinweise  
 Für maximale Interoperabilität sollte der Gatewayclient nur den SQL-92-Standard zum SQL-Mustervergleich (die Platzhalterzeichen % und _) voraussetzen.  
  
 Die Privileginformationen zum Lese- und Schreibzugriff des aktuellen Benutzers für eine bestimmte Tabelle werden nicht immer geprüft. Deshalb ist der Zugriff nicht sichergestellt. Dieses Resultset enthält nicht nur Tabellen und Sichten, sondern auch Synonyme und Aliasnamen für Gateways zu DBMS-Produkten, die diese Typen unterstützen. Wenn das Server Attribut **ACCESSIBLE_TABLES** im Resultset für **sp_server_info**Y ist, werden nur Tabellen zurückgegeben, auf die der aktuelle Benutzer zugreifen kann.  
  
 **sp_tables** entspricht **SQLTables** in ODBC. Die zurückgegebenen Ergebnisse werden nach **TABLE_TYPE**, **TABLE_QUALIFIER**, **TABLE_OWNER**und **table_name**geordnet.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert SELECT-Berechtigung für das Schema.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>A. Zurückgeben einer Liste von Objekten, die in der aktuellen Umgebung abgefragt werden können  
 Im folgenden Beispiel wird eine Liste von Objekten zurückgegeben, bei denen es sich um Abfragen in der aktuellen Umgebung handeln kann.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>B. Zurückgeben von Informationen zu den Tabellen in einem angegebenen Schema  
 Im folgenden Beispiel werden Informationen zu den Tabellen zurückgegeben, die zum `Person`-Schema in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank gehören.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>C. Zurückgeben einer Liste von Objekten, die in der aktuellen Umgebung abgefragt werden können  
 Im folgenden Beispiel wird eine Liste von Objekten zurückgegeben, bei denen es sich um Abfragen in der aktuellen Umgebung handeln kann.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>D: Zurückgeben von Informationen zu den Tabellen in einem angegebenen Schema  
 Im folgenden Beispiel werden Informationen zu den Dimensions Tabellen in der-Datenbank zurückgegeben `AdventureWorksPDW201` .  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys. Synonyme &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

