---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: 5aded31238cbcfa5abc33b981531796d1b910e13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342113"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt an, dass sich die Vergleichsoperatoren Gleich (=) und Ungleich (<>) bei Verwendung mit NULL-Werten in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ISO-konform verhalten müssen.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntax

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Syntax für [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] und [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Syntax für [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] und [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
Wenn ANSI_NULLS auf ON festgelegt ist, gibt eine SELECT-Anweisung, in der WHERE *column_name* = **NULL** verwendet wird, auch dann 0 Zeilen zurück, wenn sich NULL-Werte in *column_name* befinden. Eine SELECT-Anweisung, die WHERE *column_name* <> **NULL** verwendet, gibt auch dann 0 Zeilen zurück, wenn sich Werte ungleich NULL in *column_name* befinden.  
  
Wenn ANSI_NULLS auf OFF festgelegt ist, entsprechen die Vergleichsoperatoren Gleich (=) und Ungleich (<>) nicht dem ISO-Standard. Eine SELECT-Anweisung, in der WHERE *column_name* = **NULL** verwendet wird, gibt die Zeilen zurück, die NULL-Werte in *column_name* enthalten. Eine SELECT-Anweisung, die WHERE *column_name* <> **NULL** verwendet, gibt die Zeilen mit Werten ungleich NULL in der Spalte zurück. Außerdem gibt eine SELECT-Anweisung, in der *column_name* <> *XYZ_value* verwendet wird, alle Zeilen zurück, die nicht gleich *XYZ_value* und nicht NULL sind.  
  
Wenn ANSI_NULLS auf ON festgelegt ist, werden alle Vergleiche mit einem NULL-Wert zu UNKNOWN ausgewertet. Wenn SET ANSI_NULLS auf OFF festgelegt ist, werden alle Datenvergleiche mit einem NULL-Wert zu TRUE ausgewertet, falls der Datenwert NULL ist. Falls SET ANSI_NULLS nicht angegeben ist, gilt die Einstellung der Option ANSI_NULLS der aktuellen Datenbank. Weitere Informationen zur Datenbankoption ANSI_NULLS finden Sie unter [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  

Die folgende Tabelle stellt dar, wie die Einstellung von ANSI_NULLS die Ergebnisse mehrerer boolescher Ausdrücke mithilfe von NULL-Werten und Werten ungleich NULL beeinflusst.  
  
|Boolescher Ausdruck|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|UNKNOWN|TRUE|  
|1 = NULL|UNKNOWN|FALSE|  
|NULL <> NULL|UNKNOWN|FALSE|  
|1 <> NULL|UNKNOWN|TRUE|  
|NULL > NULL|UNKNOWN|UNKNOWN|  
|1 > NULL|UNKNOWN|UNKNOWN|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

SET ANSI_NULLS ON hat nur dann Auswirkungen auf einen Vergleich, wenn einer der Operanden des Vergleichs entweder eine Variable, die NULL ist, oder ein Literal NULL ist. Falls beide Seiten des Vergleichs Spalten oder zusammengesetzte Ausdrücke sind, hat die Einstellung keine Auswirkungen auf den Vergleich.  
  
Ein Skript wird unabhängig von der Datenbankoption ANSI_NULLS und der Einstellung von SET ANSI_NULLS wie beabsichtigt ausgeführt, wenn Sie IS NULL und IS NOT NULL in Vergleichen verwenden, die möglicherweise NULL-Werte enthalten.  
  
ANSI_NULLS muss zum Ausführen von verteilten Abfragen auf ON festgelegt sein.  
  
ANSI_NULLS muss auch beim Erstellen oder Ändern von Indizes für berechnete Spalten oder indizierte Sichten auf ON festgelegt sein. Wenn SET ANSI_NULLS auf OFF festgelegt ist, schlagen die CREATE-, UPDATE-, INSERT- und DELETE-Anweisungen in Tabellen mit Indizes auf berechneten Spalten oder indizierten Sichten fehl. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt einen Fehler zurück, der alle SET-Optionen auflistet, die gegen die erforderlichen Werte verstoßen. Wenn SET ANSI_NULLS auf OFF festgelegt ist, ignoriert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Ausführen einer SELECT-Anweisung die Indexwerte für berechnete Spalten oder Sichten und löst den SELECT-Vorgang so auf, als seien keine derartigen Indizes für die Tabelle oder Sicht vorhanden.  
  
> [!NOTE]  
> ANSI_NULLS ist eine der sieben SET-Optionen, für die bestimmte Werte beim Verwenden von Indizes auf berechneten Spalten oder indizierten Sichten festgelegt sein müssen. Die Optionen `ANSI_PADDING`, `ANSI_WARNINGS`, `ARITHABORT`, `QUOTED_IDENTIFIER` und `CONCAT_NULL_YIELDS_NULL` müssen ebenfalls auf ON festgelegt werden, und `NUMERIC_ROUNDABORT` muss auf OFF festgelegt werden.  
  
 Der ODBC-Treiber von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client und der OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] legen ANSI_NULLS beim Herstellen einer Verbindung automatisch auf ON fest. Diese Einstellung kann in ODBC-Datenquellen, in ODBC-Verbindungsattributen oder in OLE DB-Verbindungseigenschaften konfiguriert werden, die in der Anwendung festgelegt werden, bevor die Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt wird. Die Standardeinstellung für SET ANSI_NULLS ist OFF.  
  
Ist SET ANSI_DEFAULTS auf ON festgelegt, ist ANSI_NULLS aktiviert.  
  
Die Einstellung von ANSI_NULLS wird zur Ausführungs- bzw. Laufzeit und nicht zur Analysezeit definiert.  
  
Um die aktuelle Einstellung anzuzeigen, führen Sie die folgende Abfrage aus:
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden mithilfe der Vergleichsoperatoren Gleich (`=`) und Ungleich (`<>`) Vergleiche mit `NULL`-Werten und mit Werten ungleich NULL in einer Tabelle ausgeführt. Das Beispiel zeigt ebenfalls, dass `IS NULL` durch die `SET ANSI_NULLS`-Einstellung nicht beeinflusst wird.  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

Legen Sie ANSI_NULLS jetzt auf ON fest, und führen Sie einen Test durch.

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

Legen Sie ANSI_NULLS jetzt auf OFF fest, und führen Sie einen Test durch.  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41; (= &#40;ist gleich&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [&#60;&#62; &#40;Ungleich&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
