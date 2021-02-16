---
description: Programmgesteuertes Hinzufügen von Datenflusskomponenten
title: Programmgesteuertes Hinzufügen von Datenflusskomponenten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow task [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: c06065cf-43e5-4b6b-9824-7309d7f5e35e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd3a122993913d524547d6c9e57945956f1099de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351000"
---
# <a name="adding-data-flow-components-programmatically"></a>Programmgesteuertes Hinzufügen von Datenflusskomponenten

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn Sie einen Datenfluss erstellen, beginnen Sie, indem Sie Komponenten hinzufügen. Anschließend werden diese Komponenten konfiguriert und miteinander verbunden, um den Datenfluss zur Laufzeit einzurichten. In diesem Abschnitt wird das Hinzufügen einer Komponente zum Datenflusstask, das Erstellen der Entwurfszeitinstanz der Komponente und das anschließende Konfigurieren der Komponente beschrieben. Informationen zum Verbinden von Komponenten finden Sie unter [Programmgesteuertes Verbinden von Datenflusskomponenten](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
## <a name="adding-a-component"></a>Hinzufügen einer Komponente  
 Rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaDataCollection100.New%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.ComponentMetaDataCollection%2A>-Auflistung auf, um eine neue Komponente zu erstellen und diese dem Datenflusstask hinzuzufügen. Diese Methode gibt die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle der Komponente zurück. An diesem Punkt enthält die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle jedoch keine spezifischen Informationen zu einer der Komponenten. Legen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>-Eigenschaft so fest, dass der Typ der Komponente identifiziert wird. Der Datenflusstask verwendet den Wert dieser Eigenschaft zum Erstellen einer Instanz der Komponente zur Laufzeit.  
  
 Der in der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>-Eigenschaft angegebene Wert kann die CLSID, PROGID oder <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A>-Eigenschaft der Komponente sein. Die CLSID wird in der Regel im Eigenschaftenfenster als Wert der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>-Eigenschaft der Komponente angezeigt. Informationen zum Erhalten dieser Eigenschaft und anderer Eigenschaften verfügbarer Komponenten finden Sie unter [Discovering Data Flow Components Programmatically](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md) (Programmgesteuertes Auffinden von Datenflusskomponenten).  
  
## <a name="adding-a-managed-component"></a>Hinzufügen einer verwalteten Komponente  
 Sie können die CLSID oder PROGID nicht verwenden, um dem Datenfluss eine der verwalteten Datenflusskomponenten hinzuzufügen, da diese Werte auf einen Wrapper und nicht auf die Komponente selbst zeigen. Stattdessen können Sie die **CreationName**-Eigenschaft oder die **AssemblyQualifiedName**-Eigenschaft verwenden, wie im folgenden Beispiel gezeigt.  
  
 Wenn Sie die **AssemblyQualifiedName**-Eigenschaft verwenden möchten, müssen Sie in Ihrem [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Projekt einen Verweis auf die Assembly hinzufügen, die die verwaltete Komponente enthält. Diese Assemblys sind nicht auf der Registerkarte für .NET des Dialogfelds **Verweis hinzufügen** aufgeführt. Normalerweise finden Sie die Assembly im Ordner **C:\Programme\Microsoft SQL Server\100\DTS\PipelineComponents**.  
  
 Die integrierten verwalteten Datenflusskomponenten umfassen:  
  
-   [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Quelle  
  
-   XML-Quelle  
  
-   DataReader-Ziel  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Ziel  
  
-   Skriptkomponente  
  
 Im folgenden Codebeispiel werden beide Methoden zum Hinzufügen einer verwalteten Komponente zu einem Datenfluss veranschaulicht:  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Microsoft.SqlServer.Dts.Runtime.Package package = new Microsoft.SqlServer.Dts.Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Microsoft.SqlServer.Dts.Runtime.TaskHost thMainPipe = (Microsoft.SqlServer.Dts.Runtime.TaskHost)e;  
      MainPipe dataFlowTask = (MainPipe)thMainPipe.InnerObject;  
  
      // The Application object will be used to obtain the CreationName  
      //  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
      Application app = new Application();  
  
      // Add a first ADO NET source to the data flow.  
      //  The CreationName property requires an Application instance.  
      IDTSComponentMetaData100 component1 = dataFlowTask.ComponentMetaDataCollection.New();  
      component1.Name = "DataReader Source";  
      component1.ComponentClassID = app.PipelineComponentInfos["DataReader Source"].CreationName;  
  
      // Add a second ADO NET source to the data flow.  
      //  The AssemblyQualifiedName property requires a reference to the assembly.  
      IDTSComponentMetaData100 component2 = dataFlowTask.ComponentMetaDataCollection.New();  
      component2.Name = "DataReader Source";  
      component2.ComponentClassID = typeof(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName;  
    }  
  }  
}  
  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' The Application object will be used to obtain the CreationName  
    '  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
    Dim app As New Application()  
  
    ' Add a first ADO NET source to the data flow.  
    '  The CreationName property requires an Application instance.  
    Dim component1 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component1.Name = "DataReader Source"  
    component1.ComponentClassID = app.PipelineComponentInfos("DataReader Source").CreationName  
  
    ' Add a second ADO NET source to the data flow.  
    '  The AssemblyQualifiedName property requires a reference to the assembly.  
    Dim component2 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component2.Name = "DataReader Source"  
    component2.ComponentClassID = _  
      GetType(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName  
  
  End Sub  
  
End Module  
```  
  
## <a name="creating-the-design-time-instance-of-the-component"></a>Erstellen der Entwurfszeitinstanz der Komponente  
 Rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A>-Methode zur Erstellung der Entwurfszeitinstanz der Komponente auf, die durch die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>-Eigenschaft identifiziert wird. Diese Methode gibt das <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper>-Objekt zurück, das der verwaltete Wrapper für die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>-Schnittstelle ist.  
  
 Eine Komponente sollte, wann immer möglich, mithilfe der Methoden der Entwurfszeitinstanz geändert werden und nicht durch direktes Ändern der Metadaten der Komponente. Es gibt zwar Elemente in den Metadaten, die Sie direkt festlegen müssen, z. B. Verbindungen, es empfiehlt sich jedoch im Allgemeinen nicht, die Metadaten direkt zu ändern, da in diesem Fall die Komponente keine Änderungen mehr überwachen und überprüfen kann.  
  
## <a name="assigning-connections"></a>Zuweisen von Verbindungen  
 Manche Komponenten, beispielsweise die OLE DB-Quellkomponente, erfordern eine Verbindung zu externen Daten und verwenden zu diesem Zweck ein vorhandenes <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekt in dem Paket. Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnectionCollection100.Count%2A>-Eigenschaft der .<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>-Auflistung gibt die Anzahl von <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekten zur Laufzeit an, die für die Komponente erforderlich sind. Wenn die Anzahl größer als 0 (null) ist, erfordert die Komponente eine Verbindung. Weisen Sie der Komponente einen Verbindungs-Manager aus dem Paket zu, indem Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.ConnectionManager%2A>-Eigenschaft und die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.Name%2A>-Eigenschaft der ersten Verbindung in der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> angeben. Beachten Sie, dass der Name des Verbindungs-Managers in der Verbindungsauflistung zur Laufzeit dem Namen des Verbindungs-Managers entsprechen muss, auf den vom Paket verwiesen wird.  
  
## <a name="setting-the-values-of-custom-properties"></a>Festlegen der Werte benutzerdefinierter Eigenschaften  
 Rufen Sie nach dem Erstellen der Entwurfszeitinstanz der Komponente die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>-Methode auf. Diese Methode ähnelt einem Konstruktor, da sie eine neu erstellte Komponente initialisiert, indem benutzerdefinierte Eigenschaften und Eingabe- und Ausgabeobjekte erstellt werden. Rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>-Methode nicht mehrmals in einer Komponente auf, da sich die Komponente möglicherweise selbst zurücksetzt und alle zuvor an ihren Metadaten vorgenommenen Änderungen verloren gehen.  
  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.CustomPropertyCollection%2A>-Eigenschaft der Komponente enthält eine Auflistung von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100>-Objekten, die für die Komponente spezifisch sind. Im Gegensatz zu anderen Programmierungsmodellen, bei denen die Eigenschaften eines Objekts in dem Objekt immer sichtbar sind, werden die Auflistungen benutzerdefinierter Eigenschaften von Komponenten nur aufgefüllt, wenn Sie die  <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>-Methode aufrufen. Verwenden Sie nach dem Aufrufen der Methode die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.SetComponentProperty%2A>-Methode der Entwurfszeitinstanz der Komponente, um ihren benutzerdefinierten Eigenschaften Werte zuzuweisen. Diese Methode akzeptiert ein Name/Wert-Paar, das die benutzerdefinierte Eigenschaft identifiziert und seinen neuen Wert bereitstellt.  
  
## <a name="initializing-output-columns"></a>Initialisieren von Ausgabespalten  
 Nachdem Sie dem Task eine Komponente hinzugefügt und diese konfiguriert haben, initialisieren Sie die Auflistung von Spalten in <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> des Objekts. Dieser Schritt ist insbesondere bei Quellkomponenten relevant. Möglicherweise werden dadurch jedoch keine Spalten für die Transformation und Zielkomponenten initialisiert, da diese im Allgemeinen von den Spalten abhängig sind, die sie von Upstreamkomponenten empfangen.  
  
 Rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A>-Methode auf, um die Spalten in den Ausgaben einer Quellkomponente zu initialisieren. Da Komponenten nicht automatisch eine Verbindung zu externen Datenquellen herstellen, rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.AcquireConnections%2A>-Methode auf, bevor Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A>-Methode aufrufen, um der Komponente Zugriff zu ihrer externen Datenquelle zu gewähren und die Möglichkeit zu geben, ihre eigene Metadatenspalte zu füllen. Um die Verbindung freizugeben, ist schließlich ein Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReleaseConnections%2A>-Methode erforderlich.  
  
## <a name="next-step"></a>Nächster Schritt  
 Nach dem Hinzufügen und Konfigurieren der Komponente besteht der nächste Schritt darin, Pfade zwischen Komponenten zu erstellen. Dies wird im Thema [Creating a Path](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md) (Erstellen eines Pfads) erläutert.  
  
## <a name="sample"></a>Beispiel  
 Im folgenden Codebeispiel werden die OLE DB-Quellkomponenten einem Datenflusstask hinzugefügt, eine Entwurfszeitinstanz der Komponente erstellt und die Eigenschaften der Komponente konfiguriert. Für dieses Beispiel ist ein zusätzlicher Verweis auf die Microsoft.SqlServer.SQLTask-Assembly erforderlich.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Runtime.Package package = new Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Runtime.TaskHost thMainPipe = e as Runtime.TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Add an OLEDB connection manager to the package.  
      ConnectionManager cm = package.Connections.Add("OLEDB");  
      cm.Name = "OLEDB ConnectionManager";  
      cm.ConnectionString = "Data Source=(local);" +   
        "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" +   
        "Integrated Security=SSPI;"  
  
      // Add an OLE DB source to the data flow.  
      IDTSComponentMetaData100 component =   
        dataFlowTask.ComponentMetaDataCollection.New();  
      component.Name = "OLEDBSource";  
      component.ComponentClassID = "DTSAdapter.OleDbSource.1";  
      // You can also use the CLSID of the component instead of the PROGID.  
      //component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
      // Get the design time instance of the component.  
      CManagedComponentWrapper instance = component.Instantiate();  
  
      // Initialize the component  
      instance.ProvideComponentProperties();  
  
      // Specify the connection manager.  
      if (component.RuntimeConnectionCollection.Count > 0)  
      {  
        component.RuntimeConnectionCollection[0].ConnectionManager =   
          DtsConvert.GetExtendedInterface(package.Connections[0]);  
        component.RuntimeConnectionCollection[0].ConnectionManagerID =   
          package.Connections[0].ID;      }  
  
      // Set the custom properties.  
      instance.SetComponentProperty("AccessMode", 2);  
      instance.SetComponentProperty("SqlCommand",   
        "Select * from Production.Product");  
  
      // Reinitialize the metadata.  
      instance.AcquireConnections(null);  
      instance.ReinitializeMetaData();  
      instance.ReleaseConnections();  
  
      // Add other components to the data flow and connect them.  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Add an OLEDB connection manager to the package.  
    Dim cm As ConnectionManager = package.Connections.Add("OLEDB")  
    cm.Name = "OLEDB ConnectionManager"  
    cm.ConnectionString = "Data Source=(local);" & _  
      "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;"  
  
    ' Add an OLE DB source to the data flow.  
    Dim component As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component.Name = "OLEDBSource"  
    component.ComponentClassID = "DTSAdapter.OleDbSource.1"  
    ' You can also use the CLSID of the component instead of the PROGID.  
    'component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
    ' Get the design time instance of the component.  
    Dim instance As CManagedComponentWrapper = component.Instantiate()  
  
    ' Initialize the component.  
    instance.ProvideComponentProperties()  
  
    ' Specify the connection manager.  
    If component.RuntimeConnectionCollection.Count > 0 Then  
      component.RuntimeConnectionCollection(0).ConnectionManager = _  
        DtsConvert.GetExtendedInterface(package.Connections(0))  
      component.RuntimeConnectionCollection(0).ConnectionManagerID = _  
        package.Connections(0).ID  
    End If  
  
    ' Set the custom properties.  
    instance.SetComponentProperty("AccessMode", 2)  
    instance.SetComponentProperty("SqlCommand", _  
      "Select * from Production.Product")  
  
    ' Reinitialize the metadata.  
    instance.AcquireConnections(vbNull)  
    instance.ReinitializeMetaData()  
    instance.ReleaseConnections()  
  
    ' Add other components to the data flow and connect them.  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Blogbeitrag, [EzAPI – Updated for SQL Server 2012 (EzAPI: für SQL Server 2012 aktualisiert)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), auf blogs.msdn.com.  

## <a name="see-also"></a>Weitere Informationen  
 [Programmgesteuertes Verbinden von Datenflusskomponenten](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
  
  
