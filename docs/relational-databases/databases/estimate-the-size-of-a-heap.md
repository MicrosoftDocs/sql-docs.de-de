---
title: Schätzen der Größe eines Heaps | Microsoft-Dokumentation
description: Verwenden Sie dieses Verfahren, um einzuschätzen, wie viel Speicherplatz zum Speichern von Daten in einem Heap in SQL Server erforderlich ist.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- disk space [SQL Server], indexes
- estimating heap size
- size [SQL Server], heap
- space [SQL Server], indexes
- heaps
ms.assetid: 81fd5ec9-ce0f-4c2c-8ba0-6c483cea6c75
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31a157c04afc4890c8818a118c83f5a3c5458e84
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474111"
---
# <a name="estimate-the-size-of-a-heap"></a>Schätzen der Größe eines Heaps
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Mit den folgenden Schritten können Sie den Umfang des Speicherplatzes schätzen, der zum Speichern von Daten in einem Heap erforderlich ist:  
  
1.  Geben Sie die Anzahl der Zeilen an, die die Tabelle enthalten wird:  
  
     **_Num_Rows_**  = Anzahl der Zeilen in der Tabelle  
  
2.  Geben Sie die Anzahl der Spalten mit fester und mit variabler Länge an, und berechnen Sie den Speicherplatz, der für deren Speicherung erforderlich ist:  
  
     Berechnen Sie den Speicherplatz, der zum Speichern jeder dieser Spaltengruppen innerhalb der Datenzeile erforderlich ist. Die Größe einer Spalte hängt von der Angabe für Datentyp und -länge ab.  
  
     **_Num_Cols_**  = Gesamtanzahl der Spalten (mit fester und variabler Länge)  
  
     **_Fixed_Data_Size_**  = Gesamtzahl der Bytes in allen Spalten fester Länge  
  
     **_Num_Variable_Cols_**  = Anzahl der Spalten variabler Länge  
  
     **_Max_Var_Size_**  = Maximale gesamte Bytegröße aller Spalten variabler Länge  
  
3.  Ein Teil der Zeile, der als NULL-Bitmuster (Null_Bitmap) bezeichnet wird, wird für das Verwalten der NULL-Zulässigkeit der Spalte reserviert. Berechnen Sie dessen Größe:  
  
     **_Null_Bitmap_**  = 2 + ((**_Num_Cols_** + 7) / 8)  
  
     Nur der ganzzahlige Teil dieses Ausdrucks darf verwendet werden. Der Rest wird verworfen.  
  
4.  Berechnen Sie die Größe der Daten variabler Länge:  
  
     Wenn die Tabelle Spalten variabler Länge enthält, müssen Sie den Speicherplatz ermitteln, der von den Spalten innerhalb der Zeile verwendet wird:  
  
     **_Variable_Data_Size_**  = 2 + (**_Num_Variable_Cols_** × 2) + **_Max_Var_Size_**  
  
     Die zu **_Max_Var_Size_** hinzugefügten Bytes dienen der Nachverfolgung der einzelnen Spalten mit variabler Länge. Bei dieser Formel wird angenommen, dass alle Spalten variabler Länge zu 100 % gefüllt sind. Wenn sich abzeichnet, dass ein niedrigerer Prozentsatz des Speicherplatzes für Spalten variabler Länge verwendet wird, können Sie den Wert **_Max_Var_Size_** mithilfe dieses Prozentsatzes anpassen, um einen genaueren Schätzwert für die Gesamtgröße der Tabelle zu erhalten.  
  
    > [!NOTE]  
    >  Sie können **varchar**-, **nvarchar**-, **varbinary**- oder **sql_variant** -Spalten kombinieren, mit dem Ergebnis, dass die definierte Tabellengesamtbreite größer als 8.060 Byte ist. Die Länge jeder einzelnen Spalte unterliegt auch weiterhin der Beschränkung von 8.000 Byte für eine Spalte des Typs **varchar**, **nvarchar, varbinary** oder **sql_variant**. Die kombinierte Breite kann jedoch den Grenzwert von 8.060 Byte in einer Tabelle überschreiten.  
  
     Wenn keine Spalten variabler Länge vorhanden sind, legen Sie **_Variable_Data_Size_** auf 0 fest.  
  
5.  Berechnen Sie die Gesamtzeilenlänge:  
  
     **_Row_Size_**  = **_Fixed_Data_Size_** + **_Variable_Data_Size_** + **_Null_Bitmap_** + 4  
  
     Der Wert 4 in der Formel ist der Zeilenüberschriftenaufwand der Datenzeile.  
  
6.  Berechnen Sie die Anzahl der Zeilen pro Seite (8.096 freie Byte pro Seite):  
  
     **_Rows_Per_Page_**  = 8.096 / (**_Row_Size_** + 2)  
  
     Da sich eine Zeile nicht auf zwei Seiten erstreckt, muss die Anzahl der Zeilen pro Seite auf die nächste ganze Zeile abgerundet werden. Die Angabe 2 in der Formel bezieht sich auf den Eingang der Zeile in das Slotarray der Seite.  
  
7.  Berechnen Sie die Anzahl der Seiten, die zum Speichern aller Zeilen benötigt werden:  
  
     **_Num_Pages_**  = **_Num_Rows_** / **_Rows_Per_Page_**  
  
     Die geschätzte Seitenanzahl muss auf die nächste ganze Seite aufgerundet werden.  
  
8.  Berechnen Sie den Umfang des Speicherplatzes, der zum Speichern der Daten im Heap erforderlich ist (insgesamt 8.192 Byte pro Seite):  

     Heapgröße (Bytes) = 8.192 × **_Num_Pages_**  
  
 In dieser Berechnung wird Folgendes nicht berücksichtigt:  
  
-   Partitionierung  
  
     Der Speicherplatzaufwand aus der Partitionierung ist geringfügig, seine Berechnung jedoch komplex. Er muss nicht berücksichtigt werden.  
  
-   Zuordnungsseiten  
  
     Mindestens eine IAM-Seite wird zum Nachverfolgen der für einen Heap zugeordneten Seiten verwendet, der Speicherplatzaufwand ist jedoch nur minimal. Außerdem ist kein Algorithmus verfügbar, um deterministisch genau zu berechnen, wie viele IAM-Seiten verwendet werden.  
  
-   LOB-Werte (Large Object)  
  
     Der Algorithmus zum Berechnen des genauen Speicherplatzes, der zum Speichern der Werte der LOB-Datentypen **varchar(max)** , **varbinary(max)** , **nvarchar(max)** , **text**, **ntextxml** und **image** erforderlich ist, ist komplex. Es reicht aus, lediglich die Durchschnittsgröße der erwarteten LOB-Werte zu addieren und diesen Wert zur Heapgesamtgröße zu addieren.  
  
-   Komprimierung  
  
     Sie können die Größe eines komprimierten Heaps nicht im Voraus berechnen.  
  
-   Spalten mit geringer Dichte  
  
     Informationen zu den Speicherplatzanforderungen von Sparsespalten finden Sie unter [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heaps &#40;Tabellen ohne gruppierte Indizes&#41;](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md)   
 [Beschreibung von gruppierten und nicht gruppierten Indizes](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [Erstellen gruppierter Indizes](../../relational-databases/indexes/create-clustered-indexes.md)   
 [Erstellen nicht gruppierter Indizes](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [Schätzen der Größe einer Tabelle](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [Schätzen der Größe eines gruppierten Indexes](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Schätzen der Größe eines nicht gruppierten Index](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [Schätzen der Größe einer Datenbank](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
