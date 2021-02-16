---
description: Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben
title: Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben | Microsoft-Dokumentation
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
- custom data flow components [Integration Services], transformation components
- ProcessInput method
- buffer allocations [Integration Services]
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- data flow components [Integration Services], transformation components
ms.assetid: b694d21f-9919-402d-9192-666c6449b0b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 87a53ff91f4ae7d6237fb3f9640420dd964b607f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348230"
---
# <a name="developing-a-custom-transformation-component-with-synchronous-outputs"></a>Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Transformationskomponenten mit synchronen Ausgaben empfangen Zeilen von Upstreamkomponenten und lesen oder modifizieren die Werte in den Spalten der betreffenden Zeilen bei der Weitergabe dieser Zeilen an Downstreamkomponenten. Sie können auch zusätzliche Ausgabespalten definieren, die sich aus den von den Upstreamkomponenten erhaltenen Spalten ableiten. Es werden dem Datenfluss jedoch keine zusätzlichen Zeilen hinzugefügt. Weitere Informationen zu den Unterschieden zwischen synchronen und asynchronen Komponenten finden Sie unter [Grundlegendes zu synchronen und asynchronen Transformationen](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Diese Komponentenart ist für Aufgaben geeignet, bei denen die Daten nach der Bereitstellung für die Komponente inline modifiziert werden und die Komponente nicht alle Zeilen erkennen muss, um Daten verarbeiten zu können. Es ist die am einfachsten zu entwickelnde Komponente, da Transformationen mit synchronen Ausgaben normalerweise keine Verbindung zu externen Datenquellen herstellen, keine externen Metadatenspalten verwalten oder Ausgabepuffern Zeilen hinzufügen.  
  
 Das Erstellen einer Transformationskomponente mit synchronen Ausgaben umfasst auch das Hinzufügen von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> mit den für die Komponente ausgewählten Upstreamspalten und eines <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>-Objekts, das von der Komponente erstellte, abgeleitete Spalten enthalten kann. Dazu gehören auch das Implementieren der Entwurfszeitmethoden und das Bereitstellen von Code, der die Spalten der eingehenden Pufferzeilen während der Ausführung liest oder modifiziert.  
  
 In diesem Abschnitt finden Sie Informationen, die zum Implementieren einer benutzerdefinierten Transformationskomponente benötigt werden, und stellt Codebeispiele für ein besseres Verständnis der Konzepte bereit.  
  
## <a name="design-time"></a>Entwurfszeit  
 Zum Entwurfszeitcode dieser Komponente zählt auch das Erstellen von Ein- und Ausgaben, das Hinzufügen der von der Komponente generierten zusätzlichen Ausgabespalten und das Überprüfen der Konfiguration dieser Komponente.  
  
### <a name="creating-the-component"></a>Erstellen der Komponente  
 Die Eingaben, Ausgaben und benutzerdefinierten Eigenschaften der Komponente werden normalerweise während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>-Methode erstellt. Es gibt zwei Möglichkeiten zum Hinzufügen der Ein- und Ausgaben einer Transformationskomponente mit synchronen Ausgaben. Sie können die Basisklassenimplementierung der Methode verwenden und die damit erstellten Standardein- und -ausgaben dann modifizieren, oder Sie können die Ein- und Ausgabe selbst explizit hinzufügen.  
  
 Im folgenden Codebeispiel wird eine Implementierung von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> gezeigt, die die Ein- und Ausgabeobjekte explizit hinzufügt. Der Aufruf der Basisklasse hätte dasselbe Ergebnis zur Folge und ist in einem Kommentar enthalten.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "SynchronousComponent", ComponentType = ComponentType.Transform)]  
    public class SyncComponent : PipelineComponent  
    {  
  
        public override void ProvideComponentProperties()  
        {  
            // Add the input.  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            // Add the output.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
            output.SynchronousInputID = input.ID;  
  
            // Alternatively, you can let the base class add the input and output  
            // and set the SynchronousInputID of the output to the ID of the input.  
            // base.ProvideComponentProperties();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="SynchronousComponent", ComponentType:=ComponentType.Transform)> _  
Public Class SyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Add the input.  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
        input.Name = "Input"  
  
        ' Add the output.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
        output.SynchronousInputID = Input.ID  
  
        ' Alternatively, you can let the base class add the input and output  
        ' and set the SynchronousInputID of the output to the ID of the input.  
        ' base.ProvideComponentProperties();  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Erstellen und Konfigurieren von Ausgabespalten  
 Auch wenn von Transformationskomponenten mit synchronen Ausgaben Puffern keine Zeilen hinzugefügt werden, können ihrer Ausgabe darüber zusätzliche Ausgabespalten hinzugefügt werden. Wenn eine Komponente eine Ausgabespalte hinzufügt, werden die Werte für die neue Ausgabespalte normalerweise zur Laufzeit von den Daten in einer oder mehrerer der Spalten abgeleitet, die von einer Upstreamkomponente bereitgestellt werden.  
  
 Nachdem eine Ausgabespalte erstellt wurde, müssen die dazugehörigen Datentypeigenschaften festgelegt werden. Das Festlegen der Datentypeigenschaften einer Ausgabespalte erfordert spezielle Schritte und wird durch das Aufrufen der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>-Methode ausgeführt. Diese Methode ist erforderlich, da die Eigenschaften <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> im Einzelnen schreibgeschützt sind und jede Eigenschaft von den Einstellungen der anderen abhängt. Diese Methode stellt sicher, dass die Werte der Eigenschaften konsistent festgelegt werden, und der Datenflusstask überprüft diese korrekte Festlegung.  
  
 Der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> der Spalte bestimmt die Werte, die für die anderen Eigenschaften festgelegt werden. Die folgende Tabelle zeigt die Anforderungen an die abhängigen Eigenschaften für jeden <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. Bei den nicht aufgelisteten Datentypen sind die abhängigen Eigenschaften auf 0 (null) festgelegt.  
  
|DataType|Länge|Skalieren|Genauigkeit|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Größer 0 und kleiner oder gleich 28|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Größer 0 und kleiner oder gleich 28 sowie kleiner als Genauigkeit|Größer oder gleich 1 und kleiner oder gleich 38|0|  
|DT_BYTES|Größer 0|0|0|0|  
|DT_STR|Größer als 0 und kleiner als 8000|0|0|Nicht 0 und eine gültige Codepage|  
|DT_WSTR|Größer 0 und kleiner 4000|0|0|0|  
  
 Da die Beschränkungen der Datentypeigenschaften vom Datentyp der Ausgabespalte abhängen, müssen Sie bei der Arbeit mit verwalteten Typen den richtigen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Datentyp wählen. Die Basisklasse stellt drei Hilfsmethoden bereit, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>, die Entwickler verwalteter Komponenten bei der Auswahl eines [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Datentyps für einen verwalteten Typ unterstützen. Diese Methoden wandeln verwaltete Datentypen in [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Datentypen um und umgekehrt.  
  
## <a name="run-time"></a>Runtime  
 Generell wird die Implementierung der Laufzeitkomponente zwei Kategorien zugeordnet. Diese sind das Suchen der Ein- und Ausgabespalten der Komponente im Puffer und das Lesen oder Schreiben dieser Spaltenwerte in den eingehenden Pufferzeilen.  
  
### <a name="locating-columns-in-the-buffer"></a>Suchen von Spalten im Puffer  
 Die Anzahl der Spalten in den Puffern, die bei der Ausführung für eine Komponente bereitgestellt werden, ist wahrscheinlich höher als die Anzahl der Spalten in den Ein- oder Ausgabeauflistungen der Komponente. Dies liegt daran, dass jeder Puffer alle Ausgabespalten enthält, die in den Komponenten in einem Datenfluss definiert werden. Komponentenentwickler müssen die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A>-Methode des <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> verwenden, um sicherzustellen, dass die Pufferspalten den Spalten für die Ein- und Ausgabe korrekt zugeordnet sind. Diese Methode lokalisiert eine Spalte im angegebenen Puffer mithilfe seiner Herkunfts-ID. Normalerweise werden Spalten während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> gesucht, da dies die erste Laufzeitmethode ist, bei der die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>-Eigenschaft verfügbar ist.  
  
 Das folgende Codebeispiel zeigt eine Komponente, die Indizes der Spalten in der Auflistung ihrer Eingabe- und Ausgabespalte während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> sucht. Die Spaltenindizes werden in einem Array mit ganzen Zahlen gespeichert, und die Komponente kann während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> darauf zugreifen.  
  
```csharp  
int []inputColumns;  
int []outputColumns;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    inputColumns = new int[input.InputColumnCollection.Count];  
    outputColumns = new int[output.OutputColumnCollection.Count];  
  
    for(int col=0; col < input.InputColumnCollection.Count; col++)  
    {  
        IDTSInputColumn100 inputColumn = input.InputColumnCollection[col];  
        inputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID);  
    }  
  
    for(int col=0; col < output.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 outputColumn = output.OutputColumnCollection[col];  
        outputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID);  
    }  
  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ReDim inputColumns(input.InputColumnCollection.Count)  
    ReDim outputColumns(output.OutputColumnCollection.Count)  
  
    For col As Integer = 0 To input.InputColumnCollection.Count  
  
        Dim inputColumn As IDTSInputColumn100 = input.InputColumnCollection(col)  
        inputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID)  
    Next  
  
    For col As Integer = 0 To output.OutputColumnCollection.Count  
  
        Dim outputColumn As IDTSOutputColumn100 = output.OutputColumnCollection(col)  
        outputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID)  
    Next  
  
End Sub  
```  
  
### <a name="processing-rows"></a>Verarbeiten von Zeilen  
 Komponenten empfangen <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>-Objekte, die Zeilen und Spalten in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>-Methode enthalten. Bei dieser Methode werden die Zeilen im Puffer durchlaufen, und die während <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> erkannten Spalten werden gelesen und modifiziert. Diese Methode wird vom Datenflusstask wiederholt aufgerufen, bis von der Upstreamkomponente keine Zeilen mehr bereitgestellt werden.  
  
 Eine einzelne Spalte im Puffer wird mithilfe der Indexerzugriffsmethode für Arrays oder mit der **Get**- bzw. **Set**-Methode gelesen oder geschrieben. Die **Get**- und **Set**-Methode sind effizienter und sollten dann verwendet werden, wenn der Datentyp der Spalte in dem Puffer bekannt ist.  
  
 Im folgenden Codebeispiel wird eine Implementierung der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>-Methode zur Verarbeitung von eingehenden Zeilen veranschaulicht.  
  
```csharp  
public override void ProcessInput( int InputID, PipelineBuffer buffer)  
{  
       while( buffer.NextRow())  
       {  
            for(int x=0; x < inputColumns.Length;x++)  
            {  
                if(!buffer.IsNull(inputColumns[x]))  
                {  
                    object columnData = buffer[inputColumns[x]];  
                    // TODO: Modify the column data.  
                    buffer[inputColumns[x]] = columnData;  
                }  
            }  
  
      }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal InputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For x As Integer = 0 To inputColumns.Length  
  
                if buffer.IsNull(inputColumns(x)) = false then  
  
                    Dim columnData As Object = buffer(inputColumns(x))  
                    ' TODO: Modify the column data.  
                    buffer(inputColumns(x)) = columnData  
  
                End If  
            Next  
  
        End While  
End Sub  
```  
  
## <a name="sample"></a>Beispiel  
 Im folgenden Beispiel wird eine einfache Transformationskomponente mit synchronen Ausgaben gezeigt, die die Werte aller Zeichenfolgenzeilen in Großbuchstaben umwandelt. In diesem Beispiel werden nicht alle Methoden und Funktionen dargestellt, die in diesem Thema erläutert wurden. Es veranschaulicht die wichtigen Methoden, die jede benutzerdefinierte Transformationskomponente mit synchronen Ausgaben überschreiben muss, enthält jedoch keinen Code für eine Überprüfung zur Entwurfszeit.  
  
```csharp  
using System;  
using System.Collections;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Uppercase  
{  
  [DtsPipelineComponent(DisplayName = "Uppercase")]  
  public class Uppercase : PipelineComponent  
  {  
    ArrayList m_ColumnIndexList = new ArrayList();  
  
    public override void ProvideComponentProperties()  
    {  
      base.ProvideComponentProperties();  
      ComponentMetaData.InputCollection[0].Name = "Uppercase Input";  
      ComponentMetaData.OutputCollection[0].Name = "Uppercase Output";  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        if (column.DataType == DataType.DT_STR || column.DataType == DataType.DT_WSTR)  
        {  
          m_ColumnIndexList.Add((int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID));  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        foreach (int columnIndex in m_ColumnIndexList)  
        {  
          string str = buffer.GetString(columnIndex);  
          buffer.SetString(columnIndex, str.ToUpper());  
        }  
      }  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.Collections   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace Uppercase   
  
 <DtsPipelineComponent(DisplayName="Uppercase")> _   
 Public Class Uppercase   
 Inherits PipelineComponent   
   Private m_ColumnIndexList As ArrayList = New ArrayList   
  
   Public  Overrides Sub ProvideComponentProperties()   
     MyBase.ProvideComponentProperties   
     ComponentMetaData.InputCollection(0).Name = "Uppercase Input"   
     ComponentMetaData.OutputCollection(0).Name = "Uppercase Output"   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     For Each column As IDTSInputColumn100 In inputColumns   
       If column.DataType = DataType.DT_STR OrElse column.DataType = DataType.DT_WSTR Then   
         m_ColumnIndexList.Add(CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer))   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       For Each columnIndex As Integer In m_ColumnIndexList   
         Dim str As String = buffer.GetString(columnIndex)   
         buffer.SetString(columnIndex, str.ToUpper)   
       Next   
     End While   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln einer benutzerdefinierten Transformationskomponente mit asynchronen Ausgaben](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)   
 [Grundlegendes zu synchronen und asynchronen Transformationen](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
