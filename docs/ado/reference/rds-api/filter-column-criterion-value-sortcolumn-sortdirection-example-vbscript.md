---
description: Filter Column, Filterkriterium, FilterValue, SortColumn und SortDirection Properties und Reset Method example (VBScript)
title: Legen Sie RDS fest. DataControl-Server und Bindung an die HTML-Tabelle (VBScript) | Microsoft-Dokumentation
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
- FilterColumn property [ADO], VBScript example
- FilterCriterion property [ADO], VBScript example
- SortDirection property [RDS], VBScript example
- Reset method [ADO], VBScript example
- SortColumn property [RDS], VBScript example
- FilterValue property [ADO], VBScript example
ms.assetid: 8a74802f-34d6-4676-bf94-07df5f8bff66
author: rothja
ms.author: jroth
ms.openlocfilehash: 7654130b4247d489cbb4a9a58c07b0c63c49c727
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053161"
---
# <a name="filtercolumn-filtercriterion-filtervalue-sortcolumn-and-sortdirection-properties-and-reset-method-example-vbscript"></a>Filter Column, Filterkriterium, FilterValue, SortColumn und SortDirection Properties und Reset Method example (VBScript)
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
 Der folgende Code zeigt, wie das RDS-festgelegt wird [. Der DataControl](./datacontrol-object-rds.md) - **Server** Parameter bei Design Time und bindet ihn mithilfe einer Datenquelle an eine Daten abhängige HTML-Tabelle. Schneiden Sie den folgenden Code aus, und fügen Sie ihn in Editor oder einen anderen Text-Editor ein, und speichern Sie ihn als **filtercolumnvsb. ASP**.  
  
```  
<!-- BeginFilterColumnVBS -->  
<HTML>  
<HEAD>  
<META name="VI60_DefaultClientScript" Content="VBScript">  
  
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0">  
<TITLE>FilterColumn, FilterCriterion, FilterValue, SortColumn, and SortDirection   
Properties and Reset Method Example (VBScript)</TITLE>  
</HEAD>  
<BODY>  
<h1>FilterColumn, FilterCriterion, FilterValue, SortColumn, and SortDirection   
Properties and Reset Method Example (VBScript)</h1>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=RDS HEIGHT=1 WIDTH=1>  
   <PARAM NAME="SQL" VALUE="Select FirstName, LastName, Title, ReportsTo, Extension from Employees">  
   <PARAM NAME="SERVER" VALUE="<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=SQLOLEDB;Initial Catalog=Northwind;Integrated Security=SSPI">  
</OBJECT>  
  
Sort Column: <SELECT NAME="cboSortColumn">   
              <OPTION VALUE=""></OPTION>  
                  <OPTION VALUE=ID>ID</OPTION>  
                  <OPTION VALUE=FirstName>FirstName</OPTION>  
                  <OPTION VALUE=LastName>LastName</OPTION>  
                  <OPTION VALUE=Title>Title</OPTION>  
                  <OPTION VALUE=Title>ReportsTo</OPTION>  
                  <OPTION VALUE=Phone>Extension</OPTION>  
             </SELECT>  
             <br>  
Sort Direction: <SELECT NAME="cboSortDir">   
              <OPTION VALUE=""></OPTION>  
                  <OPTION VALUE=TRUE>Ascending</OPTION>  
                  <OPTION VALUE=FALSE>Descending</OPTION>  
                </SELECT>  
<HR WIDTH="25%">  
Filter Column: <SELECT NAME="cboFilterColumn">   
              <OPTION VALUE=""></OPTION>  
                  <OPTION VALUE=FirstName>FirstName</OPTION>  
                  <OPTION VALUE=LastName>LastName</OPTION>  
                  <OPTION VALUE=Title>Title</OPTION>  
                  <OPTION VALUE=Room>ReportsTo</OPTION>  
                  <OPTION VALUE=Phone>Extension</OPTION>  
             </SELECT>  
             <br>  
Filter Criterion: <SELECT NAME="cboCriterion">   
                    <OPTION VALUE=""></OPTION>  
                    <OPTION VALUE="=">=</OPTION>  
                    <OPTION VALUE=">">></OPTION>  
                    <OPTION VALUE="<"><</OPTION>  
                    <OPTION VALUE=">=">>=</OPTION>  
                    <OPTION VALUE="<="><=</OPTION>  
                    <OPTION VALUE="<>"><></OPTION>  
                  </SELECT>   
              <br>  
Filter Value: <INPUT NAME="txtFilterValue">  
<HR WIDTH="25%">  
<INPUT TYPE=BUTTON NAME=Clear VALUE="CLEAR ALL">    
<INPUT TYPE=BUTTON NAME=SortFilter VALUE="APPLY">  
  
<HR>  
<TABLE DATASRC=#RDS ID="DataTable">  
<THEAD>  
  <TR>  
    <TH>FirstName</TH>  
    <TH>LastName</TH>  
    <TH>Title</TH>  
    <TH>Reports To</TH>  
    <TH>Extension</TH>  
  </TR>  
</THEAD>  
<TBODY>  
  <TR>  
    <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
    <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
    <TD><SPAN DATAFLD="Title"></SPAN></TD>  
    <TD><SPAN DATAFLD="ReportsTo"></SPAN></TD>  
    <TD><SPAN DATAFLD="Extension"></SPAN></TD>  
  </TR>  
</TBODY>  
</TABLE>  
  
<Script Language="VBScript">  
<!--  
Const adFilterNone = 0  
  
Sub SortFilter_OnClick  
   Dim vCriterion  
   Dim vSortDir  
   Dim vSortCol  
   Dim vFilterCol  
  
   ' The value of SortColumn will be the   
   ' value of what the user picks in the  
   ' cboSortColumn box.  
   vSortCol = cboSortColumn.options(cboSortColumn.selectedIndex).value  
  
   If(vSortCol <> "") then  
      RDS.SortColumn = vSortCol  
   End If  
  
   ' The value of SortDirection will be the   
   ' value of what the user specifies in the  
   ' cboSortdirection box.  
  
   If (vSortCol <> "") then  
      vSortDir = cboSortDir.options(cboSortDir.selectedIndex).value  
      If (vSortDir = "") then  
         MsgBox "You must select a direction for the sort."  
         Exit Sub  
      Else  
         If vSortDir = "Ascending" Then vSortDir = "TRUE"  
         If vSortDir = "Descending" Then vSortDir = "FALSE"  
         RDS.SortDirection = vSortDir  
      End If  
   End If  
  
   ' The value of FilterColumn will be the   
   ' value of what the user specifies in the  
   ' cboFilterColumn box.  
   vFilterCol = cboFilterColumn.options(cboFilterColumn.selectedIndex).value  
  
   If(vFilterCol <> "") then  
      RDS.FilterColumn = vFilterCol  
   End If  
  
   ' The value of FilterCriterion will be the   
   ' text value of what the user specifies in the  
   ' cboCriterion box.  
   vCriterion = cboCriterion.options(cboCriterion.selectedIndex).value  
   If (vCriterion <> "") Then  
      RDS.FilterCriterion = vCriterion  
   End If  
  
   ' txtFilterValue is a rich text box  
   ' control. The value of FilterValue will be the   
   ' text value of what the user specifies in the  
   ' txtFilterValue box.  
   If (txtFilterValue.value <> "") Then  
      RDS.FilterValue = txtFilterValue.value  
   End If  
  
   ' Execute the sort and filter on a client-side  
   ' Recordset based on the specified sort and filter  
   ' properties. Calling Reset refreshes the result set  
   ' that is displayed in the data-bound controls to  
   ' display the filtered, sorted recordset.  
   RDS.Reset  
End Sub  
  
Sub Clear_onClick()  
   'clear the HTML input controls  
   cboSortColumn.selectedIndex = 0  
   cboSortDir.selectedIndex = 0  
   cboFilterColumn.selectedIndex = 0  
   cboCriterion.selectedIndex = 0  
   txtFilterValue.value = ""  
  
   'clear the filter  
   RDS.FilterCriterion = ""  
   RDS.Reset(FALSE)  
End Sub  
-->  
</Script>  
  
</BODY>  
</HTML>  
<!-- EndFilterColumnVBS -->  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)   
 [FilterColumn-Eigenschaft (RDS)](./filtercolumn-property-rds.md)   
 [Filterkriterium-Eigenschaft (RDS)](./filtercriterion-property-rds.md)   
 [FilterValue-Eigenschaft (RDS)](./filtervalue-property-rds.md)   
 [Reset-Methode (RDS)](./reset-method-rds.md)   
 [SortColumn-Eigenschaft (RDS)](./sortcolumn-property-rds.md)   
 [SortDirection-Eigenschaft (RDS)](./sortdirection-property-rds.md)