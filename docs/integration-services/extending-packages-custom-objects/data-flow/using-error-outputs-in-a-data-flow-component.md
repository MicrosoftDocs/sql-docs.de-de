---
description: Verwenden von Fehlerausgaben in einer Datenflusskomponente
title: Verwenden von Fehlerausgaben in einer Datenflusskomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- errors [Integration Services], alternative outputs
- synchronous error outputs [Integration Services]
- alternative error outputs [Integration Services]
- custom data flow components [Integration Services], error outputs
- data flow components [Integration Services], error outputs
- populating error columns [Integration Services]
- redirecting error output [Integration Services]
- error outputs [Integration Services]
- asynchronous error outputs [Integration Services]
ms.assetid: a2a3e7c8-1de2-45b3-97fb-60415d3b0934
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d5beef1e593f8182ac09142cb9a186fc901e5918
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343616"
---
# <a name="using-error-outputs-in-a-data-flow-component"></a>Verwenden von Fehlerausgaben in einer Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Spezielle <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>-Objekte, die als Fehlerausgaben bezeichnet werden, können zu Komponenten hinzugefügt werden, sodass die Komponente Zeilen, die bei der Ausführung nicht verarbeitet werden können, umleiten kann. Die Probleme, die bei einer Komponente auftreten können, lassen sich meist in Fehler oder abgeschnittene Daten einteilen und sind komponentenspezifisch. Komponenten, die Fehlerausgaben bereitstellen, bieten den Benutzern der Komponente die Flexibilität, Fehlerbedingungen durch Herausfiltern von Fehlerzeilen aus dem Resultset, durch Behandeln der Komponente als fehlerhaft, wenn ein Problem auftritt, oder durch Ignorieren von Fehlern und Fortsetzen des Vorgangs zu behandeln.  
  
 Um Fehlerausgaben in einer Komponente zu implementieren oder zu unterstützen müssen Sie zunächst die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.UsesDispositions%2A>-Eigenschaft der Komponente auf **true** festlegen. Dann müssen Sie eine Ausgabe zu der Komponente hinzufügen, bei der die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.IsErrorOut%2A>-Eigenschaft auf **true** festgelegt ist. Zuletzt muss die Komponente Code enthalten, der Zeilen an die Fehlerausgabe umleitet, wenn Fehler auftreten oder Daten abgeschnitten werden. In diesem Thema werden diese drei Schritte beschrieben und die Unterschiede zwischen synchronen und asynchronen Fehlerausgaben erklärt.  
  
## <a name="creating-an-error-output"></a>Erstellen einer Fehlerausgabe  
 Sie erstellen eine Fehlerausgabe, indem Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputCollection100.New%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.OutputCollection%2A>-Eigenschaft aufrufen und dann die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.IsErrorOut%2A>-Eigenschaft der neuen Ausgabe auf **true** festlegen. Wenn die Ausgabe asynchron ist, müssen keine weiteren Schritte für die Ausgabe durchgeführt werden. Wenn die Ausgabe synchron ist und wenn eine andere Ausgabe vorhanden ist, die mit dieser Eingabe synchron ist, müssen Sie auch die Eigenschaften <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.ExclusionGroup%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> festlegen. Beide Eigenschaften müssen dieselben Werte wie die andere Ausgabe aufweisen, die mit dieser Eingabe synchron ist. Wenn diese Eigenschaften nicht auf einen Wert ungleich null festgelegt werden, werden die von der Eingabe bereitgestellten Zeilen an die beiden Ausgaben gesendet, die mit der Eingabe synchron sind.  
  
 Wenn bei einer Komponente während der Ausführung Fehler auftreten oder Daten abgeschnitten werden, setzt die Komponente den Vorgang anhand der Einstellungen der Eigenschaften <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100.ErrorRowDisposition%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100.TruncationRowDisposition%2A> der Eingabe oder Ausgabe oder Eingabe- oder Ausgabespalte fort, an der der Fehler aufgetreten ist. Der Wert dieser Eigenschaften muss standardmäßig auf <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition.RD_NotUsed> festgelegt werden. Wenn die Fehlerausgabe der Komponente mit einer Downstreamkomponente verbunden ist, wird diese Eigenschaft vom Benutzer der Komponente festgelegt, sodass der Benutzer bestimmen kann, wie die Komponente den Fehler oder die abgeschnittenen Daten behandelt.  
  
## <a name="populating-error-columns"></a>Auffüllen von Fehlerspalten  
 Beim Erstellen einer Fehlerausgabe fügt der Datenflusstask automatisch zwei Spalten zur Ausgabespaltenauflistung hinzu. Diese Spalten werden von Komponenten verwendet, um die ID der Spalte anzugeben, die den Fehler oder die abgeschnittenen Daten verursacht hat, und um den komponentenspezifischen Fehlercode bereitzustellen. Diese Spalten werden automatisch erstellt. Die in den Spalten enthaltenen Werte müssen jedoch von der Komponente festgelegt werden.  
  
 Welche Methode zum Festlegen der Werte dieser Spalten verwendet wird, hängt davon ab, ob es sich um eine synchrone oder asynchrone Fehlerausgabe handelt. Komponenten mit synchronen Ausgaben rufen die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.DirectErrorRow%2A>-Methode auf, die im nächsten Abschnitt ausführlich beschrieben wird, und stellen den Fehlercode und die Fehlerspaltenwerte als Parameter bereit. Bei Komponenten mit asynchronen Ausgaben gibt es zwei Möglichkeiten zum Festlegen der Werte dieser Spalten. Sie können entweder die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetErrorInfo%2A>-Methode des Ausgabepuffers aufrufen und die Werte bereitstellen, oder die Fehlerspalten mithilfe von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> im Puffer suchen und die Werte für die Spalten direkt festlegen. Da sich die Namen der Spalten oder der Speicherort in der Ausgabespaltenauflistung jedoch möglicherweise geändert haben, stellt die zweite Möglichkeit keine zuverlässige Methode dar. Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetErrorInfo%2A>-Methode legt die Werte automatisch in diesen Fehlerspalten fest, ohne dass sie manuell gesucht werden müssen.  
  
 Wenn Sie die Fehlerbeschreibung zu einem bestimmten Fehlercode abrufen müssen, können Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle verwenden, die über die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>-Eigenschaft der Komponente verfügbar ist.  
  
 In den folgenden Codebeispielen wird eine Komponente gezeigt, die über eine Eingabe und zwei Ausgaben, darunter eine Fehlerausgabe, verfügt. Im ersten Beispiel wird gezeigt, wie eine zur Eingabe synchrone Fehlerausgabe erstellt wird. Im zweiten Beispiel wird gezeigt, wie eine asynchrone Fehlerausgabe erstellt wird.  
  
```csharp  
public override void ProvideComponentProperties()  
{  
    // Specify that the component has an error output.  
    ComponentMetaData.UsesDispositions = true;  
    // Create the input.  
    IDTSInput100 input = ComponentMetaData.InputCollection.New();  
    input.Name = "Input";  
    input.ErrorRowDisposition = DTSRowDisposition.RD_NotUsed;  
    input.ErrorOrTruncationOperation = "A string describing the possible error or truncation that may occur during execution.";  
  
    // Create the default output.  
    IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
    output.Name = "Output";  
    output.SynchronousInputID = input.ID;  
    output.ExclusionGroup = 1;  
  
    // Create the error output.  
    IDTSOutput100 errorOutput = ComponentMetaData.OutputCollection.New();  
    errorOutput.IsErrorOut = true;  
    errorOutput.Name = "ErrorOutput";  
    errorOutput.SynchronousInputID = input.ID;  
    errorOutput.ExclusionGroup = 1;  
  
}  
```  
  
```vb  
Public  Overrides Sub ProvideComponentProperties()   
  
 ' Specify that the component has an error output.  
 ComponentMetaData.UsesDispositions = True   
  
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New   
  
 ' Create the input.  
 input.Name = "Input"   
 input.ErrorRowDisposition = DTSRowDisposition.RD_NotUsed   
 input.ErrorOrTruncationOperation = "A string describing the possible error or truncation that may occur during execution."   
  
 ' Create the default output.  
 Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
 output.Name = "Output"   
 output.SynchronousInputID = input.ID   
 output.ExclusionGroup = 1   
  
 ' Create the error output.  
 Dim errorOutput As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
 errorOutput.IsErrorOut = True   
 errorOutput.Name = "ErrorOutput"   
 errorOutput.SynchronousInputID = input.ID   
 errorOutput.ExclusionGroup = 1   
  
End Sub  
```  
  
 Mit dem folgenden Beispielcode wird eine asynchrone Fehlerausgabe erstellt.  
  
```csharp  
public override void ProvideComponentProperties()  
{  
    // Specify that the component has an error output.  
    ComponentMetaData.UsesDispositions = true;  
  
    // Create the input.  
    IDTSInput100 input = ComponentMetaData.InputCollection.New();  
    input.Name = "Input";  
    input.ErrorRowDisposition = DTSRowDisposition.RD_NotUsed;  
    input.ErrorOrTruncationOperation = "A string describing the possible error or truncation that may occur during execution.";  
  
    // Create the default output.  
    IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
    output.Name = "Output";  
  
    // Create the error output.  
    IDTSOutput100 errorOutput = ComponentMetaData.OutputCollection.New();  
    errorOutput.Name = "ErrorOutput";  
    errorOutput.IsErrorOut = true;  
}  
```  
  
```vb  
Public  Overrides Sub ProvideComponentProperties()   
  
 ' Specify that the component has an error output.  
 ComponentMetaData.UsesDispositions = True   
  
 ' Create the input.  
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New   
  
 ' Create the default output.  
 input.Name = "Input"   
 input.ErrorRowDisposition = DTSRowDisposition.RD_NotUsed   
 input.ErrorOrTruncationOperation = "A string describing the possible error or truncation that may occur during execution."   
  
 ' Create the error output.  
 Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
 output.Name = "Output"   
 Dim errorOutput As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
 errorOutput.Name = "ErrorOutput"   
 errorOutput.IsErrorOut = True   
  
End Sub  
```  
  
## <a name="redirecting-a-row-to-an-error-output"></a>Umleiten einer Zeile an eine Fehlerausgabe  
 Nach dem Hinzufügen einer Fehlerausgabe zu einer Komponente müssen Sie Code bereitstellen, der den Fehler oder die abgeschnittenen Daten je nach Komponente behandelt und die Zeilen mit dem Fehler oder den abgeschnittenen Daten an die Fehlerausgabe umleitet. Hierzu haben Sie zwei Möglichkeiten, je nachdem, ob es sich um eine synchrone oder asynchrone Fehlerausgabe handelt.  
  
### <a name="redirecting-a-row-with-synchronous-outputs"></a>Umleiten einer Zeile mit synchronen Ausgaben  
 Zeilen werden an synchrone Ausgaben gesendet, indem die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.DirectErrorRow%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>-Klasse aufgerufen wird. Der Methodenaufruf enthält die ID der Fehlerausgabe, den komponentendefinierten Fehlercode und den Index der Spalte, die von der Komponente nicht verarbeitet werden konnte, als Parameter.  
  
 Im folgenden Codebeispiel wird gezeigt, wie eine Zeile in einem Puffer mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.DirectErrorRow%2A>-Methode an eine synchrone Fehlerausgabe geleitet wird.  
  
```csharp  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
        IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
  
        // This code sample assumes the component has two outputs, one the default,  
        // the other the error output. If the errorOutputIndex returned from GetErrorOutputInfo  
        // is 0, then the default output is the second output in the collection.  
        int defaultOutputID = -1;  
        int errorOutputID = -1;  
        int errorOutputIndex = -1;  
  
        GetErrorOutputInfo(ref errorOutputID,ref errorOutputIndex);  
  
        if (errorOutputIndex == 0)  
            defaultOutputID = ComponentMetaData.OutputCollection[1].ID;  
        else  
            defaultOutputID = ComponentMetaData.OutputCollection[0].ID;  
  
        while (buffer.NextRow())  
        {  
            try  
            {  
                // TODO: Implement code to process the columns in the buffer row.  
  
                // Ideally, your code should detect potential exceptions before they occur, rather  
                // than having a generic try/catch block such as this.   
                // However, because the error or truncation implementation is specific to each component,  
                // this sample focuses on actually directing the row, and not a single error or truncation.  
  
                // Unless an exception occurs, direct the row to the default   
                buffer.DirectRow(defaultOutputID);  
            }  
            catch  
            {  
                // Yes, has the user specified to redirect the row?  
                if (input.ErrorRowDisposition == DTSRowDisposition.RD_RedirectRow)  
                {  
                    // Yes, direct the row to the error output.  
                    // TODO: Add code to include the errorColumnIndex.  
                    buffer.DirectErrorRow(errorOutputID, 0, errorColumnIndex);  
                }  
                else if (input.ErrorRowDisposition == DTSRowDisposition.RD_FailComponent || input.ErrorRowDisposition == DTSRowDisposition.RD_NotUsed)  
                {  
                    // No, the user specified to fail the component, or the error row disposition was not set.  
                    throw new Exception("An error occurred, and the DTSRowDisposition is either not set, or is set to fail component.");  
                }  
                else  
                {  
                    // No, the user specified to ignore the failure so   
                    // direct the row to the default output.  
                    buffer.DirectRow(defaultOutputID);  
                }  
  
            }  
        }  
}  
```  
  
```vb  
Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
   Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)   
  
   ' This code sample assumes the component has two outputs, one the default,  
   ' the other the error output. If the errorOutputIndex returned from GetErrorOutputInfo  
   ' is 0, then the default output is the second output in the collection.  
   Dim defaultOutputID As Integer = -1   
   Dim errorOutputID As Integer = -1   
   Dim errorOutputIndex As Integer = -1   
  
   GetErrorOutputInfo(errorOutputID, errorOutputIndex)   
  
   If errorOutputIndex = 0 Then   
     defaultOutputID = ComponentMetaData.OutputCollection(1).ID   
   Else   
     defaultOutputID = ComponentMetaData.OutputCollection(0).ID   
   End If   
  
   While buffer.NextRow   
     Try   
       ' TODO: Implement code to process the columns in the buffer row.  
  
       ' Ideally, your code should detect potential exceptions before they occur, rather  
       ' than having a generic try/catch block such as this.   
       ' However, because the error or truncation implementation is specific to each component,  
       ' this sample focuses on actually directing the row, and not a single error or truncation.  
  
       ' Unless an exception occurs, direct the row to the default   
       buffer.DirectRow(defaultOutputID)   
     Catch   
       ' Yes, has the user specified to redirect the row?  
       If input.ErrorRowDisposition = DTSRowDisposition.RD_RedirectRow Then   
         ' Yes, direct the row to the error output.  
         ' TODO: Add code to include the errorColumnIndex.  
         buffer.DirectErrorRow(errorOutputID, 0, errorColumnIndex)   
       Else   
         If input.ErrorRowDisposition = DTSRowDisposition.RD_FailComponent OrElse input.ErrorRowDisposition = DTSRowDisposition.RD_NotUsed Then   
           ' No, the user specified to fail the component, or the error row disposition was not set.  
           Throw New Exception("An error occurred, and the DTSRowDisposition is either not set, or is set to fail component.")   
         Else   
           ' No, the user specified to ignore the failure so   
           ' direct the row to the default output.  
           buffer.DirectRow(defaultOutputID)   
         End If   
       End If   
     End Try   
   End While   
End Sub  
```  
  
### <a name="redirecting-a-row-with-asynchronous-outputs"></a>Umleiten einer Zeile mit asynchronen Ausgaben  
 Komponenten mit asynchronen Ausgaben senden Zeilen anders als Komponenten mit synchronen Fehlerausgaben an eine Fehlerausgabe, indem sie eine Zeile explizit zur Ausgabe <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> hinzufügen. Beim Implementieren einer Komponente, die asynchrone Fehlerausgaben verwendet, müssen Spalten, die Downstreamkomponenten bereitgestellt werden, zur Fehlerausgabe hinzugefügt werden. Zudem muss der Ausgabepuffer für die Fehlerausgabe, die der Komponente während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>-Methode bereitgestellt wird, im Cache gespeichert werden. Die Einzelheiten zur Implementierung einer Komponente mit asynchronen Ausgängen werden im Thema [Entwickeln einer benutzerdefinierten Transformationskomponente mit asynchronen Ausgaben](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md) ausführlich behandelt. Wenn der Fehlerausgabe Spalten nicht explizit hinzugefügt werden, enthält die dem Ausgabepuffer hinzugefügte Pufferzeile nur die beiden Fehlerspalten.  
  
 Um eine Zeile an eine asynchrone Fehlerausgabe zu senden, müssen Sie eine Zeile zum Fehlerausgabepuffer hinzufügen. Es kann vorkommen, dass eine Zeile bereits zum Nichtfehlerausgabepuffer hinzugefügt wurde. In diesem Fall müssen Sie diese Zeile mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.RemoveRow%2A>-Methode entfernen. Als Nächstes legen Sie die Werte der Ausgabepufferspalten fest. Abschließend rufen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetErrorInfo%2A>-Methode auf, um den komponentenspezifischen Fehlercode und den Fehlerspaltenwert bereitzustellen.  
  
 Im folgenden Beispiel wird die Verwendung einer Fehlerausgabe für eine Komponente mit asynchronen Ausgaben dargestellt. Wenn der simulierte Fehler auftritt, fügt die Komponente eine Zeile zum Fehlerausgabepuffer hinzu, kopiert die Werte, die zuvor zum Nichtfehlerausgabepuffer hinzugefügt wurden, in den Fehlerausgabepuffer, entfernt die Zeile, die zum Nichtfehlerausgabepuffer hinzugefügt wurde, und legt abschließend den Fehlercode und die Fehlerspaltenwerte durch Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetErrorInfo%2A>-Methode fest.  
  
```csharp  
int []columnIndex;  
int errorOutputID = -1;  
int errorOutputIndex = -1;  
  
public override void PreExecute()  
{  
    IDTSOutput100 defaultOutput = null;  
  
    this.GetErrorOutputInfo(ref errorOutputID, ref errorOutputIndex);  
    foreach (IDTSOutput100 output in ComponentMetaData.OutputCollection)  
    {  
        if (output.ID != errorOutputID)  
            defaultOutput = output;  
    }  
  
    columnIndex = new int[defaultOutput.OutputColumnCollection.Count];  
  
    for(int col =0 ; col < defaultOutput.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 column = defaultOutput.OutputColumnCollection[col];  
        columnIndex[col] = BufferManager.FindColumnByLineageID(defaultOutput.Buffer, column.LineageID);  
    }  
}  
  
public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
{  
    for( int x=0; x < outputs; x++ )  
    {  
        if (outputIDs[x] == errorOutputID)  
            this.errorBuffer = buffers[x];  
        else  
            this.defaultBuffer = buffers[x];  
    }  
  
    int rows = 100;  
  
    Random random = new Random(System.DateTime.Now.Millisecond);  
  
    for (int row = 0; row < rows; row++)  
    {  
        try  
        {  
            defaultBuffer.AddRow();  
  
            for (int x = 0; x < columnIndex.Length; x++)  
                defaultBuffer[columnIndex[x]] = random.Next();  
  
            // Simulate an error.  
            if ((row % 2) == 0)  
                throw new Exception("A simulated error.");  
        }  
        catch  
        {  
            // Add a row to the error buffer.  
            errorBuffer.AddRow();  
  
            // Get the values from the default buffer  
            // and copy them to the error buffer.  
            for (int x = 0; x < columnIndex.Length; x++)  
                errorBuffer[columnIndex[x]] = defaultBuffer[columnIndex[x]];  
  
            // Set the error information.  
            errorBuffer.SetErrorInfo(errorOutputID, 1, 0);  
  
            // Remove the row that was added to the default buffer.  
            defaultBuffer.RemoveRow();  
        }  
    }  
  
    if (defaultBuffer != null)  
        defaultBuffer.SetEndOfRowset();  
  
    if (errorBuffer != null)  
        errorBuffer.SetEndOfRowset();  
}  
```  
  
```vb  
Private columnIndex As Integer()   
Private errorOutputID As Integer = -1   
Private errorOutputIndex As Integer = -1   
  
Public  Overrides Sub PreExecute()   
 Dim defaultOutput As IDTSOutput100 = Nothing   
 Me.GetErrorOutputInfo(errorOutputID, errorOutputIndex)   
 For Each output As IDTSOutput100 In ComponentMetaData.OutputCollection   
   If Not (output.ID = errorOutputID) Then   
     defaultOutput = output   
   End If   
 Next   
 columnIndex = New Integer(defaultOutput.OutputColumnCollection.Count) {}   
 Dim col As Integer = 0   
 While col < defaultOutput.OutputColumnCollection.Count   
   Dim column As IDTSOutputColumn100 = defaultOutput.OutputColumnCollection(col)   
   columnIndex(col) = BufferManager.FindColumnByLineageID(defaultOutput.Buffer, column.LineageID)   
   System.Math.Min(System.Threading.Interlocked.Increment(col),col-1)   
 End While   
End Sub   
  
Public  Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())   
 Dim x As Integer = 0   
 While x < outputs   
   If outputIDs(x) = errorOutputID Then   
     Me.errorBuffer = buffers(x)   
   Else   
     Me.defaultBuffer = buffers(x)   
   End If   
   System.Math.Min(System.Threading.Interlocked.Increment(x),x-1)   
 End While   
 Dim rows As Integer = 100   
 Dim random As Random = New Random(System.DateTime.Now.Millisecond)   
 Dim row As Integer = 0   
 While row < rows   
   Try   
     defaultBuffer.AddRow   
     Dim x As Integer = 0   
     While x < columnIndex.Length   
       defaultBuffer(columnIndex(x)) = random.Next   
       System.Math.Min(System.Threading.Interlocked.Increment(x),x-1)   
     End While   
     ' Simulate an error.  
     If (row Mod 2) = 0 Then   
       Throw New Exception("A simulated error.")   
     End If   
   Catch   
     ' Add a row to the error buffer.  
     errorBuffer.AddRow   
     ' Get the values from the default buffer  
     ' and copy them to the error buffer.  
     Dim x As Integer = 0   
     While x < columnIndex.Length   
       errorBuffer(columnIndex(x)) = defaultBuffer(columnIndex(x))   
       System.Math.Min(System.Threading.Interlocked.Increment(x),x-1)   
     End While   
     ' Set the error information.  
     errorBuffer.SetErrorInfo(errorOutputID, 1, 0)   
     ' Remove the row that was added to the default buffer.  
     defaultBuffer.RemoveRow   
   End Try   
   System.Math.Min(System.Threading.Interlocked.Increment(row),row-1)   
 End While   
 If Not (defaultBuffer Is Nothing) Then   
   defaultBuffer.SetEndOfRowset   
 End If   
 If Not (errorBuffer Is Nothing) Then   
   errorBuffer.SetEndOfRowset   
 End If   
End Sub  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Fehlerbehandlung in Daten](../../../integration-services/data-flow/error-handling-in-data.md)   
 [Using Error Outputs](../../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md) (Verwenden von Fehlerausgaben)  
  
  
