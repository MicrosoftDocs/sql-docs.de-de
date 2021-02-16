---
description: Auslösen und Definieren von Ereignissen in einer Datenflusskomponente
title: Auslösen und Definieren von Ereignissen in einer Datenflusskomponente | Microsoft-Dokumentation
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
- data flow components [Integration Services], events
- events [Integration Services], custom
- custom events [Integration Services]
- custom data flow components [Integration Services], events
- events [Integration Services], raising
- predefined events [Integration Services]
ms.assetid: 1d8c5358-9384-47a8-b7cb-7b0650384119
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d6e0a5d7694323f22a9262df37353f8117b9666
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343665"
---
# <a name="raising-and-defining-events-in-a-data-flow-component"></a>Auslösen und Definieren von Ereignissen in einer Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Komponentenentwickler können eine Teilmenge der Ereignisse, die in der <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>-Schnittstelle definiert sind, auslösen, indem sie die in der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>-Eigenschaft verfügbaren Methoden aufrufen. Außerdem können Sie benutzerdefinierte Ereignisse mit der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A>-Auflistung definieren und sie während der Ausführung mit der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>-Methode auslösen. In diesem Abschnitt wird beschrieben, wie Sie ein Ereignis erstellen und auslösen können und es werden Richtlinien bereitgestellt, wann Ereignisse zur Entwurfszeit ausgelöst werden sollen.  
  
## <a name="raising-events"></a>Auslösen von Ereignissen  
 Komponenten lösen Ereignisse mit den **Fire\<X>** -Methoden der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle aus. Sie können während des Entwurfs und der Ausführung von Komponenten Ereignisse auslösen. In der Regel werden während der Überprüfung beim Entwurf der Komponenten die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A>- und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A>-Methoden aufgerufen. Diese Ereignisse zeigen Meldungen im Bereich **Fehlerliste** von [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] an und stellen für die Benutzer der Komponente Feedback bereit, wenn eine Komponente nicht richtig konfiguriert ist.  
  
 Komponenten können auch zu einem beliebigen Zeitpunkt während der Ausführung Ereignisse auslösen. Ereignisse ermöglichen es Komponentenentwicklern, während der Ausführung einer Komponente Feedback für deren Benutzer bereitzustellen. Bei Aufruf der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A>-Methode während der Ausführung ist ein Versagen des Pakets wahrscheinlich.  
  
## <a name="defining-and-raising-custom-events"></a>Definieren und Auslösen von benutzerdefinierten Ereignissen  
  
### <a name="defining-a-custom-event"></a>Definieren eines benutzerdefinierten Ereignisses  
 Benutzerdefinierte Ereignisse werden erstellt, indem die <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A>-Auflistung aufgerufen wird. Diese Auflistung wird durch den Datenflusstask festgelegt und dem Komponentenentwickler über die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>-Basisklasse als Eigenschaft bereitgestellt. Diese Klasse enthält durch den Datenflusstask definierte sowie durch die Komponente während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>-Methode definierte benutzerdefinierte Ereignisse.  
  
 Die benutzerdefinierten Ereignisse einer Komponente werden nicht permanent im Paket-XML beibehalten. Daher wird sowohl während der Entwicklung als auch während der Ausführung die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>-Methode aufgerufen, um der Komponente die Definition der von ihr ausgelösten Ereignisse zu ermöglichen.  
  
 Der *allowEventHandlers*-Parameter der <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A>-Methode gibt an, ob die Komponente die Erstellung von <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekten für das Ereignis zulässt. Beachten Sie, dass <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> synchron sind. Daher setzt die Komponente die Ausführung erst dann fort, wenn ein an das benutzerdefinierte Ereignis angefügter <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> die Ausführung abgeschlossen hat. Wenn der Parameter *allowEventHandlers* den Wert **TRUE** aufweist, wird jeder Parameter des Ereignisses automatisch allen <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekten über Variablen, die automatisch von der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Runtime erstellt und aufgefüllt werden, zur Verfügung gestellt.  
  
### <a name="raising-a-custom-event"></a>Auslösen eines benutzerdefinierten Ereignisses  
 Komponenten lösen benutzerdefinierter Ereignisse durch Aufrufen der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>-Methode aus, und stellen den Namen, den Text und die Parameter des Ereignisses bereit. Wenn der *allowEventHandlers*-Parameter **true** entspricht, dann werden alle <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers>, die für das benutzerdefinierte Ereignis erstellt wurden, vom [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Runtime-Engine ausgeführt.  
  
### <a name="custom-event-sample"></a>Beispiel für ein benutzerdefiniertes Ereignis  
 Im folgenden Codebeispiel wird eine Komponente gezeigt, die während der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>-Methode ein benutzerdefiniertes Ereignis definiert und dann durch Aufrufen der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>-Methode das Ereignis zur Laufzeit auslöst.  
  
```csharp  
public override void RegisterEvents()  
{  
    string [] parameterNames = new string[2]{"RowCount", "StartTime"};  
    ushort [] parameterTypes = new ushort[2]{ DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)};  
    string [] parameterDescriptions = new string[2]{"The number of rows to sort.", "The start time of the Sort operation."};  
    EventInfos.Add("StartingSort","Fires when the component begins sorting the rows.",false,ref parameterNames, ref parameterTypes, ref parameterDescriptions);  
}  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
    while (buffer.NextRow())  
    {  
       // Process buffer rows.  
    }  
  
    IDTSEventInfo100 eventInfo = EventInfos["StartingSort"];  
    object []arguments = new object[2]{buffer.RowCount, DateTime.Now };  
    ComponentMetaData.FireCustomEvent("StartingSort", "Beginning sort operation.", ref arguments, ComponentMetaData.Name, ref FireSortEventAgain);  
}  
```  
  
```vb  
Public  Overrides Sub RegisterEvents()   
  Dim parameterNames As String() = New String(2) {"RowCount", "StartTime"}   
  Dim parameterTypes As System.UInt16() = New System.UInt16(2) {DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)}   
  Dim parameterDescriptions As String() = New String(2) {"The number of rows to sort.", "The start time of the Sort operation."}   
  EventInfos.Add("StartingSort", "Fires when the component begins sorting the rows.", False, parameterNames, parameterTypes, parameterDescriptions)   
End Sub   
  
Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
  While buffer.NextRow   
  End While   
  Dim eventInfo As IDTSEventInfo100 = EventInfos("StartingSort")   
  Dim arguments As Object() = New Object(2) {buffer.RowCount, DateTime.Now}   
  ComponentMetaData.FireCustomEvent("StartingSort", _  
    "Beginning sort operation.", arguments, _  
    ComponentMetaData.Name, FireSortEventAgain)   
End Sub  
```  

## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Ereignishandler &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Hinzufügen eines Ereignishandlers zu einem Paket](../../integration-services-ssis-event-handlers.md)  
  
