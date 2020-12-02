---
description: binary und varbinary (Transact-SQL)
title: binary und varbinary (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- binary_TSQL
- varbinary_TSQL
- binary
- varbinary
dev_langs:
- TSQL
helpviewer_keywords:
- varbinary data type
- binary [SQL Server], about binary data type
ms.assetid: bcce65f9-10db-4b3e-bfaf-dfc06c6f820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 842cde194f39b32b2140c09afed458f903dc772d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130816"
---
# <a name="binary-and-varbinary-transact-sql"></a>binary und varbinary (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Binäre Datentypen mit fester Länge bzw. mit variabler Länge.
  
## <a name="arguments"></a>Argumente

**binary** [ ( _n_ ) ] Binärdaten mit einer festen Länge von _n_ Bytes, wobei _n_ einen Wert von 1 bis 8000 aufweisen kann. Die Speichergröße beträgt _n_ Byte.
  
**varbinary** [ ( _n_ | **max**) ] Binärdaten variabler Länge. _n_ kann einen Wert von 1 bis 8000 aufweisen. **max** gibt an, dass die maximale Speichergröße 2^31-1 Byte beträgt. Die Speicherplatzgröße ist die tatsächliche Länge der eingegebenen Daten + 2 Byte. Die eingegebenen Daten können 0 Byte lang sein. Das ANSI SQL-Synonym für **varbinary** ist **binary varying**.
  
## <a name="remarks"></a>Hinweise  
Die Standardlänge beträgt 1, wenn _n_ in einer Datendefinitions- oder Variablendeklarationsanweisung nicht angegeben ist. Wenn _n_ in der CAST-Funktion nicht angegeben ist, beträgt die Standardlänge 30.

| Datentyp | Verwenden Sie |
| --- | --- |
| **binary** | , wenn die Dateneinträge einer Spalte jeweils gleich lang sind.|
| **varbinary** | , wenn sich die Dateneinträge einer Spalte in ihrer Größe erheblich unterscheiden.|
| **varbinary(max)** | , wenn die Spaltendateneinträge 8000 Byte überschreiten.|


## <a name="converting-binary-and-varbinary-data"></a>Konvertieren von binary- und varbinary-Daten
Wenn Daten von einem string-Datentyp in einen **binary**- oder **varbinary**-Datentyp anderer Länge konvertiert werden, füllt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Daten nach rechts auf oder schneidet sie rechts ab. Zu diesen String-Datentypen zählen:

* **char** 
* **varchar**
* **nchar**
* **nvarchar**
* **binary**
* **varbinary**
* **text**
* **ntext**
* **image**

Bei der Konvertierung anderer Datentypen in **binary** oder **varbinary** werden die Daten nach links aufgefüllt oder links abgeschnitten. Für die Auffüllung werden hexadezimale Nullen verwendet.
  
Das Konvertieren von Daten in die Datentypen **binary** und **varbinary** ist hilfreich, wenn **binary**-Daten die einfachste Möglichkeit zum Verschieben von Daten darstellen. Irgendwann konvertieren Sie möglicherweise einen Werttyp in einen binären Wert mit ausreichender Größe und konvertieren den Wert dann wieder zurück. Diese Konvertierung ergibt stets denselben Wert, wenn beide Konvertierungen mit der gleichen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden. Die binäre Darstellung eines Werts kann sich zwischen den Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ändern.
  
Sie können die Datentypen **int**, **smallint** und **tinyint** in die Datentypen **binary** oder **varbinary** konvertieren. Wenn Sie allerdings den **binary**-Wert wieder zurück in einen ganzzahligen Wert konvertieren, kann das Ergebnis von der ursprünglichen Ganzzahl abweichen, falls der Wert abgeschnitten wurde. Die folgende SELECT-Anweisung zeigt beispielsweise, dass der ganzzahlige Wert `123456` als binärer Wert `0x0001e240` gespeichert wird:
  
```sql
SELECT CAST( 123456 AS BINARY(4) );  
```  
  
Diese `SELECT`-Anweisung zeigt, dass die vorangestellten Nullen automatisch abgeschnitten werden, wenn das **binary**-Ziel für die Aufnahme des gesamten Werts zu klein ist. Die gleiche Zahl wird daher als `0xe240` gespeichert:
  
```sql
SELECT CAST( 123456 AS BINARY(2) );  
```  
  
Der folgende Batch zeigt, dass sich das automatische Abschneiden des Werts auf arithmetische Operationen auswirken kann, ohne dass ein Fehler ausgelöst wird:
  
```sql
DECLARE @BinaryVariable2 BINARY(2);  
  
SET @BinaryVariable2 = 123456;  
SET @BinaryVariable2 = @BinaryVariable2 + 1;  
  
SELECT CAST( @BinaryVariable2 AS INT);  
GO  
```  
  
Das endgültige Ergebnis ist `57921` und nicht `123457`.
  
> [!NOTE]  
>  Konvertierungen zwischen einem beliebigen Datentyp und den **binary**-Datentypen sind bei unterschiedlichen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen nicht unbedingt identisch.  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
