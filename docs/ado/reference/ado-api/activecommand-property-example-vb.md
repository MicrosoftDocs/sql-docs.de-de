---
description: ActiveCommand-Eigenschaft – Beispiel (VB)
title: ActiveCommand-Eigenschaft (Beispiel) (VB) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- ActiveCommand property [ADO], Visual Basic example
ms.assetid: 23b06499-62df-4f46-88eb-6da392f9b456
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ad18d73acedbbee77157337606ca58bc7569f97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031623"
---
# <a name="activecommand-property-example-vb"></a>ActiveCommand-Eigenschaft – Beispiel (VB)
In diesem Beispiel wird die [ActiveCommand](./activecommand-property-ado.md) -Eigenschaft veranschaulicht.  
  
 Einer Unterroutine wird ein [Recordset](./recordset-object-ado.md) -Objekt zugewiesen, dessen **ActiveCommand** -Eigenschaft verwendet wird, um den Befehls Text und den Parameter anzuzeigen, mit dem das **Recordset** erstellt wurde.  
  
```  
'BeginActiveCommandVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
        'recordset and connection variables  
    Dim cmd As ADODB.Command  
    Dim rst As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
        'record variables  
    Dim strPrompt As String  
    Dim strName As String  
  
    Set Cnxn = New ADODB.Connection  
    Set cmd = New ADODB.Command  
  
    strPrompt = "Enter an author's name (e.g., Ringer): "  
    strName = Trim(InputBox(strPrompt, "ActiveCommandX Example"))  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
  
        'create SQL command string  
    cmd.CommandText = "SELECT * FROM Authors WHERE au_lname = ?"  
    cmd.Parameters.Append cmd.CreateParameter("LastName", adChar, adParamInput, 20, strName)  
  
    Cnxn.Open strCnxn  
    cmd.ActiveConnection = Cnxn  
  
        'create the recordset by executing command string  
    Set rst = cmd.Execute(, , adCmdText)  
        'see the results  
    Call ActiveCommandXprint(rst)  
  
    ' clean up  
    Cnxn.Close  
    Set rst = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rst Is Nothing Then  
        If rst.State = adStateOpen Then rst.Close  
    End If  
    Set rst = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndActiveCommandVB  
```  
  
 Die **activecommandxprint** -Routine erhält nur ein **Recordset** -Objekt, aber Sie muss den Befehls Text und den Parameter ausgeben, mit dem das **Recordset** erstellt wurde. Dies ist möglich, da die **ActiveCommand** -Eigenschaft des **Recordset** -Objekts das zugeordnete [Befehls](./command-object-ado.md) Objekt ergibt.  
  
 Die [CommandText](./commandtext-property-ado.md) -Eigenschaft des **Befehls** Objekts gibt den parametrisierten Befehl aus, mit dem das **Recordset** erstellt wurde. Die [Parameter](./parameters-collection-ado.md) Auflistung des **Befehls** Objekts gibt den Wert zurück, der für den Parameter Platzhalter ("**?**") des Befehls ersetzt wurde.  
  
 Zum Schluss werden eine Fehlermeldung oder der Name und die ID des Autors ausgegeben.  
  
```  
'BeginActiveCommandPrintVB  
Public Sub ActiveCommandXprint(rstp As ADODB.Recordset)  
  
    Dim strName As String  
  
    strName = rstp.ActiveCommand.Parameters.Item("LastName").Value  
  
    Debug.Print "Command text = '"; rstp.ActiveCommand.CommandText; "'"  
    Debug.Print "Parameter = '"; strName; "'"  
  
    If rstp.BOF = True Then  
       Debug.Print "Name = '"; strName; "', not found."  
    Else  
       Debug.Print "Name = '"; rstp!au_fname; " "; rstp!au_lname; _  
             "', author ID = '"; rstp!au_id; "'"  
    End If  
  
    rstp.Close  
    Set rstp = Nothing  
End Sub  
'EndActiveCommandPrintVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ActiveCommand-Eigenschaft (ADO)](./activecommand-property-ado.md)   
 [Command-Objekt (ADO)](./command-object-ado.md)   
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)