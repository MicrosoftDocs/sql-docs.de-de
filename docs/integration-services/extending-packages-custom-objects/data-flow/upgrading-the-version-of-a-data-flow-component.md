---
description: Aktualisieren der Version einer Datenflusskomponente
title: Aktualisieren der Version einer Datenflusskomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- PerformUpgrade method
- custom data flow components [Integration Services], upgrading version
- data flow components [Integration Services], upgrading version
- upgrading data flow components [Integration Services]
ms.assetid: c2a298c6-01b3-4ad1-884d-6108165eb56e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c544070aba868f257e20e9028893a89172c0726
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343625"
---
# <a name="upgrading-the-version-of-a-data-flow-component"></a>Aktualisieren der Version einer Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Pakete, die mit einer älteren Version der Komponente erstellt wurden, enthalten möglicherweise Metadaten, die nicht mehr gültig sind, beispielsweise benutzerdefinierte Eigenschaften, deren Verwendung in neueren Versionen der Komponenten geändert wurde. Sie können die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>-Basisklasse überschreiben, um die zuvor in älteren Paketen gespeicherten Metadaten zu aktualisieren, sodass die aktuellen Eigenschaften der Komponente wiedergegeben werden.  
  
> [!NOTE]  
>  Wenn Sie eine benutzerdefinierte Komponente für eine neue Version von [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] neu kompilieren, müssen Sie den Wert der <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.CurrentVersion%2A>-Eigenschaft nicht ändern, wenn sich die Eigenschaften der Komponente nicht geändert haben.  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel enthält Code aus Version 2.0 einer fiktiven Datenflusskomponente. Die neue Versionsnummer wird in der <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.CurrentVersion%2A>-Eigenschaft von <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> definiert. Die Komponente weist eine Eigenschaft auf, durch die definiert wird, wie numerische Werte behandelt werden, die einen bestimmten Schwellenwert überschreiten. In Version 1.0 der fiktiven Komponente hieß diese Eigenschaft `RaiseErrorOnInvalidValue`, und es war ein boolescher Wert von True oder False zulässig. In Version 2.0 der fiktiven Komponente wurde die Eigenschaft in `InvalidValueHandling` umbenannt. Es ist nun einer von vier möglichen Werten aus einer benutzerdefinierten Enumeration zulässig.  
  
 Von der überschriebenen <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A>-Methode im folgenden Beispiel werden die folgenden Aktionen ausgeführt:  
  
-   Ruft die aktuelle Version der Komponente ab.  
  
-   Ruft den Wert der alten benutzerdefinierten Eigenschaft ab.  
  
-   Entfernt die alte Eigenschaft aus der Auflistung mit den benutzerdefinierten Eigenschaften.  
  
-   Legt, wenn möglich, den Wert der neuen benutzerdefinierten Eigenschaft basierend auf dem Wert der alten Eigenschaft fest.  
  
-   Legt die Versionsmetadaten auf die aktuelle Version der Komponente fest  
  
> [!NOTE]  
>  Die Datenfluss-Engine übergibt seine eigene Versionsnummer an die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A>-Methode im *pipelineVersion*-Parameter. Dieser Parameter ist in Version 1.0 von [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] nicht von Nutzen, kann aber in nachfolgenden Versionen hilfreich sein.  
  
 Im Beispielcode werden nur die beiden Enumerationswerte verwendet, die direkt den vorherigen booleschen Werten der benutzerdefinierten Eigenschaft zugeordnet sind. Die anderen verfügbaren Enumerationswerte können vom Benutzer über die benutzerdefinierte Benutzeroberfläche der Komponente im erweiterten Editor oder programmgesteuert ausgewählt werden. Informationen zum Anzeigen von Enumerationswerten für eine benutzerdefinierte Eigenschaft im erweiterten Editor finden Sie in [Entwurfszeitmethoden einer Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md) unter „Erstellen von benutzerdefinierten Eigenschaften“.  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
<DtsPipelineComponent(ComponentType:=ComponentType.Transform, CurrentVersion:=2)> _  
Public Class PerformUpgrade  
  Inherits PipelineComponent  
  
  ' Define the set of possible values for the new custom property.  
  Private Enum InvalidValueHandling  
    Ignore  
    FireInformation  
    FireWarning  
    FireError  
  End Enum  
  
  Public Overloads Overrides Sub PerformUpgrade(ByVal pipelineVersion As Integer)  
  
    ' Obtain the current component version from the attribute.  
    Dim componentAttribute As DtsPipelineComponentAttribute = _  
      CType(Attribute.GetCustomAttribute(Me.GetType, _  
      GetType(DtsPipelineComponentAttribute), False), _  
      DtsPipelineComponentAttribute)  
    Dim currentVersion As Integer = componentAttribute.CurrentVersion  
  
    ' If the component version saved in the package is less than  
    '  the current version, Version 2, perform the upgrade.  
    If ComponentMetaData.Version < currentVersion Then  
  
      ' Get the current value of the old custom property, RaiseErrorOnInvalidValue,   
      ' and then remove the property from the custom property collection.  
      Dim oldValue As Boolean = False  
      Try  
        Dim oldProperty As IDTSCustomProperty100 = _  
          ComponentMetaData.CustomPropertyCollection("RaiseErrorOnInvalidValue")  
        oldValue = CType(oldProperty.Value, Boolean)  
        ComponentMetaData.CustomPropertyCollection.RemoveObjectByIndex("RaiseErrorOnInvalidValue")  
      Catch ex As Exception  
        ' If the old custom property is not available, ignore the error.  
      End Try  
  
      ' Set the value of the new custom property, InvalidValueHandling,  
      '  by using the appropriate enumeration value.  
      Dim newProperty As IDTSCustomProperty100 = _  
        ComponentMetaData.CustomPropertyCollection("InvalidValueHandling")  
      If oldValue = True Then  
        newProperty.Value = InvalidValueHandling.FireError  
      Else  
        newProperty.Value = InvalidValueHandling.Ignore  
      End If  
  
    End If  
  
    ' Update the saved component version metadata to the current version.  
    ComponentMetaData.Version = currentVersion  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
[DtsPipelineComponent(ComponentType = ComponentType.Transform, CurrentVersion = 2)]  
public class PerformUpgradeCS :  
  PipelineComponent  
  
  // Define the set of possible values for the new custom property.  
{  
  private enum InvalidValueHandling  
  {  
    Ignore,  
    FireInformation,  
    FireWarning,  
    FireError  
  };  
  
  public override void PerformUpgrade(int pipelineVersion)  
  {  
  
    // Obtain the current component version from the attribute.  
    DtsPipelineComponentAttribute componentAttribute =   
      (DtsPipelineComponentAttribute)Attribute.GetCustomAttribute(this.GetType(), typeof(DtsPipelineComponentAttribute), false);  
    int currentVersion = componentAttribute.CurrentVersion;  
  
    // If the component version saved in the package is less than  
    //  the current version, Version 2, perform the upgrade.  
    if (ComponentMetaData.Version < currentVersion)  
  
    // Get the current value of the old custom property, RaiseErrorOnInvalidValue,   
    // and then remove the property from the custom property collection.  
    {  
      bool oldValue = false;  
      try  
      {  
        IDTSCustomProperty100 oldProperty =   
          ComponentMetaData.CustomPropertyCollection["RaiseErrorOnInvalidValue"];  
        oldValue = (bool)oldProperty.Value;  
        ComponentMetaData.CustomPropertyCollection.RemoveObjectByIndex("RaiseErrorOnInvalidValue");  
      }  
      catch (Exception ex)  
      {  
        // If the old custom property is not available, ignore the error.  
      }  
  
      // Set the value of the new custom property, InvalidValueHandling,  
      //  by using the appropriate enumeration value.  
      IDTSCustomProperty100 newProperty =   
         ComponentMetaData.CustomPropertyCollection["InvalidValueHandling"];  
      if (oldValue == true)  
      {  
        newProperty.Value = InvalidValueHandling.FireError;  
      }  
      else  
      {  
        newProperty.Value = InvalidValueHandling.Ignore;  
      }  
  
    }  
  
    // Update the saved component version metadata to the current version.  
    ComponentMetaData.Version = currentVersion;  
  
  }  
  
}  
```
