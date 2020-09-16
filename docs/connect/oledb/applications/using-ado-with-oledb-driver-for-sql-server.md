---
title: Verwenden von ADO mit dem OLE DB-Treiber
description: Erfahren Sie, wie Sie ADO mit dem OLE DB-Treiber einschließlich neuer Features wie mehrerer aktiver Resultsets, Abfragebenachrichtigungen, benutzerdefinierter Typen oder des XML-Datentyps verwenden.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, ADO
- data access [OLE DB Driver for SQL Server], ADO
- ADO [OLE DB Driver for SQL Server]
- MSOLEDBSQL, ADO
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 38446b1bd20ac251bcb25a57e9dee0c4debdb6ab
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88860653"
---
# <a name="using-ado-with-ole-db-driver-for-sql-server"></a>Verwenden von ADO mit dem OLE DB-Treiber für SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Um die neuen Funktionen von [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], beispielsweise mehrere aktive Resultsets (MARS), Abfragebenachrichtigungen, benutzerdefinierte Typen (User-Defined Types, UDTs) oder den neuen Datentyp **XML** verwenden zu können, sollten bestehende Anwendungen, die ADO (ActiveX Data Objects) verwenden, den OLE DB-Anbieter von OLE DB-Treiber für SQL Server als Datenzugriffsanbieter nutzen.  
  
 Damit ADO die neuen Funktionen der letzten Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwenden kann, wurde der OLE DB-Treiber für SQL Server, der die Kernfunktionen von OLE DB erweitert, um einige Erweiterungen ergänzt. Diese Erweiterungen erlauben es ADO-Anwendungen, neuere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Features zu verwenden und zwei Datentypen zu verarbeiten, die in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] eingeführt wurden: **XML** und **UDT**. Diese Erweiterungen machen sich auch Erweiterungen der Datentypen **varchar**, **nvarchar** und **varbinary** zunutze. Der OLE DB-Treiber für SQL Server fügt für ADO-Anwendungen die Initialisierungseigenschaft SSPROP_INIT_DATATYPECOMPATIBILITY dem DBPROPSET_SQLSERVERDBINIT-Eigenschaftensatz hinzu, damit die neuen Datentypen in einer mit ADO kompatiblen Weise verfügbar gemacht werden. Darüber hinaus definiert der OLE DB-Treiber für SQL Server auch ein neues Verbindungszeichenfolgenschlüsselwort namens **DataTypeCompatibility**, das in der Verbindungszeichenfolge festgelegt wird.  

> [!NOTE]  
>  Vorhandene ADO-Anwendungen können über den SQLOLEDB-Anbieter auf XML, UDT, umfangreiche Textwerte und Werte von Binärfeldern zugreifen und diese aktualisieren. Die neuen größeren Datentypen **varchar(max)** , **nvarchar(max)** und **varbinary(max)** werden als die ADO-Typen **adLongVarChar**, **adLongVarWChar** bzw. **adLongVarBinary** zurückgegeben. XML-Spalten werden als **adLongVarChar** zurückgegeben, und UDT-Spalten werden als **adVarBinary** zurückgegeben. Wenn Sie jedoch den OLE DB-Treiber für SQL Server (MSOLEDBSQL) anstelle von SQLOLEDB verwenden, müssen Sie sicherstellen, dass Sie für das Schlüsselwort **DataTypeCompatibility** den Wert „80“ festlegen, damit die neuen Datentypen ordnungsgemäß den ADO-Datentypen zugeordnet werden.  

## <a name="enabling-ole-db-driver-for-sql-server-from-ado"></a>Aktivieren des OLE DB-Treibers für SQL Server über ADO  
 Um die Verwendung von OLE DB-Treiber für SQL Server zu ermöglichen, müssen ADO-Anwendungen die folgenden Schlüsselwörter in ihrer Verbindungszeichenfolge angeben:  

-   `Provider=MSOLEDBSQL`  

-   `DataTypeCompatibility=80`  

 Weitere Informationen zu den vom OLE DB-Treiber für SQL Server unterstützten Schlüsselwörtern für ADO-Verbindungszeichenfolgen finden Sie unter [Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server](using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

 Es folgt ein Beispiel, in dem eine ADO-Verbindungszeichenfolge eingerichtet wird, mit der die Verwendung von des OLE DB-Treibers für SQL Server in vollem Umfang ermöglicht und die MARS-Funktion aktiviert wird:  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  
```  

## <a name="examples"></a>Beispiele  
 In den folgenden Abschnitten finden Sie Beispiele zur Verwendung von ADO mit dem OLE DB-Treiber für SQL Server.  

### <a name="retrieving-xml-column-data"></a>Abrufen von XML-Spaltendaten  
 In diesem Beispiel wird ein Recordset verwendet, um die Daten aus einer XML-Spalte der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Beispieldatenbank **AdventureWorks** abzurufen und anzuzeigen.  

```  
Dim con As New ADODB.Connection  
Dim rst As New ADODB.Recordset  
Dim sXMLResult As String  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _   
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the xml data as a recordset.  
Set rst.ActiveConnection = con  
rst.Source = "SELECT AdditionalContactInfo FROM Person.Contact " _  
   & "WHERE AdditionalContactInfo IS NOT NULL"  
rst.Open  

' Display the data in the recordset.  
While (Not rst.EOF)  
   sXMLResult = rst.Fields("AdditionalContactInfo").Value  
   Debug.Print (sXMLResult)  
   rst.MoveNext  
End While  

con.Close  
Set con = Nothing  
```  

> [!NOTE]  
>  Recordset-Filter werden bei XML-Spalten nicht unterstützt. Wenn sie verwendet werden, wird ein Fehler zurückgegeben.  

### <a name="retrieving-udt-column-data"></a>Abrufen von UDT-Spaltendaten  
 In diesem Beispiel wird ein **Command**-Objekt verwendet, um eine SQL-Abfrage auszuführen, die einen UDT zurückgibt. Der UDT wird aktualisiert, und neue Daten werden anschließend wieder in die Datenbank eingefügt. In diesem Beispiel wird davon ausgegangen, dass der UDT **Point** bereits in der Datenbank registriert wurde.  

```  
Dim con As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
Dim strOldUDT As String  
Dim strNewUDT As String  
Dim aryTempUDT() As String  
Dim strTempID As String  
Dim i As Integer  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the UDT value.  
Set cmd.ActiveConnection = con  
cmd.CommandText = "SELECT ID, Pnt FROM dbo.Points.ToString()"  
Set rst = cmd.Execute  
strTempID = rst.Fields(0).Value  
strOldUDT = rst.Fields(1).Value  

' Do something with the UDT by adding i to each point.  
arytempUDT = Split(strOldUDT, ",")  
i = 3  
strNewUDT = LTrim(Str(Int(aryTempUDT(0)) + i)) + "," + _  
   LTrim(Str(Int(aryTempUDT(1)) + i))  

' Insert the new value back into the database.  
cmd.CommandText = "UPDATE dbo.Points SET Pnt = '" + strNewUDT + _  
   "' WHERE ID = '" + strTempID + "'"  
cmd.Execute  

con.Close  
Set con = Nothing  
```  

### <a name="enabling-and-using-mars"></a>Aktivieren und Verwenden von MARS  
 In diesem Beispiel wird die Verbindungszeichenfolge so eingerichtet, dass MARS über den OLE DB-Treiber für SQL Server aktiviert wird, und dann werden zwei Recordset-Objekte erstellt, die über die gleiche Verbindung ausgeführt werden sollen.  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  

Dim recordset1 As New ADODB.Recordset  
Dim recordset2 As New ADODB.Recordset  

Dim recordsaffected As Integer  
Set recordset1 =  con.Execute("SELECT * FROM Table1", recordsaffected, adCmdText)  
Set recordset2 =  con.Execute("SELECT * FROM Table2", recordsaffected, adCmdText)  

con.Close  
Set con = Nothing  
```  

 In früheren Versionen des OLE DB-Anbieters hätte dieser Code bewirkt, dass für den zweiten Execute-Aufruf eine Standardverbindung erstellt wird, weil in diesen Versionen nur ein aktives Resultset pro Verbindung geöffnet werden konnte. Weil die Standardverbindung nicht in den OLE DB-Verbindungspool aufgenommen wurde, bedeutete dies zusätzlichen Aufwand. Wenn der OLE DB-Anbieter von OLE DB-Treiber für SQL Server verfügbar macht, sind mehrere aktive Resultsets in einer Verbindung zulässig.  

## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Anwendungen mit dem OLE DB-Treiber für SQL Server](building-applications-with-oledb-driver-for-sql-server.md)  
