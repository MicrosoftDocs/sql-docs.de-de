---
description: ToString (Datenbank-Engine)
title: ToString (Datenbank-Engine)
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ToString
- ToString_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ToString [Database Engine]
ms.assetid: 5fc11ca5-c26d-4518-9512-67aa0270f110
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1d1460c83488f7ba708f737e7c9bb7bd46819152
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037463"
---
# <a name="tostring-database-engine"></a>ToString (Datenbank-Engine)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine Zeichenfolge mit der logischen Darstellung von *this* zurück. ToString wird implizit aufgerufen, wenn eine Konvertierung von einem **hierarchyid**-Typ in einen Zeichenfolgentyp stattfindet. Fungiert als Gegenstück zu [Parse &#40;Datenbank-Engine&#41;](../../t-sql/data-types/parse-database-engine.md).
  
## <a name="syntax"></a>Syntax  

```syntaxsql
-- Transact-SQL syntax
node.ToString  ( )
-- This is functionally equivalent to the following syntax  
-- which implicitly calls ToString():  
CAST(node AS nvarchar(4000))  
```  
  
```syntaxsql
-- CLR syntax
string ToString  ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen

**SQL Server-Rückgabetyp: nvarchar(4000)**
  
**CLR-Rückgabetyp: String**
  
## <a name="remarks"></a>Bemerkungen  
Gibt die logische Position in der Hierarchie zurück. Beispielsweise stellt `/2/1/` die vierte Zeile ([!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) in der folgenden hierarchischen Struktur eines Dateisystems dar:
  
```sql
/        C:\  
/1/      C:\Database Files  
/2/      C:\Program Files  
/2/1/    C:\Program Files\Microsoft SQL Server  
/2/2/    C:\Program Files\Microsoft Visual Studio  
/3/      C:\Windows  
```  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-transact-sql-example-in-a-table"></a>A. Transact-SQL-Beispiel in einer Tabelle  
Im folgenden Beispiel wird sowohl die `OrgNode`-Spalte als auch der **hierarchyid**-Datentyp in einem leichter lesbaren Format zurückgegeben:
  
```sql
SELECT OrgNode,  
OrgNode.ToString() AS Node  
FROM HumanResources.EmployeeDemo  
ORDER BY OrgNode ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
OrgNode   Node  
0x        /  
0x58      /1/  
0x5AC0    /1/1/  
0x5B40    /1/2/  
0x5BC0    /1/3/  
0x5C20    /1/4/  
...  
```  
  
### <a name="b-converting-transact-sql-values-without-a-table"></a>B. Konvertieren von Transact-SQL-Werten ohne Tabelle  
Im folgenden Codebeispiel wird mithilfe von `ToString` ein **hierarchyid**-Wert in eine Zeichenfolge und mithilfe von `Parse` ein Zeichenfolgenwert in eine **hierarchyid** konvertiert.
  
```sql
DECLARE @StringValue AS nvarchar(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="c-clr-example"></a>C. CLR-Beispiel  
Im folgenden Codeausschnitt wird die ToString()-Methode aufgerufen:
  
```sql
this.ToString()  
```  
  
## <a name="see-also"></a>Weitere Informationen
[hierarchyid-Datentyp-Methodenverweis](./hierarchyid-data-type-method-reference.md)  
[Hierarchische Daten &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
