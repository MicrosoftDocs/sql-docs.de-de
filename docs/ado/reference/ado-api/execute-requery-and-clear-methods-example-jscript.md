---
description: Beispiel für Execute, Requery und Clear Methods (JScript)
title: Beispiel für Execute, Requery und Clear Methods (JScript) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- JScript
helpviewer_keywords:
- Requery method [ADO], JScript example
- Clear method [ADO], JScript example
- Execute method [ADO], JScript example
ms.assetid: 51a87e91-c9d9-4e49-af47-79cce2c4cfe0
author: rothja
ms.author: jroth
ms.openlocfilehash: b7d0e9eef28942ad53a234811b2ee061e9685a1a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034140"
---
# <a name="execute-requery-and-clear-methods-example-jscript"></a>Beispiel für Execute, Requery und Clear Methods (JScript)
In diesem Beispiel wird die **Execute** -Methode veranschaulicht, wenn Sie sowohl über ein [Befehls](../../../ado/reference/ado-api/command-object-ado.md) Objekt als auch über ein [Verbindungs](../../../ado/reference/ado-api/connection-object-ado.md) Objekt ausgeführt wird Außerdem wird die [Requery](../../../ado/reference/ado-api/requery-method.md) -Methode verwendet, um aktuelle Daten in einem [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)abzurufen, und die [Clear](../../../ado/reference/ado-api/clear-method-ado.md) -Methode, um den Inhalt der [Fehler](../../../ado/reference/ado-api/errors-collection-ado.md) Auflistung zu löschen. (Der Zugriff auf die **Fehler** Sammlung erfolgt über das **Verbindungs** Objekt der [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) -Eigenschaft des [Recordsets](../../../ado/reference/ado-api/recordset-object-ado.md).) Nennen Sie die Datei " **executejs. ASP**".  
  
```  
<!-- BeginExecuteJS -->  
<%@LANGUAGE="JScript"%>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<%  
    strLastName = new String(Request.Form("AuthorLName"));  
  
    if (strLastName.indexOf("undefined") > -1)  
        strLastName = "";  
%>  
  
<html>  
  
<head>  
<title>Execute, Requery and Clear Methods Example (JScript)</title>  
<style>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
-->  
</style>  
</head>  
  
<body bgcolor="White">  
<h1>Execute, Requery and Clear Methods Example (JScript)</h1>  
<%  
    if (strLastName.length > 0)  
    {  
        // command and recordset variables  
        var Connect = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='pubs';Integrated Security='SSPI';";  
        var Cnxn = Server.CreateObject("ADODB.Connection");  
        var cmdAuthor = Server.CreateObject("ADODB.Command");  
        var rsAuthor = Server.CreateObject("ADODB.Recordset");  
        var rsAuthor2 = Server.CreateObject("ADODB.Recordset");  
        var SQLAuthor2, strMessage, strMessage2;  
        var Err, ErrCount;  
  
        try  
        {  
            // open connection  
            Cnxn.Open(Connect);  
  
            // command object parameters  
            cmdAuthor.CommandText = "SELECT * FROM Authors WHERE au_lname = ?";  
            cmdAuthor.Parameters.Append(cmdAuthor.CreateParameter("Last Name", adChar, adParamInput, 20, strLastName));  
            cmdAuthor.ActiveConnection = Cnxn;  
  
            // recordset from command.execute  
            rsAuthor = cmdAuthor.Execute();  
  
            // recordset from connection.execute  
            SQLAuthor2 = "SELECT * FROM Authors";  
            rsAuthor2 = Cnxn.Execute(SQLAuthor2);  
  
                // check for errors  
                ErrCount = Cnxn.errors.count;  
                if(ErrCount !== 0) //write the errors  
                {  
                    for(Err = 0; Err = ErrCount; Err++){  
                        Err = Cnxn.errors.item;  
                        Response.Write(Err);  
                    }  
                    // clean out any existing errors  
                    Cnxn.Errors.Clear;  
                }  
  
                // show the data      
            Response.Write("<HR><HR>");  
  
                // first recordset  
            Response.Write("<b>Command.Execute results</b>")  
            while (!rsAuthor.EOF)  
            {  
                // build output string by starting a new line  
                strMessage = "<P>";  
                strMessage += "<br>";  
  
                // recordset data   
                strMessage += rsAuthor("au_fname") + " ";   
                strMessage += rsAuthor("au_lname") + " ";  
  
                // end the line  
                strMessage += "</P>";  
  
                // show the results  
                Response.Write(strMessage);  
  
                // get next record  
                rsAuthor.MoveNext;  
            }  
  
            Response.Write("<HR><HR>");  
  
            // second recordset  
            Response.Write("<b>Connection.Execute results</b>")  
            while (!rsAuthor2.EOF)  
            {  
                // start a new line  
                strMessage2 = "<P>";  
  
                // first and last name are in first column  
                strMessage2 += rsAuthor2("au_fname") + " "   
                strMessage2 += rsAuthor2("au_lname") + " ";  
  
                // end the line  
                strMessage2 += "</P>";  
  
                // show results  
                Response.Write(strMessage2);  
  
                // get next record  
                rsAuthor2.MoveNext;  
            }  
        }  
        catch (e)  
        {  
            Response.Write(e.message);  
        }  
        finally  
        {  
            // clean up  
            if (rsAuthor.State == adStateOpen)  
                rsAuthor.Close;  
            if (rsAuthor2.State == adStateOpen)  
                rsAuthor2.Close;  
            if (Cnxn.State == adStateOpen)  
                Cnxn.Close;  
            rsAuthor1 = null;  
            rsAuthor2 = null;  
            Cnxn = null;  
        }  
    }  
%>  
  
<hr>  
  
<form method="POST" action="ExecuteJS.asp" id=form1 name=form1>  
  <p align="left">Enter last name of author to find (e.g., Ringer): <input type="text" name="AuthorLName" size="40"></p>  
  <p align="left"><input type="submit" value="Submit" name="B1"><input type="reset" value="Reset" name="B2"></p>  
</form>  
</body>  
  
</html>  
<!-- EndExecuteJS -->  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Clear-Methode (ADO)](../../../ado/reference/ado-api/clear-method-ado.md)   
 [Command-Objekt (ADO)](../../../ado/reference/ado-api/command-object-ado.md)   
 [Verbindungs Objekt (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Error-Objekt](../../../ado/reference/ado-api/error-object.md)   
 [Execute-Methode (ADO-Befehl)](../../../ado/reference/ado-api/execute-method-ado-command.md)   
 [Execute-Methode (ADO-Verbindung)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [Recordset-Objekt (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Requery-Methode](../../../ado/reference/ado-api/requery-method.md)
