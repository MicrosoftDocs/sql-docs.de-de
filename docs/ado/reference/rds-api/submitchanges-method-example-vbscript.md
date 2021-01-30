---
description: SubmitChanges-Methode – Beispiel (VBScript)
title: SubmitChanges-Methode (Beispiel) (VBScript) | Microsoft-Dokumentation
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- SubmitChanges method [ADO], VBScript example
ms.assetid: 619bc7fd-ad0a-44ea-9678-ad40a662c258
author: rothja
ms.author: jroth
ms.openlocfilehash: faea3ba69a0a1f0f5a1aedc6bd48dca137ff391c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168756"
---
# <a name="submitchanges-method-example-vbscript"></a>SubmitChanges-Methode – Beispiel (VBScript)
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
 Das folgende Code Fragment zeigt, wie die [SubmitChanges](./submitchanges-method-rds.md) -Methode mit einem [RDS verwendet wird. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
 Um dieses Beispiel zu testen, schneiden Sie diesen Code aus, und fügen Sie ihn in ein normales ASP-Dokument ein, und nennen Sie ihn **submitchangesctrlvb. ASP**. Das ASP-Skript identifiziert Ihren Server.  
  
```  
<!-- BeginCancelUpdateVBS -->  
<%@Language=VBScript%>  
  
<%'Option Explicit%>  
<% 'use the following META tag instead of adovbs.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0">  
<TITLE></TITLE>  
</HEAD>  
<BODY>  
<CENTER>  
<H1>Remote Data Service</H1>  
<H2>SubmitChanges and CancelUpdate Methods</H2>  
  
<%  ' to integrate/test this code replace the Server property value and   
    ' the Data Source value in the Connect property with identical values%>  
  
<HR>  
<OBJECT id="RDS" classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" HEIGHT="1" WIDTH="1"></OBJECT>  
<SCRIPT Language="VBScript">  
  
     'set RDS properties for control just created  
    RDS.Server = "https://<%=Request.ServerVariables("SERVER_NAME")%>"  
    RDS.Connect = "Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind';"  
    RDS.SQL = "Select * from Employees"  
    RDS.Refresh  
</SCRIPT>  
  
<TABLE DATASRC="#RDS">  
<THEAD>  
<TR ID="ColHeaders">  
   <TH>ID</TH>  
   <TH>FName</TH>  
   <TH>LName</TH>  
   <TH>Title</TH>  
   <TH>Hire Date</TH>  
   <TH>Birth Date</TH>  
   <TH>Extension</TH>  
   <TH>Home Phone</TH>  
</TR>  
</THEAD>  
<TBODY>  
<TR>  
   <TD> <INPUT DATAFLD="EmployeeID" size=4 id=text1 name=text1> </TD>  
   <TD> <INPUT DATAFLD="FirstName" size=10 id=text2 name=text2> </TD>  
   <TD> <INPUT DATAFLD="LastName" size=10 id=text3 name=text3> </TD>  
   <TD> <INPUT DATAFLD="Title" size=10 id=text4 name=text4> </TD>  
   <TD> <INPUT DATAFLD="HireDate" size=10 id=text5 name=text5> </TD>  
   <TD> <INPUT DATAFLD="BirthDate" size=10 id=text6 name=text6> </TD>  
   <TD> <INPUT DATAFLD="Extension" size=10 id=text7 name=text7> </TD>  
   <TD> <INPUT DATAFLD="HomePhone" size=8 id=text8 name=text8> </TD>  
</TR>  
</TBODY>  
</TABLE>  
<HR>  
<INPUT TYPE=button NAME="SubmitChange" VALUE="Submit Changes">  
<INPUT TYPE=button NAME="CancelChange" VALUE="Cancel Update">  
<BR>  
<H4>Alter a current entry on the grid. Move off that Row. <BR>  
Submit the Changes to your DBMS or cancel the updates. </H4>  
</CENTER>  
<SCRIPT Language="VBScript">  
  
Sub SubmitChange_OnClick  
  
    msgbox "Changes will be made"  
    RDS.SubmitChanges     
    RDS.Refresh  
  
End Sub  
  
Sub CancelChange_OnClick  
  
    msgbox "Changes will be cancelled"  
    RDS.CancelUpdate  
    RDS.Refresh  
  
End Sub  
</SCRIPT>  
  
</BODY>  
</HTML>  
<!-- EndCancelUpdateVBS -->  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)   
 [SubmitChanges-Methode (RDS)](./submitchanges-method-rds.md)