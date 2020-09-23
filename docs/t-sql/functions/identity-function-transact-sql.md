---
description: IDENTITY (Funktion) (Transact-SQL)
title: IDENTITY (Funktion) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 364bcd6eb68ca7c529c56fae70109b79cbc1dc18
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91114727"
---
# <a name="identity-function-transact-sql"></a>IDENTITY (Funktion) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Wird nur in einer SELECT-Anweisung mit einer INTO *table*-Klausel verwendet, um eine Identitätsspalte in eine neue Tabelle einzufügen. Die IDENTITY-Funktion ähnelt der mit CREATE TABLE und ALTER TABLE verwendeten IDENTITY-Eigenschaft, ist jedoch nicht mit ihr identisch.  
  
> [!NOTE]  
>  Weitere Informationen zu einer automatisch inkrementierten Zahl, die in mehreren Tabellen verwendet oder aus Anwendungen aufgerufen werden kann, ohne dass auf eine Tabelle verwiesen wird, finden Sie unter [Sequenznummern](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *data_type*  
 Der Datentyp der Identitätsspalte. Gültige Datentypen für eine Identitätsspalte sind beliebige Datentypen aus der Integerdatentypkategorie (mit Ausnahme des **bit**-Datentyps) oder der **decimal**-Datentyp.  
  
 *seed*  
 Der ganzzahlige Wert, der der ersten Zeile in der Tabelle zugewiesen werden soll. Jeder nachfolgenden Zeile wird jeweils der nächste Identitätswert zugewiesen, der sich aus dem letzten IDENTITY-Wert plus dem *increment*-Wert ergibt. Ist weder der Ausgangswert (*seed*) noch der inkrementelle Wert (*increment*) angegeben, gilt für beide der Standardwert 1.  
  
 *increment*  
 Der ganzzahlige Wert, der dem *seed*-Wert für nachfolgende Zeilen in der Tabelle hinzugefügt werden soll.  
  
 *column_name*  
 Der Name der Spalte, die in die neue Tabelle eingefügt werden soll.  
  
## <a name="return-types"></a>Rückgabetypen  
 Gibt denselben Wert zurück wie *data_type*.  
  
## <a name="remarks"></a>Hinweise  
 Da diese Funktion eine Spalte in einer Tabelle erstellt, muss für die Spalte ein Name in der Auswahlliste angegeben werden. Dies kann auf zwei Arten geschehen:  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle Zeilen aus der `Contact`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank in eine neue Tabelle namens `NewContact` eingefügt. Die IDENTITY-Funktion bewirkt, dass die Identifikationsnummern in der `NewContact`-Tabelle bei 100 anstatt bei 1 beginnen.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;Eigenschaft&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  
