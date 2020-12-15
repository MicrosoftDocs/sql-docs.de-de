---
title: Verwenden von Update Gram in einer Beispiel-ASP-Anwendung (SQLXML)
description: Sehen Sie sich ein Beispiel für die Verwendung eines SQLXML-Update Grams in einer Active Server Pages (ASP)-Anwendung an.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- ASP applications [SQLXML]
- Active Server Pages
- updategrams [SQLXML], ASP applications
ms.assetid: 10eff799-4c39-4b52-8b38-7ea6f68454a8
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f3cb1db0f03b911f695b85c6efed8a43240d8258
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97459953"
---
# <a name="using-an-updategram-in-a-sample-asp-application-sqlxml-40"></a>Verwenden eines Updategrams in einer Beispiel-ASP-Anwendung (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Diese ASP (Active Server Pages)-Anwendung ermöglicht es Ihnen, Kundendaten in der Tabelle Person.Contact der Beispieldatenbank AdventureWorks in Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zu aktualisieren. Die Anwendung führt Folgendes aus:  
  
-   Sie fordert den Benutzer zur Eingabe einer Kontakt-ID auf.  
  
-   Verwendet diesen Kunden-ID-Wert, um eine Vorlage auszuführen, mit der Kontaktinformationen aus der Tabelle Person.Contact abgerufen werden.  
  
-   Sie zeigt diese Informationen in einem HTML-Formular an.  
  
 Der Benutzer kann die Kontaktinformationen außer der Kontakt-ID (weil die Spalte ContactID den Primärschlüssel darstellt) dann aktualisieren. Nachdem der Benutzer die Informationen übermittelt hat, wird ein Updategram ausgeführt, und alle Formularparameter werden dem Updategram übergeben.  
  
 Die folgende Vorlage ist die erste Vorlage (GetContact.xml). Speichern Sie diese Vorlage in dem Verzeichnis, das dem virtuellen Namen des **Vorlagen** Typs zugeordnet ist.  
  
```  
<root xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <sql:header>  
      <sql:param name="cid"></sql:param>  
   </sql:header>  
   <sql:query>  
      SELECT  *   
      FROM    Person.Contact  
      WHERE   ContactID=@cid   
      FOR XML AUTO  
   </sql:query>  
</root>  
```  
  
 Die folgende Vorlage ist die zweite Vorlage (UpdateContact.xml). Speichern Sie diese Vorlage in dem Verzeichnis, das dem virtuellen Namen des **Vorlagen** Typs zugeordnet ist.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:header>  
   <updg:param name="cid"/>  
   <updg:param name="title" />  
   <updg:param name="firstname" />  
   <updg:param name="lastname" />  
   <updg:param name="emailaddress" />  
   <updg:param name="phone" />  
</updg:header>  
<updg:sync >  
   <updg:before>  
      <Person.Contact ContactID="$cid" />   
   </updg:before>  
   <updg:after>  
      <Person.Contact ContactID="$cid"   
       Title="$title"  
       FirstName="$firstname"  
       LastName="$lastname"  
       EmailAddress="$emailaddress"  
       Phone="$phone"/>  
   </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 Der folgende Code stellt die ASP-Beispielanwendung (SampleASP.asp) dar. Speichern Sie sie in dem Verzeichnis, das einem virtuellen Stammverzeichnis zugeordnet ist, welches Sie mit dem Hilfsprogramm Internetdienste-Manager erstellen. (Dieses virtuelle Stammverzeichnis wird nicht mit dem IIS-Hilfsprogramm Virtual Directory Management für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erstellt, weil das IIS-Hilfsprogramm Virtual Directory Management für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf ASP-Anwendungen weder zugreifen noch diese identifizieren kann.)  
  
> [!NOTE]  
>  Im Code müssen Sie "ServerName" durch den Namen des Servers ersetzen, auf dem Microsoft Internet-Informationsdienste (IIS) ausgeführt werden.  
  
```  
<% LANGUAGE=VBSCRIPT %>  
<%  
  Dim ContactID  
  ContactID=Request.Form("cid")  
%>  
<html>  
<body>  
<%  
  'If a ContactID value is not yet provided, display this form.  
  if ContactID="" then  
%>  
<!-- If the ContactID has not been specified, display the form that allows users to enter an ID. -->  
<form action="AdventureWorksContacts.asp" method="POST">  
<br>  
Enter ContactID: <input type=text name="cid"><br>  
<input type=submit value="Submit this ID" ><br><br>  
<-- Otherwise, if a ContactID is entered, display the second part of the form where the user can change customer information. -->  
<%  
  else  
%>  
<form name="Contacts" action="https://localhost/AdventureWorks/Template/UpdateContact.xml" method="POST">  
You may update customer information below.<br><br>  
<!-- A comment goes here to separate the parts of the application or page. -->  
<br>  
<%  
  ' Load the document in the parser and extract the values to populate the form.  
    Set objXML=Server.CreateObject("MSXML2.DomDocument")  
    ObjXML.setProperty "ServerHTTPRequest", TRUE  
  
    objXML.async=False  
    objXML.Load("https://localhost/AdventureWorks/Template/GetContact.xml?cid=" & ContactID)  
    set objCustomer=objXML.documentElement.childNodes.Item(0)  
  
  ' In retrieving data from the database, if a value in the column is NULL there  
  '  is no attribute for the corresponding element. In this case,  
  ' skip the error generation and go to the next attribute.  
  
  On Error Resume Next  
  
  Response.Write "Contact ID: <input type=text readonly=true style='background-color:silver' name=cid value="""  
  Response.Write objCustomer.attributes(0).value  
  Response.Write """><br><br>"  
  
  Response.Write "Title: <input type=text name=title value="""  
  Response.Write objCustomer.attributes(1).value  
  Response.Write """><br><br>"  
  
  Response.Write "First Name: <input type=text name=firstname value="""  
  Response.Write objCustomer.attributes(2).value  
  Response.Write """><br>"  
  
  Response.Write "Last Name: <input type=text name=lastname value="""  
  Response.Write objCustomer.attributes(3).value  
  Response.Write """><br><br>"  
  
  Response.Write "Email Address: <input type=text name=emailaddress value="""  
  Response.Write objCustomer.attributes(4).value  
  Response.Write """><br><br>"  
  
  Response.Write "Phone: <input type=text name=phone value="""  
  Response.Write objCustomer.attributes(9).value  
  Response.Write """><br><br>"  
  
  set objCustomer=Nothing  
  Set objXML=Nothing  
%>  
<input type="submit" value="Submit this change" ><br><br>  
<input type=hidden name="contenttype" value="text/xml">  
<input type=hidden name="eeid" value="<%=ContactID%>"><br><br>  
<% end if %>  
  
</form>  
</body>  
</html>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsüberlegungen zu Update grams &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
