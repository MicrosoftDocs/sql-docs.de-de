---
title: Rowsets und SQL Server-Cursor (OLE DB-Treiber)
description: Erfahren Sie, wie SQL Server Resultsets an Consumer zurückgibt und wie sich Standardresultsets von Servercursorn im OLE DB-Treiber für SQL Server unterscheiden.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB rowsets, cursors
- OLE DB Driver for SQL Server, rowsets
- rowsets [OLE DB], cursors
- properties [OLE DB]
- cursors [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbcac0300b3abe337e39bccb84a78f74e0de61bb
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88859977"
---
# <a name="rowsets-and-sql-server-cursors"></a>Rowsets und SQL Server-Cursor
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gibt Consumern Resultsets mit zwei Methoden zurück:  
  
-   Standardresultsets mit folgenden Funktionen:  
  
    -   Minimierung der Verwaltung  
  
    -   Maximale Leistung beim Abrufen der Daten  
  
    -   Unterstützung der standardmäßigen schreibgeschützten Vorwärtscursorfunktion  
  
    -   Zeilenweise Rückgabe an den Consumer  
  
    -   Unterstützung von nur jeweils einer aktiven Anweisung für eine Verbindung  
  
         Nachdem eine Anweisung ausgeführt wurde, kann die nächste Anweisung über diese Verbindung erst ausgeführt werden, wenn alle Ergebnisse vom Consumer abgerufen wurden oder die Anweisung abgebrochen wurde.  
  
    -   Unterstützung aller [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisungen  
  
-   Servercursor mit folgenden Funktionen:  
  
    -   Unterstützung aller Cursorfunktionen  
  
    -   Mögliche Rückgabe von Zeilenblöcken an den Consumer  
  
    -   Unterstützung mehrerer aktiver Anweisungen über eine einzelne Verbindung  
  
    -   Abwägen von Cursorfunktionalität gegenüber der Leistung  
  
         Die Unterstützung der Cursorfunktionalität kann die Leistung in Bezug auf ein Standardresultset verringern. Dieser Nachteil kann ausgeglichen werden, wenn der Consumer die Cursorfunktionalität zum Abrufen einer kleineren Zeilenmenge verwenden kann.  
  
    -   Keine Unterstützung einer [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung, die mehr als ein einzelnes Resultset zurückgibt  
  
 Consumer können andere Cursorverhaltensweisen in einem Rowset anfordern, indem sie bestimmte Rowseteigenschaften festlegen. Wenn der Consumer keine Rowseteigenschaft festlegt oder alle Rowseteigenschaften auf den Standardwert setzt, implementiert der OLE DB-Treiber für SQL Server das Rowset mithilfe eines Standardresultsets. Wird eine dieser Eigenschaften nicht auf den Standardwert festgelegt, implementiert der OLE DB-Treiber für SQL Server das Rowset mithilfe eines Servercursors.  
  
 Die folgenden Rowseteigenschaften weisen den OLE DB-Treiber für SQL Server an, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Cursor zu verwenden. Einige Eigenschaften können mit anderen problemlos kombiniert werden. Ein Rowset mit den Eigenschaften DBPROP_IRowsetScroll und DBPROP_IRowsetChange entspricht beispielsweise einem Lesezeichenrowset mit sofortigem Updateverhalten. Andere Eigenschaften schließen sich hingegen gegenseitig aus. Zum Beispiel kann ein Rowset, das DBPROP_OTHERINSERT aufweist, keine Lesezeichen enthalten.  
  
|Eigenschafts-ID|value|Rowsetverhalten|  
|-----------------|-----------|---------------------|  
|DBPROP_SERVERCURSOR|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset ist sequenziell und unterstützt nur den Bildlauf vorwärts und das Abrufen. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.|  
|DBPROP_CANSCROLLBACKWARDS oder DBPROP_CANFETCHBACKWARDS|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt das Durchführen eines Bildlaufs und das Abrufen in beiden Richtungen. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.|  
|DBPROP_BOOKMARKS oder DBPROP_LITERALBOOKMARKS|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset ist sequenziell und unterstützt nur den Bildlauf vorwärts und das Abrufen. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.|  
|DBPROP_OWNUPDATEDELETE oder DBPROP_OWNINSERT oder DBPROP_OTHERUPDATEDELETE|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt das Durchführen eines Bildlaufs und das Abrufen in beiden Richtungen. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.|  
|DBPROP_OTHERINSERT|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt das Durchführen eines Bildlaufs und das Abrufen in beiden Richtungen. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten, wenn ein Index für die Spalten vorhanden ist, auf die verwiesen wird.<br /><br /> DBPROP_OTHERINSERT kann nicht VARIANT_TRUE sein, wenn das Rowset Lesezeichen enthält. Wenn Sie versuchen, ein Rowset mit dieser Visibility-Eigenschaft und Lesezeichen zu erstellen, wird ein Fehler ausgegeben.|  
|DBPROP_IRowsetLocate oder DBPROP_IRowsetScroll|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt das Durchführen eines Bildlaufs und das Abrufen in beiden Richtungen. Lesezeichen und die absolute Positionierung durch die **IRowsetLocate**-Schnittstelle werden im Rowset unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.<br /><br /> DBPROP_IRowsetLocate und DBPROP_IRowsetScroll erfordern Lesezeichen im Rowset. Wenn Sie versuchen, ein Rowset mit Lesezeichen und der DBPROP_OTHERINSERT-Eigenschaft zu erstellen, die auf VARIANT_TRUE festgelegt ist, tritt ein Fehler auf.|  
|DBPROP_IRowsetChange oder DBPROP_IRowsetUpdate|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können über das Rowset aktualisiert werden. Das Rowset ist sequenziell und unterstützt nur den Bildlauf vorwärts und das Abrufen. Die relative Zeilenpositionierung wird unterstützt. Alle Befehle, die für aktualisierbare Cursor verfügbar sind, können diese Schnittstellen unterstützen.|  
|DBPROP_IRowsetLocate oder DBPROP_IRowsetScroll und DBPROP_IRowsetChange oder DBPROP_IRowsetUpdate|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können über das Rowset aktualisiert werden. Das Rowset unterstützt das Durchführen eines Bildlaufs und das Abrufen in beiden Richtungen. Lesezeichen und die absolute Positionierung durch **IRowsetLocate** werden im Rowset unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten.|  
|DBPROP_IMMOBILEROWS|VARIANT_FALSE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt nur den Bildlauf vorwärts. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten, wenn ein Index für die Spalten vorhanden ist, auf die verwiesen wird.<br /><br /> DBPROP_IMMOBILEROWS ist nur in Rowsets verfügbar, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Zeilen anzeigen können, die von Befehlen in anderen Sitzungen oder von anderen Benutzern eingefügt wurden. Wenn Sie versuchen, ein Rowset mit einer auf VARIANT_FALSE festgelegten Eigenschaft für ein Rowset zu öffnen, für das DBPROP_OTHERINSERT nicht VARIANT_TRUE sein kann, tritt ein Fehler auf.|  
|DBPROP_REMOVEDELETED|VARIANT_TRUE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Daten können nicht über das Rowset aktualisiert werden. Das Rowset unterstützt nur den Bildlauf vorwärts. Die relative Zeilenpositionierung wird unterstützt. Befehlstext kann eine ORDER BY-Klausel enthalten, sofern keine Beschränkung durch eine andere Eigenschaft vorliegt.|  
  
 Ein von einem Servercursor unterstütztes Rowset für den OLE DB-Treiber für SQL Server kann problemlos mit der **IOpenRowset::OpenRowset**-Methode in einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Basistabelle oder einer Sicht erstellt werden. Geben Sie die Tabelle oder die Sicht mit Namen an, indem die erforderlichen Eigenschaftensätze für das Rowset im *rgPropertySets*-Parameter übergeben werden.  
  
 Befehlstext, der ein Rowset erstellt, ist eingeschränkt, wenn der Consumer erfordert, dass das Rowset von einem Servercursor unterstützt wird. Der Befehlstext ist insbesondere entweder auf eine einzelne SELECT-Anweisung, die ein einzelnes Rowsetergebnis zurückgibt, oder auf eine gespeicherte Prozedur beschränkt, die eine einzelne SELECT-Anweisung implementiert, mit der ein einzelnes Rowsetergebnis zurückgegeben wird.  
  
 Diese beiden Tabellen zeigen die Zuordnungen verschiedener OLE DB-Eigenschaften sowie die Cursormodelle an. Sie geben außerdem Aufschluss darüber, welche Rowseteigenschaften festgelegt werden sollen, um einen bestimmten Cursormodelltyp zu verwenden.  
  
 Jede Zelle in der Tabelle enthält einen Wert der Rowseteigenschaft für das bestimmte Cursormodell. Der Datentyp aller bereits in diesem Thema aufgeführten Rowseteigenschaft lautet VT_BOOL und der Standardwert ist VARIANT_FALSE. In der Tabelle werden die folgenden Symbole verwendet.  
  
 F = Standardwert (VARIANT_FALSE)  
  
 T = VARIANT_TRUE  
  
 \- = VARIANT_TRUE oder VARIANT_FALSE  
  
 Um ein bestimmtes Cursormodell zu verwenden, suchen Sie die dem Cursormodell entsprechende Spalte, und suchen Sie alle Rowseteigenschaften mit dem Wert "T" in der Spalte. Legen Sie diese Rowseteigenschaften auf VARIANT_TRUE fest, um das gewünschte Cursormodell zu verwenden. Die Rowseteigenschaften mit dem Wert "-" können entweder auf VARIANT_TRUE oder VARIANT_FALSE festgelegt werden.  
  
|Rowseteigenschaften/Cursormodelle|Standard<br /><br /> result<br /><br /> set<br /><br /> (RO)|Schnell<br /><br /> forward-<br /><br /> Machine Learning<br /><br /> (RO)|statischen<br /><br /> (RO)|Keyset<br /><br /> driven<br /><br /> (RO)|  
|--------------------------------------|-------------------------------------------|--------------------------------------------|-----------------------|----------------------------------|  
|DBPROP_SERVERCURSOR|F|T|T|T|  
|DBPROP_DEFERRED|F|F|-|-|  
|DBPROP_IrowsetChange|F|F|F|F|  
|DBPROP_IrowsetLocate|F|F|-|-|  
|DBPROP_IrowsetScroll|F|F|-|-|  
|DBPROP_IrowsetUpdate|F|F|F|F|  
|DBPROP_BOOKMARKS|F|F|-|-|  
|DBPROP_CANFETCHBACKWARDS|F|F|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|F|F|-|-|  
|DBPROP_CANHOLDROWS|F|F|-|-|  
|DBPROP_LITERALBOOKMARKS|F|F|-|-|  
|DBPROP_OTHERINSERT|F|T|F|F|  
|DBPROP_OTHERUPDATEDELETE|F|T|F|T|  
|DBPROP_OWNINSERT|F|T|F|T|  
|DBPROP_OWNUPDATEDELETE|F|T|F|T|  
|DBPROP_QUICKSTART|F|F|-|-|  
|DBPROP_REMOVEDELETED|F|F|F|-|  
|DBPROP_IrowsetResynch|F|F|F|-|  
|DBPROP_CHANGEINSERTEDROWS|F|F|F|F|  
|DBPROP_SERVERDATAONINSERT|F|F|F|-|  
|DBPROP_UNIQUEROWS|-|F|F|F|  
|DBPROP_IMMOBILEROWS|-|-|-|T|  
  
|Rowseteigenschaften/Cursormodelle|Dynamic (RO)|Keyset (R/W)|Dynamic (R/W)|  
|--------------------------------------|--------------------|---------------------|----------------------|  
|DBPROP_SERVERCURSOR|T|T|T|  
|DBPROP_DEFERRED|-|-|-|  
|DBPROP_IrowsetChange|F|-|-|  
|DBPROP_IrowsetLocate|F|-|F|  
|DBPROP_IrowsetScroll|F|-|F|  
|DBPROP_IrowsetUpdate|F|-|-|  
|DBPROP_BOOKMARKS|F|-|F|  
|DBPROP_CANFETCHBACKWARDS|-|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|-|-|-|  
|DBPROP_CANHOLDROWS|F|-|F|  
|DBPROP_LITERALBOOKMARKS|F|-|F|  
|DBPROP_OTHERINSERT|T|F|T|  
|DBPROP_OTHERUPDATEDELETE|T|T|T|  
|DBPROP_OWNINSERT|T|T|T|  
|DBPROP_OWNUPDATEDELETE|T|T|T|  
|DBPROP_QUICKSTART|-|-|-|  
|DBPROP_REMOVEDELETED|T|-|T|  
|DBPROP_IrowsetResynch|-|-|-|  
|DBPROP_CHANGEINSERTEDROWS|F|-|F|  
|DBPROP_SERVERDATAONINSERT|F|-|F|  
|DBPROP_UNIQUEROWS|F|F|F|  
|DBPROP_IMMOBILEROWS|F|T|F|  
  
 Für einen spezifischen Satz von Rowseteigenschaften wird das Cursormodell, das ausgewählt wird, wie folgt bestimmt.  
  
 Rufen Sie von der angegebenen Auflistung von Rowseteigenschaften eine Teilmenge von den in den vorherigen Tabellen aufgelisteten Eigenschaften ab. Teilen Sie diese Eigenschaften abhängig vom Flagwert – erforderlich (T, F) oder optional (-) – der jeweiligen Rowseteigenschaften in zwei Untergruppen auf. Beginnen Sie für die einzelnen Cursormodelle in der ersten Tabelle, und gehen Sie von links nach rechts vor. Vergleichen Sie die Werte der Eigenschaften in den beiden Untergruppen mit den Werten der entsprechenden Eigenschaften in dieser Spalte. Das Cursormodell, für das keine fehlende Übereinstimmung mit den erforderlichen Eigenschaften und die geringste Zahl fehlender Übereinstimmungen mit den optionalen Eigenschaften gefunden wird, wird ausgewählt. Wenn dies auf mehr als ein Cursormodell zutrifft, wird das am weitesten links stehende Modell ausgewählt.  
  
## <a name="sql-server-cursor-block-size"></a>Blockgröße des SQL Server-Cursors  
 Wenn ein [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Cursor ein Rowset für den OLE DB-Treiber für SQL Server unterstützt, wird die Blockgröße des Cursors durch die Anzahl der Elemente im Arrayparameter des Zeilenhandles der **IRowset::GetNextRows**- oder der **IRowsetLocate::GetRowsAt**-Methode definiert. Die von den Handles im Array angegebenen Zeilen sind die Elemente des Cursorblocks.  
  
 Für Rowsets, die Lesezeichen unterstützen, werden die Elemente des Cursorblocks durch die mit der **IRowsetLocate::GetRowsByBookmark**-Methode abgerufenen Zeilenhandles definiert.  
  
 Der Cursorblock ist unabhängig von der Methode, die zum Auffüllen des Rowsets und zur Bildung des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Cursorblocks verwendet wird, aktiv, bis die nächste Methode zum Abrufen von Zeilen für das Rowset ausgeführt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Rowsets](../../oledb/ole-db-rowsets/rowsets.md)  
  
  
