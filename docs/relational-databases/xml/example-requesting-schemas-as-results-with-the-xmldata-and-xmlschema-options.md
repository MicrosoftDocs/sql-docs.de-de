---
title: Anfordern von Schemas als Ergebnisse mithilfe von XMLDATA und XMLSCHEMA | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Optionen XMLDATA und XMLSCHEMA im RAW-Modus mit der FOR XML-Klausel verwenden, um ein XML-DATA-Schema oder ein XSD-Schema im Abfrageergebnis anzufordern.
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- RAW mode, requesting schema example
- RAW mode, with XMLDATA and XMLSCHEMA
ms.assetid: 3504ca38-be66-42b2-8dab-f499c9584840
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
ms.openlocfilehash: e7dfada43a7f899339f9f6ab59ef94dda9853c36
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85632931"
---
# <a name="request-schemas-as-results-with-xmldata--xmlschema"></a>Anfordern von Schemas als Ergebnisse mithilfe von XMLDATA und XMLSCHEMA
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Die folgende Abfrage gibt das XML-Datenschema zurück, das die Dokumentstruktur beschreibt.  
  
## <a name="example"></a>Beispiel  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW, XMLDATA  
GO  
```  
  
 Dies ist das Ergebnis:  
  
```  
<Schema name="Schema1" xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:datatypes">  
  <ElementType name="row" content="empty" model="closed">  
    <AttributeType name="ProductModelID" dt:type="i4" />  
    <AttributeType name="Name" dt:type="string" />  
    <attribute type="ProductModelID" />  
    <attribute type="Name" />  
  </ElementType>  
</Schema>  
<row xmlns="x-schema:#Schema1" ProductModelID="122" Name="All-Purpose Bike Stand" />  
<row xmlns="x-schema:#Schema1" ProductModelID="119" Name="Bike Wash" />  
```  
  
> [!NOTE]  
>  Das <`Schema`> ist als Namespace deklariert. Damit Namespacekonflikte vermieden werden, wenn mehrere XML-Datenschemas in verschiedenen FOR XML-Abfragen angefordert werden, wird der Namespacebezeichner – in diesem Beispiel `Schema1` – bei jeder Abfrageausführung geändert. Der Namespacebezeichner setzt sich aus **Schema**_**n**_ zusammen, wobei _**n**_ eine ganze Zahl ist.  
  
 Durch Angeben der Option `XMLSCHEMA` können Sie das XSD-Schema für das Ergebnis anfordern.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW, XMLSCHEMA  
GO  
```  
  
 Dies ist das Ergebnis:  
  
```  
<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">  
  <xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="row">  
    <xsd:complexType>  
      <xsd:attribute name="ProductModelID" type="sqltypes:int" use="required" />  
      <xsd:attribute name="Name" use="required">  
        <xsd:simpleType sqltypes:sqlTypeAlias="[AdventureWorks].[dbo].[Name]">  
          <xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033" sqltypes:sqlCompareOptions="IgnoreCase IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">  
            <xsd:maxLength value="50" />  
          </xsd:restriction>  
        </xsd:simpleType>  
      </xsd:attribute>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" ProductModelID="122" Name="All-Purpose Bike Stand" />  
<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" ProductModelID="119" Name="Bike Wash" />  
  
```  
  
 Sie können den Zielnamespace-URI als optionales Argument für XMLSCHEMA in FOR XML angeben. Auf diese Weise wird der angegebene Zielnamespace im Schema zurückgegeben. Dieser Zielnamespace bleibt bei jeder Ausführung der Abfrage gleich. Die folgende geänderte Version der vorherigen Abfrage enthält z. B. den Namespace-URI `'urn:example.com'`als Argument.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW, XMLSCHEMA ('urn:example.com')  
GO  
```  
  
 Dies ist das Ergebnis:  
  
```  
<xsd:schema targetNamespace="urn:example.com" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">  
  <xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="row">  
    <xsd:complexType>  
      <xsd:attribute name="ProductModelID" type="sqltypes:int" use="required" />  
      <xsd:attribute name="Name" use="required">  
        <xsd:simpleType sqltypes:sqlTypeAlias="[AdventureWorks].[dbo].[Name]">  
          <xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033" sqltypes:sqlCompareOptions="IgnoreCase IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">  
            <xsd:maxLength value="50" />  
          </xsd:restriction>  
        </xsd:simpleType>  
      </xsd:attribute>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
<row xmlns="urn:example.com" ProductModelID="122" Name="All-Purpose Bike Stand" />  
<row xmlns="urn:example.com" ProductModelID="119" Name="Bike Wash" />  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden des RAW-Modus mit FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)  
  
  
