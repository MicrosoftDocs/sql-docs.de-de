---
description: Codieren eines benutzerdefinierten Verbindungs-Managers
title: Codieren eines benutzerdefinierten Verbindungs-Managers | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], coding
ms.assetid: b12b6778-1f01-4a7d-984d-73f2f7630aa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3ad9aa72bb972204bc762ff69692c82c4dda5963
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348932"
---
# <a name="coding-a-custom-connection-manager"></a>Codieren eines benutzerdefinierten Verbindungs-Managers

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Nachdem Sie eine Klasse erstellt haben, die von der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>-Basisklasse erbt, und das <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>-Attribut auf die Klasse angewendet haben, müssen Sie die Implementierung der Eigenschaften und Methoden der Basisklasse überschreiben, um die benutzerdefinierte Funktionalität bereitzustellen.  
  
 Beispiele für benutzerdefinierte Verbindungs-Manager finden Sie unter [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Verbindungs-Manager](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md). Die in diesem Thema dargestellten Codebeispiele stammen aus dem SQL Server Custom Connection Manager-Beispiel.  
  
> [!NOTE]  
>  Die meisten Tasks, Quellen und Ziele, die in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] integriert wurden, können nur mit bestimmten Typen integrierter Verbindungs-Manager verwendet werden. Daher können diese Beispiele nicht mit den integrierten Tasks und Komponenten getestet werden.  
  
## <a name="configuring-the-connection-manager"></a>Konfigurieren des Verbindungs-Managers  
  
### <a name="setting-the-connectionstring-property"></a>Festlegen der ConnectionString-Eigenschaft  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A>-Eigenschaft ist eine wichtige Eigenschaft und die einzige für einen benutzerdefinierten Verbindungs-Manager eindeutige Eigenschaft. Der Verbindungs-Manager verwendet den Wert dieser Eigenschaft, um eine Verbindung zur externen Datenquelle herzustellen. Wenn Sie mehrere andere Eigenschaften miteinander kombinieren, z. B. Servername und Datenbankname, um die Verbindungszeichenfolge zu erstellen, können Sie mit einer Hilfsfunktion die Zeichenfolge zusammenstellen, indem Sie bestimmte Werte in einer Verbindungszeichenfolgenvorlage durch den neuen vom Benutzer bereitgestellten Wert ersetzen. Im folgenden Beispiel wird eine Implementierung der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A>-Eigenschaft dargestellt, die zum Zusammenstellen der Zeichenfolge eine Hilfsfunktion benötigt.  
  
```vb  
' Default values.  
Private _serverName As String = "(local)"  
Private _databaseName As String = "AdventureWorks"  
Private _connectionString As String = String.Empty  
  
Private Const CONNECTIONSTRING_TEMPLATE As String = _  
  "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI"  
  
Public Property ServerName() As String  
  Get  
    Return _serverName  
  End Get  
  Set(ByVal value As String)  
    _serverName = value  
  End Set  
End Property  
  
Public Property DatabaseName() As String  
  Get  
    Return _databaseName  
  End Get  
  Set(ByVal value As String)  
    _databaseName = value  
  End Set  
End Property  
  
Public Overrides Property ConnectionString() As String  
  Get  
    UpdateConnectionString()  
    Return _connectionString  
  End Get  
  Set(ByVal value As String)  
    _connectionString = value  
  End Set  
End Property  
  
Private Sub UpdateConnectionString()  
  
  Dim temporaryString As String = CONNECTIONSTRING_TEMPLATE  
  
  If Not String.IsNullOrEmpty(_serverName) Then  
    temporaryString = temporaryString.Replace("<servername>", _serverName)  
  End If  
  If Not String.IsNullOrEmpty(_databaseName) Then  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName)  
  End If  
  
  _connectionString = temporaryString  
  
End Sub  
```  
  
```csharp  
// Default values.  
private string _serverName = "(local)";  
private string _databaseName = "AdventureWorks";  
private string _connectionString = String.Empty;  
  
private const string CONNECTIONSTRING_TEMPLATE = "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI";  
  
public string ServerName  
{  
  get  
  {  
    return _serverName;  
  }  
  set  
  {  
    _serverName = value;  
  }  
}  
  
public string DatabaseName  
{  
  get  
  {  
    return _databaseName;  
  }  
  set  
  {  
    _databaseName = value;  
  }  
}  
  
public override string ConnectionString  
{  
  get  
  {  
    UpdateConnectionString();  
    return _connectionString;  
  }  
  set  
  {  
    _connectionString = value;  
  }  
}  
  
private void UpdateConnectionString()  
{  
  
  string temporaryString = CONNECTIONSTRING_TEMPLATE;  
  
  if (!String.IsNullOrEmpty(_serverName))  
  {  
    temporaryString = temporaryString.Replace("<servername>", _serverName);  
  }  
  
  if (!String.IsNullOrEmpty(_databaseName))  
  {  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName);  
  }  
  
  _connectionString = temporaryString;  
  
}  
```  
  
### <a name="validating-the-connection-manager"></a>Überprüfen des Verbindungs-Managers  
 Sie überschreiben die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A>-Methode, um sicherzustellen, dass der Verbindungs-Manager ordnungsgemäß konfiguriert wurde. Sie sollten zumindest das Format der Verbindungszeichenfolge überprüfen und sicherstellen, dass die Werte für alle Argumente angegeben wurden. Die Ausführung kann nicht fortgesetzt werden, bis der Verbindungs-Manager <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> von der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A>-Methode zurückgibt.  
  
 Im folgenden Codebeispiel wird eine Implementierung von <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> gezeigt, mit der sichergestellt wird, dass der Benutzer den Servernamen für die Verbindung angegeben hat.  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  If String.IsNullOrEmpty(_serverName) Then  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0)  
    Return DTSExecResult.Failure  
  Else  
    Return DTSExecResult.Success  
  End If  
  
End Function  
```  
  
```csharp  
public override Microsoft.SqlServer.Dts.Runtime.DTSExecResult Validate(Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  if (String.IsNullOrEmpty(_serverName))  
  {  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0);  
    return DTSExecResult.Failure;  
  }  
  else  
  {  
    return DTSExecResult.Success;  
  }  
  
}  
```  
  
### <a name="persisting-the-connection-manager"></a>Beibehalten des Verbindungs-Managers  
 In der Regel müssen Sie keine benutzerdefinierte Persistenz für einen Verbindungs-Manager implementieren. Die benutzerdefinierte Persistenz ist nur erforderlich, wenn die Eigenschaften eines Objekts komplexe Datentypen verwenden. Weitere Informationen finden Sie unter [Developing Custom Objects for Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md) (Entwickeln von benutzerdefinierten Objekten für Integration Services).  
  
## <a name="working-with-the-external-data-source"></a>Arbeiten mit der externen Datenquelle  
 Die Methoden, die eine Verbindung zu einer externen Datenquelle unterstützen, sind die wichtigsten Methoden eines benutzerdefinierten Verbindungs-Managers. Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>-Methode und die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A>-Methode werden zu unterschiedlichen Zeitpunkten während des Entwurfs und der Ausführung aufgerufen.  
  
### <a name="acquiring-the-connection"></a>Abrufen der Verbindung  
 Sie müssen entscheiden, welcher Objekttyp für die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>-Methode geeignet ist, um vom benutzerdefinierten Verbindungs-Manager zurückgegeben zu werden. Ein Dateiverbindungs-Manager gibt beispielsweise nur eine Zeichenfolge zurück, die einen Pfad und einen Dateinamen enthält, ein ADO.NET-Verbindungs-Manager gibt hingegen ein verwaltetes Verbindungsobjekt, das bereits offen ist, zurück. Ein OLE DB-Verbindungs-Manager gibt ein systemeigenes OLE DB-Verbindungsobjekt zurück, das nicht vom verwalteten Code verwendet werden kann. Der benutzerdefinerte SQL Server-Verbindungs-Manager, dem die Codeabschnitte in diesem Themen entnommen wurden, gibt ein offenes **SqlConnection**-Objekt zurück.  
  
 Benutzer Ihres Verbindungs-Managers müssen vorab wissen, welche Objekttypen verwendet werden, sodass sie das zurückgegebene Objekt in den entsprechenden Typ umwandeln können und auf dessen Methoden und Eigenschaften zugreifen können.  
  
```vb  
Public Overrides Function AcquireConnection(ByVal txn As Object) As Object  
  
  Dim sqlConnection As New SqlConnection  
  
  UpdateConnectionString()  
  
  With sqlConnection  
    .ConnectionString = _connectionString  
    .Open()  
  End With  
  
  Return sqlConnection  
  
End Function  
```  
  
```csharp  
public override object AcquireConnection(object txn)  
{  
  
  SqlConnection sqlConnection = new SqlConnection();  
  
  UpdateConnectionString();  
  
  {  
    sqlConnection.ConnectionString = _connectionString;  
    sqlConnection.Open();  
  }  
  
  return sqlConnection;  
  
}  
```  
  
### <a name="releasing-the-connection"></a>Freigeben der Verbindung  
 Die auszuführende Aktion für die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A>-Methode ist vom Objekttyp, der mit der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>-Methode zurückgegeben wird, abhängig. Wenn ein offenes Verbindungsobjekt vorliegt, sollten Sie es schließen, um von ihm verwendete Ressourcen freizugeben. Wenn <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> nur einen Zeichenfolgenwert zurückgegeben hat, muss keine Aktion ausgeführt werden.  
  
```vb  
Public Overrides Sub ReleaseConnection(ByVal connection As Object)  
  
  Dim sqlConnection As SqlConnection  
  
  sqlConnection = DirectCast(connection, SqlConnection)  
  
  If sqlConnection.State <> ConnectionState.Closed Then  
    sqlConnection.Close()  
  End If  
  
End Sub  
```  
  
```csharp  
public override void ReleaseConnection(object connection)  
{  
  SqlConnection sqlConnection;  
  sqlConnection = (SqlConnection)connection;  
  if (sqlConnection.State != ConnectionState.Closed)  
    sqlConnection.Close();  
}  
```  
 
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen eines benutzerdefinierten Verbindungs-Managers](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Verbindungs-Manager](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
