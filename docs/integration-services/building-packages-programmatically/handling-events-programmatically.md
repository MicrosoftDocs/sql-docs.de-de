---
description: Programmgesteuerte Behandlung von Ereignissen
title: Programmgesteuerte Behandlung von Ereignissen | Microsoft-Dokumentation
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
- Integration Services packages, events
- SQL Server Integration Services packages, events
- SSIS events, programmatically handling
- packages [Integration Services], events
- DtsEventHandler object
- SSIS packages, events
- SSIS events
- events [Integration Services], programmatically
- tasks [Integration Services], events
- IDTSEvents interface
ms.assetid: 0f00bd66-efd5-4f12-9e1c-36195f739332
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a2800cbfc74bf87225f93d480d54777134be8f37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355048"
---
# <a name="handling-events-programmatically"></a>Programmgesteuerte Behandlung von Ereignissen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Laufzeit stellt eine Auflistung von Ereignissen bereit, die vor, während und nach der Überprüfung und Ausführung eines Pakets auftreten. Diese Ereignisse können auf zwei Weisen aufgezeichnet werden. Die erste Methode besteht in der Implementierung der <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>-Schnittstelle in einer Klasse und der Bereitstellung der Klasse als Parameter für die **Execute**- und **Validate**-Methoden des Pakets. Die zweite Methode besteht in der Erstellung von <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekten, die andere [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Objekte enthalten können, wie z. B. Tasks und Loops, die ausgeführt werden, wenn ein Ereignis in <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> auftritt. In diesem Abschnitt werden diese beiden Methoden beschrieben und zur Veranschaulichung ihrer Verwendung Codebeispiele bereitgestellt.  
  
## <a name="receiving-idtsevents-callbacks"></a>Empfangen von IDTSEvents-Rückrufen  
 Entwickler, die Pakete programmgesteuert erstellen und ausführen, können während des Prüfungs-und Ausführungsprozesses über die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>-Schnittstelle Ereignisbenachrichtigungen erhalten. Dazu wird eine Klasse erstellt, die die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>-Schnittstelle implementiert, wobei diese Klasse als Parameter für die **Validate**- und **Execute**-Methoden eines Pakets bereitgestellt wird. Die Methoden der Klasse werden dann von der Runtime-Engine aufgerufen, wenn die Ereignisse auftreten.  
  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents>-Klasse ist eine Klasse, die bereits die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>-Schnittstelle implementiert; eine weitere Alternative für die direkte Implementierung von <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> besteht daher in der Ableitung von <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> und dem Überschreiben der bestimmten Ereignisse, auf die Sie reagieren wollen. Dann stellen Sie Ihre Klasse als Parameter für die **Validate**- und **Execute**-Methoden von <xref:Microsoft.SqlServer.Dts.Runtime.Package> bereit, um Ereignisrückrufe zu erhalten.  
  
 Im folgenden Codebeispiel wird eine Klasse veranschaulicht, die von <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> abgeleitet wird und die die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnPreExecute%2A>-Methode überschreibt. Die Klasse wird dann als Parameter für die **Validate**- und **Execute**-Methoden des Pakets bereitgestellt.  
  
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
      MyEventsClass eventsClass = new MyEventsClass();  
  
      p.Validate(null, null, eventsClass, null);  
      p.Execute(null, null, eventsClass, null, null);  
  
      Console.Read();  
    }  
  }  
  class MyEventsClass : DefaultEvents  
  {  
    public override void OnPreExecute(Executable exec, ref bool fireAgain)  
    {  
      // TODO: Add custom code to handle the event.  
      Console.WriteLine("The PreExecute event of the " +  
        exec.ToString() + " has been raised.");  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim eventsClass As MyEventsClass = New MyEventsClass()  
  
    p.Validate(Nothing, Nothing, eventsClass, Nothing)  
    p.Execute(Nothing, Nothing, eventsClass, Nothing, Nothing)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
Class MyEventsClass  
  Inherits DefaultEvents  
  
  Public Overrides Sub OnPreExecute(ByVal exec As Executable, ByRef fireAgain As Boolean)  
  
    ' TODO: Add custom code to handle the event.  
    Console.WriteLine("The PreExecute event of the " & _  
      exec.ToString() & " has been raised.")  
  
  End Sub  
  
End Class  
```  
  
## <a name="creating-dtseventhandler-objects"></a>Erstellen von DtsEventHandler-Objekten  
 Die Runtime-Engine stellt durch das <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekt eine Ereignisbehandlung und ein Benachrichtigungssystem bereit, die sich durch Robustheit und hohe Flexibilität auszeichnen. Mit diesen Objekten können Sie innerhalb des Ereignishandlers ganze Workflows entwerfen, die nur dann ausgeführt werden, wenn das Ereignis, zu dem der Ereignishandler gehört, eintritt. Das <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekt ist ein Container, der ausgeführt wird, wenn das entsprechende Ereignis auf seinem übergeordneten Objekt ausgelöst wird. Mit dieser Architektur können Sie isolierte Workflows erstellen, die als Antwort auf Ereignisse, die in einem Container auftreten, ausgeführt werden. Da <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekte synchron sind, wird die Ausführung erst dann fortgesetzt, wenn die an das Ereignis angefügten Ereignishandler zurückgegeben wurden.  
  
 Am folgenden Code wird das Erstellen eines <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekts veranschaulicht. Der Code fügt <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> zur <xref:Microsoft.SqlServer.Dts.Runtime.Package.Executables%2A>-Auflistung des Pakets hinzu und erzeugt dann ein <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>-Objekt für das <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A> Ereignis der Task. <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> wird zum Ereignishandler hinzugefügt, der ausgeführt wird, wenn das <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A>-Ereignis beim ersten <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> eintritt. In diesem Beispiel wird davon ausgegangen, dass Sie eine Datei haben, die C:\Windows\Temp\DemoFile.txt für Tests genannt wird. Wenn Sie das Beispiel zum ersten Mal ausführen, kann die Datei erfolgreich kopiert werden, und der Ereignishandler wird nicht aufgerufen. Wenn Sie das Beispiel zum zweiten Mal ausführen, kann der erste <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> die Datei nicht kopieren (denn der Wert von <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask.OverwriteDestinationFile%2A> ist **false**), und der Ereignishandler wird aufgerufen. Der zweite <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> löscht die Quelldatei und das Paket meldet das Auftreten des Fehlers.  
  
## <a name="example"></a>Beispiel  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string f = "DemoFile.txt";  
      Executable e;  
      TaskHost th;  
      FileSystemTask fst;  
  
      Package p = new Package();  
  
      p.Variables.Add("sourceFile", true, String.Empty,  
        @"C:\Windows\Temp\" + f);  
      p.Variables.Add("destinationFile", true, String.Empty,  
        Path.Combine(Directory.GetCurrentDirectory(), f));  
  
      // Create a first File System task and add it to the package.  
      // This task tries to copy a file from one folder to another.  
      e = p.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask1";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.CopyFile;  
      fst.OverwriteDestinationFile = false;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
      fst.Destination = "destinationFile";  
      fst.IsDestinationPathVariable = true;  
  
      // Add an event handler for the FileSystemTask's OnError event.  
      DtsEventHandler ehOnError = (DtsEventHandler)th.EventHandlers.Add("OnError");  
  
      // Create a second File System task and add it to the event handler.  
      // This task deletes the source file if the event handler is called.  
      e = ehOnError.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask2";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.DeleteFile;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
  
      DTSExecResult vr = p.Validate(null, null, null, null);  
      Console.WriteLine("ValidationResult = " + vr.ToString());  
      if (vr == DTSExecResult.Success)  
      {  
        DTSExecResult er = p.Execute(null, null, null, null, null);  
        Console.WriteLine("ExecutionResult = " + er.ToString());  
        if (er == DTSExecResult.Failure)  
          if (!File.Exists(@"C:\Windows\Temp\" + f))  
            Console.WriteLine("Source file has been deleted by the event handler.");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim f As String = "DemoFile.txt"  
    Dim e As Executable  
    Dim th As TaskHost  
    Dim fst As FileSystemTask  
  
    Dim p As Package = New Package()  
  
    p.Variables.Add("sourceFile", True, String.Empty, _  
      "C:\Windows\Temp\" & f)  
    p.Variables.Add("destinationFile", True, String.Empty, _  
      Path.Combine(Directory.GetCurrentDirectory(), f))  
  
    ' Create a first File System task and add it to the package.  
    ' This task tries to copy a file from one folder to another.  
    e = p.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.CopyFile  
    fst.OverwriteDestinationFile = False  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
    fst.Destination = "destinationFile"  
    fst.IsDestinationPathVariable = True  
  
    ' Add an event handler for the FileSystemTask's OnError event.  
    Dim ehOnError As DtsEventHandler = CType(th.EventHandlers.Add("OnError"), DtsEventHandler)  
  
    ' Create a second File System task and add it to the event handler.  
    ' This task deletes the source file if the event handler is called.  
    e = ehOnError.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.DeleteFile  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
  
    Dim vr As DTSExecResult = p.Validate(Nothing, Nothing, Nothing, Nothing)  
    Console.WriteLine("ValidationResult = " + vr.ToString())  
    If vr = DTSExecResult.Success Then  
      Dim er As DTSExecResult = p.Execute(Nothing, Nothing, Nothing, Nothing, Nothing)  
      Console.WriteLine("ExecutionResult = " + er.ToString())  
      If er = DTSExecResult.Failure Then  
        If Not File.Exists("C:\Windows\Temp\" + f) Then  
          Console.WriteLine("Source file has been deleted by the event handler.")  
        End If  
      End If  
    End If  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Ereignishandler &#40;SSIS&#41;](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Hinzufügen eines Ereignishandlers zu einem Paket](../integration-services-ssis-event-handlers.md)  
  
