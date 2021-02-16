---
description: Programmgesteuertes Hinzufügen von Verbindungen
title: Programmgesteuertes Hinzufügen von Verbindungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- connection managers [Integration Services], packages
- Integration Services packages, connections
- connections [Integration Services], packages
- SSIS packages, connections
- packages [Integration Services], connections
- intrinsic properties [Integration Services]
- SQL Server Integration Services packages, connections
- ConnectionManager class
- SSIS connection managers
- adding package connections
ms.assetid: d90716d1-4c65-466c-b82c-4aabbee1e3e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3d15ad56ceb2568f2071e4a4170e95afac24e886
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344921"
---
# <a name="adding-connections-programmatically"></a>Programmgesteuertes Hinzufügen von Verbindungen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Klasse stellt physische Verbindungen zu externen Datenquellen dar. Durch die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Klassen werden die Implementierungsdetails der Verbindung von der Laufzeit isoliert. Daher kann die Laufzeit mit den einzelnen Verbindungs-Managern auf eine konsistente, vorhersehbare Weise interagieren. Verbindungs-Manager enthalten eine Reihe von Basiseigenschaften, die alle Verbindungen gemeinsam haben, z. B. die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>-, die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ID%2A>- <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A> und die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>-Eigenschaft. Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>-Eigenschaft und die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>-Eigenschaft sind jedoch in der Regel die einzigen Eigenschaften, die zur Konfiguration eines Verbindungs-Managers erforderlich sind. Im Gegensatz zu anderen Programmierungsmodellen, bei denen Verbindungsklassen Methoden wie die **Open**- oder die **Connect**-Methode verfügbar machen, um eine physische Verbindung mit der Datenquelle herzustellen, verwaltet die Runtime-Engine alle Verbindungen für das Paket während der Ausführung.  
  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.Connections>-Klasse ist eine Auflistung der Verbindungs-Manager, die dem Paket hinzugefügt wurden und zur Laufzeit für die Verwendung zur Verfügung stehen. Sie können der Auflistung mithilfe der <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A>-Methode der Auflistung mehrere Verbindungs-Manager hinzufügen und eine Zeichenfolge bereitstellen, die den Typ des Verbindungs-Managers angibt. Die <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A>-Methode gibt die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Instanz zurück, die dem Paket hinzugefügt wurde.  
  
## <a name="intrinsic-properties"></a>Systeminterne Eigenschaften  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Klasse macht eine Reihe von Eigenschaften verfügbar, die alle Verbindungen gemeinsam haben. In einigen Fällen benötigen Sie jedoch Zugriff auf Eigenschaften, die für den speziellen Verbindungstyp eindeutig sind. Die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Properties%2A>-Auflistung der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Klasse bietet Zugriff auf diese Eigenschaften. Die Eigenschaften können mithilfe des Indexers oder des Eigenschaftennamens und der **GetValue**-Methode aus der Auflistung abgerufen werden, und die Werte werden mithilfe der **setValue**-Methode festgelegt. Die Eigenschaften der zugrunde liegenden Verbindungsobjekteigenschaften können auch festgelegt werden, indem eine tatsächliche Instanz des Objekts abgerufen und die Eigenschaften direkt festgelegt werden. Um die zugrunde liegende Verbindung abzurufen, verwenden Sie die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.InnerObject%2A>-Eigenschaft des Verbindungs-Managers. In der folgenden Codezeile ist eine C#-Zeile dargestellt, die von einem ADO.NET-Verbindungs-Manager mit der zugrunde liegenden Klasse, <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.ConnectionManagerAdoNetClass>, erstellt wird.  
  
 `ConnectionManagerAdoNetClass cmado = cm.InnerObject as ConnectionManagerAdoNet;`  
  
 Dadurch wird das Objekt des verwalteten Verbindungs-Managers in das zugrunde liegende Verbindungsobjekt umgewandelt. Wenn Sie C++ verwenden, wird die **QueryInterface**-Methode des <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekts aufgerufen, und die Oberfläche des zugrunde liegenden Verbindungsobjekts wird angefordert.  
  
 In der folgenden Tabelle sind die in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthaltenen Verbindungs-Manager sowie die Zeichenfolge aufgeführt, die in der `package.Connections.Add("xxx")`-Anweisung verwendet wird. Eine Liste aller Verbindungs-Manager finden Sie unter [Integration Services-Verbindungen &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
|String|Ziel-Editor für Dimensionsverarbeitung|  
|------------|------------------------|  
|„OLEDB“|Verbindungs-Manager für OLE DB-Verbindungen.|  
|„ODBC“|Verbindungs-Manager für ODBC-Verbindungen.|  
|„ADO“|Verbindungs-Manager für ADO-Verbindungen.|  
|„ADO.NET:SQL“|Verbindungs-Manager für ADO.NET-Verbindungen (SQL-Datenanbieter).|  
|„ADO.NET:OLEDB“|Verbindungs-Manager für ADO.NET-Verbindungen (OLE DB-Datenanbieter).|  
|„FLATFILE“|Verbindungs-Manager für Flatfileverbindungen.|  
|„FILE“|Verbindungs-Manager für Dateiverbindungen.|  
|„MULTIFLATFILE“|Verbindungs-Manager für mehrere Flatfileverbindungen.|  
|„MULTIFILE“|Verbindungs-Manager für mehrere Dateiverbindungen.|  
|„SQLMOBILE“|Verbindungs-Manager für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Verbindungen.|  
|„MSOLAP100“|Verbindungs-Manager für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Verbindungen.|  
|„FTP“|Verbindungs-Manager für FTP-Verbindungen.|  
|„HTTP“|Verbindungs-Manager für HTTP-Verbindungen.|  
|„MSMQ“|Verbindungs-Manager für Message Queuing-Verbindungen (auch bekannt als MSMQ).|  
|„SMTP“|Verbindungs-Manager für SMTP-Verbindungen.|  
|„WMI“|Verbindungs-Manager für WMI (Microsoft Windows Management Instrumentation)-Verbindungen.|  
  
 Im folgenden Codebeispiel wird das Hinzufügen einer OLE DB- und Dateiverbindung zur <xref:Microsoft.SqlServer.Dts.Runtime.Package.Connections%2A>-Auflistung eines <xref:Microsoft.SqlServer.Dts.Runtime.Package> veranschaulicht. In dem Beispiel werden dann die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>-Eigenschaft, die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>-Eigenschaft und die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A>-Eigenschaft festgelegt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      // Create a package, and retrieve its connections.  
      Package pkg = new Package();  
      Connections pkgConns = pkg.Connections;  
  
      // Add an OLE DB connection to the package, using the   
      // method defined in the AddConnection class.  
      CreateConnection myOLEDBConn = new CreateConnection();  
      myOLEDBConn.CreateOLEDBConnection(pkg);  
  
      // View the new connection in the package.  
      Console.WriteLine("Connection description: {0}",  
         pkg.Connections["SSIS Connection Manager for OLE DB"].Description);  
  
      // Add a second connection to the package.  
      CreateConnection myFileConn = new CreateConnection();  
      myFileConn.CreateFileConnection(pkg);  
  
      // View the second connection in the package.  
      Console.WriteLine("Connection description: {0}",  
        pkg.Connections["SSIS Connection Manager for Files"].Description);  
  
      Console.WriteLine();  
      Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count);  
  
      Console.Read();  
    }  
  }  
  // <summary>  
  // This class contains the definitions for multiple  
  // connection managers.  
  // </summary>  
  public class CreateConnection  
  {  
    // Private data.  
    private ConnectionManager ConMgr;  
  
    // Class definition for OLE DB Provider.  
    public void CreateOLEDBConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("OLEDB");  
      ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" +  
        "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" +  
        "Data Source=(local);";  
      ConMgr.Name = "SSIS Connection Manager for OLE DB";  
      ConMgr.Description = "OLE DB connection to the AdventureWorks database.";  
    }  
    public void CreateFileConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("File");  
      ConMgr.ConnectionString = @"\\<yourserver>\<yourfolder>\books.xml";  
      ConMgr.Name = "SSIS Connection Manager for Files";  
      ConMgr.Description = "Flat File connection";  
    }  
  }  
  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    ' Create a package, and retrieve its connections.  
    Dim pkg As New Package()  
    Dim pkgConns As Connections = pkg.Connections  
  
    ' Add an OLE DB connection to the package, using the   
    ' method defined in the AddConnection class.  
    Dim myOLEDBConn As New CreateConnection()  
    myOLEDBConn.CreateOLEDBConnection(pkg)  
  
    ' View the new connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for OLE DB").Description)  
  
    ' Add a second connection to the package.  
    Dim myFileConn As New CreateConnection()  
    myFileConn.CreateFileConnection(pkg)  
  
    ' View the second connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for Files").Description)  
  
    Console.WriteLine()  
    Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
' This class contains the definitions for multiple  
' connection managers.  
  
Public Class CreateConnection  
  ' Private data.  
  Private ConMgr As ConnectionManager  
  
  ' Class definition for OLE DB provider.  
  Public Sub CreateOLEDBConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("OLEDB")  
    ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" & _  
      "Data Source=(local);"  
    ConMgr.Name = "SSIS Connection Manager for OLE DB"  
    ConMgr.Description = "OLE DB connection to the AdventureWorks database."  
  End Sub  
  
  Public Sub CreateFileConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("File")  
    ConMgr.ConnectionString = "\\<yourserver>\<yourfolder>\books.xml"  
    ConMgr.Name = "SSIS Connection Manager for Files"  
    ConMgr.Description = "Flat File connection"  
  End Sub  
  
End Class  
```  
  
 **Beispielausgabe:**  
  
 `Connection description: OLE DB connection to the AdventureWorks database.`  
  
 `Connection description: Flat File connection.`  
  
 `Number of connections in package: 2`  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Technischer Artikel, [Verbindungszeichenfolgen](https://go.microsoft.com/fwlink/?LinkId=220743), unter carlprothman.net.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services (SSIS) Connections (Integration Services-Verbindungen (SSIS))](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Erstellen von Verbindungs-Managern](../connection-manager/integration-services-ssis-connections.md)  
  
