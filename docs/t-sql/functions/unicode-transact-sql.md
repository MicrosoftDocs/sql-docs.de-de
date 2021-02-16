---
description: UNICODE (Transact-SQL)
title: UNICODE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UNICODE
- UNICODE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first character of input expression [SQL Server]
- UNICODE function
- Unicode [SQL Server], UNICODE function
ms.assetid: 5e3c40b2-8401-4741-9f2a-bae70eaa4da6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 78a6f81504e3fd6f39f32d52cb3afd45b7f7a035
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342588"
---
# <a name="unicode-transact-sql"></a>UNICODE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt laut Definition des Unicode-Standards eine ganze Zahl für das erste Zeichen des Eingabeausdrucks zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
UNICODE ( 'ncharacter_expression' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
**'** *ncharacter_expression* **'**  
Ein **nchar**- oder **nvarchar**-Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen  
**int**  
  
## <a name="remarks"></a>Bemerkungen  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen vor [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt die UNICODE-Funktion einen UCS-2-Codepunkt im Bereich 000000 bis 00FFFF zurück, mit dem die 65.535 Zeichen in Unicode BMP (Basic Multilingual Plane) dargestellt werden können. Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] gilt Folgendes: Wenn eine Sortierung mit aktivierten [zusätzlichen Zeichen](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) verwendet wird, gibt UNICODE einen UTF-16-Codepunkt im Bereich 000000 bis 10FFFF zurück. Weitere Informationen zur Unicode-Unterstützung in [!INCLUDE[ssde_md](../../includes/ssde_md.md)] finden Sie unter [Sortierung und Unicode-Unterstützung](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn). 
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-unicode-and-the-nchar-function"></a>A. Verwenden von UNICODE und der NCHAR-Funktion  
 Im folgenden Beispiel werden die Funktionen `UNICODE` und `NCHAR` zur Ausgabe des UNICODE-Werts des ersten Zeichens der Zeichenfolge `Åkergatan` 24 sowie zur Ausgabe des tatsächlichen ersten Zeichens, und zwar `Å`, verwendet.  
  
```sql  
DECLARE @nstring NCHAR(12);  
SET @nstring = N'Åkergatan 24';  
SELECT UNICODE(@nstring), NCHAR(UNICODE(@nstring));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -   
197         Å  
```  
  
### <a name="b-using-substring-unicode-and-convert"></a>B. Verwenden von SUBSTRING, UNICODE und CONVERT  
 Im folgenden Beispiel werden die Funktionen `SUBSTRING`, `UNICODE` und `CONVERT` zur Ausgabe der Zeichennummer, des Unicode-Zeichens und des UNICODE-Werts für jedes Zeichen in der Zeichenfolge `Åkergatan 24` verwendet.  
  
```sql  
-- The @position variable holds the position of the character currently  
-- being processed. The @nstring variable is the Unicode character   
-- string to process.  
DECLARE @position INT, @nstring NCHAR(12);  
-- Initialize the current position variable to the first character in   
-- the string.  
SET @position = 1;  
-- Initialize the character string variable to the string to process.   
-- Notice that there is an N before the start of the string, which   
-- indicates that the data following the N is Unicode data.  
SET @nstring = N'Åkergatan 24';  
-- Print the character number of the position of the string you are at,   
-- the actual Unicode character you are processing, and the UNICODE   
-- value for this particular character.  
PRINT 'Character #' + ' ' + 'Unicode Character' + ' ' + 'UNICODE Value';  
WHILE @position <= LEN(@nstring)  
-- While these are still characters in the character string,  

BEGIN;  
   SELECT @position AS [position],   
      SUBSTRING(@nstring, @position, 1) AS [character],  
      UNICODE(SUBSTRING(@nstring, @position, 1)) AS [code_point];  
   SET @position = @position + 1;  
END; 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Character # Unicode Character UNICODE Value  
  
----------- ----------------- -----------   
1           Å                 197           
  
----------- ----------------- -----------   
2           k                 107           
  
----------- ----------------- -----------   
3           e                 101           
  
----------- ----------------- -----------   
4           r                 114           
  
----------- ----------------- -----------   
5           g                 103           
  
----------- ----------------- -----------   
6           a                 97            
  
----------- ----------------- -----------   
7           t                 116           
  
----------- ----------------- -----------   
8           a                 97            
  
----------- ----------------- -----------   
9           n                 110           
  
----------- ----------------- -----------   
10                            32            
  
----------- ----------------- -----------   
11          2                 50            
  
----------- ----------------- -----------   
12          4                 52  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ASCII &#40;Transact-SQL&#41;](../../t-sql/functions/ascii-transact-sql.md)  
 [CHAR &#40;Transact-SQL&#41;](../../t-sql/functions/char-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)   
 [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen (Transact-SQL))](../../t-sql/functions/string-functions-transact-sql.md)   
 [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  

