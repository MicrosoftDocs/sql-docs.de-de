---
description: Genauigkeit, Dezimalstellen und Länge (Transact-SQL)
title: Genauigkeit, Dezimalstellen und Länge (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], length
- data types [SQL Server], scale
- precision [SQL Server], data types
- lengths [SQL Server], data types
- number of digits
- scale [SQL Server]
- scale [SQL Server], data types
- data types [SQL Server], precision
ms.assetid: fbc9ad2c-0d3b-4e98-8fdd-4d912328e40a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: da816b6e55bc1578a820a53d6e1036b685086713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338044"
---
# <a name="precision-scale-and-length-transact-sql"></a>Genauigkeit, Dezimalstellen und Länge (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Genauigkeit gibt die Anzahl der Ziffern einer Zahl an. Dezimalstellen gibt die Anzahl der Nachkommastellen an. Die Zahl 123,45 hat z. B. eine Genauigkeit von 5 und 2 Dezimalstellen.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist die maximale Standardgenauigkeit der Datentypen **numeric** und **decimal** 38. In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist das Standardmaximum 28.
  
Die Länge für einen numerischen Datentyp ist die Anzahl der Bytes, die zum Speichern der Zahl verwendet werden. Für varchar und char stellt die Länge einer Zeichenfolge die Anzahl der Bytes dar. Für nvarchar und nchar stellt die Länge einer Zeichenfolge die Anzahl der Bytepaare dar. Die Länge der Datentypen **binary**, **varbinary** und **image** entspricht der Anzahl von Bytes. Ein **int**-Datentyp kann z.B. 10 Stellen enthalten, wird in 4 Bytes gespeichert und nimmt keine Dezimaltrennzeichen an. Der **int**-Datentyp hat eine Genauigkeit von 10 und eine Länge von 4. Er weist 0 Dezimalstellen auf.
  
Wenn zwei Ausdrücke vom Datentyp **char**, **varchar**, **binary** oder **varbinary** verkettet werden, ist die Länge des sich ergebenden Ausdrucks die Summe der Längen der beiden Quellausdrücke bis zu einer Länge von 8.000 Bytes.
  
Wenn zwei Ausdrücke vom Datentyp **nchar** oder **nvarchar** verkettet werden, ist die Länge des sich ergebenden Ausdrucks die Summe der Längen der beiden Ausgangsausdrücke bis zu einer Länge von 4.000 Bytepaaren.
  
Wenn zwei Ausdrücke vom selben Datentyp, aber mit verschiedenen Längen mit UNION, EXCEPT oder INTERSECT verglichen werden, entspricht die sich ergebende Länge der längeren Länge der beiden Ausdrücke.
  
Genauigkeit und Dezimalstellenanzahl der numerischen Datentypen außer **decimal** sind nicht veränderbar. Wenn ein arithmetischer Operator zwei Ausdrücke vom gleichen Typ verwendet, so ist das Ergebnis vom selben Datentyp. Er besitzt die Genauigkeit und die Dezimalstellenanzahl, die für diesen Datentyp definiert sind. Wenn ein Operator zwei Ausdrücke mit verschiedenen numerischen Datentypen verwendet, bestimmen die Rangfolgeregeln für Datentypen den Datentyp des Ergebnisses. Das Ergebnis hat die für seinen Datentyp definierte Genauigkeit und Dezimalstellenanzahl.
  
Die folgende Tabelle definiert, wie Genauigkeit und Dezimalstellen des Ergebnisses berechnet werden, wenn das Ergebnis einer Operation vom Datentyp **decimal** ist. Das Ergebnis ist vom Datentyp **decimal**, wenn eine der beiden folgenden Aussagen zutrifft:
-   Beide Ausdrücke sind vom Datentyp **decimal**.  
-   Ein Ausdruck ist vom Datentyp **decimal**, und der andere Ausdruck ist von einem Datentyp mit einer niedrigeren Rangfolge als **decimal**.  
  
Die Operandenausdrücke werden als Ausdruck e1 mit der Genauigkeit p1 und der Dezimalstellenanzahl s1 und Ausdruck e2 mit der Genauigkeit p2 und der Dezimalstellenanzahl s2 bezeichnet. Die Genauigkeit und Dezimalstellenanzahl für einen Ausdruck, der nicht vom Datentyp **decimal** ist, entspricht der für den Datentyp des Ausdrucks definierten Genauigkeit und Dezimalstellenanzahl. Die Funktion max(a,b) bedeutet Folgendes: Verwende den größeren Wert von „a“ oder „b“. Entsprechend bedeutet die Funktion min(a,b): Verwende den kleineren Wert von „a“ oder „b“.
  
|Vorgang|Genauigkeit des Ergebnisses|Dezimalstellen des Ergebnisses *|  
|---|---|---|
|e1 + e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 - e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 * e2|p1 + p2 + 1|s1 + s2|  
|e1 / e2|p1 - s1 + s2 + max(6, s1 + p2 + 1)|max(6, s1 + p2 + 1)|  
|e1 { UNION &#124; EXCEPT &#124; INTERSECT } e2|max(s1, s2) + max(p1-s1, p2-s2)|max(s1, s2)|  
|e1 % e2|min(p1-s1, p2 -s2) + max( s1,s2 )|max(s1, s2)|  
  
\* Die Genauigkeit und Dezimalstellen des Ergebnisses haben ein absolutes Maximum von 38. Wenn die Genauigkeit des Ergebnisses 38 überschreitet, wird es auf 38 reduziert. Die zugehörige Dezimalstellenanzahl wird ebenfalls reduziert, um zu verhindern, dass der ganzzahlige Teil eines Ergebnisses abgeschnitten wird. In einigen Fällen (z.B. bei der Multiplikation oder Division) wird der Skalierungsfaktor nicht reduziert, um die Dezimalgenauigkeit beizubehalten. Dadurch kann jedoch ein Überlauffehler ausgelöst werden.

Bei Additions- und Subtraktionsvorgängen werden `max(p1 - s1, p2 - s2)`-Platzhalter benötigt, um den ganzzahligen Teil der Dezimalzahl zu speichern. Wenn nicht genügend Speicherplatz vorhanden ist (d.h. `max(p1 - s1, p2 - s2) < min(38, precision) - scale`), wird die Anzahl der Dezimalstellen reduziert, um genügend Speicherplatz für den ganzzahligen Teil bereitzustellen. Die resultierenden Dezimalstellen sind `MIN(precision, 38) - max(p1 - s1, p2 - s2)`. Die Stellen hinter dem Dezimaltrennzeichen werden also möglicherweise gerundet, um in diese zu passen.

Bei Multiplikations- und Divisionsvorgängen werden `precision - scale`-Platzhalter benötigt, um den ganzzahligen Teil des Ergebnisses zu speichern. Die Dezimalstellen können mithilfe von folgenden Regeln reduziert werden:
1.  Die resultierende Anzahl der Dezimalstellen wird auf `min(scale, 38 - (precision-scale))` reduziert, wenn der ganzzahlige Teil kleiner als 32 ist, da die Anzahl nicht größer als `38 - (precision-scale)` sein darf. Das Ergebnis kann in diesem Fall gerundet werden.
1. Die Anzahl der Dezimalstellen wird nicht geändert, wenn diese kleiner als sechs und der ganzzahlige Teil größer als 32 ist. In diesem Fall wird möglicherweise ein Überlauffehler ausgelöst, wenn das Ergebnis nicht in „decimal(38, scale)“ passt. 
1. Die Anzahl der Dezimalstellen wird auf sechs festgelegt, wenn diese größer als sechs und der ganzzahlige Teil größer als 32 ist. In diesem Fall werden der integrale Bestandteil und die Dezimalstellen reduziert, und der sich ergebende Typ lautet decimal(38,6). Das Ergebnis wird möglicherweise auf sechs Dezimalstellen gerundet. Andernfalls wird ein Überlauffehler ausgelöst, wenn der ganzzahlige Teil nicht in 32 Ziffern passt.

## <a name="examples"></a>Beispiele
Der folgende Ausdruck gibt das Ergebnis `0.00000090000000000` ohne Rundung zurück, da das Ergebnis in `decimal(38,17)` passt:
```sql
select cast(0.0000009000 as decimal(30,20)) * cast(1.0000000000 as decimal(30,20)) [decimal 38,17]
```
In diesem Fall beträgt die Genauigkeit 61 und die Dezimalstellen betragen 40.
Der ganzzahlige Teil (precision-scale = 21) ist kleiner als 32. Dies entspricht also Fall (1) in den Multiplikationsregeln, und die Anzahl der Dezimalstellen wird als `min(scale, 38 - (precision-scale)) = min(40, 38 - (61-40)) = 17` berechnet. Der Ergebnistyp ist `decimal(38,17)`.

Der folgende Ausdruck gibt das Ergebnis `0.000001` zurück, das in `decimal(38,6)` passen soll:
```sql
SELECT CAST(0.0000009000 AS DECIMAL(30,10)) * CAST(1.0000000000 AS DECIMAL(30,10)) [decimal(38, 6)]
```
In diesem Fall beträgt die Genauigkeit 61 und die Dezimalstellen betragen 20.
Die Dezimalstellen sind größer als 6 und der integrale Bestandteil (`precision-scale = 41`) größer als 32. Dies entspricht also Fall (3) in den Multiplikationsregeln, und der Ergebnistyp ist `decimal(38,6)`.

## <a name="see-also"></a>Weitere Informationen
[Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
