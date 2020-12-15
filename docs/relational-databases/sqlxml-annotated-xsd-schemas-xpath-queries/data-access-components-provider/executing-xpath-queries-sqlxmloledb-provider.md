---
title: Ausführen von XPath-Abfragen (SQLXMLOLEDB-Anbieter)
description: Erfahren Sie, wie Sie beim Ausführen von XPath-Abfragen SQLXMLOLEDB-anbieterspezifische Eigenschaften verwenden.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- queries [SQLXML], SQLXMLOLEDB Provider
- Base Path property
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- Mapping Schema property
ms.assetid: 19063222-dc9c-48ae-a55f-778103674a9e
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 95b1ac8bbbcc07eb671a32f72e944b80fdb7e0d7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467131"
---
# <a name="executing-xpath-queries-sqlxmloledb-provider"></a>Ausführen von XPath-Abfragen (SQLXMLOLEDB-Anbieter)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Dieses Beispiel veranschaulicht die Verwendung der folgenden SQLXMLOLEDB-anbieterspezifischen Eigenschaften:  
  
-   **ClientSideXML**  
  
-   **Basispfad**  
  
-   **Zuordnungsschema**  
  
 In dieser Beispiel-ADO-Anwendung wird eine XPath-Abfrage (Stamm) mit einem XSD-Zuordnungsschema (MySchema.xml) angegeben. Das Schema verfügt über ein- **\<Contacts>** Element mit den Attributen **ContactID**, **FirstName** und **LastName** . Im Schema wird die Standardzuordnung verwendet: ein Elementname wird der gleichnamigen Tabelle zugeordnet, und die Attribute eines einfachen Typs werden den gleichnamigen Spalten zugeordnet.  
  
```  
<xsd:schema xmlns:xsd='http://www.w3.org/2001/XMLSchema'  
   xmlns:sql='urn:schemas-microsoft-com:mapping-schema'>  
 <xsd:element name= 'root' sql:is-constant='1'>   
    <xsd:complexType>  
       <xsd:sequence>  
         <xsd:element ref = 'Contacts'/>  
       </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
  <xsd:element name='Contacts' sql:relation='Person.Contact'>   
     <xsd:complexType>  
          <xsd:attribute name='ContactID' type='xsd:integer' />  
          <xsd:attribute name='FirstName' type='xsd:string'/>   
          <xsd:attribute name='LastName' type='xsd:string' />   
     </xsd:complexType>  
   </xsd:element>  
</xsd:schema>  
```  
  
 Die Mapping-Schema Eigenschaft stellt das Zuordnungsschema bereit, für das die XPath-Abfrage ausgeführt wird. Das Zuordnungsschema kann ein XSD- oder ein XDR-Schema sein. Die Eigenschaft Basispfad stellt den Dateipfad zum Zuordnungsschema bereit.  
  
 Die ClientSideXML-Eigenschaft ist auf true festgelegt. Deshalb wird das XML-Dokument auf dem Client generiert.  
  
 In der Anwendung wird eine XPath-Abfrage direkt angegeben. Deshalb muss der XPath-Dialekt {ec2a4293-e898-11d2-b1b7-00c04f680c56} eingeschlossen werden.  
  
> [!NOTE]  
>  Im Code müssen Sie den Namen der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in der Verbindungszeichenfolge bereitstellen. In diesem Beispiel wird überdies die Verwendung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) als Datenanbieter beschrieben, was die Installation zusätzlicher Netzwerkclientsoftware erforderlich macht. Weitere Informationen finden Sie unter [System Anforderungen für SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
Sub main()  
Dim oTestStream As New ADODB.Stream  
Dim oTestConnection As New ADODB.Connection  
Dim oTestCommand As New ADODB.Command  
  
oTestConnection.Open "provider=SQLXMLOLEDB.4.0;data provider=SQLNCLI11;data source=SqlServerName;initial catalog=AdventureWorks;Integrated Security= SSPI;"  
  
oTestCommand.ActiveConnection = oTestConnection  
oTestCommand.Properties("ClientSideXML") = True  
  
oTestCommand.CommandText = "root"  
oTestStream.Open  
oTestCommand.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
oTestCommand.Properties("Output Stream").Value = oTestStream  
oTestCommand.Properties("Base Path").Value = "c:\Schemas\SQLXML4\XPathDirect\"  
oTestCommand.Properties("Mapping Schema").Value = "mySchema.xml"  
oTestCommand.Properties("Output Encoding") = "utf-8"  
oTestCommand.Execute , , adExecuteStream  
oTestStream.Position = 0  
oTestStream.Charset = "utf-8"  
Debug.Print oTestStream.ReadText(adReadAll)  
  
End Sub  
Sub Form_Load()  
 main  
End Sub  
```  
  
  
