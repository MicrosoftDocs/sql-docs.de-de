---
title: XSD-Standard Zuordnung zu Tabellen/Spalten (SQLXML)
description: Erfahren Sie, wie Elemente und Attribute in einem XSD-Schema standardmäßig Tabellen und Spalten in SQLXML 4,0 zugeordnet werden.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XSD schemas [SQLXML], mapping attributes and elements
- mapping schema [SQLXML], default mapping
- element mapping [SQLXML], default mapping
- element mapping [SQLXML]
- table mapping [SQLXML]
- annotated XSD schemas, mapping attributes and elements
- table/view mapping [SQLXML]
- column mapping [SQLXML]
- attribute mapping [SQLXML], default mapping
- default schema mapping
- table mapping [SQLXML], default mapping
- testing XPath query against schema
- xml data type [SQL Server], SQLXML
- table/view mapping [SQLXML], default mapping
- element/attribute mapping [SQLXML]
ms.assetid: 9a18e92a-6cfb-4a14-993a-663a95aabb63
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fbe7cea458f317138f4ad63698c3ff68af0eacd7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415797"
---
# <a name="default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-40"></a>Standardzuordnung von XSD-Elementen und -Attributen zu Tabellen und Spalten (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Standardmäßig wird ein Element des komplexen Typs in einem mit Anmerkungen versehenen XSD-Schema der Tabelle (Sicht) mit dem gleichen Namen in der angegebenen Datenbank zugeordnet, und ein Element oder Attribut des einfachen Typs wird der Spalte mit demselben Namen in der Tabelle zugeordnet.  
  
## <a name="examples"></a>Beispiele  
 Es müssen bestimmte Anforderungen erfüllt sein, damit aus den folgenden Beispielen funktionierende Beispiele erstellt werden können. Weitere Informationen finden Sie unter [Anforderungen zum Ausführen von SQLXML-Beispielen](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-default-mapping"></a>A. Angeben der Standardzuordnung  
 In diesem Beispiel werden keine Anmerkungen im XSD-Schema angegeben. Das **\<Person.Contact>** -Element weist einen komplexen Typ auf und wird daher standardmäßig der Person. Contact-Tabelle in der AdventureWorks-Datenbank zugeordnet. Alle Attribute (ContactID, FirstName, LastName) des-Elements weisen einen **\<Person.Contact>** einfachen Typ auf und werden standardmäßig Spalten mit denselben Namen in der Person. Contact-Tabelle zugeordnet.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact" >  
     <xsd:complexType>  
       <xsd:attribute name="ContactID"  type="xsd:string" />   
       <xsd:attribute name="FirstName"   type="xsd:string" />   
       <xsd:attribute name="LastName"    type="xsd:string" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>So testen Sie eine XPath-Beispiel Abfrage für das Schema  
  
1.  Kopieren Sie den oben stehenden Schemacode, und fügen Sie ihn in eine Textdatei ein. Speichern Sie die Datei unter dem Dateinamen MySchema.xml.  
  
2.  Kopieren Sie die folgende Vorlage, und fügen Sie sie in eine Textdatei ein. Speichern Sie die Datei unter dem Namen MySchemaT im gleichen Verzeichnis, in dem Sie MySchema.xml gespeichert haben.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="MySchema.xml">  
            /Person.Contact  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Der für das Zuordnungsschema (MySchema.xml) angegebene Verzeichnispfad bezieht sich auf das Verzeichnis, in dem die Vorlage gespeichert wird. Es kann auch ein absoluter Pfad angegeben werden. Beispiel:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchema.xml"  
    ```  
  
3.  Erstellen und verwenden Sie das SQLXML 4.0-Testskript (Sqlxml4test.vbs), um die Vorlage auszuführen.  
  
     Weitere Informationen finden Sie unter [Verwenden von ADO zum Ausführen von SQLXML 4,0-Abfragen](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Im Folgenden wird ein Teil des Resultsets aufgeführt:  
  
```  
<?xml version="1.0" encoding="UTF-8" ?>  
<ROOT>  
  <Person.Contact ContactID="1" FirstName="Gustavo" LastName="Achong"/>  
  <Person.Contact ContactID="2" FirstName="Catherine" LastName="Abel"/>  
   ...  
</ROOT>  
```  
  
### <a name="b-mapping-an-xml-element-to-a-database-column"></a>B. Zuordnen eines XML-Elements zu einer Datenbankspalte  
 In diesem Beispiel findet die Standardzuordnung auch statt, da keine Anmerkungen verwendet werden. Das **\<Person.Contact>** -Element weist einen komplexen Typ auf und wird der Tabelle mit demselben Namen in der Datenbank zugeordnet. Die Elemente **\<FirstName>** und und das Mitarbeiter-ID- **\<LastName>** Attribut sind vom einfachen Typ und werden daher den gleichnamigen Spalten zugeordnet.  Der einzige Unterschied zwischen diesem und dem vorherigen Beispiel besteht darin, dass für die Zuordnung der Felder FirstName und LastName Elemente verwendet werden.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact">  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="FirstName" type="xsd:string" />   
        <xsd:element name="LastName" type="xsd:string" />   
      </xsd:sequence>  
      <xsd:attribute name="ContactID" type="xsd:integer" />   
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>So testen Sie eine XPath-Beispiel Abfrage für das Schema  
  
1.  Kopieren Sie den oben stehenden Schemacode, und fügen Sie ihn in eine Textdatei ein. Speichern Sie die Datei unter dem Dateinamen MySchemaElements.xml.  
  
2.  Erstellen Sie die folgende Vorlage (MySchemaElementsT.xml), und speichern Sie sie im gleichen Verzeichnis wie im vorherigen Schritt.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="MySchemaElements.xml">  
            /Person.Contact  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Der für das Zuordnungsschema angegebene Verzeichnispfad bezieht sich auf das Verzeichnis, in dem die Vorlage gespeichert wird. Es kann auch ein absoluter Pfad angegeben werden. Beispiel:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchemaElements.xml"  
    ```  
  
3.  Erstellen und verwenden Sie das SQLXML 4.0-Testskript (Sqlxml4test.vbs), um die Vorlage auszuführen.  
  
     Weitere Informationen finden Sie unter [Verwenden von ADO zum Ausführen von SQLXML 4,0-Abfragen](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Im Folgenden wird ein Teil des Resultsets aufgeführt:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact ContactID="1">  
    <FirstName>Gustavo</FirstName>  
    <LastName>Achong</LastName>  
  </Person.Contact>  
   ...  
</ROOT>  
```  
  
### <a name="c-mapping-an-xml-element-to-an-xml-data-type-column"></a>C. Zuordnen eines XML-Elements zu einer XML-Datentypspalte  
 In diesem Beispiel findet die Standardzuordnung auch statt, da keine Anmerkungen verwendet werden. Das **\<Production.ProductModel>** -Element weist einen komplexen Typ auf und wird der Tabelle mit demselben Namen in der Datenbank zugeordnet. Das **ProductModelID** -Attribut ist vom einfachen Typ und wird daher den gleichnamigen Spalten zugeordnet. Der einzige Unterschied zwischen diesem und den vorherigen Beispielen besteht darin, dass das-Element mit dem **\<Instructions>** **xsd: anyType** -Typ einer Spalte entspricht, die den **XML** -Datentyp verwendet.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Production.ProductModel">  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Instructions" type="xsd:anyType" />   
      </xsd:sequence>  
      <xsd:attribute name="ProductModelID" type="xsd:integer" />   
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Der **XML** -Datentyp wurde in eingeführt [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>So testen Sie eine XPath-Beispiel Abfrage für das Schema  
  
1.  Kopieren Sie den oben stehenden Schemacode, und fügen Sie ihn in eine Textdatei ein. Speichern Sie die Datei unter dem Dateinamen MySchemaXmlAnyElements.xml.  
  
2.  Erstellen Sie die folgende Vorlage (MySchemaXmlAnyElementsT.xml), und speichern Sie sie im gleichen Verzeichnis wie im vorherigen Schritt.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="MySchemaXmlAnyElements.xml">  
            /Production.ProductModel[@ProductModelID=7]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Der für das Zuordnungsschema angegebene Verzeichnispfad bezieht sich auf das Verzeichnis, in dem die Vorlage gespeichert wird. Es kann auch ein absoluter Pfad angegeben werden. Beispiel:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchemaXmlAnyElements.xml"  
    ```  
  
3.  Erstellen und verwenden Sie das SQLXML 4.0-Testskript (Sqlxml4test.vbs), um die Vorlage auszuführen.  
  
     Weitere Informationen finden Sie unter [Verwenden von ADO zum Ausführen von SQLXML 4,0-Abfragen](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Im Folgenden wird ein Teil des Resultsets aufgeführt:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Production.ProductModel ProductModelID="7">  
    <Instructions>  
      <root xmlns="http:  
//schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstru  
ctions">  
...  
      </root>  
    <Instructions>  
  </Production.ProductModel>  
</ROOT>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überlegungen zur Schema Sicherheit mit Anmerkungen &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)   
 [XML-Daten &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [XML-Datentypunterstützung für SQLXML 4.0](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)  
  
  
