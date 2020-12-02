---
description: Indizes in berechneten Spalten
title: Indizes in berechneten Spalten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, index creation
- index creation [SQL Server], computed columns
- imprecise columns
- persisted computed columns
- precise [SQL Server]
ms.assetid: 8d17ac9c-f3af-4bbb-9cc1-5cf647e994c4
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea5366a8080ccbfb4d48c8bcfda8928ed54932d1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88408116"
---
# <a name="indexes-on-computed-columns"></a>Indizes in berechneten Spalten
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Sie können Indizes für berechnete Spalten definieren, sofern die folgenden Anforderungen erfüllt sind:  
  
-   Anforderungen hinsichtlich des Besitzes  
-   Anforderungen hinsichtlich des Determinismus  
-   Anforderungen hinsichtlich der Präzision  
-   Anforderungen hinsichtlich des Datentyps  
-   Anforderungen hinsichtlich der SET-Option  
  
#### <a name="ownership-requirements"></a>Anforderungen hinsichtlich des Besitzes
  
Alle Funktionsverweise in der berechneten Spalte müssen denselben Besitzer wie die Tabelle aufweisen.  
  
## <a name="determinism-requirements"></a>Anforderungen hinsichtlich des Determinismus  

Ausdrücke gelten als deterministisch, wenn sie für eine bestimmte Gruppen von Eingaben stets dasselbe Ergebnis zurückgeben. Die **IsDeterministic** -Eigenschaft der [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) -Funktion teilt mit, ob der Ausdruck einer berechneten Spalte ( *computed_column_expression* ) deterministisch ist.  
Der *computed_column_expression* muss deterministisch sein. Ein *computed_column_expression* ist deterministisch, wenn alle folgenden Bedingungen zutreffen:  
  
-   Alle Funktionen, auf die der Ausdruck verweist, sind deterministisch und präzise. Zu diesen Funktionen zählen benutzerdefinierte und integrierte Funktionen. Weitere Informationen finden Sie unter [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md). Funktionen können unpräzise sein, wenn die berechnete Spalte als PERSISTED markiert ist. Weitere Informationen finden Sie unter [Erstellen von Indizes für persistente berechnete Spalten](#BKMK_persisted) nachfolgend in diesem Thema.  
  
-   Alle Spalten, auf die der Ausdruck verweist, stammen aus der Tabelle, die die berechnete Spalte enthält.  
  
-   Kein Spaltenverweis ruft Daten aus mehreren Zeilen ab. Beispielsweise hängen Aggregatfunktionen wie SUM oder AVG von Daten aus mehreren Zeilen ab, und ein *computed_column_expression* wäre dadurch nicht deterministisch.  
  
-   Der *computed_column_expression* kann weder auf Systemdaten noch auf Benutzerdaten zugreifen.  
  
Jede berechnete Spalte, die einen CLR-Ausdruck (Common Language Runtime) enthält, muss deterministisch und als PERSISTED markiert sein, bevor die Spalte indiziert werden kann. Ausdrücke des CLR-benutzerdefinierten Typs sind in den Definitionen berechneter Spalten zulässig. Berechnete Spalten, deren Typ ein CLR-benutzerdefinierter Typ ist, können indiziert werden, sofern der Typ vergleichbar ist. Weitere Informationen finden Sie unter [Benutzerdefinierte CLR-Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  

#### <a name="cast-and-convert"></a>CAST und CONVERT

Wenn Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]auf Zeichenfolgenliterale des Date-Datentyps in indizierten berechneten Spalten verweisen, ist es ratsam, das Literal explizit in den gewünschten Datentyp zu konvertieren, indem Sie ein deterministisches Datenformat verwenden. Eine Liste der deterministischen Datenformatstile finden Sie unter [CAST und CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). 

Weitere Informationen finden Sie unter [Nicht deterministische Konvertierung von Datumsliteralzeichenfolgen in DATE-Werte](../../t-sql/data-types/nondeterministic-convert-date-literals.md).

#### <a name="compatibility-level"></a>Kompatibilitätsgrad

Die implizierte Konvertierung von Nicht-Unicode-Zeichendaten zwischen Sortierungen wird als nicht deterministisch erachtet, wenn der Kompatibilitätsgrad nicht auf 80 oder niedriger festgelegt ist.  

Beträgt die Einstellung des Kompatibilitätsgrades der Datenbank 90, können Sie keine Indizes für berechnete Spalten erstellen, die diese Ausdrücke enthalten. Vorhandene berechnete Spalten, die diese Ausdrücke aus einer aktualisierten Datenbank enthalten, sind jedoch verwaltbar. Bei Verwendung indizierter berechneter Spalten, die implizite Konvertierungen von Zeichenfolgen in Datumsangaben enthalten, sollten Sie sicherstellen, dass die LANGUAGE- und DATEFORMAT-Einstellungen in der Datenbank und den Anwendungen konsistent sind, um mögliche Beschädigungen der Indizes zu vermeiden.

Der Kompatibilitätsgrad 90 entspricht SQL Server 2005.



## <a name="precision-requirements"></a>Anforderungen hinsichtlich der Präzision
  
 Der *computed_column_expression* muss präzise sein. Ein *computed_column_expression* ist präzise, wenn mindestens eine der folgenden Bedingungen zutrifft:  
  
-   Es handelt sich um keinen Ausdruck des **float** - oder **real** -Datentyps.  
-   In der Definition des Ausdrucks wird kein **float** - oder **real** -Datentyp verwendet. Beispielsweise ist in der folgenden Anweisung die `y` -Spalte eine **int** und deterministisch, aber nicht präzise.  
  
    ```sql  
    CREATE TABLE t2 (a int, b int, c int, x float,   
       y AS CASE x   
             WHEN 0 THEN a   
             WHEN 1 THEN b   
             ELSE c   
          END);  
    ```  
  
> [!NOTE]  
> Jeder **float** - oder **real** -Ausdruck gilt als nicht präzise und kann nicht als Schlüssel eines Indexes verwendet werden; ein **float** - oder **real** -Ausdruck kann in einer indizierten Sicht, jedoch nicht als Schlüssel verwendet werden. Dies gilt auch für berechnete Spalten. Jede Funktion, jeder Ausdruck oder jede benutzerdefinierte Funktion gilt als unpräzise, wenn sie irgendeinen **float** - oder **real** Ausdruck enthält. Das gilt auch für logische (Vergleiche).  
  
Die **IsPrecise** -Eigenschaft der COLUMNPROPERTY-Funktion teilt mit, ob der Ausdruck einer berechneten Spalte ( *computed_column_expression* ) präzise ist.  


## <a name="data-type-requirements"></a>Anforderungen hinsichtlich des Datentyps
  
-   Der *computed_column_expression* , der für die berechnete Spalte definiert ist, kann nicht zu einem der Datentypen **text**, **ntext** oder **image** ausgewertet werden.  
-   Berechnete Spalten, die aus den Datentypen **image**, **ntext**, **text**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** und **xml** abgeleitet wurden, können indiziert werden, solange der Datentyp der berechneten Spalte als Indexschlüsselspalte zulässig ist.  
-   Berechnete Spalten, die aus den Datentypen **image**, **ntext** und **text** abgeleitet wurden, können Nichtschlüsselspalten (eingeschlossene Spalten) in einem nicht gruppierten Index sein, so lange der Datentyp der berechneten Spalte für Nichtschlüssel-Indexspalten zulässig ist.  


## <a name="set-option-requirements"></a>Anforderungen hinsichtlich der SET-Option
  
-   Die ANSI_NULLS-Option auf Verbindungsebene muss auf ON festgelegt sein, wenn die CREATE TABLE- oder ALTER TABLE-Anweisung, die die berechnete Spalte definiert, ausgeführt wird. Die [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md) -Funktion meldet mithilfe der **IsAnsiNullsOn** -Eigenschaft, ob die Option auf ON festgelegt ist.  
-   Für die Verbindung, für die der Index erstellt wird, und für alle Verbindungen, die versuchen, INSERT-, UPDATE- oder DELETE-Anweisungen auszuführen, die Werte des Indexes ändern, müssen sechs SET-Optionen auf ON und eine SET-Option auf OFF festgelegt sein. Der Optimierer ignoriert einen Index für eine berechnete Spalte für alle SELECT-Anweisungen, die von einer Verbindung ausgeführt werden, die diese Optionseinstellungen nicht aufweist.  
  
    -   Die NUMERIC_ROUNDABORT-Option muss auf OFF festgelegt sein, und die folgenden Optionen müssen auf ON festgelegt sein:  
    -   ANSI_NULLS  
    -   ANSI_PADDING  
    -   ANSI_WARNINGS  
    -   ARITHABORT  
    -   CONCAT_NULL_YIELDS_NULL  
    -   QUOTED_IDENTIFIER  
  
> [!NOTE]
> Durch Festlegen von ANSI_WARNINGS auf ON wird implizit ARITHABORT auf ON festgelegt, wenn der Kompatibilitätsgrad der Datenbank auf 90 oder höher festgelegt ist.  
  
## <a name="creating-indexes-on-persisted-computed-columns"></a><a name="BKMK_persisted"></a> Erstellen von Indizes für persistente berechnete Spalten  

Manchmal können Sie eine berechnete Spalte erstellen, die mit einem deterministischen, aber unpräzisen Ausdruck definiert ist. Dies ist möglich, wenn die Spalte in der CREATE TABLE- oder ALTER TABLE-Anweisung als PERSISTED gekennzeichnet ist.

Das bedeutet, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] die berechneten Werte in der Tabelle speichert und sie aktualisiert, wenn andere Spalten, von denen die berechnete Spalte abhängt, aktualisiert werden. [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet diese persistenten Werte, wenn ein Index für die Spalte erstellt wird und wenn in einer Abfrage auf den Index verwiesen wird.

Diese Option ermöglicht Ihnen das Erstellen eines Indexes für eine berechnete Spalte, wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] nicht präzise nachweisen kann, ob eine Funktion, die berechnete Spaltenausdrücke zurückgibt, insbesondere eine CLR-Funktion, die in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]erstellt wurde, sowohl deterministisch als auch präzise ist.  


  
## <a name="related-content"></a>Verwandte Inhalte  
 [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)    
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
  
  
