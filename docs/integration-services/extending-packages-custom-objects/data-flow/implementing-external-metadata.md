---
description: Implementieren externer Metadaten
title: Implementieren externer Metadaten | Microsoft-Dokumentation
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
- disconnected validation [Integration Services]
- connected validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- metadata [Integration Services]
- data flow components [Integration Services], validating
- data flow components [Integration Services], external metadata
- custom data flow components [Integration Services], external metadata
- external metadata [Integration Services]
ms.assetid: 8f5bd3ed-3e79-43a4-b6c1-435e4c2cc8cc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9b24d33156162028fdef7e0806a5ac2f8d9f0ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343675"
---
# <a name="implementing-external-metadata"></a>Implementieren externer Metadaten

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn die Verbindung einer Komponente mit einer Datenquelle getrennt ist, können Sie mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100>-Schnittstelle die Spalten in den Eingabe- und Ausgabespaltenauflistungen anhand der Spalten der externen Datenquelle überprüfen. Mit dieser Schnittstelle können Sie eine Momentaufnahme der Spalten der externen Datenquelle verwalten und diese Spalten den Eingabe- und Ausgabespaltenauflistungen der Komponente zuordnen.  
  
 Die Implementierung externer Metadatenspalten erhöht den Verwaltungsaufwand und die Komplexität bei der Komponentenentwicklung, da Sie eine weitere Spaltenauflistung verwalten und überprüfen müssen. Diese Entwicklungsarbeit kann sich aber lohnen, da Sie teure Roundtrips zum Server zu Überprüfungszwecken vermeiden können.  
  
## <a name="populating-external-metadata-columns"></a>Auffüllen von externen Metadatenspalten  
 Externe Metadatenspalten werden der Auflistung meist bei der Erstellung der entsprechenden Eingabe- oder Ausgabespalten hinzugefügt. Neue Spalten werden durch Aufrufen der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.New%2A>-Methode erstellt. Die Eigenschaften der Spalte werden anschließend entsprechend der externen Datenquelle festgelegt.  
  
 Die externe Metadatenspalte wird der entsprechenden Eingabe- oder Ausgabespalte durch Zuweisen der ID der externen Metadatenspalte zur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A>-Eigenschaft der Eingabe- oder Ausgabespalte zugeordnet. So können Sie die externe Metadatenspalte für eine spezifische Eingabe- oder Ausgabespalte leicht mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.GetObjectByID%2A>-Methode der Auflistung finden.  
  
 Im folgenden Beispiel werden die Erstellung einer externen Metadatenspalte und die anschließende Zuordnung der Spalte zu einer Ausgabespalte durch Festlegen der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A>-Eigenschaft veranschaulicht.  
  
```csharp  
public void CreateExternalMetaDataColumn(IDTSOutput100 output, int outputColumnID )  
{  
    IDTSOutputColumn100 oColumn = output.OutputColumnCollection.GetObjectByID(outputColumnID);  
    IDTSExternalMetadataColumn100 eColumn = output.ExternalMetadataColumnCollection.New();  
  
    eColumn.DataType = oColumn.DataType;  
    eColumn.Precision = oColumn.Precision;  
    eColumn.Scale = oColumn.Scale;  
    eColumn.Length = oColumn.Length;  
    eColumn.CodePage = oColumn.CodePage;  
  
    oColumn.ExternalMetadataColumnID = eColumn.ID;  
}  
```  
  
```vb  
Public Sub CreateExternalMetaDataColumn(ByVal output As IDTSOutput100, ByVal outputColumnID As Integer)   
 Dim oColumn As IDTSOutputColumn100 = output.OutputColumnCollection.GetObjectByID(outputColumnID)   
 Dim eColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New   
 eColumn.DataType = oColumn.DataType   
 eColumn.Precision = oColumn.Precision   
 eColumn.Scale = oColumn.Scale   
 eColumn.Length = oColumn.Length   
 eColumn.CodePage = oColumn.CodePage   
 oColumn.ExternalMetadataColumnID = eColumn.ID   
End Sub  
```  
  
## <a name="validating-with-external-metadata-columns"></a>Überprüfen anhand externer Metadatenspalten  
 Die Überprüfung erfordert weitere Schritte für Komponenten, die eine externe Metadatenspaltenauflistung verwalten, denn Sie müssen die Überprüfung anhand einer weiteren Spaltenauflistung vornehmen. Bei der Überprüfung kann zwischen einer verbundenen und einer getrennten Überprüfung unterschieden werden.  
  
### <a name="connected-validation"></a>Verbundene Überprüfung  
 Besteht eine Verbindung einer Komponente mit einer externen Datenquelle, werden die Spalten in den Eingabe- oder Ausgabeauflistungen direkt anhand der externen Datenquelle überprüft. Darüber hinaus müssen die Spalten in der externen Metadatensammlung überprüft werden. Dies ist erforderlich, da die externe Metadatensammlung unter **Erweiterter Editor** in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] verändert werden kann, und Änderungen, die an der Sammlung vorgenommen werden, nicht erkennbar sind. Daher müssen die Komponenten, wenn eine Verbindung besteht, sicherstellen, dass die Spalten in der externen Metadatensammlung weiterhin den Spalten der externen Datenquelle entsprechen.  
  
 Sie können unter **Erweiterter Editor** die externe Metadatensammlung ausblenden, indem Sie für die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.IsUsed%2A>-Eigenschaft der Sammlung den Wert **false** festlegen. Dadurch wird jedoch auch die Registerkarte **Spaltenzuordnung** des Editors ausgeblendet, über die Benutzer Spalten der Eingabe- und Ausgabeauflistungen den Spalten in der externen Metadatenspaltenauflistung zuordnen können. Das Festlegen dieser Eigenschaft auf den Wert **false** verhindert nicht, dass Entwickler die Auflistung programmgesteuert verändern, bietet jedoch Schutz für die externe Metadatenspaltenauflistung einer Komponente, die ausschließlich in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] verwendet wird.  
  
### <a name="disconnected-validation"></a>Getrennte Überprüfung  
 Ist die Verbindung einer Komponente mit einer externen Datenquelle getrennt, ist die Überprüfung einfacher, da die Spalten in der Eingabe- oder Ausgabeauflistung direkt anhand der Spalten der externen Metadatensammlung und nicht anhand der externen Datenquelle überprüft werden. Eine Komponente sollte eine getrennte Überprüfung ausführen, wenn keine Verbindung mit der externen Datenquelle besteht oder die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A>-Eigenschaft den Wert **false** aufweist.  
  
 Im folgenden Codebeispiel wird die Implementierung einer Komponente, die eine Überprüfung anhand der externen Metadatenspaltenauflistung vornimmt, gezeigt.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    if( this.isConnected && ComponentMetaData.ValidateExternalMetaData )  
    {  
        // TODO: Perform connected validation.  
    }  
    else  
    {  
        // TODO: Perform disconnected validation.  
    }  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
 If Me.isConnected AndAlso ComponentMetaData.ValidateExternalMetaData Then   
  ' TODO: Perform connected validation.  
 Else   
  ' TODO: Perform disconnected validation.  
 End If   
End Function  
```  

## <a name="see-also"></a>Siehe auch  
 [Datenfluss](../../../integration-services/data-flow/data-flow.md)  
  
  
