---
description: Verwenden eines Recordset-Objekts
title: Verwenden eines Recordset-Objekts | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 01c630d8-eb35-4bd0-a99f-7c0f85316cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 44fc7a242887419ba3013113632c8d1acd2a3337
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032302"
---
# <a name="using-a-recordset-object"></a>Verwenden eines Recordset-Objekts
Alternativ können Sie " **Recordset. Open** " verwenden, um implizit eine Verbindung herzustellen und in einem einzelnen Vorgang einen Befehl über diese Verbindung auszugeben. Beispielsweise in Visual Basic:  
  
```  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
  
' Create and Open the Recordset object.  
Set oRs = New ADODB.Recordset  
oRs.CursorLocation = adUseClient  
oRs.Open sSQL, sConn, adOpenStatic, _  
               adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
oRs.Close          
Set oRs = Nothing  
```  
  
 Beachten Sie, dass **ORS. Open** eine Verbindungs Zeichenfolge (*sConn*) anstelle eines **Verbindungs** Objekts (*oconn*) als Wert des **ActiveConnection** -Parameters verwendet. Außerdem wird der Client seitige Cursortyp erzwungen, indem die **CursorLocation** -Eigenschaft für das **Recordset** -Objekt festgelegt wird. Vergleichen Sie dies wieder mit dem **HelloData** -Beispiel.
