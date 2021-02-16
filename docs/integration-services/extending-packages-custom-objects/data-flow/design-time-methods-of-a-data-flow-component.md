---
description: Entwurfszeitmethoden einer Datenflusskomponente
title: Entwurfszeitmethoden einer Datenflusskomponente | Microsoft-Dokumentation
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
- custom data flow components [Integration Services], method execution sequence
- ProcessInput method
- method execution sequence [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], method execution sequence
ms.assetid: b5a121a1-b87c-441b-a42c-2cec628dc81c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 44a6aa6eb512307f8a4c99c15137f246ea7ec8cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348914"
---
# <a name="design-time-methods-of-a-data-flow-component"></a>Entwurfszeitmethoden einer Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Vor der Ausführung befindet sich der Datenflusstask im so genannten Entwurfszeitstatus, während er inkrementelle Änderungen durchläuft. Zu den Änderungen kann das Hinzufügen oder Entfernen von Komponenten, das Hinzufügen oder Entfernen von Pfadobjekten zur Verbindung von Komponenten sowie Änderungen an den Metadaten der Komponenten gehören. Wenn Metadaten-Änderungen auftreten, kann die Komponente die Änderungen überwachen und darauf reagieren. Zum Beispiel kann eine Komponente bestimmte Änderungen nicht zulassen oder zusätzliche Änderungen als Reaktion auf eine Änderung vornehmen. Zur Entwurfszeit interagiert der Designer mit einer Komponente durch die Entwurfszeitschnittstelle <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>.  
  
## <a name="design-time-implementation"></a>Entwurfszeitimplementierung  
 Die Entwurfszeitschnittstelle einer Komponente wird von der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>-Schnittstelle beschrieben. Auch wenn Sie diese Schnittstelle nicht explizit implementieren, sollten Sie die in der Schnittstelle definierten Methoden kennen, um zu verstehen, welche Methoden der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>-Basisklasse den Entwurfszeitinstanzen einer Komponente entsprechen.  
  
 Wenn eine Komponente in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] geladen wird, wird die Entwurfszeitinstanz der Komponente instanziiert und die Methoden der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>-Schnittstelle beim Bearbeiten der Komponente aufgerufen. Die Implementierung der Basisklasse lässt Sie nur die Methoden, die die Komponente erfordert, überschreiben. In vielen Fällen überschreiben Sie diese Methoden um zu verhindern, dass an einer Komponente unerwünschte Bearbeitungen vorgenommen werden. Um beispielsweise Benutzer zu hindern, eine Ausgabe zu einer Komponente hinzuzufügen, überschreiben Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.InsertOutput%2A>-Methode. Andernfalls wird beim Aufrufen der Implementierung dieser Methode durch die Basisklasse eine Ausgabe zur Komponente hinzugefügt  
  
 Unabhängig vom Zweck oder der Funktion Ihrer Komponente sollten Sie die Methoden <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> überschreiben. Weitere Informationen zu <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> finden Sie unter [Überprüfen einer Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md).  
  
## <a name="providecomponentproperties-method"></a>ProvideComponentProperties-Methode  
 Die Initialisierung einer Komponente erfolgt in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>-Methode. Diese Methode wird vom [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer aufgerufen, wenn eine Komponente zum Datenflusstask hinzugefügt wird, und ist ähnlich wie ein Klassenkonstruktor. Komponentenentwickler sollten ihre Eingaben, Ausgaben und benutzerdefinierten Eigenschaften während dieses Methodenaufrufs initialisieren und erstellen. Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>-Methode unterscheidet sich von einem Konstruktor, da sie nicht jedes Mal aufgerufen wird, wenn die Entwurfszeitinstanz oder Laufzeitinstanz der Komponenten instanziiert wird.  
  
 Die Basisklassenimplementierung der Methode fügt eine Eingabe und eine Ausgabe zur Komponente hinzu und weist die ID der Eingabe der Eigenschaft <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> zu. In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] werden jedoch die Eingabe- und Ausgabeobjekte, die von der Basisklasse hinzugefügt werden, nicht benannt. Pakete, die eine Komponente mit Eingabe- oder Ausgabeobjekten enthalten, deren Name-Eigenschaft nicht festgelegt ist, werden nicht erfolgreich geladen. Bei Verwendung der Basisimplementierung müssen Sie also der Name-Eigenschaft der Standardeingabe und -ausgabe explizit Werte zuweisen.  
  
```csharp  
public override void ProvideComponentProperties()  
{  
    /// TODO: Reset the component.  
    /// TODO: Add custom properties.  
    /// TODO: Add input objects.  
    /// TODO: Add output objects.  
}  
```  
  
```vb  
Public Overrides Sub ProvideComponentProperties()  
    ' TODO: Reset the component.  
    ' TODO: Add custom properties.  
    ' TODO: Add input objects.  
    ' TODO: Add output objects.  
End Sub  
```  
  
### <a name="creating-custom-properties"></a>Erstellen von benutzerdefinierten Eigenschaften  
 Komponentenentwickler können beim Aufrufen der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>-Methode der Komponente benutzerdefinierte Eigenschaften (<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100>) hinzufügen. Benutzerdefinierte Eigenschaften verfügen nicht über eine Datentypeigenschaft. Der Datentyp einer benutzerdefinierten Eigenschaft wird durch den Datentyp des Werts bestimmt, den Sie der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.Value%2A>-Eigenschaft zuweisen. Nachdem Sie der benutzerdefinierten Eigenschaft jedoch einen Anfangswert zugeordnet haben, können Sie keinen Wert mit einem anderen Datentyp zuweisen.  
  
> [!NOTE]  
>  Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100>-Schnittstelle bietet eingeschränkte Unterstützung für Eigenschaftenwerte des Typs **Object**. Das einzige Objekt, das Sie als Wert einer benutzerdefinierten Eigenschaft zuordnen können, ist ein Array einfacher Typen wie Zeichenfolgen oder Ganzzahlen.  
  
 Sie können festlegen, dass die benutzerdefinierte Eigenschaft Eigenschaftsausdrücke unterstützt, indem Sie den Wert der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.ExpressionType%2A>-Eigenschaft auf **CPET_NOTIFY** aus der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSCustomPropertyExpressionType>-Enumeration einstellen, wie im folgenden Beispiel gezeigt. Sie müssen keinen Code hinzufügen, um den vom Benutzer eingegebenen Eigenschaftsausdruck zu verarbeiten oder zu überprüfen. Sie können einen Standardwert für die Eigenschaft festlegen, den Wert überprüfen und den Wert normal lesen und verwenden.  
  
```csharp  
IDTSCustomProperty100 myCustomProperty;  
...  
myCustomProperty.ExpressionType = DTSCustomPropertyExpressionType.CPET_NOTIFY;  
```  
  
```vb  
Dim myCustomProperty As IDTSCustomProperty100  
...  
myCustomProperty.ExpressionType = DTSCustomPropertyExpressionType.CPET_NOTIFY  
```  
  
 Sie können festlegen, dass Benutzer einen benutzerdefinierten Eigenschaftswert aus einer Enumeration auswählen können, indem Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A>-Eigenschaft verwenden, wie im folgenden Beispiel gezeigt. Dabei gilt die Annahme, dass Sie eine öffentliche Enumeration namens **MyValidValues** definiert haben.  
  
```csharp  
IDTSCustomProperty100 customProperty = outputColumn.CustomPropertyCollection.New();  
customProperty.Name = "My Custom Property";  
// This line associates the type with the custom property.  
customProperty.TypeConverter = typeof(MyValidValues).AssemblyQualifiedName;  
// Now you can use the enumeration values directly.  
customProperty.Value = MyValidValues.ValueOne;    
```  
  
```vb  
Dim customProperty As IDTSCustomProperty100 = outputColumn.CustomPropertyCollection.New   
customProperty.Name = "My Custom Property"   
' This line associates the type with the custom property.  
customProperty.TypeConverter = GetType(MyValidValues).AssemblyQualifiedName   
' Now you can use the enumeration values directly.  
customProperty.Value = MyValidValues.ValueOne  
```  
  
 Weitere Informationen finden Sie unter „Verallgemeinerte Typkonvertierung“ und „Implementieren eines Typkonverters“ in der [MSDN Library](../../../sql-server/index.yml).  
  
 Sie können ein benutzerdefiniertes Bearbeitungsdialogfeld für den Wert der benutzerdefinierten Eigenschaft angeben, indem Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A>-Eigenschaft wie im folgenden Beispiel gezeigt verwenden. Erstellen Sie zuerst einen benutzerdefinierten Typeditor, der von **System.Drawing.Design.UITypeEditor** erbt, falls Sie keine geeignete vorhandene Typ-Editorklasse für die Benutzeroberfläche finden.  
  
```csharp  
public class MyCustomTypeEditor : UITypeEditor  
{  
...  
}  
```  
  
```vb  
Public Class MyCustomTypeEditor  
  Inherits UITypeEditor   
  ...  
End Class  
```  
  
 Geben Sie danach diese Klasse als Wert der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A>-Eigenschaft der benutzerdefinierten Eigenschaft an.  
  
```csharp  
IDTSCustomProperty100 customProperty = outputColumn.CustomPropertyCollection.New();  
customProperty.Name = "My Custom Property";  
// This line associates the editor with the custom property.  
customProperty.UITypeEditor = typeof(MyCustomTypeEditor).AssemblyQualifiedName;  
```  
  
```vb  
Dim customProperty As IDTSCustomProperty100 = outputColumn.CustomPropertyCollection.New   
customProperty.Name = "My Custom Property"   
' This line associates the editor with the custom property.  
customProperty.UITypeEditor = GetType(MyCustomTypeEditor).AssemblyQualifiedName  
```  
  
 Weitere Informationen finden Sie unter „Implementieren eines Typ-Editors für die Benutzeroberfläche“ in der [MSDN Library](../../../sql-server/index.yml).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Laufzeitmethoden einer Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/run-time-methods-of-a-data-flow-component.md)  
  
