---
description: Überprüfen einer Datenflusskomponente
title: Überprüfen einer Datenflusskomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- ReinitializeMetaData method
- Validate method
- component validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- data flow components [Integration Services], validating
- validation [Integration Services]
ms.assetid: 1a7d5925-b387-4e31-af7f-c7f3c5151040
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fc86f379cdbd324c47adcbdb9b610399ca2f28af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354379"
---
# <a name="validating-a-data-flow-component"></a>Überprüfen einer Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>-Basisklasse wird bereitgestellt, um die Ausführung einer Komponente zu verhindern, die nicht korrekt konfiguriert wurde. Verwenden Sie diese Methode, wenn Sie überprüfen wollen, ob eine Komponente die erwartete Anzahl an Eingabe- und Ausgabeobjekten aufweist, die Werte der benutzerdefinierten Eigenschaften der Komponente zulässig sind, und Verbindungen, falls erforderlich, angegeben sind. Sie können diese Methode auch verwenden, um zu überprüfen, ob die Spalten in der Eingabe- und Ausgabeauflistung die korrekten Datentypen enthalten und der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType> jeder Spalte ordnungsgemäß für die Komponente festgelegt wurde. Durch die Basisklassenimplementierung wird der Überprüfungsprozess unterstützt, indem die Eingabespaltenauflistung der Komponente überprüft und sichergestellt wird, dass jede Spalte in der Auflistung auf eine Spalte in <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputCollection100> der Upstreamkomponente verweist.  
  
## <a name="validate-method"></a>Validate-Methode  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>-Methode wird wiederholt aufgerufen, wenn eine Komponente im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer bearbeitet wird. Sie können an den Designer und die Benutzer der Komponente über den <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus>-Enumerationsrückgabewert und durch die Anzeige von Warnungen und Fehlern ein Feedback bereitstellen. Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus>-Enumeration enthält drei Werte, die verschiedene Fehlerstadien anzeigen, und einen vierten <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISVALID>-Wert, der angibt, ob die Komponente ordnungsgemäß konfiguriert und bereit zur Ausführung ist.  
  
 Der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA>-Wert zeigt an, dass ein Fehler in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> besteht, und dass die Komponente die Fehler beheben kann. Wenn eine Komponente einen Metadatenfehler feststellt, den sie beheben kann, sollte sie den Fehler nicht in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>-Methode beheben und <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> sollte bei der Überprüfung nicht geändert werden. Stattdessen sollte die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>-Methode nur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> zurückgeben, und die Komponente sollte den Fehler in einem Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>-Methode, wie weiter unten in diesem Abschnitt beschrieben, beheben.  
  
 Der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISBROKEN>-Wert zeigt an, dass die Komponente einen Fehler aufweist, der durch Bearbeitung der Komponente im Designer behoben werden kann. Der Fehler wird typischerweise durch eine benutzerdefinierte Eigenschaft oder eine erforderliche Verbindung verursacht, die nicht angegeben oder nicht ordnungsgemäß eingerichtet wurde.  
  
 Der endgültige Fehlerwert ist <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISCORRUPT>, wodurch angezeigt wird, dass die Komponente Fehler gefunden hat, die nur auftreten dürften, wenn die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>-Eigenschaft entweder durch Bearbeiten des Paket-XML oder durch Verwenden des Objektmodells direkt geändert wurde. Dieser Fehlertyp tritt z. B. auf, wenn eine Komponente nur eine einzelne Eingabe hinzugefügt hat, aber bei der Überprüfung festgestellt wird, dass mehr als eine Eingabe in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> vorhanden ist. Fehler, die diesen Rückgabewert generieren, können nur behoben werden, indem die Komponente durch Verwenden der Schaltfläche **Zurücksetzen** im Dialogfeld **Erweiterter Editor** zurückgesetzt wird.  
  
 Neben der Rückgabe von Fehlerwerten stellen Komponenten Feedback bereit, indem sie während der Überprüfung Warnungen und Fehler anzeigen. Dieser Mechanismus wird durch die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A>- und <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>-Methoden bereitgestellt. Wenn diese Methoden aufgerufen werden, werden diese Ereignisse im Fenster **Fehlerliste** von [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] angezeigt. Komponentenentwickler können dann für die Benutzer direktes Feedback über die aufgetretenen Fehler bereitstellen bzw. erläutern, wie diese behoben werden können.  
  
 Im folgenden Codebeispiel wird eine überschriebene Implementierung von <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> veranschaulicht.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    bool pbCancel = false;  
  
    // Validate that there is one input.  
    if (ComponentMetaData.InputCollection.Count != 1)  
    {  
    ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, out pbCancel);  
    return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Validate that the UserName custom property is set.  
    if (ComponentMetaData.CustomPropertyCollection["UserName"].Value == null || ((string)ComponentMetaData.CustomPropertyCollection["UserName"].Value).Length == 0)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISBROKEN;  
    }  
  
    // Validate that there is one output.  
    if (ComponentMetaData.OutputCollection.Count != 1)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Let the base class verify that the input column reflects the output   
    // of the upstream component.  
    return base.Validate();  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
  
 Dim pbCancel As Boolean = False  
  
 ' Validate that there is one input.  
 If Not (ComponentMetaData.InputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Validate that the UserName custom property is set.  
 If ComponentMetaData.CustomPropertyCollection("UserName").Value Is Nothing OrElse CType(ComponentMetaData.CustomPropertyCollection("UserName").Value, String).Length = 0 Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISBROKEN   
 End If   
  
 ' Validate that there is one output.  
 If Not (ComponentMetaData.OutputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Let the base class verify that the input column reflects the output   
 ' of the upstream component.  
  
 Return MyBase.Validate   
  
End Function  
```  
  
## <a name="reinitializemetadata-method"></a>ReinitializeMetaData-Methode  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>-Methode wird vom [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer aufgerufen, wenn Sie eine Komponente bearbeiten, die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> von der zugehörigen <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>-Methode zurückgibt. Komponenten sollten Code enthalten, der die Probleme erkennt und behebt, die während der Überprüfung von der Komponente identifiziert wurden.  
  
 Im folgenden Beispiel wird eine Komponente gezeigt, die während der Überprüfung Probleme feststellt und diese mittels <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>-Methode behebt.  
  
```csharp  
private bool areInputColumnsValid = true;  
public override DTSValidationStatus Validate()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
    bool Cancel = false;  
    foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
    {  
        try  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
        }  
        catch  
        {  
            ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, out Cancel);  
            areInputColumnsValid = false;  
            return DTSValidationStatus.VS_NEEDSNEWMETADATA;  
        }  
    }  
  
    return DTSValidationStatus.VS_ISVALID;  
}  
public override void ReinitializeMetaData()  
{  
    if (!areInputColumnsValid)  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection[0];  
        IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
        foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
  
            if (vColumn == null)  
                input.InputColumnCollection.RemoveObjectByID(column.ID);  
        }  
        areInputColumnsValid = true;  
    }  
}  
```  
  
```vb  
Private areInputColumnsValid As Boolean = True   
  
Public  Overrides Function Validate() As DTSValidationStatus   
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
 Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
 Dim Cancel As Boolean = False   
 For Each column As IDTSInputColumn100 In input.InputColumnCollection   
   Try   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
   Catch   
     ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, Cancel)   
     areInputColumnsValid = False   
     Return DTSValidationStatus.VS_NEEDSNEWMETADATA   
   End Try   
 Next   
 Return DTSValidationStatus.VS_ISVALID   
End Function   
  
Public  Overrides Sub ReinitializeMetaData()   
 If Not areInputColumnsValid Then   
   Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
   Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
   For Each column As IDTSInputColumn100 In input.InputColumnCollection   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
     If vColumn Is Nothing Then   
       input.InputColumnCollection.RemoveObjectByID(column.ID)   
     End If   
   Next   
   areInputColumnsValid = True   
 End If   
End Sub  
```  
  
