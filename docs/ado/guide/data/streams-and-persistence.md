---
description: Datenströme und Persistenz
title: Streams und Persistenz | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
author: rothja
ms.author: jroth
ms.openlocfilehash: 60e006733fd8ef5bd958328420ab43c1cbabc50e
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88979421"
---
# <a name="streams-and-persistence"></a>Datenströme und Persistenz
Das [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) - [Objekt speichert](../../../ado/reference/ado-api/save-method.md) Method speichert oder speichert ein **Recordset** in einer *Datei, und*die [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) -Methode stellt das **Recordset** aus dieser Datei wieder her.  
  
 Mit ADO 2,7 oder höher können die Methoden " **Save** " und " **Open** " auch ein **Recordset** in einem [Streamobjekt](../../../ado/reference/ado-api/stream-object-ado.md) speichern. Diese Funktion ist besonders nützlich, wenn Sie mit Remote Data Service (RDS) und Active Server Seiten (ASP) arbeiten.  
  
 Weitere Informationen darüber, wie die Persistenz allein auf ASP-Seiten verwendet werden kann, finden Sie in der aktuellen ASP-Dokumentation.  
  
 Im folgenden finden Sie einige Szenarien, die zeigen, wie **Streamobjekte** und Persistenz verwendet werden können.  
  
## <a name="scenario-1"></a>Szenario 1  
 In diesem Szenario wird einfach ein **Recordset** in einer Datei und dann in einem **Stream**gespeichert. Anschließend wird der persistente Stream in einem anderen **Recordset**geöffnet.  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>Szenario 2  
 In diesem Szenario wird ein **Recordset** in einem **Stream** im XML-Format beibehalten. Anschließend wird der Daten **Strom** in eine Zeichenfolge gelesen, die Sie untersuchen, bearbeiten oder anzeigen können.  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>Szenario 3  
 Dieser Beispielcode zeigt ASP-Code, der ein **Recordset** als XML direkt an das **Response** -Objekt speichert:  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>Szenario 4  
 In diesem Szenario schreibt der ASP-Code den Inhalt des **Recordsets** im ADTG-Format in den Client. Der [Microsoft-Cursor Dienst für OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) kann diese Daten verwenden, um ein nicht verbundenes **Recordset**zu erstellen.  
  
 Eine neue Eigenschaft in der RDS- [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md), [URL](../../../ado/reference/rds-api/url-property-rds.md), verweist auf die ASP-Seite, die das **Recordset**generiert. Dies bedeutet, dass ein **Recordset** -Objekt ohne RDS abgerufen werden kann, indem das serverseitige [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) -Objekt verwendet wird, oder der Benutzer, der ein Geschäftsobjekt schreibt. Dadurch wird das RDS-Programmiermodell erheblich vereinfacht.  
  
 Server seitiger Code, benannt https://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 Client seitiger Code:  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="https://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 Entwickler haben auch die Möglichkeit, ein **Recordset** -Objekt auf dem Client zu verwenden:  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "https://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Open-Methode (ADO-Recordset)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Record-Objekt (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Save-Methode](../../../ado/reference/ado-api/save-method.md)
