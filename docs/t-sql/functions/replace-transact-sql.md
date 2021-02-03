---
title: REPLACE (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Funktion REPLACE, die alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert ersetzt.
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REPLACE_TSQL
- REPLACE
dev_langs:
- TSQL
helpviewer_keywords:
- first string expression [SQL Server]
- replacing string expression
- third string expressions [SQL Server]
- second string expressions [SQL Server]
- REPLACE function
ms.assetid: 8a7aaaf2-62e3-46c0-8e44-fa22290dd86b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbd685e29e9773eea41be391b1bdfd7b1c66fdb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182301"
---
# <a name="replace-transact-sql"></a>REPLACE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ersetzt alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
REPLACE ( string_expression , string_pattern , string_replacement )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *string_expression*  
 Der [Zeichenfolgenausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der gesucht werden soll. *string_expression* kann von einem Zeichen- oder Binärdatentyp sein.  
  
 *string\_pattern*  
 Die zu suchende Teilzeichenfolge. *string_pattern* kann von einem Zeichen- oder Binärdatentyp sein. *string_pattern* darf keine leere Zeichenfolge ('') sein und die maximal zulässige Anzahl von Byte auf einer Seite nicht überschreiten.  
  
 *string\_replacement*  
 Die Ersetzungszeichenfolge. *string_replacement* kann von einem Zeichen- oder Binärdatentyp sein.  
  
## <a name="return-types"></a>Rückgabetypen  
 Gibt **nvarchar** zurück, wenn eines der Eingabeargumente vom Datentyp **nvarchar** ist. Andernfalls wird von REPLACE **varchar** zurückgegeben.  
  
 Gibt NULL zurück, wenn eines der Argumente NULL ist.  
  
 Wenn *string_expression* nicht vom Typ **varchar(max)** oder **nvarchar(max) ist, schneidet REPLACE** den Rückgabewert bei 8.000 Byte ab. Für die Rückgabe von Werten über 8.000 Byte muss *string_expression* explizit in einen Datentyp für umfangreichere Werten umgewandelt werden.  
  
## <a name="remarks"></a>Bemerkungen  
 REPLACE führt Vergleiche auf der Basis der Sortierung der Eingabe durch. Zum Ausführen eines Vergleichs in einer angegebenen Sortierung können Sie mithilfe von [COLLATE](~/t-sql/statements/collations.md) eine ausdrückliche Sortierung auf die Eingabe anwenden.  
  
 0x0000 (**char(0)** ) ist ein nicht definiertes Zeichen in Windows-Sortierungen und kann nicht in REPLACE enthalten sein.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel ersetzt die Zeichenfolge `cde` in `abcdefghi` durch `xxx`.  
  
```sql  
SELECT REPLACE('abcdefghicde','cde','xxx');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
abxxxfghixxx  
(1 row(s) affected)  
```  
  
 Das folgende Beispiel verwendet die `COLLATE`-Funktion.  
  
```sql  
SELECT REPLACE('This is a Test'  COLLATE Latin1_General_BIN,  
'Test', 'desk' );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
This is a desk  
(1 row(s) affected)  
```  

  
## <a name="see-also"></a>Weitere Informationen  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/string-functions-transact-sql.md)  
  
