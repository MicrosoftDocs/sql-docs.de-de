---
description: Programmgesteuertes Verbinden von Tasks
title: Programmgesteuertes Verbinden von Tasks | Microsoft-Dokumentation
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
- tasks [Integration Services], precedence constraints
- precedence constraints [Integration Services], connecting tasks
- constraints [Integration Services]
ms.assetid: 23668e88-cef4-4009-a9cf-38e607eab7a2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 554d6550e73b93e94d192cf66b1209aaa1b27292
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355058"
---
# <a name="connecting-tasks-programmatically"></a>Programmgesteuertes Verbinden von Tasks

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Eine Rangfolgeneinschränkung, die in dem Objektmodell durch die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>-Klasse dargestellt wird, legt die Reihenfolge, in der die <xref:Microsoft.SqlServer.Dts.Runtime.Executable>-Objekte in einem Paket ausgeführt werden, fest. Durch die Rangfolgeneinschränkung kann die Ausführung der Container und Tasks in einem Paket von dem Ergebnis der Ausführung eines vorherigen Tasks oder Containers abhängig gemacht werden. Rangfolgeneinschränkungen werden zwischen <xref:Microsoft.SqlServer.Dts.Runtime.Executable>-Objektpaaren durch Aufrufen der <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints.Add%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints>-Auflistung für das Containerobjekt eingerichtet. Nachdem Sie eine Einschränkung zwischen zwei ausführbaren Objekten erstellt haben, legen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A>-Eigenschaft fest, um die Kriterien für die Ausführung des zweiten ausführbaren in der Einschränkung definierten Objekts festzulegen.  
  
 Abhängig von dem Wert, den Sie, wie in der folgenden Tabelle beschrieben, für die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.EvalOp%2A>-Eigenschaft festlegen, können Sie in einer einzigen Rangfolgeneinschränkung sowohl eine Einschränkung als auch einen Ausdruck verwenden:  
  
|Wert der EvalOp-Eigenschaft|BESCHREIBUNG|  
|----------------------------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Constraint>|Gibt an, dass das Ausführungsergebnis bestimmt, ob der eingeschränkte Container oder Task ausgeführt wird. Legen Sie für die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A>-Eigenschaft von <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> den gewünschten Wert aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Enumeration fest.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Expression>|Gibt an, dass der Wert eines Ausdrucks bestimmt, ob der eingeschränkte Container oder Task ausgeführt wird. Legen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A>-Eigenschaft von <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> fest.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionAndConstraint>|Gibt an, dass das Einschränkungsergebnis auftreten und der Ausdruck ausgewertet werden muss, damit der eingeschränkte Container oder Task ausgeführt wird. Legen Sie sowohl die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A>- als auch die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A>-Eigenschaften von <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> fest, und legen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A>-Eigenschaft als **true** fest.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionOrConstraint>|Gibt an, dass entweder das Einschränkungsergebnis auftreten oder der Ausdruck ausgewertet werden muss, damit der eingeschränkte Container oder Task ausgeführt wird. Legen Sie sowohl die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A>- als auch die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A>-Eigenschaften von <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> fest, und legen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A>-Eigenschaft als **false** fest.|  
  
 Im folgenden Codebeispiel wird das Hinzufügen von zwei Tasks zu einem Paket veranschaulicht. Zwischen ihnen wird eine <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> erstellt, die verhindert, dass der zweite Task vor Beendung des ersten Tasks ausgeführt wird.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
  
      // Add a File System task.  
      Executable eFileTask1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost1 = eFileTask1 as TaskHost;  
  
      // Add a second File System task.  
      Executable eFileTask2 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost2 = eFileTask2 as TaskHost;  
  
      // Put a precedence constraint between the tasks.  
      // Set the constraint to specify that the second File System task cannot run  
      // until the first File System task finishes.  
      PrecedenceConstraint pcFileTasks =   
        p.PrecedenceConstraints.Add((Executable)thFileHost1, (Executable)thFileHost2);  
      pcFileTasks.Value = DTSExecResult.Completion;  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task.  
    Dim eFileTask1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost1 As TaskHost = CType(eFileTask1, TaskHost)  
  
    ' Add a second File System task.  
    Dim eFileTask2 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost2 As TaskHost = CType(eFileTask2, TaskHost)  
  
    ' Put a precedence constraint between the tasks.  
    ' Set the constraint to specify that the second File System task cannot run  
    ' until the first File System task finishes.  
    Dim pcFileTasks As PrecedenceConstraint = _  
      p.PrecedenceConstraints.Add(CType(thFileHost1, Executable), CType(thFileHost2, Executable))  
    pcFileTasks.Value = DTSExecResult.Completion  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>Weitere Informationen  
 [Programmgesteuertes Hinzufügen des Datenflusstasks](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
  
