---
title: "Nicht verbrauchte Daten mit ' SQL: overflow-field ' (SQLXML)"
description: 'Erfahren Sie, wie Sie das SQL: overflow-field in SQLXML 4,0 zum Abrufen von Daten verwenden, die von der OPENXML-Funktion nicht verbraucht wurden.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- unconsumed data
- storing unconsumed data
- retrieving unconsumed data
- annotated XSD schemas, unconsumed data
- overflow data [SQLXML]
- sql:overflow-field
ms.assetid: 8526998d-b47d-4a32-8dc2-7f50a8d11097
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96673a6e5a07879ec2c8d455a3976c5537ea2b2a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415701"
---
# <a name="retrieving-unconsumed-data-using-the-sqloverflow-field-sqlxml-40"></a>Abrufen von nicht verbrauchten Daten mithilfe von 'sql:overflow-field' (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Wenn Datensätze mithilfe der [!INCLUDE[tsql](../../includes/tsql-md.md)] OPENXML-Funktion aus einem XML-Dokument in eine Datenbank eingefügt werden, können alle nicht verbrauchten Daten aus dem XML-Quelldokument in einer Spalte gespeichert werden. Beim Abrufen von Daten aus einer Datenbank mithilfe von Schemas mit Anmerkungen können Sie das **SQL: overflow-field-** Attribut angeben, um die Spalte in der Tabelle zu identifizieren, in der die Überlauf Daten gespeichert werden. Das **SQL: overflow-field-** Attribut kann für angegeben werden **\<element>** .  
  
 Anschließend gibt es folgende Möglichkeiten, diese Daten abzurufen:  
  
-   Attribute, die in der Überlauf Spalte gespeichert sind, werden dem-Element hinzugefügt, das die **SQL: overflow-field-** Anmerkung enthält.  
  
-   Die untergeordneten Elemente und ihre Nachfolger, die in der Überlaufspalte der Datenbank gespeichert sind, werden nach dem im Schema explizit angegebenen Inhalt als untergeordnete Elemente hinzugefügt. (Die Reihenfolge wird nicht beibehalten.)  
  
## <a name="examples"></a>Beispiele  
 Es müssen bestimmte Anforderungen erfüllt sein, damit aus den folgenden Beispielen funktionierende Beispiele erstellt werden können. Weitere Informationen finden Sie unter [Anforderungen zum Ausführen von SQLXML-Beispielen](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-sqloverflow-field-for-an-element"></a>A. Angeben von "sql:overflow-field" für ein Element  
 Im folgenden Beispiel wird vorausgesetzt, dass das folgende Skript ausgeführt wurde, damit eine Tabelle mit dem Namen Customers2 in der tempdb-Datenbank vorhanden ist:  
  
```  
USE tempdb  
CREATE TABLE Customers2 (  
CustomerID       VARCHAR(10),   
ContactName    VARCHAR(30),   
AddressOverflow    NVARCHAR(500))  
  
GO  
INSERT INTO Customers2 VALUES (  
'ALFKI',   
'Joe',  
'<Address>  
  <Address1>Maple St.</Address1>  
  <Address2>Apt. E105</Address2>  
  <City>Seattle</City>  
  <State>WA</State>  
  <Zip>98147</Zip>  
 </Address>')  
GO  
```  
  
 Außerdem müssen Sie ein virtuelles Verzeichnis für die tempdb-Datenbank und einen virtuellen Vorlagen **Namen mit dem Namen "Template"** erstellen.  
  
 Im folgenden Beispiel ruft das Zuordnungsschema die nicht verbrauchten Daten ab, die in der Spalte AddressOverflow der Tabelle Customers2 gespeichert sind.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="Customers2" sql:overflow-field="AddressOverflow" >  
    <xsd:complexType>  
      <xsd:attribute name="CustomerID"  type="xsd:integer"/>  
      <xsd:attribute name="ContactName"  type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>So testen Sie eine XPath-Beispiel Abfrage für das Schema  
  
1.  Kopieren Sie den oben stehenden Schemacode, und fügen Sie ihn in eine Textdatei ein. Speichern Sie die Datei unter dem Dateinamen Overflow.xml.  
  
2.  Kopieren Sie die folgende Vorlage, und fügen Sie sie in eine Textdatei ein. Speichern Sie die Datei unter dem Namen OverflowT.xml im gleichen Verzeichnis, in dem Sie Overflow.xml gespeichert haben. Die Abfrage in der Vorlage wählt die Datensätze in der Customers2-Tabelle aus.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="Overflow.xml">  
            /Customers2  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Der für das Zuordnungsschema (Overflow.xml) angegebene Verzeichnispfad bezieht sich auf das Verzeichnis, in dem die Vorlage gespeichert ist. Es kann auch ein absoluter Pfad angegeben werden. Beispiel:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\Overflow.xml"  
    ```  
  
3.  Erstellen und verwenden Sie das SQLXML 4.0-Testskript (Sqlxml4test.vbs), um die Vorlage auszuführen.  

     Weitere Informationen finden Sie unter [Verwenden von ADO zum Ausführen von SQLXML 4,0-Abfragen](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Customers2 CustomerID="ALFKI" ContactName="Joe">  
    <Address1>Maple St.</Address1>   
    <Address2>Apt. E105</Address2>   
    <City>Seattle</City>   
    <State>WA</State>   
    <Zip>98147</Zip>   
  </Customers2>  
</ROOT>  
```  
  
  
