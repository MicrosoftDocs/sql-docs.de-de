---
description: Delete-Methode – Beispiel (VBScript)
title: Delete-Methode (Beispiel) (VBScript) | Microsoft-Dokumentation
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
- Delete method [ADO], VBScript example
ms.assetid: 78935d6d-1c1a-4306-a83a-1763210c69f9
author: rothja
ms.author: jroth
ms.openlocfilehash: 28f9eb55b4829331f6aea26d75989b07c70232aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034390"
---
# <a name="delete-method-example-vbscript"></a>Delete-Methode – Beispiel (VBScript)
In diesem Beispiel wird die [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md) -Methode verwendet, um einen angegebenen Datensatz aus einem [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)zu entfernen.  
  
 Verwenden Sie das folgende Beispiel in einer Active Server Seite (ASP). Um dieses voll funktionsfähige Beispiel anzuzeigen, müssen Sie entweder die Datenquelle "AdvWorks. mdb" (installiert mit dem SDK) im Verzeichnis "c:\Programme\Microsoft Platform sdk\samples\dataaccess\rds\rdstest\advworks.mdb" aufweisen oder den Pfad im Beispielcode bearbeiten, um den tatsächlichen Speicherort dieser Datei anzugeben Dies ist eine Microsoft Access-Datenbankdatei.  
  
 Verwenden Sie **Suchen** , um die Datei adovsb. Inc zu suchen, und platzieren Sie Sie in dem Verzeichnis, das Sie verwenden möchten. Schneiden Sie den folgenden Code aus, und fügen Sie ihn in Editor oder einen anderen Text-Editor ein, und speichern Sie ihn als **deletevsb. ASP**. Sie können das Ergebnis in einem beliebigen Client Browser anzeigen.  
  
 Verwenden Sie zum Ausführen des Beispiels zuerst das [AddNew](../../../ado/reference/ado-api/addnew-method-example-vbscript.md) -Beispiel, um einige Datensätze hinzuzufügen. Anschließend können Sie versuchen, Sie zu löschen. Zeigen Sie das Ergebnis in einem beliebigen Client Browser an.  
  
```  
<!-- BeginDeleteVBS -->  
<%@ Language=VBScript %>  
<% ' use this meta tag instead of ADOVBS.inc%>  
<!-- METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4"  -->  
<HTML>  
<HEAD>  
<TITLE>ADO Delete Method</TITLE>  
</HEAD>  
<STYLE>  
<!--  
TH {  
   background-color: #008080;   
   font-family: 'Arial Narrow','Arial',sans-serif;   
   font-size: xx-small;  
   color: white;  
   }  
TD {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Arial Narrow','Arial',sans-serif;   
   font-size: xx-small;  
    }  
-->  
</STYLE>  
  
<BODY>   
<H3>ADO Delete Method</H3>  
  
<%  
    ' to integrate this code replace the DataSource value in the connection string  
  
    ' connection and recordset variables  
   Dim Cnxn, strCnxn  
   Dim rsCustomers, strSQLCustomers  
  
    ' create and open connection  
   Set Cnxn = Server.CreateObject("ADODB.Connection")   
   strCnxn="Provider='sqloledb';Data Source=" & _  
            Request.ServerVariables("SERVER_NAME") & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
   Cnxn.Open  strCnxn  
    ' create and open recordset  
   Set rsCustomers = Server.CreateObject("ADODB.Recordset")  
   strSQLCustomers = "Customers"  
   rsCustomers.Open strSQLCustomers, Cnxn, adOpenKeyset, adLockOptimistic, adCmdTable  
  
   ' Move to designated record and delete it  
   If Not IsEmpty(Request.Form("WhichRecord")) Then  
      'Get value to move from Form Post method  
      Moves = Request.Form("WhichRecord")  
  
      rsCustomers.Move CInt(Moves)  
      If Not rsCustomers.EOF or rsCustomers.BOF Then  
          ' handle any db errors  
         On Error Resume Next  
         rsCustomers.Delete 1  
         If Cnxn.Errors.Count <> 0 Then  
            Response.Write "Cannot delete since there are related records in other tables."  
            Response.End  
         End If  
         rsCustomers.MoveFirst  
         On Error GoTo 0  
      Else  
         Response.Write "Not a Valid Record Number"  
         rsCustomers.MoveFirst  
      End If  
   End If  
%>  
  
<!-- BEGIN column header row for Customer Table-->  
<TABLE COLSPAN=8 CELLPADDING=5 BORDER=0>  
<TR>  
   <TH>Rec. #</TH>  
   <TH>Company Name</TH>  
   <TH>Contact Name</TH>  
   <TH>City</TH>  
</TR>  
  
   <%   
   ' Display ADO Data from Customer Table   
   ' Loop through Recordset adding one row to HTML Table each pass  
   Dim iCount  
   iCount = 0  
   Do Until rsCustomers.EOF %>  
   <TR>  
     <TD> <%= CStr(iCount) %>  
     <TD> <%= rsCustomers("CompanyName")%> </TD>  
     <TD> <%= rsCustomers("ContactName")%> </TD>  
     <TD> <%= rsCustomers("City")%> </TD>  
   </TR>  
   <%   
     iCount = iCount + 1  
     rsCustomers.MoveNext   
   Loop   
   %>  
</TABLE>  
  
<!-- Do Client side Input Data Validation Move to named record and Delete it -->  
<Center>  
<H4>Clicking Button Will Remove Designated Record</H4>  
<H5>There are <%=rsCustomers.RecordCount%> Records in this Set</H5>  
  
<Form Method=Post Action="Deletevbs.asp" Name=Form>  
   <Input Type=Text Name="WhichRecord" Size=3>   
   <Input Type=Button Name=cmdDelete Value="Delete Record">  
</Form>  
  
</BODY>  
  
<Script Language = "VBScript">  
Sub cmdDelete_OnClick  
   If IsNumeric(Document.Form.WhichRecord.Value) Then  
      Document.Form.WhichRecord.Value = CInt(Document.Form.WhichRecord.Value)  
      Dim Response  
      Response = MsgBox("Are You Sure About Deleting This Record?", vbYesNo,  "ADO-ASP Example")  
  
      If Response = vbYes Then  
         Document.Form.Submit  
      End If  
   Else  
      MsgBox "You Must Enter a Valid Record Number",,"ADO-ASP Example"  
   End If  
End Sub  
</Script>  
  
<%  
    ' clean up  
    If rsCustomers.State = adStateOpen then  
        rsCustomers.Close  
    End If  
    If Cnxn.State = adStateOpen then  
        Cnxn.Close  
    End If  
%>  
</HTML>  
<!-- EndDeleteVBS -->  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Delete-Methode (ADO-Recordset)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Recordset-Objekt (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
