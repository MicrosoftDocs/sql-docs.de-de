---
description: CREATE XML SCHEMA COLLECTION (Transact-SQL)
title: CREATE XML SCHEMA COLLECTION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE XML SCHEMA COLLECTION
- CREATE_XML_SCHEMA_COLLECTION_TSQL
- CREATE XML SCHEMA
- CREATE_XML_SCHEMA_TSQL
- COLLECTION
- COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML SCHEMA COLLECTION statement
- importing schema components
- schema collections [SQL Server], creating
- multiple schema namespaces
- XML schema collections [SQL Server], creating
ms.assetid: 350684e8-b3f6-4b58-9dbc-0f05cc776ebb
author: MightyPen
ms.author: genemi
ms.openlocfilehash: cb9a8dbb4b67a514d56febaed13a33d7f7eb2a8a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96131383"
---
# <a name="create-xml-schema-collection-transact-sql"></a>CREATE XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Importiert die Schemakomponenten in eine Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE XML SCHEMA COLLECTION [ <relational_schema>. ]sql_identifier AS Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *relational_schema*  
 Identifiziert den Namen des relationalen Schemas. Wird kein Wert angegeben, wird vom relationalen Standardschema ausgegangen.  
  
 *sql_identifier*  
 Der SQL-Bezeichner für die XML-Schemaauflistung.  
  
 *Ausdruck*  
 Eine Zeichenfolgenkonstante oder eine skalare Variable. Ist vom Typ **Varchar**, **Varbinary**, **Nvarchar** oder **xml**.  
  
## <a name="remarks"></a>Bemerkungen  
 Mithilfe von [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md) können Sie der Collection auch neue Namespaces bzw. vorhandenen Namespaces in der Auflistung auch neue Komponenten hinzufügen.  
  
 Informationen zum Entfernen von Collections finden Sie unter [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Erstellen von XML SCHEMA COLLECTION ist zumindest eine der folgenden Berechtigungsgruppen erforderlich:  
  
-   CONTROL-Berechtigung auf dem Server  
  
-   ALTER ANY DATABASE-Berechtigung auf dem Server  
  
-   ALTER-Berechtigung für die Datenbank  
  
-   CONTROL-Berechtigung in der Datenbank  
  
-   ALTER ANY SCHEMA-Berechtigung und CREATE XML SCHEMA COLLECTION-Berechtigung in der Datenbank  
  
-   ALTER- oder CONTROL-Berechtigung für das relationale Schema und CREATE XML SCHEMA COLLECTION-Berechtigung in der Datenbank  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>A. Erstellen einer XML-Schemaauflistung in der Datenbank  
 Im folgenden Beispiel wird die XML-Schemaauflistung `ManuInstructionsSchemaCollection` erstellt. Diese Auflistung hat nur einen Schemanamespace.  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note collection name specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 Alternativ können Sie die Schemaauflistung einer Variablen zuweisen und die Variable in der `CREATE XML SCHEMA COLLECTION`-Anweisung wie folgt angeben:  
  
```  
DECLARE @MySchemaCollection nvarchar(max)  
Set @MySchemaCollection  = N' copy the schema collection here'  
CREATE XML SCHEMA COLLECTION MyCollection AS @MySchemaCollection   
```  
  
 Die Variable im Beispiel ist vom Datentyp `nvarchar(max)`. Möglich ist auch der Datentyp **xml**. In diesem Fall wird die Variable implizit in eine Zeichenfolge konvertiert.  
  
 Weitere Informationen finden Sie unter [Anzeigen einer gespeicherten XML-Schemaauflistung](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Schemaauflistungen können in einer Spalte vom Typ **xml** gespeichert werden. Führen Sie in diesem Fall Folgendes aus, um eine XML-Schemaauflistung zu erstellen:  
  
1.  Rufen Sie die Schemaauflistung mithilfe einer SELECT-Anweisung aus der Spalte ab, und weisen Sie sie einer Variablen vom Datentyp **xml** oder **varchar** zu.  
  
2.  Geben Sie den Variablennamen in der CREATE XML SCHEMA COLLECTION-Anweisung an.  
  
 CREATE XML SCHEMA COLLECTION speichert nur die Schemakomponenten, die SQL Server versteht; es wird nicht der gesamte Inhalt des XML-Schemas in der Datenbank gespeichert. Wenn Sie den ursprünglichen Zustand der XML-Schemaauflistung wiederherstellen möchten, sollten Sie daher Ihre XML-Schemas in einer Datenbankspalte oder in einem anderen Ordner auf dem Computer speichern.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. Angeben mehrerer Schemanamespaces in einer Schemaauflistung  
 Sie können mehrere XML-Schemas angeben, wenn Sie eine XML-Schemaauflistung erstellen. Beispiel:  
  
```sql  
CREATE XML SCHEMA COLLECTION MyCollection AS N'  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->    
</xsd:schema>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->  
</xsd:schema>';  
```  
  
 Im folgenden Beispiel wird die XML-Schemaauflistung `ProductDescriptionSchemaCollection` erstellt, die zwei XML-Schemanamespaces enthält.  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO -- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. Importieren eines Schemas ohne Zielnamespaceangabe  
 Falls ein Schema, das kein **targetNamespace**-Attribut enthält, in eine Collection importiert wird, werden dessen Komponenten wie im folgenden Beispiel dem Zielnamespace mit einer leeren Zeichenfolge zugeordnet. Ist nicht mindestens ein in der Auflistung importiertes Schema zugeordnet, hat dies zur Folge, dass mehrere (potenziell unzusammenhängende) Schemakomponenten dem leeren Standard-Zeichenfolgennamespace zugeordnet werden.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
go  
-- Query will return the names of all the collections that   
--contain a schema with no target namespace.  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
### <a name="d-using-an-xml-schema-collection-and-batches"></a>D: Verwenden von XML-Schemaauflistungen und Batches  
 Es kann nicht auf denselben Batch einer Schemaauflistung verwiesen werden, in dem die Auflistung erstellt wurde. Wenn Sie auf eine Auflistung in demselben Batch verweisen, in dem diese erstellt wurde, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass die Auflistung nicht vorhanden ist. Das folgende Beispiel ist funktionsfähig; wenn Sie jedoch `GO` entfernen und somit auf die XML-Schemaauflistung verweisen, um eine `xml`-Variable einzugeben, wird ein Fehler zurückgegeben.  
  
```sql  
CREATE XML SCHEMA COLLECTION mySC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="string"/>  
</schema>  
';  
GO  
CREATE TABLE T (Col1 xml (mySC));  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ALTER XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [Anforderungen und Einschränkungen für XML-Schemaauflistungen auf dem Server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
