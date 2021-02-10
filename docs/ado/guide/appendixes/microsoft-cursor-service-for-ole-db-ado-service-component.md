---
description: Microsoft-Cursor Dienst für OLE DB (ADO-Dienst Komponente)
title: Microsoft-Cursor Dienst für OLE DB (ADO-Dienst Komponente) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], cursor service for OLE DB
- cursor service for OLE DB [ADO]
ms.assetid: 420d0989-7cfb-4c66-a7b5-f4199d13165d
author: rothja
ms.author: jroth
ms.openlocfilehash: 12224a64ffa8d67b212deacd3bd8794d6519cfb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029384"
---
# <a name="microsoft-cursor-service-for-ole-db-overview"></a>Übersicht über den Microsoft-Cursor Dienst für OLE DB
Der Microsoft-Cursor Dienst für OLE DB ergänzt die Cursor Unterstützungsfunktionen von Datenanbietern. Folglich nimmt der Benutzer eine relativ einheitliche Funktionalität von allen Datenanbietern an.

 Der Cursor Dienst stellt dynamische Eigenschaften zur Verfügung und verbessert das Verhalten bestimmter Methoden. Die Eigenschaft dynamische [Optimierung](../../reference/ado-api/optimize-property-dynamic-ado.md) ermöglicht beispielsweise das Erstellen temporärer Indizes, um bestimmte Vorgänge, wie z. b. die [Find](../../reference/ado-api/find-method-ado.md) -Methode, zu vereinfachen.

 Der Cursor Dienst ermöglicht die Unterstützung für Batch Aktualisierungen in allen Fällen. Außerdem simuliert Sie mehr fähige Cursor Typen, z. b. dynamische Cursor, wenn ein Datenanbieter nur weniger fähige Cursor bereitstellen kann, z. b. statische Cursor.

## <a name="keyword"></a>Schlüsselwort
 Um diese Dienst Komponente aufzurufen, legen Sie die [Cursor Location](../../reference/ado-api/cursorlocation-property-ado.md) -Eigenschaft des [Recordsets](../../reference/ado-api/recordset-object-ado.md) oder des [Verbindungs](../../reference/ado-api/connection-object-ado.md) Objekts auf **adUseClient** fest.

```vb
connection.CursorLocation=adUseClient
recordset.CursorLocation=adUseClient
```

## <a name="dynamic-properties"></a>Dynamische Eigenschaften
 Wenn der Cursor Dienst für OLE DB aufgerufen wird, werden die folgenden dynamischen Eigenschaften der [Properties](../../reference/ado-api/properties-collection-ado.md) -Auflistung des **Recordset** -Objekts hinzugefügt. Die vollständige Liste der dynamischen Eigenschaften für das **Verbindungs** -und **Recordset** -Objekt wird im ADO.net- [Eigenschafts Index](../../reference/ado-api/ado-dynamic-property-index.md)aufgeführt. Die zugeordneten OLE DB Eigenschaftsnamen werden nach Bedarf in Klammern nach dem ADO-Eigenschaftsnamen eingeschlossen.

 Änderungen an dynamischen Eigenschaften sind für die zugrunde liegende Datenquelle nicht sichtbar, nachdem der Cursor Dienst aufgerufen wurde. Beispielsweise ist das Festlegen der Eigenschaft *Befehl* Timeout für ein **Recordset** für den zugrunde liegenden Datenanbieter nicht sichtbar.

```vb

Recordset1.CursorLocation = adUseClient     'invokes cursor service
Recordset1.Open "authors", _
    "Provider=SQLOLEDB;Data Source=DBServer;User Id=MyUserID;" & _
    "Password=MyPassword;Initial Catalog=pubs;",,adCmdTable
Recordset1.Properties.Item("Command Time out") = 50
' 'Command Time out' property on DBServer is still default (30).

```

 Wenn die Anwendung den Cursor Dienst erfordert, Sie aber dynamische Eigenschaften für den zugrunde liegenden Anbieter festlegen müssen, legen Sie die Eigenschaften vor dem Aufrufen des Cursor Dienstanbieters fest. Befehls Objekt-Eigenschaften Einstellungen werden unabhängig von der Cursorposition immer an den zugrunde liegenden Datenanbieter übermittelt. Daher können Sie auch ein Command-Objekt verwenden, um die Eigenschaften zu einem beliebigen Zeitpunkt festzulegen.

> [!NOTE]
>  Die dynamische Eigenschaft DBPROP_SERVERDATAONINSERT wird vom Cursor Dienst nicht unterstützt, auch wenn Sie vom zugrunde liegenden Datenanbieter unterstützt wird.

|Eigenschaftenname|BESCHREIBUNG|
|-------------------|-----------------|
|Automatische Neuberechnung (DBPROP_ADC_AUTORECALC)|Bei mit dem Daten Strukturierungs Dienst erstellten Recordsets gibt dieser Wert an, wie oft berechnete und Aggregat Spalten berechnet werden. Der Standardwert (Wert = 1) ist die Neuberechnung, wenn der Daten Strukturierungs Dienst festlegt, dass sich die Werte geändert haben. Wenn der Wert 0 ist, werden die berechneten oder Aggregat Spalten nur bei der anfänglichen Erstellung der Hierarchie berechnet.|
|Batch Größe (DBPROP_ADC_BATCHSIZE)|Gibt die Anzahl der Update-Anweisungen an, die als Batch verarbeitet werden können, bevor Sie an den Datenspeicher gesendet werden. Je mehr Anweisungen in einem Batch, desto weniger Roundtrips zum Datenspeicher.|
|Untergeordnete Zeilen Zwischenspeichern (DBPROP_ADC_CACHECHILDROWS)|Bei Recordsets, die mit dem Daten Strukturierungs Dienst erstellt wurden, gibt dieser Wert an, ob untergeordnete Recordsets zur späteren Verwendung in einem Cache gespeichert werden.|
|Cursor-Engine-Version (DBPROP_ADC_CEVER)|Gibt die Version des verwendeten Cursor Dienstanbieter an.|
|Beibehalten des Änderungs Status (DBPROP_ADC_MAINTAINCHANGESTATUS)|Gibt den Text des Befehls an, der zum erneuten Synchronisieren einer oder mehrerer Zeilen in einem Join mehrerer Tabellen verwendet wird.|
|[Optimieren](../../reference/ado-api/optimize-property-dynamic-ado.md)|Gibt an, ob ein Index erstellt werden soll. Wenn diese Einstellung auf " **true**" festgelegt ist, wird die temporäre Erstellung von Indizes autorisiert, um die Ausführung bestimmter Vorgänge zu verbessern.|
|[Name der erneuten Form](../../reference/ado-api/reshape-name-property-dynamic-ado.md)|Gibt den Namen des **Recordsets** an. Kann innerhalb der aktuellen oder nachfolgenden Daten Strukturierungs Befehle referenziert werden.|
|[Befehl zum erneuten Synchronisieren](../../reference/ado-api/resync-command-property-dynamic-ado.md)|Gibt eine benutzerdefinierte Befehls Zeichenfolge an, die von der [Resync](../../reference/ado-api/resync-method.md) -Methode verwendet wird, wenn die [Unique Table](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) -Eigenschaft aktiviert ist.|
|[Eindeutiger Katalog](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Gibt den Namen der Datenbank an, die die Tabelle enthält, auf die in der **Unique Table** -Eigenschaft verwiesen wird.|
|[Eindeutiges Schema](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Gibt den Namen des Besitzers der Tabelle an, auf die in der **Unique Table** -Eigenschaft verwiesen wird.|
|[Eindeutige Tabelle](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Gibt den Namen der einer Tabelle in einem **Recordset** an, das aus mehreren Tabellen erstellt wurde, die durch Einfügungen, Aktualisierungen oder Löschungen geändert werden können.|
|Update Kriterien (DBPROP_ADC_UPDATECRITERIA)|Gibt an, welche Felder in der **Where** -Klausel verwendet werden, um Konflikte zu behandeln, die während eines Updates auftreten.|
|[Neusynchronisierung aktualisieren](../../reference/ado-api/update-resync-property-dynamic-ado.md) (DBPROP_ADC_UPDATERESYNC)|Gibt an, ob die **Resync** -Methode nach der [UpdateBatch](../../reference/ado-api/updatebatch-method.md) -Methode (und ihrem Verhalten) implizit aufgerufen wird, wenn die **eindeutige Tabellen** Eigenschaft wirksam ist.|

 Sie können auch eine dynamische Eigenschaft festlegen oder abrufen, indem Sie Ihren Namen als Index für die **Properties** -Sammlung angeben. Beispielsweise können Sie den aktuellen Wert der dynamischen Eigenschaft [optimieren](../../reference/ado-api/optimize-property-dynamic-ado.md) und dann wie folgt einen neuen Wert festlegen:

```vb
Debug.Print rs.Properties("Optimize")
rs.Properties("Optimize") = True
```

## <a name="built-in-property-behavior"></a>Integriertes Eigenschafts Verhalten
 Der Cursor Dienst für OLE DB wirkt sich auch auf das Verhalten bestimmter integrierter Eigenschaften aus.

|Eigenschaftenname|BESCHREIBUNG|
|-------------------|-----------------|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|Ergänzt die Typen von Cursorn, die für ein **Recordset** verfügbar sind.|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|Ergänzt die Typen von Sperren, die für ein **Recordset** verfügbar sind. Aktiviert Batch Updates.|
|[Sort](../../reference/ado-api/sort-property.md)|Gibt einen oder mehrere Feldnamen an, nach denen das **Recordset** sortiert ist, und gibt an, ob die einzelnen Felder in aufsteigender oder absteigender Reihenfolge sortiert sind.|

## <a name="method-behavior"></a>Methoden Verhalten
 Der Cursor Dienst für OLE DB aktiviert oder beeinflusst das Verhalten der [Append](../../reference/ado-api/append-method-ado.md) -Methode des [Feld](../../reference/ado-api/field-object.md) Objekts. und die Methoden " [Open](../../reference/ado-api/open-method-ado-recordset.md)", " [Resync](../../reference/ado-api/resync-method.md)", " [UpdateBatch](../../reference/ado-api/updatebatch-method.md)" und " [Save](../../reference/ado-api/save-method.md) " des **Recordset** -Objekts.