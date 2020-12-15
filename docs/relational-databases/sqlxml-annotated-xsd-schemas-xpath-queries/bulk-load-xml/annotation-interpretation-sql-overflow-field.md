---
title: 'SQL: overflow-field (SQLXML)'
description: 'Erfahren Sie, wie Sie die SQL: overflow-field-Anmerkung verwenden, um eine Spalte als Überlauf Spalte zu identifizieren, die alle nicht verbrauchten Daten aus dem XML-Dokument empfängt.'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- unconsumed data
- overflow data [SQLXML]
- sql:overflow-field
ms.assetid: f005182b-6151-432d-ab22-3bc025742cd3
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4d0fa21bcd570a5f5196dfbc1bdf8e1bb6186cb8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462941"
---
# <a name="annotation-interpretation---sqloverflow-field"></a>Interpretation von Anmerkungen – sql:overflow-field
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In einem Schema können Sie eine Spalte als Überlaufspalte festlegen, die alle nicht verbrauchten Daten aus dem XML-Dokument aufnimmt. Diese Spalte wird im Schema mithilfe der **sql:overflow-field** -Anmerkung angegeben. Sie können mit mehreren Überlaufspalten arbeiten.  
  
 Immer, wenn ein XML-Knoten (Element oder Attribut), für den eine **sql:overflow-field** -Anmerkung definiert ist, in den Bereich eintritt, wird die Überlaufspalte aktiviert und empfängt nicht verbrauchte Daten. Wenn der Knoten den Bereich verlässt, ist die Überlaufspalte nicht mehr aktiv und das vorherige Überlauffeld (sofern vorhanden) wird durch das XML-Massenladen aktiviert.  
  
 Bei der Speicherung der Daten in der Überlaufspalte speichert der XML-Massenladevorgang ebenfalls die Start- und Endtags des übergeordneten Elements, für das **sql:overflow-field** definiert ist.  
  
 Im folgenden Schema werden z **\<Customers>** . b. die-und- **\<CustOrder>** Elemente beschrieben. Beide Elemente geben eine Überlaufspalte an:  
  
```  
<?xml version="1.0" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
 <xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
        parent="Cust"  
        parent-key="CustomerID"  
        child="CustOrder"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
 </xsd:annotation>  
 <xsd:element name="ROOT" sql:is-constant="1">  
  <xsd:complexType>  
    <xsd:sequence>   
      <xsd:element name="Customers"   
                   sql:relation="Cust"  
                   sql:overflow-field="OverflowColumn">  
        <xsd:complexType>  
             <xsd:sequence>   
             <xsd:element name="CustomerID" type="xsd:integer"/>  
             <xsd:element name="CompanyName"  type="xsd:string"/>  
             <xsd:element name="City" type="xsd:string"/>  
             <xsd:element name="Order"  
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder"  
                          sql:overflow-field="OverflowColumn">  
               <xsd:complexType>  
                 <xsd:attribute name="OrderID"/>  
                 <xsd:attribute name="CustomerID"/>  
               </xsd:complexType>  
             </xsd:element>  
          </xsd:sequence>   
        </xsd:complexType>  
      </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Im Schema wird das **\<Customer>** -Element der Cust-Tabelle und das- **\<Order>** Element der CustOrder-Tabelle zugeordnet.  
  
 Sowohl das **\<Customer>** -Element als auch das- **\<Order>** Element identifizieren eine Überlauf Spalte. Folglich speichert XML-Massen laden alle nicht verbrauchten untergeordneten Elemente und Attribute des- **\<Customer>** Elements in der Überlauf Spalte der Cust-Tabelle und alle nicht verbrauchten untergeordneten Elemente und Attribute des- **\<Order>** Elements in der Überlauf Spalte der CustOrder-Tabelle.  
  
### <a name="to-test-a-working-sample"></a>So testen Sie ein funktionstüchtiges Beispiel  
  
1.  Speichern Sie das in diesem Beispiel bereitgestellte Schema unter dem Dateinamen SampleSchema.xml.  
  
2.  Erstellen Sie die folgenden Tabellen:  
  
    ```  
    CREATE TABLE Cust (  
              CustomerID     int         PRIMARY KEY,  
              CompanyName    varchar(20) NOT NULL,  
              City           varchar(20) DEFAULT 'Seattle',  
              OverflowColumn nvarchar(200))  
    GO  
    CREATE TABLE CustOrder (  
              OrderID        int         PRIMARY KEY,  
              CustomerID     int         FOREIGN KEY REFERENCES  
                                              Cust(CustomerID),  
              OverflowColumn nvarchar(200))  
    GO  
    ```  
  
3.  Speichern Sie die folgenden XML-Daten unter dem Dateinamen SampleXMLData.xml:  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City><![CDATA[NY]]> </City>  
          <Junk>garbage in overflow</Junk>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
     </Customers>  
     <Customers>  
       <CustomerID>1112</CustomerID>  
       <CompanyName>Toms Spezialitten</CompanyName>  
       <City><![CDATA[LA]]> </City>  
       <xyz><address>111 Maple, Seattle</address></xyz>     
       <Order OrderID="3" />  
     </Customers>  
       <Customers>  
       <CustomerID>1113</CustomerID>  
       <CompanyName>Victuailles en stock</CompanyName>  
       <Order OrderID="4" />  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Speichern Sie dieses [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition-Beispiel (VBScript) unter dem Dateinamen Sample.vbs, und führen Sie es aus, um das XML-Massenladen auszuführen:  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  
