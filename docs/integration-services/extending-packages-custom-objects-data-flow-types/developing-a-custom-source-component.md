---
description: Entwickeln einer benutzerdefinierten Quellkomponente
title: Entwickeln einer benutzerdefinierten Quellkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- validation [Integration Services], components
- external data sources [Integration Services]
- data flow components [Integration Services], source components
- output columns [Integration Services]
- custom data flow components [Integration Services], source components
- custom sources [Integration Services]
- source components [Integration Services]
ms.assetid: 4dc0f631-8fd6-4007-b573-ca67f58ca068
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cc3a5fc5e867a7f8afabd8e4373e00926534ae3f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348264"
---
# <a name="developing-a-custom-source-component"></a>Entwickeln einer benutzerdefinierten Quellkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ermöglicht es Entwicklern, in Quellkomponenten zu schreiben, die Verbindungen mit benutzerdefinierten Datenquellen herstellen und anderen Komponenten im Datenflusstask Daten aus diesen Quellen zur Verfügung stellen. Die Möglichkeit, benutzerdefinierte Quellen zu erstellen, ist hilfreich, wenn Sie Verbindungen zu Datenquellen herstellen müssen, auf die Sie über keine der bestehenden [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Quellen zugreifen können.  
  
 Quellkomponenten verfügen über eine oder mehrere Ausgaben und keine Eingabe. Zur Entwurfszeit dienen Quellkomponenten zur Erstellung und Konfiguration von Verbindungen, zum Lesen von Spaltenmetadaten aus der externen Datenquelle und zur Konfiguration der Ausgabespalten der Quelle basierend auf der externen Datenquelle. Während der Ausführung stellen sie eine Verbindung mit der externen Datenquelle her und fügen einem Ausgabepuffer Zeilen hinzu. Der Datenflusstask stellt dann Downstreamkomponenten diesen Puffer mit Datenzeilen bereit.  
  
 Einen allgemeinen Überblick über die Entwicklung von Datenflusskomponenten finden Sie unter [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="design-time"></a>Entwurfszeit  
 Zur Implementierung der Entwurfszeitfunktionen einer Quellkomponente müssen Sie eine Verbindung mit einer externen Datenquelle festlegen, der Datenquelle entsprechende Ausgabespalten hinzufügen und konfigurieren sowie überprüfen, ob die Komponente zur Ausführung bereit ist. Definitionsgemäß verfügt eine Quellkomponente über keine Eingabe und eine oder mehrere asynchrone Ausgaben.  
  
### <a name="creating-the-component"></a>Erstellen der Komponente  
 Quellkomponenten stellen mithilfe in einem Paket definierter <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekte eine Verbindung mit externen Datenquellen her. Um auf die Notwendigkeit eines Verbindungs-Managers zu verweisen, wird der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>-Auflistung der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>-Eigenschaft ein Element hinzugefügt. Diese Sammlung erfüllt zweierlei Zwecke: Sie enthält Verweise auf Verbindungs-Manager im Paket, die von der Komponente genutzt werden, und zeigt dem Designer den Bedarf für einen Verbindungs-Manager an. Beim Hinzufügen eines <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100>-Typs zur Auflistung wird im **Erweiterten Editor** die Registerkarte **Verbindungseigenschaften** angezeigt, um den Benutzer aufzufordern, im Paket eine Verbindung auszuwählen oder zu erstellen.  
  
 Im folgenden Codebeispiel wird eine Implementierung von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> gezeigt, die eine Ausgabe sowie ein <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100>-Objekt zur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> hinzufügt.  
  
```csharp  
using System;  
using System.Collections;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.OleDb;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "MySourceComponent",ComponentType = ComponentType.SourceAdapter)]  
    public class MyComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Reset the component.  
            base.RemoveAllInputsOutputsAndCustomProperties();  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll();  
  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
  
            IDTSRuntimeConnection100 connection = ComponentMetaData.RuntimeConnectionCollection.New();  
            connection.Name = "ADO.NET";  
        }  
```  
  
```vb  
Imports System.Data  
Imports System.Data.SqlClient  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
<DtsPipelineComponent(DisplayName:="MySourceComponent", ComponentType:=ComponentType.SourceAdapter)> _  
Public Class MySourceComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Allow for resetting the component.  
        RemoveAllInputsOutputsAndCustomProperties()  
        ComponentMetaData.RuntimeConnectionCollection.RemoveAll()  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
  
        Dim connection As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New()  
        connection.Name = "ADO.NET"  
  
    End Sub  
End Class  
```  
  
### <a name="connecting-to-an-external-data-source"></a>Herstellen einer Verbindung mit einer externen Datenquelle  
 Nachdem eine Verbindung zur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> hinzugefügt wurde, überschreiben Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methode, um eine Verbindung mit der externen Datenquelle herzustellen. Diese Methode wird sowohl zur Entwurfs- als auch zur Ausführungszeit aufgerufen. Die Komponente sollte eine Verbindung mit dem von der Laufzeitverbindung festgelegten Verbindungs-Manager und anschließend mit der externen Datenquelle herstellen.  
  
 Nachdem die Verbindung hergestellt ist, sollte sie von der Komponente intern zwischengespeichert und freigegeben werden, wenn die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>-Methode aufgerufen wird. Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>-Methode wird zur Entwurfs- und Ausführungszeit ebenso aufgerufen wie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methode. Entwickler überschreiben diese Methode und geben die Verbindung, die von der Komponente während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> hergestellt wurde, frei.  
  
 Im folgenden Codebeispiel wird eine Komponente gezeigt, die in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methode eine Verbindung mit einer ADO.NET-Verbindung herstellt und die Verbindung in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>-Methode beendet.  
  
```csharp  
private SqlConnection sqlConnection;  
  
public override void AcquireConnections(object transaction)  
{  
    if (ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager != null)  
    {  
        ConnectionManager cm = Microsoft.SqlServer.Dts.Runtime.DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager);  
        ConnectionManagerAdoNet cmado = cm.InnerObject as ConnectionManagerAdoNet;  
  
        if (cmado == null)  
            throw new Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.");  
  
        sqlConnection = cmado.AcquireConnection(transaction) as SqlConnection;  
        sqlConnection.Open();  
    }  
}  
  
public override void ReleaseConnections()  
{  
    if (sqlConnection != null && sqlConnection.State != ConnectionState.Closed)  
        sqlConnection.Close();  
}  
```  
  
```vb  
Private sqlConnection As SqlConnection  
  
Public Overrides Sub AcquireConnections(ByVal transaction As Object)  
  
    If Not IsNothing(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager) Then  
  
        Dim cm As ConnectionManager = Microsoft.SqlServer.Dts.Runtime.DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager)  
        Dim cmado As ConnectionManagerAdoNet = CType(cm.InnerObject, ConnectionManagerAdoNet)  
  
        If IsNothing(cmado) Then  
            Throw New Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.")  
        End If  
  
        sqlConnection = CType(cmado.AcquireConnection(transaction), SqlConnection)  
        sqlConnection.Open()  
  
    End If  
End Sub  
  
Public Overrides Sub ReleaseConnections()  
  
    If Not IsNothing(sqlConnection) And sqlConnection.State <> ConnectionState.Closed Then  
        sqlConnection.Close()  
    End If  
  
End Sub  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Erstellen und Konfigurieren von Ausgabespalten  
 Die Ausgabespalten einer Quellkomponente spiegeln die Spalten der externen Datenquelle, welche die Komponente bei der Ausführung dem Datenfluss hinzufügt, wider. Zur Entwurfszeit fügen Sie, nachdem eine Verbindung der Komponente mit einer externen Datenquelle konfiguriert wurde, Ausgabespalten hinzu. Die Methode, die eine Komponente zur Entwurfszeit zum Hinzufügen der Spalten zu ihrer output-Auflistung verwendet, kann abhängig von den Anforderungen der Komponente variieren. Sie sollten jedoch nicht während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>- oder <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methoden hinzugefügt werden. Eine Komponente, die beispielsweise eine SQL-Anweisung in einer benutzerdefinierten Eigenschaft zur Kontrolle des Datasets der Komponente enthält, könnte ihre Ausgabespalten während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetComponentProperty%2A>-Methode hinzufügen. Die Komponente prüft, ob sie über eine zwischengespeicherte Verbindung verfügt. Falls dies der Fall ist, stellt sie eine Verbindung mit der Datenquelle her und erstellt die Ausgabespalten.  
  
 Nachdem eine Ausgabespalte erstellt wurde, legen Sie ihre Datentypeigenschaften fest, indem Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>-Methode aufrufen. Diese Methode ist erforderlich, da die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>-, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>-, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A>- und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A>-Eigenschaften schreibgeschützt sind und jede Eigenschaft von den Einstellungen der anderen abhängt. Diese Methode erzwingt eine konsistente Festlegung der Werte, die anschließend durch den Datenflusstask geprüft wird.  
  
 Der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> der Spalte bestimmt die Werte, die für die anderen Eigenschaften festgelegt werden. Die folgende Tabelle zeigt die Anforderungen an die abhängigen Eigenschaften für jeden <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. Bei den nicht aufgelisteten Datentypen sind die abhängigen Eigenschaften auf 0 (null) festgelegt.  
  
|DataType|Länge|Skalieren|Genauigkeit|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Größer 0 und kleiner oder gleich 28|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Größer 0 und kleiner oder gleich 28 sowie kleiner als Genauigkeit|Größer oder gleich 1 und kleiner oder gleich 38|0|  
|DT_BYTES|Größer 0|0|0|0|  
|DT_STR|Größer als 0 und kleiner als 8000|0|0|Nicht 0 und eine gültige Codepage|  
|DT_WSTR|Größer 0 und kleiner 4000|0|0|0|  
  
 Da die Beschränkungen der Datentypeigenschaften vom Datentyp der Ausgabespalte abhängen, müssen Sie bei der Arbeit mit verwalteten Typen den richtigen [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Datentyp wählen. Die Basisklasse stellt drei Hilfsmethoden, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>, die Entwickler verwalteter Komponenten bei der Auswahl eines [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Datentyps für einen verwalteten Typ unterstützen. Diese Methoden wandeln verwaltete Datentypen in [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Datentypen um und umgekehrt.  
  
 Im folgenden Codebeispiel wird gezeigt, wie die Ausgabenspaltenauflistung einer Komponente basierend auf einem Tabellenschema aufgefüllt wird. Die Hilfsmethoden der Basisklasse dienen dazu, den Datentyp der Spalte festzulegen, und die abhängigen Eigenschaften werden basierend auf dem Datentyp festgelegt.  
  
```csharp  
SqlCommand sqlCommand;  
  
private void CreateColumnsFromDataTable()  
{  
    // Get the output.  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    // Start clean, and remove the columns from both collections.  
    output.OutputColumnCollection.RemoveAll();  
    output.ExternalMetadataColumnCollection.RemoveAll();  
  
    this.sqlCommand = sqlConnection.CreateCommand();  
    this.sqlCommand.CommandType = CommandType.Text;  
    this.sqlCommand.CommandText = (string)ComponentMetaData.CustomPropertyCollection["SqlStatement"].Value;  
    SqlDataReader schemaReader = this.sqlCommand.ExecuteReader(CommandBehavior.SchemaOnly);  
    DataTable dataTable = schemaReader.GetSchemaTable();  
  
    // Walk the columns in the schema,   
    // and for each data column create an output column and an external metadata column.  
    foreach (DataRow row in dataTable.Rows)  
    {  
        IDTSOutputColumn100 outColumn = output.OutputColumnCollection.New();  
        IDTSExternalMetadataColumn100 exColumn = output.ExternalMetadataColumnCollection.New();  
  
        // Set column data type properties.  
        bool isLong = false;  
        DataType dt = DataRecordTypeToBufferType((Type)row["DataType"]);  
        dt = ConvertBufferDataTypeToFitManaged(dt, ref isLong);  
        int length = 0;  
        int precision = (short)row["NumericPrecision"];  
        int scale = (short)row["NumericScale"];  
        int codepage = dataTable.Locale.TextInfo.ANSICodePage;  
  
        switch (dt)  
        {  
            // The length cannot be zero, and the code page property must contain a valid code page.  
            case DataType.DT_STR:  
            case DataType.DT_TEXT:  
                length = precision;  
                precision = 0;  
                scale = 0;  
                break;  
  
            case DataType.DT_WSTR:  
                length = precision;  
                codepage = 0;  
                scale = 0;  
                precision = 0;  
                break;  
  
            case DataType.DT_BYTES:  
                precision = 0;  
                scale = 0;  
                codepage = 0;  
                break;  
  
            case DataType.DT_NUMERIC:  
                length = 0;  
                codepage = 0;  
  
                if (precision > 38)  
                    precision = 38;  
  
                if (scale > 6)  
                    scale = 6;  
                break;  
  
            case DataType.DT_DECIMAL:  
                length = 0;  
                precision = 0;  
                codepage = 0;  
                break;  
  
            default:  
                length = 0;  
                precision = 0;  
                codepage = 0;  
                scale = 0;  
                break;  
  
        }  
  
        // Set the properties of the output column.  
        outColumn.Name = (string)row["ColumnName"];  
        outColumn.SetDataTypeProperties(dt, length, precision, scale, codepage);  
    }  
}  
```  
  
```vb  
Private sqlCommand As SqlCommand  
  
Private Sub CreateColumnsFromDataTable()  
  
    ' Get the output.  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ' Start clean, and remove the columns from both collections.  
    output.OutputColumnCollection.RemoveAll()  
    output.ExternalMetadataColumnCollection.RemoveAll()  
  
    Me.sqlCommand = sqlConnection.CreateCommand()  
    Me.sqlCommand.CommandType = CommandType.Text  
    Me.sqlCommand.CommandText = CStr(ComponentMetaData.CustomPropertyCollection("SqlStatement").Value)  
  
    Dim schemaReader As SqlDataReader = Me.sqlCommand.ExecuteReader(CommandBehavior.SchemaOnly)  
    Dim dataTable As DataTable = schemaReader.GetSchemaTable()  
  
    ' Walk the columns in the schema,   
    ' and for each data column create an output column and an external metadata column.  
    For Each row As DataRow In dataTable.Rows  
  
        Dim outColumn As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        Dim exColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New()  
  
        ' Set column data type properties.  
        Dim isLong As Boolean = False  
        Dim dt As DataType = DataRecordTypeToBufferType(CType(row("DataType"), Type))  
        dt = ConvertBufferDataTypeToFitManaged(dt, isLong)  
        Dim length As Integer = 0  
        Dim precision As Integer = CType(row("NumericPrecision"), Short)  
        Dim scale As Integer = CType(row("NumericScale"), Short)  
        Dim codepage As Integer = dataTable.Locale.TextInfo.ANSICodePage  
  
        Select Case dt  
  
            ' The length cannot be zero, and the code page property must contain a valid code page.  
            Case DataType.DT_STR  
            Case DataType.DT_TEXT  
                length = precision  
                precision = 0  
                scale = 0  
  
            Case DataType.DT_WSTR  
                length = precision  
                codepage = 0  
                scale = 0  
                precision = 0  
  
            Case DataType.DT_BYTES  
                precision = 0  
                scale = 0  
                codepage = 0  
  
            Case DataType.DT_NUMERIC  
                length = 0  
                codepage = 0  
  
                If precision > 38 Then  
                    precision = 38  
                End If  
  
                If scale > 6 Then  
                    scale = 6  
                End If  
  
            Case DataType.DT_DECIMAL  
                length = 0  
                precision = 0  
                codepage = 0  
  
            Case Else  
                length = 0  
                precision = 0  
                codepage = 0  
                scale = 0  
        End Select  
  
        ' Set the properties of the output column.  
        outColumn.Name = CStr(row("ColumnName"))  
        outColumn.SetDataTypeProperties(dt, length, precision, scale, codepage)  
    Next  
End Sub  
```  
  
### <a name="validating-the-component"></a>Überprüfen der Komponente  
 Sie sollten eine Quellkomponente überprüfen und sicherstellen, dass die in den Ausgabespaltenauflistungen definierten Spalten denjenigen bei der externen Datenquelle entsprechen. Manchmal ist der Abgleich der Ausgabespalten mit der externen Datenquelle nicht möglich, beispielsweise da keine Verbindung besteht oder zeitaufwendige Roundtrips zum Server vermieden werden sollen. In solchen Fällen können die Spalten in der Ausgabe dennoch mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.ExternalMetadataColumnCollection%2A> des Ausgabeobjekts überprüft werden. Weitere Informationen zu finden Sie unter [Überprüfen einer Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md).  
  
 Diese Auflistung ist sowohl für Eingabe- als auch für Ausgabeobjekte vorhanden. Sie können sie mit den Spalten der externen Datenquelle füllen. Mithilfe dieser Auflistung können Sie die Ausgabespalten überprüfen, wenn der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer offline ist, die Verbindung mit der Komponente getrennt ist oder wenn die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A>-Eigenschaft **false** lautet. Die Auflistung sollte zunächst zum Zeitpunkt der Erstellung der Ausgabespalten aufgefüllt werden. Das Hinzufügen externer Metadatenspalten zur Auflistung ist recht einfach, da die externe Metadatenspalte zunächst der Ausgabespalte entsprechen sollte. Die Datentypeigenschaften der Spalte sollten bereits korrekt festgelegt worden sein, und die Eigenschaften können direkt in das <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100>-Objekt kopiert werden.  
  
 Im folgenden Beispielcode wird eine externe Metadatenspalte auf Grundlage einer neu erstellten Ausgabespalte hinzugefügt. Dabei wird vorausgesetzt, dass die Ausgabespalte bereits erstellt wurde.  
  
```csharp  
private void CreateExternalMetaDataColumn(IDTSOutput100 output, IDTSOutputColumn100 outputColumn)  
{  
  
    // Set the properties of the external metadata column.  
    IDTSExternalMetadataColumn100 externalColumn = output.ExternalMetadataColumnCollection.New();  
    externalColumn.Name = outputColumn.Name;  
    externalColumn.Precision = outputColumn.Precision;  
    externalColumn.Length = outputColumn.Length;  
    externalColumn.DataType = outputColumn.DataType;  
    externalColumn.Scale = outputColumn.Scale;  
  
    // Map the external column to the output column.  
    outputColumn.ExternalMetadataColumnID = externalColumn.ID;  
  
}  
```  
  
```vb  
Private Sub CreateExternalMetaDataColumn(ByVal output As IDTSOutput100, ByVal outputColumn As IDTSOutputColumn100)  
  
        ' Set the properties of the external metadata column.  
        Dim externalColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New()  
        externalColumn.Name = outputColumn.Name  
        externalColumn.Precision = outputColumn.Precision  
        externalColumn.Length = outputColumn.Length  
        externalColumn.DataType = outputColumn.DataType  
        externalColumn.Scale = outputColumn.Scale  
  
        ' Map the external column to the output column.  
        outputColumn.ExternalMetadataColumnID = externalColumn.ID  
  
    End Sub  
```  
  
## <a name="run-time"></a>Runtime  
 Bei der Ausführung fügen die Komponenten Zeilen zu Ausgabepuffern hinzu, die durch den Datenflusstask erstellt und der Komponente in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Methode bereitgestellt werden. Einmal für Quellkomponenten aufgerufen, erhält die Methode einen Ausgabepuffer für jede <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> der Komponente, die mit einer Downstreamkomponente verbunden ist.  
  
### <a name="locating-columns-in-the-buffer"></a>Suchen von Spalten im Puffer  
 Der Ausgabepuffer für eine Komponente enthält die von der Komponente definierten Spalten sowie alle zur Ausgabe einer Downstreamkomponente hinzugefügten Spalten. Falls beispielsweise eine Quellkomponente drei Spalten in ihrer Ausgabe bereitstellt und die nächste Komponente eine vierte Ausgabespalte hinzufügt, enthält der Ausgabepuffer, der für die Quellkomponente bereitgestellt wird, diese vier Spalten.  
  
 Die Reihenfolge der Spalten in einer Pufferzeile wird nicht vom Index der Ausgabespalte in der Ausgabespaltenauflistung definiert. Die genaue Position einer Ausgabespalte in einer Pufferzeile kann nur mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSBufferManagerClass.FindColumnByLineageID%2A>-Methode des <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> ermittelt werden. Diese Methode sucht die Spalte mit der angegebenen Herkunfts-ID im jeweiligen Puffer und gibt ihre Position in der Zeile zurück. Die Indizes der Ausgabespalten befinden sich typischerweise in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>-Methode und werden für die Verwendung während des <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Schritts gespeichert.  
  
 Im folgenden Codebeispiel wird die Position der Ausgabespalten im Ausgabepuffer bei einem Aufruf von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> gesucht und in einer internen Struktur gespeichert. Auch der Name der Spalte wird in der Struktur gespeichert und im Codebeispiel für die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Methode im nächsten Abschnitt dieses Themas verwendet.  
  
```csharp  
ArrayList columnInformation;  
  
private struct ColumnInfo  
{  
    public int BufferColumnIndex;  
    public string ColumnName;  
}  
  
public override void PreExecute()  
{  
    this.columnInformation = new ArrayList();  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    foreach (IDTSOutputColumn100 col in output.OutputColumnCollection)  
    {  
        ColumnInfo ci = new ColumnInfo();  
        ci.BufferColumnIndex = BufferManager.FindColumnByLineageID(output.Buffer, col.LineageID);  
        ci.ColumnName = col.Name;  
        columnInformation.Add(ci);  
    }  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Me.columnInformation = New ArrayList()  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    For Each col As IDTSOutputColumn100 In output.OutputColumnCollection  
  
        Dim ci As ColumnInfo = New ColumnInfo()  
        ci.BufferColumnIndex = BufferManager.FindColumnByLineageID(output.Buffer, col.LineageID)  
        ci.ColumnName = col.Name  
        columnInformation.Add(ci)  
    Next  
End Sub  
```  
  
### <a name="processing-rows"></a>Verarbeiten von Zeilen  
 Durch Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A>-Methode, die eine neue Pufferzeile mit leeren Werten in ihren Spalten erzeugt, können dem Ausgabepuffer Zeilen hinzugefügt werden. Die Komponente weist anschließend den einzelnen Spalten Werte zu. Der Datenflusstask erstellt und überwacht die Ausgabepuffer, die einer Komponente bereitgestellt werden. Sobald sie gefüllt sind, werden die Zeilen im Puffer in die nächste Komponente verschoben. Es gibt keine Möglichkeit zu ermitteln, wann ein Zeilenbatch an die nächste Komponente gesendet wird, da die Zeilenbewegungen durch den Datenflusstask für den Komponentenentwickler transparent verlaufen und die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.RowCount%2A>-Eigenschaft in Ausgabepuffern immer den Wert Null aufweist. Sobald eine Quellkomponente das Hinzufügen von Zeilen zum Ausgabepuffer beendet hat, benachrichtigt sie den Datenflusstask durch Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A>-Methode des <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>, und die verbleibenden Zeilen im Puffer werden an die nächste Komponente weitergereicht.  
  
 Während die Quellkomponente Zeilen der externen Datenquelle liest, sollten Sie die Leistungsindikatoren Gelesene Zeilen oder Gelesene BLOB-Bytes durch Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.IncrementPipelinePerfCounter%2A>-Methode aktualisieren. Weitere Informationen finden Sie unter [Performance Counters](../../integration-services/performance/performance-counters.md).  
  
 Im folgenden Codebeispiel wird eine Komponente dargestellt, die im <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Schritt Zeilen zu einem Ausgabepuffer hinzufügt. Die Indizes der Ausgabespalten im Puffer wurden im vorangehenden Codebeispiel mithilfe von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> ermittelt.  
  
```csharp  
public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
{  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
    PipelineBuffer buffer = buffers[0];  
  
    SqlDataReader dataReader = sqlCommand.ExecuteReader();  
  
    // Loop over the rows in the DataReader,   
    // and add them to the output buffer.  
    while (dataReader.Read())  
    {  
        // Add a row to the output buffer.  
        buffer.AddRow();  
  
        for (int x = 0; x < columnInformation.Count; x++)  
        {  
            ColumnInfo ci = (ColumnInfo)columnInformation[x];  
            int ordinal = dataReader.GetOrdinal(ci.ColumnName);  
  
            if (dataReader.IsDBNull(ordinal))  
                buffer.SetNull(ci.BufferColumnIndex);  
            else  
            {  
                buffer[ci.BufferColumnIndex] = dataReader[ci.ColumnName];  
            }  
        }  
    }  
    buffer.SetEndOfRowset();  
}  
```  
  
```vb  
Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim buffer As PipelineBuffer = buffers(0)  
  
    Dim dataReader As SqlDataReader = sqlCommand.ExecuteReader()  
  
    ' Loop over the rows in the DataReader,   
    ' and add them to the output buffer.  
    While (dataReader.Read())  
  
        ' Add a row to the output buffer.  
        buffer.AddRow()  
  
        For x As Integer = 0 To columnInformation.Count  
  
            Dim ci As ColumnInfo = CType(columnInformation(x), ColumnInfo)  
  
            Dim ordinal As Integer = dataReader.GetOrdinal(ci.ColumnName)  
  
            If (dataReader.IsDBNull(ordinal)) Then  
                buffer.SetNull(ci.BufferColumnIndex)  
            Else  
                buffer(ci.BufferColumnIndex) = dataReader(ci.ColumnName)  
  
            End If  
        Next  
  
    End While  
  
    buffer.SetEndOfRowset()  
End Sub  
```  
  
## <a name="sample"></a>Beispiel  
 Das folgende Beispiel zeigt eine einfache Quellkomponente, die über einen Dateiverbindungs-Manager den binären Inhalt von Dateien in den Datenfluss lädt. In diesem Beispiel werden nicht alle Methoden und Funktionen dargestellt, die in diesem Thema erläutert wurden. Es veranschaulicht die Hauptmethoden, die jede benutzerdefinierte Quellkomponente überschreiben muss, enthält jedoch keinen Code für eine Überprüfung zur Entwurfszeit.  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace BlobSrc  
{  
  [DtsPipelineComponent(DisplayName = "BLOB Inserter Source", Description = "Inserts files into the data flow as BLOBs")]  
  public class BlobSrc : PipelineComponent  
  {  
    IDTSConnectionManager100 m_ConnMgr;  
    int m_FileNameColumnIndex = -1;  
    int m_FileBlobColumnIndex = -1;  
  
    public override void ProvideComponentProperties()  
    {  
      IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
      output.Name = "BLOB File Inserter Output";  
  
      IDTSOutputColumn100 column = output.OutputColumnCollection.New();  
      column.Name = "FileName";  
      column.SetDataTypeProperties(DataType.DT_WSTR, 256, 0, 0, 0);  
  
      column = output.OutputColumnCollection.New();  
      column.Name = "FileBLOB";  
      column.SetDataTypeProperties(DataType.DT_IMAGE, 0, 0, 0, 0);  
  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection.New();  
      conn.Name = "FileConnection";  
    }  
  
    public override void AcquireConnections(object transaction)  
    {  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection[0];  
      m_ConnMgr = conn.ConnectionManager;  
    }  
  
    public override void ReleaseConnections()  
    {  
      m_ConnMgr = null;  
    }  
  
    public override void PreExecute()  
    {  
      IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
      m_FileNameColumnIndex = (int)BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[0].LineageID);  
      m_FileBlobColumnIndex = (int)BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[1].LineageID);  
    }  
  
    public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
    {  
      string strFileName = (string)m_ConnMgr.AcquireConnection(null);  
  
      while (strFileName != null)  
      {  
        buffers[0].AddRow();  
  
        buffers[0].SetString(m_FileNameColumnIndex, strFileName);  
  
        FileInfo fileInfo = new FileInfo(strFileName);  
        byte[] fileData = new byte[fileInfo.Length];  
        FileStream fs = new FileStream(strFileName, FileMode.Open, FileAccess.Read, FileShare.Read);  
        fs.Read(fileData, 0, fileData.Length);  
  
        buffers[0].AddBlobData(m_FileBlobColumnIndex, fileData);  
  
        strFileName = (string)m_ConnMgr.AcquireConnection(null);  
      }  
  
      buffers[0].SetEndOfRowset();  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.IO   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace BlobSrc   
  
 <DtsPipelineComponent(DisplayName="BLOB Inserter Source", Description="Inserts files into the data flow as BLOBs")> _   
 Public Class BlobSrc   
 Inherits PipelineComponent   
   Private m_ConnMgr As IDTSConnectionManager100   
   Private m_FileNameColumnIndex As Integer = -1   
   Private m_FileBlobColumnIndex As Integer = -1   
  
   Public  Overrides Sub ProvideComponentProperties()   
     Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
     output.Name = "BLOB File Inserter Output"   
     Dim column As IDTSOutputColumn100 = output.OutputColumnCollection.New   
     column.Name = "FileName"   
     column.SetDataTypeProperties(DataType.DT_WSTR, 256, 0, 0, 0)   
     column = output.OutputColumnCollection.New   
     column.Name = "FileBLOB"   
     column.SetDataTypeProperties(DataType.DT_IMAGE, 0, 0, 0, 0)   
     Dim conn As IDTSRuntimeConnection90 = ComponentMetaData.RuntimeConnectionCollection.New   
     conn.Name = "FileConnection"   
   End Sub   
  
   Public  Overrides Sub AcquireConnections(ByVal transaction As Object)   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection(0)   
     m_ConnMgr = conn.ConnectionManager   
   End Sub   
  
   Public  Overrides Sub ReleaseConnections()   
     m_ConnMgr = Nothing   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)   
     m_FileNameColumnIndex = CType(BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(0).LineageID), Integer)   
     m_FileBlobColumnIndex = CType(BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(1).LineageID), Integer)   
   End Sub   
  
   Public  Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())   
     Dim strFileName As String = CType(m_ConnMgr.AcquireConnection(Nothing), String)   
     While Not (strFileName Is Nothing)   
       buffers(0).AddRow   
       buffers(0).SetString(m_FileNameColumnIndex, strFileName)   
       Dim fileInfo As FileInfo = New FileInfo(strFileName)   
       Dim fileData(fileInfo.Length) As Byte   
       Dim fs As FileStream = New FileStream(strFileName, FileMode.Open, FileAccess.Read, FileShare.Read)   
       fs.Read(fileData, 0, fileData.Length)   
       buffers(0).AddBlobData(m_FileBlobColumnIndex, fileData)   
       strFileName = CType(m_ConnMgr.AcquireConnection(Nothing), String)   
     End While   
     buffers(0).SetEndOfRowset   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln einer benutzerdefinierten Zielkomponente](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)   
 [Erstellen einer Quelle mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md)  
  
  
