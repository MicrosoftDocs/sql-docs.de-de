---
description: Erstellen, Ändern und Löschen von Indizes
title: Erstellen, Ändern und Löschen von Indizes
ms.custom: seo-dt-2019
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- indexes [SMO]
ms.assetid: ad1befa5-46e0-4895-b9d3-42852e07607b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ad90fe2deb97b947161f12c54f07a15436008f6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340086"
---
# <a name="creating-altering-and-removing-indexes"></a>Erstellen, Ändern und Löschen von Indizes

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  In der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Objects-Hierarchie (SMO) werden Indizes durch das <xref:Microsoft.SqlServer.Management.Smo.Index>-Objekt dargestellt. Die indizierten Spalten werden durch eine Auflistung von <xref:Microsoft.SqlServer.Management.Smo.IndexedColumn>-Objekten dargestellt, die von der <xref:Microsoft.SqlServer.Management.Smo.Index.IndexedColumns%2A>-Eigenschaft dargestellt werden.  
  
 Sie können einen Index für eine XML-Spalte erstellen, indem Sie die <xref:Microsoft.SqlServer.Management.Smo.Index.IsXmlIndex%2A>-Eigenschaft des <xref:Microsoft.SqlServer.Management.Smo.Index>-Objekts angeben.  
  
## <a name="examples"></a>Beispiele  
 Zum Verwenden eines angegebenen Codebeispiels müssen Sie die Programmierumgebung, Programmiervorlage und die zu verwendende Programmiersprache auswählen, um Ihre Anwendung zu erstellen. Weitere Informationen finden Sie unter [Erstellen eines Visual C-&#35; SMO-Projekts in Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-non-clustered-composite-index-in-visual-basic"></a>Erstellen eines nicht gruppierten, zusammengesetzten Indexes in Visual Basic  
 In diesem Codebeispiel wird veranschaulicht, wie ein zusammengesetzter, nicht gruppierter Index erstellt wird. Fügen Sie dem Index für einen zusammengesetzten Index mehr als eine Spalte hinzu. Legen Sie die- <xref:Microsoft.SqlServer.Management.Smo.Index.IsClustered%2A> Eigenschaft für einen nicht gruppierten Index auf **false** fest.  
  
```  
' /r:Microsoft.SqlServer.Smo.dll  
' /r:Microsoft.SqlServer.ConnectionInfo.dll  
' /r:Microsoft.SqlServer.SqlEnum.dll  
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
Public Class A  
    Public Shared Sub Main()  
        ' Connect to the local, default instance of SQL Server.   
        Dim srv As Server  
        srv = New Server()  
  
        ' Reference the AdventureWorks2012 database.   
        Dim db As Database  
        db = srv.Databases("AdventureWorks2012")  
  
        ' Declare a Table object and reference the HumanResources table.   
        Dim tb As Table  
        tb = db.Tables("Employee", "HumanResources")  
  
        ' Define an Index object variable by providing the parent table and index name in the constructor.   
        Dim idx As Index  
        idx = New Index(tb, "TestIndex")  
  
        ' Add indexed columns to the index.   
        Dim icol1 As IndexedColumn  
        icol1 = New IndexedColumn(idx, "BusinessEntityID", True)  
        idx.IndexedColumns.Add(icol1)  
        Dim icol2 As IndexedColumn  
        icol2 = New IndexedColumn(idx, "HireDate", True)  
        idx.IndexedColumns.Add(icol2)  
  
        ' Set the index properties.   
        idx.IndexKeyType = IndexKeyType.DriUniqueKey  
        idx.IsClustered = False  
        idx.FillFactor = 50  
  
        ' Create the index on the instance of SQL Server.   
        idx.Create()  
  
        ' Modify the page locks property.   
        idx.DisallowPageLocks = True  
  
        ' Run the Alter method to make the change on the instance of SQL Server.   
        idx.Alter()  
  
        ' Remove the index from the table.   
        idx.Drop()  
    End Sub  
End Class  
  
```  
  
## <a name="creating-a-non-clustered-composite-index-in-visual-c"></a>Erstellen eines nicht gruppierten, zusammengesetzten Index in Visual C#  
 In diesem Codebeispiel wird veranschaulicht, wie ein zusammengesetzter, nicht gruppierter Index erstellt wird. Fügen Sie dem Index für einen zusammengesetzten Index mehr als eine Spalte hinzu. Legen Sie die- <xref:Microsoft.SqlServer.Management.Smo.Index.IsClustered%2A> Eigenschaft für einen nicht gruppierten Index auf **false** fest.  
  
```  
// /r:Microsoft.SqlServer.Smo.dll  
// /r:Microsoft.SqlServer.ConnectionInfo.dll  
// /r:Microsoft.SqlServer.SqlEnum.dll  
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
using Microsoft.SqlServer.Management.Smo;  
  
public class A {  
   public static void Main() {  
      // Connect to the local, default instance of SQL Server.   
      Server srv;  
      srv = new Server();  
  
      // Reference the AdventureWorks2012 database.   
      Database db;  
      db = srv.Databases["AdventureWorks2012"];  
  
      // Declare a Table object and reference the HumanResources table.   
      Table tb;  
      tb = db.Tables["Employee", "HumanResources"];  
  
      // Define an Index object variable by providing the parent table and index name in the constructor.   
      Index idx;  
      idx = new Index(tb, "TestIndex");  
  
      // Add indexed columns to the index.   
      IndexedColumn icol1;  
      icol1 = new IndexedColumn(idx, "BusinessEntityID", true);  
      idx.IndexedColumns.Add(icol1);  
      IndexedColumn icol2;  
      icol2 = new IndexedColumn(idx, "HireDate", true);  
      idx.IndexedColumns.Add(icol2);  
  
      // Set the index properties.   
      idx.IndexKeyType = IndexKeyType.DriUniqueKey;  
      idx.IsClustered = false;  
      idx.FillFactor = 50;  
  
      // Create the index on the instance of SQL Server.   
      idx.Create();  
  
      // Modify the page locks property.   
      idx.DisallowPageLocks = true;  
  
      // Run the Alter method to make the change on the instance of SQL Server.   
      idx.Alter();  
  
      // Remove the index from the table.   
      idx.Drop();  
   }  
}  
  
```  
  
## <a name="creating-a-non-clustered-composite-index-in-powershell"></a>Erstellen eines nicht gruppierten, zusammengesetzten Index in PowerShell  
 In diesem Codebeispiel wird veranschaulicht, wie ein zusammengesetzter, nicht gruppierter Index erstellt wird. Fügen Sie dem Index für einen zusammengesetzten Index mehr als eine Spalte hinzu. Legen Sie die- <xref:Microsoft.SqlServer.Management.Smo.Index.IsClustered%2A> Eigenschaft für einen nicht gruppierten Index auf **false** fest.  
  
```  
# Set the path context to the local, default instance of SQL Server and to the  
#database tables in Adventureworks2012  
CD \sql\localhost\default\databases\AdventureWorks2012\Tables\  
  
#Get a reference to the table  
$tb = get-item HumanResources.Employee  
  
#Define an Index object variable by providing the parent table and index name in the constructor.   
$idx = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Index -argumentlist $tb, "TestIndex"  
  
#Add indexed columns to the index.   
$icol1 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.IndexedColumn `  
-argumentlist $idx, "BusinessEntityId", $true  
$idx.IndexedColumns.Add($icol1)  
  
$icol2 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.IndexedColumn `  
-argumentlist $idx, "HireDate", $true  
$idx.IndexedColumns.Add($icol2)  
  
#Set the index properties.   
$idx.IndexKeyType = [Microsoft.SqlServer.Management.SMO.IndexKeyType]::DriUniqueKey   
$idx.IsClustered = $false  
$idx.FillFactor = 50  
  
#Create the index on the instance of SQL Server.   
$idx.Create()  
  
#Modify the page locks property.   
$idx.DisallowPageLocks = $true  
  
#Run the Alter method to make the change on the instance of SQL Server.   
$idx.Alter()  
  
#Remove the index from the table.   
$idx.Drop();  
```  
  
## <a name="creating-an-xml-index-in-visual-basic"></a>Erstellen eines XML-Index in Visual Basic  
 Dieses Codebeispiel zeigt, wie ein XML-Index für einen XML-Datentyp erstellt wird. Der XML-Datentyp ist eine XML-Schemaauflistung mit dem Namen MySampleCollection, die in [Using XML Schemas](../../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)erstellt wird. XML-Indizes weisen einige Einschränkungen auf. Eine dieser Einschränkungen ist, dass der Index auf einer Tabelle erstellt werden muss, die bereits über einen gruppierten Primärschlüssel verfügt.  
  
```  
' /r:Microsoft.SqlServer.Smo.dll  
' /r:Microsoft.SqlServer.ConnectionInfo.dll  
' /r:Microsoft.SqlServer.SqlEnum.dll  
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
  
Public Class A  
    Public Shared Sub Main()  
        ' Connect to the local, default instance of SQL Server.   
        Dim srv As Server  
        srv = New Server()  
        Dim db1 As Database = srv.Databases("TESTDB")  
        ' Define a Table object variable and add an XML type column.   
        Dim tb As New Table(db1, "XmlTable3")  
  
        Dim mySample As New XmlSchemaCollection(db1, "Sample4", "dbo")  
        mySample.Text = "<xsd:schema xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" targetNamespace=""NS2""> <xsd:element name=""elem1"" type=""xsd:integer""/></xsd:schema>"  
        mySample.Create()  
  
        Dim col11 As Column  
  
        ' This sample requires that an XML schema type called MySampleCollection exists on the database.   
        col11 = New Column(tb, "XMLValue", DataType.Xml("Sample4"))  
  
        ' Add another integer column that can be made into a unique, primary key.   
        tb.Columns.Add(col11)  
        Dim col21 As Column  
        col21 = New Column(tb, "Number", DataType.Int)  
        col21.Nullable = False  
        tb.Columns.Add(col21)  
  
        ' Create the table of the instance of SQL Server.   
        tb.Create()  
  
        ' Create a unique, clustered, primary key index on the integer column. This is required for an XML index.   
        Dim cp As Index  
        cp = New Index(tb, "clusprimindex3")  
        cp.IsClustered = True  
        cp.IndexKeyType = IndexKeyType.DriPrimaryKey  
        Dim cpcol As IndexedColumn  
        cpcol = New IndexedColumn(cp, "Number", True)  
        cp.IndexedColumns.Add(cpcol)  
        cp.Create()  
  
        ' Define and XML Index object variable by supplying the parent table and the XML index name arguments in the constructor.   
        Dim i As Index  
        i = New Index(tb, "xmlindex")  
        Dim ic As IndexedColumn  
        ic = New IndexedColumn(i, "XMLValue", True)  
        i.IndexedColumns.Add(ic)  
  
        ' Create the XML index on the instance of SQL Server.   
        i.Create()  
    End Sub  
End Class  
  
```  
  
## <a name="creating-an-xml-index-in-visual-c"></a>Erstellen eines XML-Index in Visual C#  
 Dieses Codebeispiel zeigt, wie ein XML-Index für einen XML-Datentyp erstellt wird. Der XML-Datentyp ist eine XML-Schemaauflistung mit dem Namen MySampleCollection, die in [Using XML Schemas](../../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)erstellt wird. XML-Indizes weisen einige Einschränkungen auf. Eine dieser Einschränkungen ist, dass der Index auf einer Tabelle erstellt werden muss, die bereits über einen gruppierten Primärschlüssel verfügt.  
  
```  
// /r:Microsoft.SqlServer.Smo.dll  
// /r:Microsoft.SqlServer.ConnectionInfo.dll  
// /r:Microsoft.SqlServer.SqlEnum.dll  
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
using Microsoft.SqlServer.Management.Smo;  
  
public class A {  
   public static void Main() {  
      // Connect to the local, default instance of SQL Server.   
      Server srv;  
      srv = new Server();  
      Database db1 = srv.Databases["TESTDB"];  
      // Define a Table object variable and add an XML type column.   
      Table tb = new Table(db1, "XmlTable3");  
  
      XmlSchemaCollection mySample = new XmlSchemaCollection(db1, "Sample4", "dbo");  
      mySample.Text = "<xsd:schema xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\" targetNamespace=\"NS2\"> <xsd:element name=\"elem1\" type=\"xsd:integer\"/></xsd:schema>";  
      mySample.Create();  
  
      Column col11;  
  
      // This sample requires that an XML schema type called MySampleCollection exists on the database.   
      col11 = new Column(tb, "XMLValue", DataType.Xml("Sample4"));  
  
      // Add another integer column that can be made into a unique, primary key.   
      tb.Columns.Add(col11);  
      Column col21;  
      col21 = new Column(tb, "Number", DataType.Int);  
      col21.Nullable = false;  
      tb.Columns.Add(col21);  
  
      // Create the table of the instance of SQL Server.   
      tb.Create();  
  
      // Create a unique, clustered, primary key index on the integer column. This is required for an XML index.   
      Index cp;  
      cp = new Index(tb, "clusprimindex3");  
      cp.IsClustered = true;  
      cp.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      IndexedColumn cpcol;  
      cpcol = new IndexedColumn(cp, "Number", true);  
      cp.IndexedColumns.Add(cpcol);  
      cp.Create();  
  
      // Define and XML Index object variable by supplying the parent table and the XML index name arguments in the constructor.   
      Index i;  
      i = new Index(tb, "xmlindex");  
      IndexedColumn ic;  
      ic = new IndexedColumn(i, "XMLValue", true);  
      i.IndexedColumns.Add(ic);  
  
      // Create the XML index on the instance of SQL Server.   
      i.Create();  
   }  
}  
  
```  
  
## <a name="creating-an-xml-index-in-powershell"></a>Erstellen eines XML-Index in PowerShell  
 Dieses Codebeispiel zeigt, wie ein XML-Index für einen XML-Datentyp erstellt wird. Der XML-Datentyp ist eine XML-Schemaauflistung mit dem Namen MySampleCollection, die in [Using XML Schemas](../../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)erstellt wird. XML-Indizes weisen einige Einschränkungen auf. Eine dieser Einschränkungen ist, dass der Index auf einer Tabelle erstellt werden muss, die bereits über einen gruppierten Primärschlüssel verfügt.  
  
```  
# Set the path context to the local, default instance of SQL Server and get a reference to adventureworks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
#Define a Table object variable and add an XML type column.   
#This sample requires that an XML schema type called MySampleCollection exists on the database.   
#See sample on Creating an XML schema to do this  
$tb = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Table -argumentlist $db, "XmlTable"  
$Type = [Microsoft.SqlServer.Management.SMO.DataType]::Xml("MySampleCollection")  
$col1 =  New-Object -TypeName Microsoft.SqlServer.Management.SMO.Column -argumentlist $tb,"XMLValue", $Type  
$tb.Columns.Add($col1)  
  
#Add another integer column that can be made into a unique, primary key.   
$Type = [Microsoft.SqlServer.Management.SMO.DataType]::Int  
$col2 =  New-Object -TypeName Microsoft.SqlServer.Management.SMO.Column -argumentlist $tb,"Number", $Type  
$col2.Nullable = $false  
$tb.Columns.Add($col2)  
  
#Create the table of the instance of SQL Server.   
$tb.Create()  
  
#Create a unique, clustered, primary key index on the integer column. This is required for an XML index.   
#Define an Index object variable by providing the parent table and index name in the constructor.   
$cp = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Index -argumentlist $tb, "clusprimindex"          
$cp.IsClustered = $true;  
$cp.IndexKeyType = [Microsoft.SqlServer.Management.SMO.IndexKeyType]::DriPrimaryKey;  
  
#Create and add an indexed column to the index.   
$cpcol = New-Object -TypeName Microsoft.SqlServer.Management.SMO.IndexedColumn `  
-argumentlist $cp, "Number", $true  
$cp.IndexedColumns.Add($cpcol)  
$cp.Create()  
  
#Define and XML Index object variable by supplying the parent table and  
# the XML index name arguments in the constructor.   
$i = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Index -argumentlist $tb, "xmlindex"   
  
#Create and add an indexed column to the index.   
$ic = New-Object -TypeName Microsoft.SqlServer.Management.SMO.IndexedColumn `  
-argumentlist $i, "XMLValue", $true    
$i.IndexedColumns.Add($ic)  
  
#Create the XML index on the instance of SQL Server  
$i.Create()  
```  
  
## <a name="see-also"></a>Siehe auch  
 <xref:Microsoft.SqlServer.Management.Smo.Index>  
  
  
