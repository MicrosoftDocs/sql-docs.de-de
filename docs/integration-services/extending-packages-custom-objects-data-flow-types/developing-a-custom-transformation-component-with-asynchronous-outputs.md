---
description: Entwickeln einer benutzerdefinierten Transformationskomponente mit asynchronen Ausgaben
title: Entwickeln einer benutzerdefinierten Transformationskomponente mit asynchronen Ausgaben | Microsoft-Dokumentation
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
- custom data flow components [Integration Services], transformation components
- asynchronous outputs [Integration Services]
- ProcessInput method
- cache [Integration Services]
- buffer allocations [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], transformation components
ms.assetid: 1c3e92c7-a4fa-4fdd-b9ca-ac3069536274
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6eeddfc5c4fff29b27462889d39748c8df2a01e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348252"
---
# <a name="developing-a-custom-transformation-component-with-asynchronous-outputs"></a>Entwickeln einer benutzerdefinierten Transformationskomponente mit asynchronen Ausgaben

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie eine Komponente mit asynchronen Ausgaben dann, wenn eine Transformation keine Zeilen ausgeben kann, solange die Komponente nicht alle ihre Eingabezeilen empfangen hat, oder wenn die Transformation nicht genau eine Ausgabezeile für jede als Eingabe empfangene Zeile erstellt. Die Transformation für das Aggregieren kann z. B. erst dann eine Summe über mehrere Zeilen errechnen, wenn sie alle Zeilen gelesen hat. Dagegen können Sie jederzeit eine Komponente mit synchronen Ausgaben verwenden, wenn Sie jede Datenzeile beim Durchlaufen verändern. Sie können die Daten für jede vorhandene Zeile verändern, oder Sie können eine oder mehrere neue Spalten erstellen, wovon jede einen Wert für jede einzelne der Eingabzeilen aufweist. Weitere Informationen zu den Unterschieden zwischen synchronen und asynchronen Komponenten finden Sie unter [Grundlegendes zu synchronen und asynchronen Transformationen](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Transformationskomponenten mit asynchronen Ausgaben sind eindeutig, da sie gleichzeitig als Ziel- und als Quellkomponenten fungieren. Diese Art der Komponente empfängt Zeilen von Upstreamkomponenten und fügt Zeilen hinzu, die von Downstreamkomponenten verwendet werden. Keine andere Datenflusskomponente führt beide dieser Vorgänge aus.  
  
 Die Spalten von Upstreamkomponenten, die einer Komponente mit synchronen Ausgaben zur Verfügung stehen, stehen den Komponenten, die sich unterhalb der Komponente befinden, automatisch zur Verfügung. Daher muss eine Komponente mit synchronen Ausgaben keine Ausgabespalten definieren, um für die nächste Komponente Spalten und Zeilen bereitzustellen. Komponenten mit asynchronen Ausgaben müssen dagegen Ausgabespalten definieren, und Zeilen für die Downstreamkomponenten bereitstellen. Daher muss eine Komponente mit asynchronen Ausgaben sowohl in der Entwurfs- als auch in der Ausführungszeit mehr Tasks ausführen, und der Komponentenentwickler muss mehr Code implementieren.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält mehrere Transformationen mit asynchronen Ausgaben. So erfordert z. B. die Transformation zum Sortieren alle Zeilen, bevor sie sie sie sortieren kann, wobei sie asynchrone Ausgaben verwendet. Nachdem sie alle Zeilen empfangen hat, sortiert sie sie und fügt sie der Ausgabe hinzu.  
  
 Dieser Abschnitt erklärt im Detail, wie Transformationen mit asynchronen Ausgaben entwickelt werden. Weitere Informationen zur Entwicklung von Quellkomponenten finden Sie unter [Entwickeln einer benutzerdefinierten Quellkomponente](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="design-time"></a>Entwurfszeit  
  
### <a name="creating-the-component"></a>Erstellen der Komponente  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> -Eigenschaft des <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>-Objekts identifiziert, ob eine Ausgabe synchron oder asynchron ist. Um eine asynchrone Ausgabe zu erstellen, fügen Sie die Ausgabe der Komponente hinzu, und legen Sie <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> auf 0 (null) fest. Durch Festlegen dieser Eigenschaft wird außerdem bestimmt, ob der Datenflusstask <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>-Objekte sowohl für die Eingabe als auch für die Ausgabe der Komponente zuordnet, oder ob ein einzelner Puffer zugewiesen wird, den die beiden Objekte gemeinsam verwenden.  
  
 Im folgenden Beispielcode wird eine Komponente, die in ihrer <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>-Implementierung eine asynchrone Ausgabe erstellt, veranschaulicht.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "AsyncComponent",ComponentType = ComponentType.Transform)]  
    public class AsyncComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Call the base class, which adds a synchronous input  
            // and output.  
            base.ProvideComponentProperties();  
  
            // Make the output asynchronous.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
            output.SynchronousInputID = 0;  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="AsyncComponent", ComponentType:=ComponentType.Transform)> _  
Public Class AsyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Call the base class, which adds a synchronous input  
        ' and output.  
        Me.ProvideComponentProperties()  
  
        ' Make the output asynchronous.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
        output.SynchronousInputID = 0  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Erstellen und Konfigurieren von Ausgabespalten  
 Wie bereits erwähnt, fügt eine asynchrone Komponente Spalten zu ihrer Ausgabespaltenauflistung hinzu, um Spalten für die Downstreamkomponenten bereitzustellen. Je nach den Anforderungen der Komponente stehen mehrere Entwurfszeitmethoden zur Wahl. Zur Übergabe aller Spalten der Upstreamkomponenten an die Downstreamkomponenten würden Sie z. B. die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.OnInputPathAttached%2A>-Methode überschreiben, um die Spalten hinzuzufügen, da dies die erste Methode ist, in der die Eingabespalten für die Komponenten verfügbar sind.  
  
 Wenn die Komponente basierend auf den für ihre Eingabe ausgewählten Spalten Ausgabespalten erstellt, überschreiben Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetUsageType%2A>-Methode, um die Ausgabespalten auszuwählen und anzugeben, wie sie verwendet werden.  
  
 Wenn eine Komponente mit asynchronen Ausgaben basierend auf den Spalten von einer Upstreamkomponente Ausgabespalten erstellt und die verfügbaren Upstreamspalten sich ändern, dann sollte die Komponente ihre Ausgabespaltenauflistung aktualisieren. Diese Änderungen sollten von der Komponente während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> erkannt und während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> korrigiert werden.  
  
> [!NOTE]  
>  Wenn eine Ausgabespalte von der Ausgabespaltenauflistung entfernt wird, dann werden die Downstreamkomponenten im Datenfluss, die auf die Spalte verweisen, beeinträchtigt. Die Ausgabespalte muss repariert werden, ohne dass die Spalte entfernt und wieder erstellt wird, um eine Beschädigung der Downstreamkomponenten zu vermeiden. Wenn sich z. B. der Datentyp der Spalte geändert hat, müssen Sie den Datentyp aktualisieren.  
  
 Das folgende Codebeispiel zeigt eine Komponente, die für jede von der Upstreamkomponente verfügbare Spalte eine Ausgabespalte zu ihrer Ausgabespaltenauflistung hinzufügt.  
  
```csharp  
public override void OnInputPathAttached(int inputID)  
{  
   IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
   IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
   IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
   foreach (IDTSVirtualInputColumn100 vCol in vInput.VirtualInputColumnCollection)  
   {  
      IDTSOutputColumn100 outCol = output.OutputColumnCollection.New();  
      outCol.Name = vCol.Name;  
      outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage);  
   }  
}  
```  
  
```vb  
Public Overrides Sub OnInputPathAttached(ByVal inputID As Integer)  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput()  
  
    For Each vCol As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
  
        Dim outCol As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        outCol.Name = vCol.Name  
        outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage)  
  
    Next  
End Sub  
```  
  
## <a name="run-time"></a>Runtime  
 Komponenten mit asynchronen Ausgaben führen während der Laufzeit ebenfalls eine andere Sequenz von Methoden aus als andere Komponententypen. Zunächst sind sie die einzigen Komponenten, die sowohl bei der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>- als auch der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>-Methode aufgerufen werden. Komponenten mit asynchronen Ausgaben benötigen ebenfalls Zugang zu allen eingehenden Zeilen, bevor sie mit der Verarbeitung beginnen können; daher müssen sie die Eingabezeilen intern zwischenspeichern, bis alle Zeilen gelesen wurden. Schließlich empfangen Komponenten mit asynchronen Ausgaben im Gegensatz zu anderen Komponenten sowohl einen Eingabe- als auch einen Ausgabepuffer.  
  
### <a name="understanding-the-buffers"></a>Grundlegendes zu den Puffern  
 Der Eingabepuffer wird von der Komponente während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> empfangen. Dieser Puffer enthält die dem Puffer von Upstreamkomponenten hinzugefügten Zeilen. Der Puffer enthält zusätzlich zu den Spalten, die in der Ausgabe einer Upstreamkomponente bereitgestellt wurden, jedoch nicht zu der asynchronen Eingabeauflistung der Komponente hinzugefügt wurden, auch die Spalten der Ausgabe der Komponente.  
  
 Der Ausgabepuffer, der der Komponente in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> zur Verfügung gestellt wird, enthält anfänglich keine Zeilen. Die Komponente fügt Zeilen zu diesem Puffer hinzu und stellt den Puffer Downstreamkomponenten zur Verfügung, wenn dieser voll ist. Der Ausgabepuffer enthält zusätzlich zu den Spalten, die andere Downstreamkomponenten zu ihren Ausgaben hinzugefügt haben, die in der Ausgabespaltenauflistung der Komponente definierten Spalten.  
  
 Dieses Verhalten unterscheidet sich von dem der Komponenten mit synchronen Ausgaben, die einen einzelnen, gemeinsam verwendeten Puffer empfangen. Der gemeinsam verwendete Puffer einer Komponente mit synchronen Ausgaben enthält zusätzlich zu den Spalten, die zu den Ausgaben der Upstream- und Downstreamkomponenten hinzugefügt wurden, sowohl die Eingabe- als auch Ausgabespalten der Komponente.  
  
### <a name="processing-rows"></a>Verarbeiten von Zeilen  
  
#### <a name="caching-input-rows"></a>Zwischenspeichern von Eingabezeilen  
 Wenn Sie eine Komponente mit asynchronen Ausgaben schreiben, dann stehen Ihnen drei Optionen für das Hinzufügen von Zeilen zum Ausgabepuffer zur Verfügung. Sie können diese hinzufügen, sowie die Eingabezeilen empfangen werden, Sie können sie zwischenspeichern, bis die Komponente alle Zeilen von der Upstreamkomponente erhalten hat, oder Sie können sie zu einem für die Komponente geeigneten Zeitpunkt hinzufügen. Die von Ihnen gewählte Methode hängt von den Anforderungen der Komponente ab. Zum Beispiel müssen bei der Komponente zum Sortieren zunächst alle Upstreamzeilen empfangen wurden, bevor diese sortiert werden können. Deshalb wartet sie, bis alle Zeilen gelesen sind, bevor sie Zeilen zum Ausgabepuffer hinzufügt.  
  
 Die im Eingabepuffer empfangenen Zeilen müssen intern von der Komponente zwischengespeichert werden, bis sie bereit ist, diese zu verarbeiten. Die eingehenden Pufferzeilen können in einer Datentabelle, einem multidimensionalem Array oder einer beliebigen anderen internen Struktur zwischengespeichert werden.  
  
#### <a name="adding-output-rows"></a>Hinzufügen von Ausgabezeilen  
 Unabhängig davon, ob Sie Zeilen zum Ausgabepuffer hinzufügen, sobald diese empfangen werden oder nachdem alle Zeilen empfangen wurden, rufen Sie dazu die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A>-Methode für den Ausgabepuffer auf. Nachdem Sie die Zeile hinzugefügt haben, legen Sie die Werte jeder Spalte in der neuen Zeile fest.  
  
 Da sich in manchen Fällen mehr Spalten im Ausgabepuffer als in der Ausgabespaltenauflistung der Komponente befinden, müssen Sie den Index der geeigneten Spalte im Puffer finden, bevor Sie seinen Wert festlegen können. Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>-Eigenschaft gibt den Index der Spalte in der Pufferzeile mit der angegebenen Herkunfts-ID zurück, die dann verwendet wird, um der Pufferspalte den Wert zuzuweisen.  
  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>-Methode, die vor der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Methode oder der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>-Methode aufgerufen wird, ist die erste Methode, bei der die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>-Eigenschaft verfügbar ist, und sie bietet die erste Möglichkeit, die Indizes der Spalten in den Eingabe- und Ausgabepuffern zu suchen.  
  
## <a name="sample"></a>Beispiel  
 Im folgenden Beispiel wird eine einfache Transformationskomponente mit asynchronen Ausgaben gezeigt, die Zeilen zum Ausgabepuffer hinzufügt, sowie sie erhalten werden. In diesem Beispiel werden nicht alle Methoden und Funktionen dargestellt, die in diesem Thema erläutert wurden. Es veranschaulicht die wichtigen Methoden, die jede benutzerdefinierte Transformationskomponente mit asynchronen Ausgaben überschreiben muss, enthält jedoch keinen Code für eine Überprüfung zur Entwurfszeit. Außerdem wird im Code in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> vorausgesetzt, dass die Ausgabespaltenauflistung für jede Spalte in der Eingabenspaltenauflistung eine Spalte aufweist.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
   [DtsPipelineComponent(DisplayName = "AsynchronousOutput")]  
   public class AsynchronousOutput : PipelineComponent  
   {  
      PipelineBuffer outputBuffer;  
      int[] inputColumnBufferIndexes;  
      int[] outputColumnBufferIndexes;  
  
      public override void ProvideComponentProperties()  
      {  
         // Let the base class add the input and output objects.  
         base.ProvideComponentProperties();  
  
         // Name the input and output, and make the  
         // output asynchronous.  
         ComponentMetaData.InputCollection[0].Name = "Input";  
         ComponentMetaData.OutputCollection[0].Name = "AsyncOutput";  
         ComponentMetaData.OutputCollection[0].SynchronousInputID = 0;  
      }  
      public override void PreExecute()  
      {  
         IDTSInput100 input = ComponentMetaData.InputCollection[0];  
         IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
         inputColumnBufferIndexes = new int[input.InputColumnCollection.Count];  
         outputColumnBufferIndexes = new int[output.OutputColumnCollection.Count];  
  
         for (int col = 0; col < input.InputColumnCollection.Count; col++)  
            inputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[col].LineageID);  
  
         for (int col = 0; col < output.OutputColumnCollection.Count; col++)  
            outputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[col].LineageID);  
  
      }  
  
      public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
      {  
         if (buffers.Length != 0)  
            outputBuffer = buffers[0];  
      }  
      public override void ProcessInput(int inputID, PipelineBuffer buffer)  
      {  
            // Advance the buffer to the next row.  
            while (buffer.NextRow())  
            {  
               // Add a row to the output buffer.  
               outputBuffer.AddRow();  
               for (int x = 0; x < inputColumnBufferIndexes.Length; x++)  
               {  
                  // Copy the data from the input buffer column to the output buffer column.  
                  outputBuffer[outputColumnBufferIndexes[x]] = buffer[inputColumnBufferIndexes[x]];  
               }  
            }  
         if (buffer.EndOfRowset)  
         {  
            // EndOfRowset on the input buffer is true.  
            // Set EndOfRowset on the output buffer.  
            outputBuffer.SetEndOfRowset();  
         }  
      }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="AsynchronousOutput")> _  
    Public Class AsynchronousOutput  
  
        Inherits PipelineComponent  
  
        Private outputBuffer As PipelineBuffer  
        Private inputColumnBufferIndexes As Integer()  
        Private outputColumnBufferIndexes As Integer()  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Let the base class add the input and output objects.  
            Me.ProvideComponentProperties()  
  
            ' Name the input and output, and make the  
            ' output asynchronous.  
            ComponentMetaData.InputCollection(0).Name = "Input"  
            ComponentMetaData.OutputCollection(0).Name = "AsyncOutput"  
            ComponentMetaData.OutputCollection(0).SynchronousInputID = 0  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
            Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
            ReDim inputColumnBufferIndexes(input.InputColumnCollection.Count)  
            ReDim outputColumnBufferIndexes(output.OutputColumnCollection.Count)  
  
            For col As Integer = 0 To input.InputColumnCollection.Count  
                inputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(col).LineageID)  
            Next  
  
            For col As Integer = 0 To output.OutputColumnCollection.Count  
                outputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(col).LineageID)  
            Next  
  
        End Sub  
        Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
            If buffers.Length <> 0 Then  
                outputBuffer = buffers(0)  
            End If  
  
        End Sub  
  
        Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
                ' Advance the buffer to the next row.  
                While (buffer.NextRow())  
  
                    ' Add a row to the output buffer.  
                    outputBuffer.AddRow()  
                    For x As Integer = 0 To inputColumnBufferIndexes.Length  
  
                        ' Copy the data from the input buffer column to the output buffer column.  
                        outputBuffer(outputColumnBufferIndexes(x)) = buffer(inputColumnBufferIndexes(x))  
  
                    Next  
                End While  
  
            If buffer.EndOfRowset = True Then  
                ' EndOfRowset on the input buffer is true.  
                ' Set the end of row set on the output buffer.  
                outputBuffer.SetEndOfRowset()  
            End If  
        End Sub  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [Grundlegendes zu synchronen und asynchronen Transformationen](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Erstellen einer asynchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
