---
description: Entwickeln einer benutzerdefinierten Zielkomponente
title: Entwickeln einer benutzerdefinierten Zielkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
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
- destinations [Integration Services], components
- ProcessInput method
- external data sources [Integration Services]
- custom data flow components [Integration Services], destination components
- data flow components [Integration Services], destination components
ms.assetid: 24619363-9535-4c0e-8b62-1d22c6630e40
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8a86cfc875bf47d04ef31901866cd7500260ea7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336341"
---
# <a name="developing-a-custom-destination-component"></a>Entwickeln einer benutzerdefinierten Zielkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] bietet Entwicklern die Möglichkeit, benutzerdefinierte Zielkomponenten zu schreiben, die eine Verbindung mit einer beliebigen benutzerdefinierten Datenquelle herstellen und Daten in dieser speichern können. Benutzerdefinierte Zielkomponenten sind hilfreich, wenn Sie Verbindungen zu Datenquellen herstellen müssen, auf die nicht über eine der vorhandenen Quellkomponenten, die in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthalten sind, zugegriffen werden kann.  
  
 Zielkomponenten verfügen über eine oder mehrere Eingaben und keine Ausgabe. Zur Entwurfszeit erstellen und konfigurieren sie Verbindungen und lesen Spaltenmetadaten aus der externen Datenquelle. Während der Ausführung stellen sie eine Verbindung zu ihrer externen Datenquelle her und fügen Zeilen, die von den Komponenten upstream im Datenfluss empfangen wurden, zur externen Datenquelle hinzu. Wenn die externe Datenquelle vor der Ausführung der Komponente besteht, dann muss die Zielkomponente außerdem sicherstellen, dass die Datentypen der Spalten, die die Komponente empfängt, zu den Datentypen der Spalten der externen Datenquelle passen.  
  
 In diesem Abschnitt wird im Detail auf die Entwicklung von Zielkomponenten eingegangen, und es werden zur Verdeutlichung wichtiger Konzepte Codebeispiele bereitgestellt. Einen allgemeinen Überblick über die Entwicklung von Datenflusskomponenten finden Sie unter [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="design-time"></a>Entwurfszeit  
 Zur Implementierung der Entwurfszeitfunktionen einer Zielkomponente müssen Sie eine Verbindung mit einer externen Datenquelle festlegen und überprüfen, ob die Komponente richtig konfiguriert wurde. Definitionsgemäß verfügt eine Zielkomponente über einen Eingang und möglicherweise eine Fehlerausgabe.  
  
### <a name="creating-the-component"></a>Erstellen der Komponente  
 Zielkomponenten stellen mithilfe von in einem Paket definierten <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekten eine Verbindung mit externen Datenquellen her. Die Zielkomponente zeigt an, dass ein Verbindungs-Manager für den [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer und andere Benutzer der Komponente durch Hinzufügen eines Elements zur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>-Auflistung von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> erforderlich ist. Diese Auflistung erfüllt zweierlei Zwecke: erstens zeigt sie dem [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer den Bedarf für einen Verbindungs-Manager an und zweitens enthält sie, nachdem der Benutzer einen Verbindungs-Manager ausgewählt oder erstellt hat, einen Verweis zum Verbindungs-Manager im Paket, das von der Komponente verwendet wird. Beim Hinzufügen eines <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100>-Typs zur Auflistung wird im **Erweiterten Editor** die Registerkarte **Verbindungseigenschaften** angezeigt, um den Benutzer aufzufordern, im Paket für die Komponente eine Verbindung auszuwählen oder zu erstellen.  
  
 Im folgenden Codebeispiel wird eine Implementierung von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> gezeigt, die eine Eingabe und dann ein <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100>-Objekt zu <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> hinzufügt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "Destination Component",ComponentType =ComponentType.DestinationAdapter)]  
    public class DestinationComponent : PipelineComponent   
    {  
        public override void ProvideComponentProperties()  
        {  
            // Reset the component.  
            base.RemoveAllInputsOutputsAndCustomProperties();  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll();  
  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            IDTSRuntimeConnection100 connection = ComponentMetaData.RuntimeConnectionCollection.New();  
            connection.Name = "ADO.net";  
        }  
    }  
}  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Data.SqlClient  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="Destination Component", ComponentType:=ComponentType.DestinationAdapter)> _  
    Public Class DestinationComponent  
        Inherits PipelineComponent  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Reset the component.  
            Me.RemoveAllInputsOutputsAndCustomProperties()  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
            input.Name = "Input"  
  
            Dim connection As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New()  
            connection.Name = "ADO.net"  
  
        End Sub  
    End Class  
End Namespace  
```  
  
### <a name="connecting-to-an-external-data-source"></a>Herstellen einer Verbindung mit einer externen Datenquelle  
 Nachdem eine Verbindung zur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> hinzugefügt wurde, überschreiben Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methode, um eine Verbindung mit der externen Datenquelle herzustellen. Diese Methode wird zur Entwurfs- und zur Laufzeit aufgerufen. Die Komponente muss eine Verbindung mit dem von der Laufzeitverbindung festgelegten Verbindungs-Manager und anschließend mit der externen Datenquelle herstellen. Sobald die Verbindung hergestellt ist, sollte sie von der Komponente intern zwischengespeichert und freigegeben werden, wenn <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> aufgerufen wird. Entwickler überschreiben diese Methode und geben die Verbindung, die von der Komponente während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> hergestellt wurde, frei. Die beiden Methoden <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> werden zur Entwurfs- und zur Laufzeit aufgerufen.  
  
 Im folgenden Codebeispiel wird eine Komponente gezeigt, die in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>-Methode eine Verbindung mit einer ADO.NET-Verbindung herstellt und die Verbindung dann in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> beendet.  
  
```csharp  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
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
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Private sqlConnection As SqlConnection  
  
Public Overrides Sub AcquireConnections(ByVal transaction As Object)  
  
    If IsNothing(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager) = False Then  
  
        Dim cm As ConnectionManager = DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager)  
        Dim cmado As ConnectionManagerAdoNet = CType(cm.InnerObject,ConnectionManagerAdoNet)  
  
        If IsNothing(cmado) Then  
            Throw New Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.")  
        End If  
  
        sqlConnection = CType(cmado.AcquireConnection(transaction), SqlConnection)  
        sqlConnection.Open()  
  
    End If  
End Sub  
  
Public Overrides Sub ReleaseConnections()  
  
    If IsNothing(sqlConnection) = False And sqlConnection.State <> ConnectionState.Closed Then  
        sqlConnection.Close()  
    End If  
  
End Sub  
```  
  
### <a name="validating-the-component"></a>Überprüfen der Komponente  
 Zielkomponentenentwickler sollten Überprüfungen wie in [Überprüfen einer Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md) beschrieben ausführen. Darüber hinaus sollten sie überprüfen, ob die Datentypeigenschaften der in den Eingabespaltenauflistungen der Komponente definierten Spalten denjenigen bei der externen Datenquelle entsprechen. Manchmal ist der Abgleich der Eingabespalten mit der externen Datenquelle nicht möglich oder unerwünscht, z. B. wenn die Komponente oder der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer sich im Offline-Zustand befinden oder Roundtrips zum Server nicht zulässig sind. In solchen Fällen können die Spalten in der Eingabespaltenauflistung dennoch mithilfe von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100.ExternalMetadataColumnCollection%2A> des Eingabeobjekts überprüft werden.  
  
 Diese Auflistung ist sowohl für Eingabe- als auch für Ausgabeobjekte vorhanden und muss vom Komponentenentwickler mit den Spalten der externen Datenquelle gefüllt werden. Mithilfe dieser Auflistung können die Eingabespalten überprüft werden, wenn der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer offline ist, die Verbindung mit der Komponente getrennt ist oder wenn die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A>-Eigenschaft **false** lautet.  
  
 Im folgenden Beispielcode wird eine externe Metadatenspalte auf Grundlage einer vorhandenen Eingabespalte hinzugefügt.  
  
```csharp  
private void AddExternalMetaDataColumn(IDTSInput100 input,IDTSInputColumn100 inputColumn)  
{  
    // Set the properties of the external metadata column.  
    IDTSExternalMetadataColumn100 externalColumn = input.ExternalMetadataColumnCollection.New();  
    externalColumn.Name = inputColumn.Name;  
    externalColumn.Precision = inputColumn.Precision;  
    externalColumn.Length = inputColumn.Length;  
    externalColumn.DataType = inputColumn.DataType;  
    externalColumn.Scale = inputColumn.Scale;  
  
    // Map the external column to the input column.  
    inputColumn.ExternalMetadataColumnID = externalColumn.ID;  
}  
```  
  
```vb  
Private Sub AddExternalMetaDataColumn(ByVal input As IDTSInput100, ByVal inputColumn As IDTSInputColumn100)  
  
    ' Set the properties of the external metadata column.  
    Dim externalColumn As IDTSExternalMetadataColumn100 = input.ExternalMetadataColumnCollection.New()  
    externalColumn.Name = inputColumn.Name  
    externalColumn.Precision = inputColumn.Precision  
    externalColumn.Length = inputColumn.Length  
    externalColumn.DataType = inputColumn.DataType  
    externalColumn.Scale = inputColumn.Scale  
  
    ' Map the external column to the input column.  
    inputColumn.ExternalMetadataColumnID = externalColumn.ID  
  
End Sub  
```  
  
## <a name="run-time"></a>Runtime  
 Während der Ausführung wird die Zielkomponente jedes Mal bei der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>-Methode aufgerufen, wenn eine voller <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> von der Upstreamkomponente verfügbar ist. Diese Methode wird immer wieder aufgerufen, bis keine Puffer mehr verfügbar sind und die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A>-Eigenschaft **true** ist. Bei dieser Methode lesen Zielkomponenten die Spalten und Zeilen im Puffer und fügen sie zur externen Datenquelle hinzu.  
  
### <a name="locating-columns-in-the-buffer"></a>Suchen von Spalten im Puffer  
 Der Eingabepuffer für eine Komponente enthält alle der in den Ausgabespaltenauflistungen der Komponenten upstream von der Komponente im Datenfluss definierten Spalten. Falls beispielsweise eine Quellkomponente drei Spalten in ihrer Ausgabe bereitstellt und die nächste Komponente eine weitere Ausgabespalte hinzufügt, enthält der Puffer, der für die Zielkomponente bereitgestellt wird, vier Spalten, selbst wenn die Zielkomponente nur zwei Spalten schreibt.  
  
 Die Reihenfolge der Spalten im Eingabepuffer wird nicht vom Index der Spalte in der Eingabespaltenauflistung der Zielkomponente definiert. Die genaue Position von Spalten in einer Pufferzeile kann nur mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A>-Methode des <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> ermittelt werden. Diese Methode sucht die Spalte anhand der angegebenen Herkunfts-ID im jeweiligen Puffer und gibt ihre Position in der Zeile zurück. Die Indizes der Eingabespalten werden in der Regel während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>-Methode gesucht und vom Entwickler für die spätere Verwendung während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> zwischengespeichert.  
  
 Im folgenden Codebeispiel wird die Position der Eingabespalten im Puffer während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> bestimmt und speichert diese in einem Array. Das Array wird anschließend während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> verwendet, um die Werte der Spalten im Puffer zu lesen.  
  
```csharp  
int[] cols;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
  
    cols = new int[input.InputColumnCollection.Count];  
  
    for (int x = 0; x < input.InputColumnCollection.Count; x++)  
    {  
        cols[x] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[x].LineageID);  
    }  
}  
```  
  
```vb  
Private cols As Integer()  
  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
    ReDim cols(input.InputColumnCollection.Count)  
  
    For x As Integer = 0 To input.InputColumnCollection.Count  
  
        cols(x) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(x).LineageID)  
    Next x  
  
End Sub  
```  
  
### <a name="processing-rows"></a>Verarbeiten von Zeilen  
 Sobald die Position der Eingabespalten im Puffer bestimmt wurde, können diese gelesen und in die externe Datanquelle geschrieben werden.  
  
 Während die Zielkomponente Zeilen in die externe Datenquelle schreibt, sollten Sie die Leistungsindikatoren "Gelesene Zeilen" oder "Gelesene BLOB-Bytes" durch Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.IncrementPipelinePerfCounter%2A>-Methode aktualisieren. Weitere Informationen finden Sie unter [Performance Counters](../../integration-services/performance/performance-counters.md).  
  
 Im folgenden Beispiel wird eine Komponente dargestellt, die Zeilen aus dem in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> bereitgestellten Puffer liest. Die Positionen der Indizes der Spalten im Puffer wurden während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> im vorangehenden Codebeispiel bestimmt.  
  
```csharp  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
        while (buffer.NextRow())  
        {  
            foreach (int col in cols)  
            {  
                if (!buffer.IsNull(col))  
                {  
                    //  TODO: Read the column data.  
                }  
            }  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For Each col As Integer In cols  
  
                If buffer.IsNull(col) = False Then  
  
                    '  TODO: Read the column data.  
                End If  
  
            Next col  
        End While  
End Sub  
```  
  
## <a name="sample"></a>Beispiel  
 Das folgende Beispiel zeigt eine einfache Zielkomponente, die über einen Dateiverbindungs-Manager binäre Daten aus dem Datenfluss in Dateien speichert. In diesem Beispiel werden nicht alle Methoden und Funktionen dargestellt, die in diesem Thema erläutert wurden. Es veranschaulicht die Hauptmethoden, die jede benutzerdefinierte Zielkomponente überschreiben muss, enthält jedoch keinen Code für eine Überprüfung zur Entwurfszeit.  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace BlobDst  
{  
  [DtsPipelineComponent(DisplayName = "BLOB Extractor Destination", Description = "Writes values of BLOB columns to files")]  
  public class BlobDst : PipelineComponent  
  {  
    string m_DestDir;  
    int m_FileNameColumnIndex = -1;  
    int m_BlobColumnIndex = -1;  
  
    public override void ProvideComponentProperties()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection.New();  
      input.Name = "BLOB Extractor Destination Input";  
      input.HasSideEffects = true;  
  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection.New();  
      conn.Name = "FileConnection";  
    }  
  
    public override void AcquireConnections(object transaction)  
    {  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection[0];  
      m_DestDir = (string)conn.ConnectionManager.AcquireConnection(null);  
  
      if (m_DestDir.Length > 0 && m_DestDir[m_DestDir.Length - 1] != '\\')  
        m_DestDir += "\\";  
    }  
  
    public override IDTSInputColumn100 SetUsageType(int inputID, IDTSVirtualInput100 virtualInput, int lineageID, DTSUsageType usageType)  
    {  
      IDTSInputColumn100 inputColumn = base.SetUsageType(inputID, virtualInput, lineageID, usageType);  
      IDTSCustomProperty100 custProp;  
  
      custProp = inputColumn.CustomPropertyCollection.New();  
      custProp.Name = "IsFileName";  
      custProp.Value = (object)false;  
  
      custProp = inputColumn.CustomPropertyCollection.New();  
      custProp.Name = "IsBLOB";  
      custProp.Value = (object)false;  
  
      return inputColumn;  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
      IDTSCustomProperty100 custProp;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        custProp = column.CustomPropertyCollection["IsFileName"];  
        if ((bool)custProp.Value == true)  
        {  
          m_FileNameColumnIndex = (int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID);  
        }  
  
        custProp = column.CustomPropertyCollection["IsBLOB"];  
        if ((bool)custProp.Value == true)  
        {  
          m_BlobColumnIndex = (int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID);  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        string strFileName = buffer.GetString(m_FileNameColumnIndex);  
        int blobLength = (int)buffer.GetBlobLength(m_BlobColumnIndex);  
        byte[] blobData = buffer.GetBlobData(m_BlobColumnIndex, 0, blobLength);  
  
        strFileName = TranslateFileName(strFileName);  
  
        // Make sure directory exists before creating file.  
        FileInfo fi = new FileInfo(strFileName);  
        if (!fi.Directory.Exists)  
          fi.Directory.Create();  
  
        // Write the data to the file.  
        FileStream fs = new FileStream(strFileName, FileMode.Create, FileAccess.Write, FileShare.None);  
        fs.Write(blobData, 0, blobLength);  
        fs.Close();  
      }  
    }  
  
    private string TranslateFileName(string fileName)  
    {  
      if (fileName.Length > 2 && fileName[1] == ':')  
        return m_DestDir + fileName.Substring(3, fileName.Length - 3);  
      else  
        return m_DestDir + fileName;  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.IO   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Namespace BlobDst   
  
 <DtsPipelineComponent(DisplayName="BLOB Extractor Destination", Description="Writes values of BLOB columns to files")> _   
 Public Class BlobDst   
 Inherits PipelineComponent   
   Private m_DestDir As String   
   Private m_FileNameColumnIndex As Integer = -1   
   Private m_BlobColumnIndex As Integer = -1   
  
   Public  Overrides Sub ProvideComponentProperties()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New   
     input.Name = "BLOB Extractor Destination Input"   
     input.HasSideEffects = True   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New   
     conn.Name = "FileConnection"   
   End Sub   
  
   Public  Overrides Sub AcquireConnections(ByVal transaction As Object)   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection(0)   
     m_DestDir = CType(conn.ConnectionManager.AcquireConnection(Nothing), String)   
     If m_DestDir.Length > 0 AndAlso Not (m_DestDir(m_DestDir.Length - 1) = "\"C) Then   
       m_DestDir += "\"   
     End If   
   End Sub   
  
   Public  Overrides Function SetUsageType(ByVal inputID As Integer, ByVal virtualInput As IDTSVirtualInput100, ByVal lineageID As Integer, ByVal usageType As DTSUsageType) As IDTSInputColumn100   
     Dim inputColumn As IDTSInputColumn100 = MyBase.SetUsageType(inputID, virtualInput, lineageID, usageType)   
     Dim custProp As IDTSCustomProperty100   
     custProp = inputColumn.CustomPropertyCollection.New   
     custProp.Name = "IsFileName"   
     custProp.Value = CType(False, Object)   
     custProp = inputColumn.CustomPropertyCollection.New   
     custProp.Name = "IsBLOB"   
     custProp.Value = CType(False, Object)   
     Return inputColumn   
   End Function   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     Dim custProp As IDTSCustomProperty100   
     For Each column As IDTSInputColumn100 In inputColumns   
       custProp = column.CustomPropertyCollection("IsFileName")   
       If CType(custProp.Value, Boolean) = True Then   
         m_FileNameColumnIndex = CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer)   
       End If   
       custProp = column.CustomPropertyCollection("IsBLOB")   
       If CType(custProp.Value, Boolean) = True Then   
         m_BlobColumnIndex = CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer)   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       Dim strFileName As String = buffer.GetString(m_FileNameColumnIndex)   
       Dim blobLength As Integer = CType(buffer.GetBlobLength(m_BlobColumnIndex), Integer)   
       Dim blobData As Byte() = buffer.GetBlobData(m_BlobColumnIndex, 0, blobLength)   
       strFileName = TranslateFileName(strFileName)   
       Dim fi As FileInfo = New FileInfo(strFileName)   
       ' Make sure directory exists before creating file.  
       If Not fi.Directory.Exists Then   
         fi.Directory.Create   
       End If   
       ' Write the data to the file.  
       Dim fs As FileStream = New FileStream(strFileName, FileMode.Create, FileAccess.Write, FileShare.None)   
       fs.Write(blobData, 0, blobLength)   
       fs.Close   
     End While   
   End Sub   
  
   Private Function TranslateFileName(ByVal fileName As String) As String   
     If fileName.Length > 2 AndAlso fileName(1) = ":"C Then   
       Return m_DestDir + fileName.Substring(3, fileName.Length - 3)   
     Else   
       Return m_DestDir + fileName   
     End If   
   End Function   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Entwickeln einer benutzerdefinierten Quellkomponente](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)   
 [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
  
  
