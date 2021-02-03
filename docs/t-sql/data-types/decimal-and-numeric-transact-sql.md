---
title: decimal und numeric (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Datentypen decimal und numeric „Decimal“ und „numeric“ sind Synonyme für numerische Datentypen mit festgelegter Genauigkeit und Skalierung.
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- decimal
- decimal_TSQL
- numeric
- numeric_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decimal data type
- decimal data type, about decimal data type
- numeric data type
- numeric data type, about numeric data type
ms.assetid: 9d862a90-e6b7-4692-8605-92358dccccdf
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4aee37a054da84a247b34ee272b877e28d2bb023
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161265"
---
# <a name="decimal-and-numeric-transact-sql"></a>decimal und numeric (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Numerische Datentypen mit fester Genauigkeit und fester Anzahl von Dezimalstellen. decimal und numeric sind Synonyme und können austauschbar verwendet werden.
  
## <a name="arguments"></a>Argumente
**decimal**[ **(** _p_[ **,** _s_] **)** ] und **numeric**[ **(** _p_[ **,** _s_] **)** ]  
Zahlen mit fester Genauigkeit und mit fester Anzahl von Dezimalstellen. Wenn maximale Genauigkeit verwendet wird, liegen gültige Werte zwischen - 10^38 +1 und 10^38 - 1. Die ISO-Synonyme für **decimal** lauten **dec** und **dec(** _p_, _s_ **)** . Die Funktion von **numeric** ist mit der von **decimal** identisch.
  
p (Precision = Genauigkeit)  
Die maximale Gesamtanzahl der zu speichernden Dezimalstellen. Diese Zahl schließt die Ziffern links und rechts des Dezimaltrennzeichens ein. Die Genauigkeit muss ein Wert zwischen 1 und der maximalen Genauigkeit von 38 sein. Die Standardgenauigkeit beträgt 18.
  
> [!NOTE]  
>  Informatica unterstützt unabhängig von der angegebenen Präzision und dem Dezimalstellenwert nur 16 signifikante Ziffern.  
  
*s* (Dezimalstellenwert)  
Die Anzahl von Dezimalstellen die rechts vom Dezimaltrennzeichen gespeichert werden. Diese Anzahl wird von *p* subtrahiert, um die maximale Anzahl der Stellen links von der Dezimalstelle zu bestimmen. Die Anzahl von Dezimalstellen muss ein Wert zwischen 0 und *p* sein und kann nur festgelegt werden, wenn die Genauigkeit angegeben wird. Der Standardwert ist 0 (null). Daher gilt: 0 <= *s* \<= *p*. Die maximalen Speichergrößen variieren abhängig von der Genauigkeit.
  
|Precision|Speicherplatz in Bytes|  
|---|---|
|1 – 9|5|  
|10–19|9|  
|20–28|13|  
|29–38|17|  
  
> [!NOTE]  
>  Informatica (über den SQL Server PDW-Informatica-Connector verbunden) unterstützt unabhängig von der angegebenen Präzision und dem Dezimalstellenwert nur 16 signifikante Ziffern.  
  
## <a name="converting-decimal-and-numeric-data"></a>Konvertieren von decimal- und numeric-Daten
Im Fall der Datentypen **decimal** und **numeric** sieht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jede Kombination aus Genauigkeit und Anzahl von Dezimalstellen als einen anderen Datentyp an. **decimal(5,5)** und **decimal(5,0)** werden beispielsweise als unterschiedliche Datentypen erachtet.
  
In [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen wird eine Konstante mit einem Dezimaltrennzeichen automatisch in einen Wert des **numeric**-Datentyps konvertiert. Hierbei werden die mindestens erforderliche Genauigkeit und die Anzahl von Dezimalstellen verwendet. Die Konstante 12.345 wird z.B. in einen **numeric**-Wert mit einer Genauigkeit von 5 und 3 Dezimalstellen konvertiert.
  
Wenn Sie eine Konvertierung von **decimal** oder **numeric** in **float** oder **real** vornehmen, kann ein gewisses Maß an Genauigkeit verloren gehen. Wenn Sie eine Konvertierung von **int**, **smallint**, **tinyint**, **float**, **real**, **money** oder **smallmoney** in **decimal** oder **numeric** vornehmen, kann es zu einem Überlauf kommen.
  
Bei der Konvertierung einer Zahl in einen Wert des Typs **decimal** oder **numeric** mit einer geringeren Genauigkeit und einer geringeren Anzahl von Dezimalstellen wird der Wert standardmäßig von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gerundet. Wenn allerdings die Option SET ARITHABORT auf ON festgelegt ist, löst [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bei Auftreten eines Überlaufs einen Fehler aus. Eine Verringerung der Genauigkeit und der Anzahl von Dezimalstellen reicht zum Auslösen eines Fehlers nicht aus.
  
Vor [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] ist die Konvertierung von **float**-Werten in **decimal**- oder **numeric**-Werte auf Werte mit einer Genauigkeit von 17 Stellen beschränkt. Jeder **float**-Wert kleiner als 5E-18 (der entweder in der wissenschaftlichen Schreibweise als 5E-18 oder in der Dezimalschreibweise als 0,0000000000000000050000000000000005 festgelegt ist) wird auf 0 (null) abgerundet. Dies stellt ab [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] keine Einschränkung mehr dar.
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird eine Tabelle mit **decimal**- und **numeric**-Datentypen erstellt.  Werte werden in den einzelnen Spalten eingefügt. Die Ergebnisse werden mithilfe einer SELECT-Anweisung zurückgegeben.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyDecimalColumn DECIMAL(5,2)  
,MyNumericColumn NUMERIC(10,5)
  
);  
  
GO  
INSERT INTO dbo.MyTable VALUES (123, 12345.12);  
GO  
SELECT MyDecimalColumn, MyNumericColumn  
FROM dbo.MyTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
MyDecimalColumn                         MyNumericColumn  
--------------------------------------- ---------------------------------------  
123.00                                  12345.12000  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Weitere Informationen
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
