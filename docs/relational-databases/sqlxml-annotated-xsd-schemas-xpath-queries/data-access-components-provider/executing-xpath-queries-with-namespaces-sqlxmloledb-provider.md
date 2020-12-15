---
title: Ausführen von XPath-Abfragen mit Namespaces (SQLXMLOLEDB)
description: Erfahren Sie, wie Sie Namespaces in SQLXML 4,0 angeben, wenn Sie XPath-Abfragen mit dem SQLXMLOLEDB-Anbieter ausführen.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- namespaces property
- queries [SQLXML], SQLXMLOLEDB Provider
- XPath queries [SQLXML], namespaces
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- namespaces [SQLXML], XPath queries
ms.assetid: 024a4b7d-435d-47ba-9e80-2c2f640108f5
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee24d16dce8ae60ccf51a866a274b15b2737ca89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462911"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxmloledb-provider"></a>Ausführen von XPath-Abfragen mit Namespaces (SQLXMLOLEDB-Anbieter)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  XPath-Abfragen können Namespaces enthalten. Wenn die Schemaelemente mit Namespace angegeben wurden (d. h. wenn Sie einen Zielnamespace enthalten), dann müssen mit diesem Schema ausgeführte XPath-Abfragen diesen Namespace angeben.  
  
 Weil die Verwendung des Platzhalterzeichens (*) in SQLXML 4.0 nicht unterstützt wird, müssen Sie die XPath-Abfrage mithilfe eines Namespacepräfix angeben. Um dieses Präfix aufzulösen, geben Sie mit der Namespace-Eigenschaft die Namespace Bindung an.  
  
 Im folgenden Beispiel gibt die XPath-Abfrage Namespaces mit den Platzhalter Zeichen ( \* ) und den XPath-Funktionen local-Name () und Namespace-URI () an. Diese XPath-Abfrage gibt alle Elemente zurück, bei denen der lokale Name " **Contact** " lautet und der Namespace-URI " **urn: MySchema: Contacts**" ist.  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 In SQLXML 4.0 muss diese XPath-Abfrage mit einem Namespacepräfix angegeben werden. Ein Beispiel ist " **x:Contact**", wobei " **x** " das Namespace Präfix ist. Betrachten Sie folgendes XSD-Schema:  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 Weil dieses Schema einen Zielnamespace definiert, müssen XPath-Abfragen (beispielsweise "Employee"), die mit diesem Schema ausgeführt werden, einen Namespace enthalten.  
  
 Dies ist eine [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic-Beispielanwendung, die eine XPath-Abfrage (x:Employee) mit dem vorstehenden XSD-Schema ausführt. Um das Präfix aufzulösen, wird die Namespace Bindung mithilfe der Namespace-Eigenschaft angegeben.  
  
> [!NOTE]  
>  Im Code müssen Sie den Namen der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in der Verbindungszeichenfolge bereitstellen. In diesem Beispiel wird überdies die Verwendung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) als Datenanbieter angegeben, was die Installation zusätzlicher Netzwerkclientsoftware erforderlich macht. Weitere Informationen finden Sie unter [System Anforderungen für SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
Private Sub Form_Load()  
    Dim con As New ADODB.Connection  
    Dim cmd As New ADODB.Command  
    Dim stm As New ADODB.Stream  
    con.Open "provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
    Set cmd.ActiveConnection = con  
    stm.Open  
    cmd.Properties("Output Stream").Value = stm  
    cmd.Properties("Output Encoding") = "utf-8"  
    cmd.Properties("Mapping schema") = "C:\DirectoryPath\con-ex.xml"  
    cmd.Properties("namespaces") = "xmlns:x='urn:myschema:Contacts'"  
    '  Debug.Print "Set Command Dialect to DBGUID_XPATH"  
    cmd.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
    cmd.CommandText = "x:Contact"  
    cmd.Execute , , adExecuteStream   
    stm.Position = 0  
    Debug.Print stm.ReadText(adReadAll)  
End Sub  
```  
  
### <a name="to-test-this-application"></a>So testen Sie diese Anwendung  
  
1.  Speichern Sie das XSD-Beispielschema in einem Ordner.  
  
2.  Erstellen Sie ein Visual Basic-Projekt für eine ausführbare Anwendung, und kopieren Sie den Code dort hinein. Ändern Sie, falls erforderlich, den angegebenen Verzeichnispfad.  
  
3.  Fügen Sie den folgenden Projektverweis hinzu.  
  
    ```  
    "Microsoft ActiveX Data Objects 2.8 Library"  
    ```  
  
4.  Führen Sie die Anwendung aus.  

 Dies ist das Teilergebnis:  
  
```  
<y0:Employee xmlns:y0="urn:myschema:Contacts"   
             LName="Achong" CID="1" FName="Gustavo"/>  
<y0:Employee xmlns:y0="urn:myschema:Employees"   
             LName="Abel" CID="2" FName="Catherine"/>  
```  
  
 Die im XML-Dokument generierten Präfixe sind zwar zufällig gewählt, jedoch dem gleichen Namespace zugeordnet.  
  
  
