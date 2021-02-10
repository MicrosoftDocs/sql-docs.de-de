---
description: Aufrufen einer gespeicherten Prozedur als Methode für ein Connection-Objekt
title: Aufrufen einer gespeicherten Prozedur als Methode für ein Verbindungs Objekt | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 35ffdb79-a931-4271-a3bb-0cd804cf173e
author: rothja
ms.author: jroth
ms.openlocfilehash: ee564f90e5a1ff24155eaae5b4613df821138209
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037781"
---
# <a name="calling-a-stored-procedure-as-a-method-on-a-connection-object"></a>Aufrufen einer gespeicherten Prozedur als Methode für ein Connection-Objekt
Eine gespeicherte Prozedur kann so aufgerufen werden, als ob es sich um eine systemeigene Methode auf dem zugeordneten geöffneten **Verbindungs** Objekt handelt. Dies ähnelt dem Aufrufen eines benannten Befehls für das **Verbindungs** Objekt.  
  
 Im folgenden Visual Basic Codebeispiel wird eine gespeicherte Prozedur in der Northwind-Beispieldatenbank mit dem Namen CustOrdersOrders aufgerufen, die hier zur Verfügung gestellt wird.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Im folgenden Codebeispiel wird veranschaulicht, wie eine gespeicherte Prozedur aufgerufen wird, als ob es sich um eine systemeigene Methode auf einem zugeordneten geöffneten **Verbindungs** Objekt handelt.  
  
```  
Const DS = "MySQLServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
Dim objComm As New ADODB.Command  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Set a stored procedure  
  
Set objComm.ActiveConnection = objConn  
  
' Execute the stored procedure on  
' the active connection object...  
'    "ALFKI" is the required input parameter,  
'    objRs is the resultant output variable.  
objComm(1) = "ALFKI"
Set objRs = objComm.Execute

' Display the result.  
Debug.Print "Results returned from sp_CustOrdersOrders for ALFKI: "  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
 Loop  
  
'Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
Set objComm = Nothing  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Connection-Objekt (ADO)](../../reference/ado-api/connection-object-ado.md)
