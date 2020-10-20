---
description: Verwenden von Variablen in der Skriptkomponente
title: Verwenden von Variablen in der Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], using variables
ms.assetid: 92d1881a-1ef1-43ae-b1ca-48d0536bdbc2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d27181eac591f6c66166810e9662c04b6b97fc40
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196418"
---
# <a name="using-variables-in-the-script-component"></a>Verwenden von Variablen in der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Variablen speichern Werte, die von einem Paket und dessen Containern, Tasks und Ereignishandlern zur Laufzeit verwendet werden können. Weitere Informationen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md).  
  
 Sie können vorhandene Variablen für schreibgeschützten oder Lese-/Schreibzugriff mittels eines benutzerdefinierten Skripts zur Verfügung stellen, indem Sie kommagetrennte Listen von Variablen in die Felder **ReadOnlyVariables** und **ReadWriteVariables** auf der Seite **Skript** des **Transformations-Editors für Skripterstellung** eingeben. Beachten Sie, dass bei Variablennamen nach Groß-/Kleinschreibung unterschieden wird. Mithilfe der **Value**-Eigenschaft erhalten Sie Lese- und Schreibzugriff auf einzelne Variablen. Die Skriptkomponente führt erforderliche Sperrungen im Hintergrund aus, da das Skript die Variablen zur Laufzeit verarbeitet.  
  
> [!IMPORTANT]  
>  Die Auflistung von **ReadWriteVariables** ist nur in der **PostExecute**-Methode verfügbar, um das Risiko von Sperrkonflikten zu vermindern und die Leistung zu maximieren. Sie können daher den Wert einer Paketvariablen nicht direkt inkrementieren, während Sie jede Datenzeile verarbeiten. Inkrementieren Sie stattdessen den Wert einer lokalen Variablen, und legen Sie den Wert der Paketvariablen auf den Wert der lokalen Variablen in der **PostExecute**-Methode fest, nachdem alle Daten verarbeitet wurden. Sie können auch die Eigenschaft <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> verwenden, um diese Einschränkung zu umgehen, wie später in diesem Thema beschrieben ist. Wenn Sie jedoch direkt in eine Paketvariable schreiben, während jede Zeile verarbeitet wird, wird die Leistung beeinträchtigt und das Risiko von Sperrkonflikten erhöht.  
  
 Weitere Informationen über die Seite **Skript** im **Transformations-Editor für Skripterstellung** finden Sie unter [Konfigurieren der Skriptkomponente im Skriptkomponenten-Editor](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) und [Transformations-Editor für Skripterstellung &#40;Script Page&#41;](../../data-flow/transformations/script-component.md).  
  
 Die Skriptkomponente erstellt eine **Variables**-Auflistungsklasse im **ComponentWrapper**-Projektelement mit einer Accessoreigenschaft mit starker Typbindung für den Wert jeder vorkonfigurierten Variable, wobei die Eigenschaft denselben Namen wie die Variable selbst hat. Diese Auflistung wird von der **Variables**-Eigenschaft der **ScriptMain**-Klasse verfügbar gemacht. Die Accessoreigenschaft bietet entsprechend Lese- oder Lese-/Schreibberechtigung für den Wert der Variable. Wenn Sie beispielsweise eine Ganzzahlvariable namens `MyIntegerVariable` der Liste **ReadOnlyVariables** hinzugefügt haben, können Sie ihren Wert mit dem folgenden Code in das Skript abrufen:  
  
 `Dim myIntegerVariableValue As Integer = Me.Variables.MyIntegerVariable`  
  
 Für die Arbeit mit Variablen in der Skriptkomponente können Sie auch die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>-Eigenschaft (Aufruf mit `Me.VariableDispenser`) verwenden. In diesem Fall verwenden Sie nicht die benannten, typisierten Accessoreigenschaften für Variablen, sondern greifen direkt auf die Variablen zu. Bei der Verwendung von <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> müssen Sie die Sperrsemantik und die Umwandlung von Datentypen für variable Werte in Ihrem Code berücksichtigen. Wenn Sie mit Variablen arbeiten möchten, die zur Entwurfszeit nicht zur Verfügung stehen, sondern programmgesteuert zur Laufzeit erstellt werden, müssen Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>-Eigenschaft statt der benannten, typisierten Accessoreigenschaften verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Variablen &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md)   
 [Verwenden von Variablen in Paketen](../../integration-services-ssis-variables.md)  
  
