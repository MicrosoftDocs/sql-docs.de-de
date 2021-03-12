---
description: Aufrufen von Methoden
title: Aufrufen von Methoden | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- methods [SMO]
- calling methods
- SQL Server Management Objects, method calling
- SMO [SQL Server], method calling
ms.assetid: c88d5c5f-9ff0-4f84-b2b6-24c6b90fa15e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef9906590a509f05a6ef23153e273a98ff687d89
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770558"
---
# <a name="calling-methods"></a>Aufrufen von Methoden
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Methoden führen bestimmte Tasks aus, die sich auf das Objekt beziehen, z. B. die Ausgabe eines **Checkpoint** in einer Datenbank oder die Anforderung einer Aufzählungsliste mit Anmeldungen für die Instanz von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Methoden führen Vorgänge an Objekten aus. Methoden können Parameter enthalten, und sie verfügen häufig über einen Rückgabewert. Bei dem Rückgabewert kann es sich um einen einfachen Datentyp, ein komplexes Objekt oder eine Struktur handeln, die mehrere Elemente enthält.  
  
 Mithilfe der Ausnahmebehandlung können Sie feststellen, ob die Methode erfolgreich war. Weitere Informationen finden Sie unter [Handling SMO Exceptions](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-exceptions.md).  
  
## <a name="examples"></a>Beispiele  
Zum Verwenden eines angegebenen Codebeispiels müssen Sie die Programmierumgebung, Programmiervorlage und die zu verwendende Programmiersprache auswählen, um Ihre Anwendung zu erstellen. Weitere Informationen finden Sie unter  [Erstellen eines Visual C-&#35; SMO-Projekts in Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
 
  
## <a name="using-a-simple-smo-method-in-visual-basic"></a>Verwenden einer einfachen SMO-Methode in Visual Basic  
 In diesem Beispiel enthält die <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A>-Methode keine Parameter und hat keinen Rückgabewert.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define a Database object variable by supplying the parent server and the database name arguments in the constructor.
Dim db As Database
db = New Database(srv, "Test_SMO_Database")
'Call the Create method to create the database on the instance of SQL Server. 
db.Create()
```
  
## <a name="using-a-simple-smo-method-in-visual-c"></a>Verwenden einer einfachen SMO-Methode in Visual C#  
 In diesem Beispiel enthält die <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A>-Methode keine Parameter und hat keinen Rückgabewert.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define a Database object variable by supplying the parent server and the database name arguments in the constructor.   
Database db;   
db = new Database(srv, "Test_SMO_Database");   
//Call the Create method to create the database on the instance of SQL Server.   
db.Create();   
}
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-basic"></a>Verwenden einer SMO-Methode mit einem Parameter in Visual Basic  
 Das <xref:Microsoft.SqlServer.Management.Smo.Table>-Objekt verfügt über eine Methode mit dem Namen <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Diese Methode benötigt einen numerischen Parameter, der **FillFactor** angibt.  
  
```VBNET
Dim srv As Server  
srv = New Server  
Dim tb As Table  
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources")  
tb.RebuildIndexes(70)  
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-c"></a>Verwenden einer SMO-Methode mit einem Parameter in Visual C#  
 Das <xref:Microsoft.SqlServer.Management.Smo.Table>-Objekt verfügt über eine Methode mit dem Namen <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Diese Methode benötigt einen numerischen Parameter, der `FillFactor`angibt.  
  
```csharp  
{   
Server srv = default(Server);   
srv = new Server();   
Table tb = default(Table);   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
}   
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-basic"></a>Verwenden einer Enumerationsmethode in Visual Basic, die ein "DataTable"-Objekt zurückgibt  
 In diesem Abschnitt wird beschrieben, wie eine Enumerationsmethode aufgerufen wird und wie die Daten im zurückgegebenen <xref:System.Data.DataTable>-Objekt behandelt werden.  
  
 Die <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A>-Methode gibt ein <xref:System.Data.DataTable>-Objekt zurück, für das weitere Navigation erforderlich ist, auf alle verfügbaren Sortierungsinformationen zu der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zuzugreifen.  
  
```VBNET
'Connect to the local, default instance of SQL Server.  
Dim srv As Server  
srv = New Server  
'Call the EnumCollations method and return collation information to DataTable variable.  
Dim d As DataTable  
'Select the returned data into an array of DataRow.  
d = srv.EnumCollations  
'Iterate through the rows and display collation details for the instance of SQL Server.  
Dim r As DataRow  
Dim c As DataColumn  
For Each r In d.Rows  
    Console.WriteLine("==")  
    For Each c In r.Table.Columns  
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)  
    Next  
Next  
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-c"></a>Verwenden einer Enumerationsmethode in Visual C#, die ein "DataTable"-Objekt zurückgibt  
 In diesem Abschnitt wird beschrieben, wie eine Enumerationsmethode aufgerufen wird und wie die Daten im zurückgegebenen <xref:System.Data.DataTable>-Objekt behandelt werden.  
  
 Die <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A>-Methode gibt ein <xref:System.Data.DataTable>-Systemobjekt zurück. Für das <xref:System.Data.DataTable>-Objekt ist weitere Navigation erforderlich, um auf alle verfügbaren Sortierungsinformationen zu der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zuzugreifen.  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumCollations;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=========");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="constructing-an-object-in-visual-basic"></a>Erstellen eines Objekts in Visual Basic  
 Der Konstruktor eines Objekts kann mit dem **New** -Operator aufgerufen werden. Der <xref:Microsoft.SqlServer.Management.Smo.Database>-Objektkonstruktor ist überladen, und die Version des im Beispiel verwendeten <xref:Microsoft.SqlServer.Management.Smo.Database>-Objektkonstruktors akzeptiert zwei Parameter: das übergeordnete <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekt, zu dem die Datenbank gehört, und eine Zeichenfolge, die den Namen der neuen Datenbank darstellt.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.
Dim d As Database
d = New Database(srv, "Test_SMO_Database")
'Create the database on the instance of SQL Server.
d.Create()
Console.WriteLine(d.Name)
```
  
## <a name="constructing-an-object-in-visual-c"></a>Erstellen eines Objekts in Visual C#  
 Der Konstruktor eines Objekts kann mit dem **New** -Operator aufgerufen werden. Der <xref:Microsoft.SqlServer.Management.Smo.Database>-Objektkonstruktor ist überladen, und die Version des im Beispiel verwendeten <xref:Microsoft.SqlServer.Management.Smo.Database>-Objektkonstruktors akzeptiert zwei Parameter: das übergeordnete <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekt, zu dem die Datenbank gehört, und eine Zeichenfolge, die den Namen der neuen Datenbank darstellt.  
  
```csharp  
{   
Server srv;   
srv = new Server();   
Table tb;   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.   
Database d;   
d = new Database(srv, "Test_SMO_Database");   
//Create the database on the instance of SQL Server.   
d.Create();   
Console.WriteLine(d.Name);   
}  
```  
  
## <a name="copying-an-smo-object-in-visual-basic"></a>Kopieren eines SMO-Objekts in Visual Basic  
 In diesem Codebeispiel wird die <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A>-Methode zum Erstellen einer Kopie des <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekts verwendet. Das <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekt stellt eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dar.  
  
```VBNET  
'Connect to the local, default instance of SQL Server.
Dim srv1 As Server
srv1 = New Server()
'Modify the default database and the timeout period for the connection.
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012"
srv1.ConnectionContext.ConnectTimeout = 30
'Make a second connection using a copy of the ConnectionContext property and verify settings.
Dim srv2 As Server
srv2 = New Server(srv1.ConnectionContext.Copy)
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString)
```
  
## <a name="copying-an-smo-object-in-visual-c"></a>Kopieren eines SMO-Objekts in Visual C#  
 In diesem Codebeispiel wird die <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A>-Methode zum Erstellen einer Kopie des <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekts verwendet. Das <xref:Microsoft.SqlServer.Management.Smo.Server>-Objekt stellt eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dar.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv1;   
srv1 = new Server();   
//Modify the default database and the timeout period for the connection.   
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012";   
srv1.ConnectionContext.ConnectTimeout = 30;   
//Make a second connection using a copy of the ConnectionContext property and verify settings.   
Server srv2;   
srv2 = new Server(srv1.ConnectionContext.Copy);   
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString);   
}  
```  
  
## <a name="monitoring-server-processes-in-visual-basic"></a>Überwachen von Serverprozessen in Visual Basic  
 Informationen zum aktuellen Statustyp der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] können Sie über Enumerationsmethoden abrufen. Im Beispielcode wird die <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A>-Methode zum Ermitteln von Informationen zu den aktuellen Prozessen verwendet. Dieses Beispiel zeigt auch, wie mit den Spalten und Zeilen im zurückgegebenen <xref:System.Data.DataTable>-Objekt gearbeitet wird.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Call the EnumCollations method and return collation information to DataTable variable.
Dim d As DataTable
'Select the returned data into an array of DataRow.
d = srv.EnumProcesses
'Iterate through the rows and display collation details for the instance of SQL Server.
Dim r As DataRow
Dim c As DataColumn
For Each r In d.Rows
    Console.WriteLine("============================================")
    For Each c In r.Table.Columns
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)
    Next
Next
```
  
## <a name="monitoring-server-processes-in-visual-c"></a>Überwachen von Serverprozessen in Visual C#  
 Informationen zum aktuellen Statustyp der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] können Sie über Enumerationsmethoden abrufen. Im Beispielcode wird die <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A>-Methode zum Ermitteln von Informationen zu den aktuellen Prozessen verwendet. Dieses Beispiel zeigt auch, wie mit den Spalten und Zeilen im zurückgegebenen <xref:System.Data.DataTable>-Objekt gearbeitet wird.  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumProcesses;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=====");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="see-also"></a>Siehe auch  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  
