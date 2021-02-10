---
description: Empfangen von mehreren Recordsets
title: Empfangen von mehreren Recordsets | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- receiving multiple Recordsets [ADO]
- Recordset object [ADO], receiving multiple Recordsets
ms.assetid: 2a7ad7a6-f00d-4355-b0b5-d0ab957b0566
author: rothja
ms.author: jroth
ms.openlocfilehash: d0b4213b70498ea021b2267748c0f6fe3c6adf5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037120"
---
# <a name="receiving-multiple-recordsets"></a>Empfangen von mehreren Recordsets
Der [Microsoft OLE DB-Anbieter für SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) unterstützt die Rückgabe mehrerer **Recordset** -Objekte für einen einzelnen Befehl, der mehrere SQL-Anweisungen enthält, ein **Recordset** pro SQL-Anweisung. Die Reihenfolge, in der die **Recordsets** zurückgegeben werden, folgt der Reihenfolge, in der die SQL-Anweisungen in den Befehls Text eingefügt werden.  
  
 Der Microsoft OLE DB-Anbieter für SQL Server gibt auch mehrere Resultsets an ADO zurück, wenn der Befehl eine COMPUTE-Klausel enthält. Beispielsweise gibt ein Befehl, der die folgende SQL-Anweisung enthält, die Ergebnisse in zwei **Recordset** -Objekten zurück: eine für das Rowset von (*ProductID*, *ProductName*, *UnitPrice*) und die andere für den Durchschnittspreis aller Produkte in der Tabelle.  
  
```  
SELECT ProductID, ProductName, UnitPrice   
  FROM PRODUCTS   
  COMPUTE AVG(UnitPrice)  
```  
  
 Sie können die **Recordset. NextRecordset** -Methode verwenden, um die beiden-Objekte aufzulisten.  
  
 Weitere Informationen finden Sie unter [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md).
