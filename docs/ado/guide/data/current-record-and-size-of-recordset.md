---
description: Aktueller Datensatz und Größe des Recordsets
title: Aktueller Datensatz und Größe des Recordsets | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e63ff331-8655-4be7-82c6-e6cd6cc9d16d
author: rothja
ms.author: jroth
ms.openlocfilehash: 791ca36a7f5a61a0ad0208753386fba826d6d06e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033340"
---
# <a name="current-record-and-size-of-recordset"></a>Aktueller Datensatz und Größe des Recordsets
In diesem Abschnitt wird beschrieben, wie Sie die aktuelle Position des Cursors im Beispiel **Recordset** in [JScript-Code Beispiel zum Zurückgeben eines Recordsets](./jscript-code-example-to-return-a-recordset.md)suchen.  
  
## <a name="current-record"></a>Aktueller Datensatz  
 Der aktuelle Datensatz im Dataset entspricht dem, der von der Position des Cursors des **Recordset** -Objekts hervorgehoben wird. Wenn ein **Recordset** -Objekt von der Datenquelle zurückgegeben wird, als das Ergebnis des Aufrufs von **Recordset. Open**, **Command.Exeniedlich** oder **Connection.Exeniedlich** (einschließlich **Connection. namedcommand** und **Connection. StoredProcedure**), wird der Cursor so festgelegt, dass er auf den ersten Datensatz verweist. Im Beispiel DataSet ist der anfängliche aktuelle Datensatz der "Onkel Bob es Organic getrocknete Birnen"-Element.  
  
## <a name="size-of-recordset"></a>Größe des Recordsets  
 Um die Größe eines **Recordset** -Objekts zu ermitteln, können Sie den Wert der **Recordset. RecordCount** -Eigenschaft abrufen. Dieser Wert ist eine lange ganze Zahl, die die Anzahl der Datensätze im **Recordset** angibt. Wenn das Dataset für Microsoft SQL Server vom OLE DB-Anbieter zurückgegeben wird, gibt dieser Wert die Anzahl der zurückgegebenen Zeilen zurück. Das Lesen der **RecordCount** -Eigenschaft für ein geschlossenes **Recordset** verursacht einen Fehler.  
  
 Wenn die Anzahl der Datensätze nicht bestimmt werden kann, ist der Wert der-Eigenschaft-1.  
  
 Der Wert der **RecordCount** -Eigenschaft hängt auch von den Funktionen des Anbieters und dem verwendeten Cursortyp ab. Bei einem Vorwärts Cursor ist der Wert-1. Bei einem statischen oder Keysetcursor entspricht der Wert der tatsächlichen Anzahl von Datensätzen, die im **Recordset** -Objekt zurückgegeben werden. Bei einem dynamischen Cursor ist der Wert entweder-1 oder die tatsächliche Anzahl von Datensätzen, abhängig von der Datenquelle.  
  
 Ein Cursor, der **RecordCount** unterstützt, muss schwieriger arbeiten und erfordert daher eine höhere Rechenleistung, als ein Cursor **RecordCount** nicht unterstützt. Wenn Sie die Anzahl der Datensätze nicht kennen müssen, können Sie mithilfe eines anderen Cursor Typs die Leistung Ihrer Anwendung verbessern, insbesondere, wenn Sie ein großes Dataset verwenden müssen.  
  
 In einigen Fällen kann ein Anbieter oder Cursor den **RecordCount** -Wert nicht ermitteln, ohne zuerst alle Datensätze aus der Datenquelle abzurufen. Um eine genaue Zählung sicherzustellen, nennen Sie das **Recordset**. Die "" **-Methode vor** dem Aufruf von " **Recordset. RecordCount**".  
  
 Das **Recordset** -Beispiel Objekt, das mit dem [JScript-Code Beispiel](./jscript-code-example-to-return-a-recordset.md) abgerufen wird, verwendet einen Vorwärts Cursor, sodass der Aufruf von **RecordCount** für dieses Objekt immer zu "-1" führt. Wenn Sie die Codezeile ändern, die das **Recordset** aufruft. **Open** -Methode wie im folgenden Beispiel gezeigt, gibt die **RecordCount** -Eigenschaft die tatsächliche Anzahl der abgerufenen Datensätze zurück.  
  
```  
oRs.Open sSQL, sCnStr, adOpenStatic, adLockOptimistic, adCmdText   
```  
  
 Dies liegt daran, dass statische Cursor mit dem [Microsoft OLE DB-Anbieter für SQL Server](../appendixes/microsoft-ole-db-provider-for-sql-server.md) **RecordCount** unterstützen. In diesem Beispiel gibt es fünf Datensätze, sodass **RecordCount** den Wert 5 ergeben sollte.  
  
 Dieser Abschnitt enthält das folgende Thema.  
  
 [Grenzen eines Recordsets](./boundaries-of-a-recordset.md)