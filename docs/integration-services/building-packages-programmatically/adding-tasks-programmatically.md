---
description: Programmgesteuertes Hinzufügen von Tasks
title: Programmgesteuertes Hinzufügen von Tasks | Microsoft-Dokumentation
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
- tasks [Integration Services], packages
- adding package tasks
ms.assetid: 5d4652d5-228c-4238-905c-346dd8503fdf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1650323cede5f91401faa97e820917b2549d5050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355116"
---
# <a name="adding-tasks-programmatically"></a>Programmgesteuertes Hinzufügen von Tasks

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Folgenden Objekttypen in der Runtime-Engine können Tasks hinzugefügt werden:  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Package>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Sequence>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>  
  
 Diese Klassen werden als Container angesehen, und sie alle erben die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSequence.Executables%2A>-Eigenschaft. Container können eine Auflistung von Tasks enthalten. Dies sind ausführbare Objekte, die von der Laufzeit bei der Ausführung des Containers verarbeitet werden. Die Ausführungsreihenfolge der Objekte in der Auflistung hängt von den <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> ab, die für die einzelnen Tasks im Container festgelegt wurden. Rangfolgeneinschränkungen ermöglichen eine verzweigte Ausführung, abhängig vom Erfolg, Fehlschlag oder der Beendigung einer <xref:Microsoft.SqlServer.Dts.Runtime.Executable> in der Auflistung.  
  
 Jeder Container verfügt über eine <xref:Microsoft.SqlServer.Dts.Runtime.Executables>-Auflistung, die die einzelnen <xref:Microsoft.SqlServer.Dts.Runtime.Executable>-Objekte enthält. Jeder ausführbare Task erbt und implementiert die <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A>-Methode sowie die <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A>-Methode. Diese zwei Methoden werden von der Runtime-Engine aufgerufen, um jede <xref:Microsoft.SqlServer.Dts.Runtime.Executable> zu verarbeiten.  
  
 Um einem Paket einen Task hinzuzufügen, benötigen Sie einen Container mit einer Auflistung vorhandener <xref:Microsoft.SqlServer.Dts.Runtime.Executables>. Meist handelt es sich bei dem Task, den Sie der Auflistung hinzufügen, um ein Paket. Um der Auflistung des Containers die neue ausführbare Datei des Tasks hinzuzufügen, rufen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>-Methode auf. Die Methode verfügt über einen Parameter, eine Zeichenfolge, die den CLSID-, PROGID- bzw. STOCK-Moniker oder den <xref:Microsoft.SqlServer.Dts.Runtime.TaskInfo.CreationName%2A> des Tasks, den Sie hinzufügen, enthält.  
  
## <a name="task-names"></a>Tasknamen  
 Sie können einen Task zwar über einen Namen oder eine ID definieren, aber der **STOCK**-Moniker ist der am häufigsten für die <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>-Methode verwendete Parameter. Verwenden Sie die folgende Syntax, um einen Task einer ausführbaren Datei hinzuzufügen, die durch den **STOCK**-Moniker identifiziert wird:  
  
```csharp  
Executable exec = package.Executables.Add("STOCK:BulkInsertTask");  
  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add("STOCK:BulkInsertTask")  
  
```  
  
 Die folgende Liste zeigt die Namen für alle Tasks an, die nach dem **STOCK**-Moniker verwendet werden.  
  
-   ActiveXScriptTask  
  
-   BulkInsertTask  
  
-   ExecuteProcessTask  
  
-   ExecutePackageTask  
  
-   Exec80PackageTask  
  
-   FileSystemTask  
  
-   FTPTask  
  
-   MSMQTask  
  
-   PipelineTask  
  
-   ScriptTask  
  
-   SendMailTask  
  
-   SQLTask  
  
-   TransferStoredProceduresTask  
  
-   TransferLoginsTask  
  
-   TransferErrorMessagesTask  
  
-   TransferJobsTask  
  
-   TransferObjectsTask  
  
-   TransferDatabaseTask  
  
-   WebServiceTask  
  
-   WmiDataReaderTask  
  
-   WmiEventWatcherTask  
  
-   XMLTask  
  
 Wenn Sie eine explizitere Syntax bevorzugen oder der Task, den Sie hinzufügen möchten, nicht über einen STOCK-Moniker verfügt, können Sie den Task unter Verwendung seines langen Namens der ausführbaren Datei hinzufügen. Bei dieser Syntax müssen Sie auch die Versionsnummer des Tasks angeben.  
  
```csharp  
Executable exec = package.Executables.Add(  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " +  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " +  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91");  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add( _  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " & _  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " & _  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91")  
```  
  
 Sie können den langen Namen des Tasks programmgesteuert mithilfe der **AssemblyQualifiedName**-Eigenschaft der Klasse erhalten, ohne die Taskversion angeben zu müssen. Dies wird im nächsten Beispiel veranschaulicht. Für dieses Beispiel ist ein Verweis auf die Microsoft.SqlServer.SQLTask-Assembly erforderlich.  
  
```csharp  
using Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask;  
...  
      Executable exec = package.Executables.Add(  
        typeof(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName);  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask  
...  
    Dim exec As Executable = package.Executables.Add( _  
      GetType(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName)  
```  
  
 Im folgenden Codebeispiel wird die Erstellung einer <xref:Microsoft.SqlServer.Dts.Runtime.Executables>-Auflistung aus einem neuen Paket veranschaulicht. Anschließend wird der Auflistung unter Verwendung der **STOCK**-Moniker ein Dateisystem- und ein Masseneinfügungstask hinzugefügt. Für dieses Beispiel ist einVerweis auf die Microsoft.SqlServer.FileSystemTask- sowie die Microsoft.SqlServer.BulkInsertTask-Assembly erforderlich.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thBulkInsertTask = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        Console.WriteLine("Type {0}", taskHost.InnerObject.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thBulkInsertTask As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      Console.WriteLine("Type {0}", taskHost.InnerObject.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Beispielausgabe:**  
  
 `Type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
## <a name="taskhost-container"></a>TaskHost-Container  
 Bei der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Klasse handelt es sich um einen Container, der nicht auf der grafischen Benutzeroberfläche angezeigt wird, aber für die Programmierung sehr wichtig ist. Diese Klasse dient als Wrapper für alle Tasks. Tasks, die dem Paket mithilfe der<xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>-Methode als <xref:Microsoft.SqlServer.Dts.Runtime.Executable>-Objekte hinzugefügt werden, können in <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekte umgewandelt werden. Wenn ein Task in ein <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt umgewandelt ist, können Sie auf den Task zusätzliche Eigenschaften und Methoden anwenden. Darüber hinaus können Sie über die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A>-Eigenschaft des <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> direkt auf den Task zugreifen. Abhängig von Ihren Anforderungen können Sie den Task als <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt beibehalten, um die Eigenschaften des Tasks in der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>-Auflistung anwenden zu können. Der Vorteil, den die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> bieten, besteht darin, dass Sie allgemeineren Code schreiben können. Falls Sie für einen Task sehr spezifischen Code benötigen, sollten Sie den Task in das entsprechende Objekt umwandeln.  
  
 Das folgende Codebeispiel zeigt die Umwandlung eines <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, thBulkInsertTask, der einen <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask> enthält, in ein <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>-Objekt.  
  
```csharp  
BulkInsertTask myTask = thBulkInsertTask.InnerObject as BulkInsertTask;  
```  
  
```vb  
Dim myTask As BulkInsertTask = CType(thBulkInsertTask.InnerObject, BulkInsertTask)  
```  
  
 Im folgenden Codebeispiel wird veranschaulicht, wie eine ausführbare Datei in einen <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> umgewandelt wird. Anschließend wird über die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A>-Eigenschaft der im Host enthaltene ausführbare Dateityp ermittelt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask1 = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thFileSystemTask2 = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask)  
        {  
          // Do work with FileSystemTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        else if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask)  
        {  
          // Do work with BulkInsertTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        // Add additional statements to check InnerObject, if desired.  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask1 As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thFileSystemTask2 As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      If TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask Then  
        ' Do work with FileSystemTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      ElseIf TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask Then  
        ' Do work with BulkInsertTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      End If  
      ' Add additional statements to check InnerObject, if desired.  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Beispielausgabe:**  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>-Anweisung gibt eine ausführbare Datei zurück, die aus dem neu geschaffenen <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt in ein <xref:Microsoft.SqlServer.Dts.Runtime.Executable>-Objekt umgewandelt wurde.  
  
 Um Eigenschaften festzulegen oder Methoden auf dem neuen Objekt aufzurufen, haben Sie zwei Optionen:  
  
1.  Verwenden Sie die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>-Auflistung des <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Verwenden Sie beispielsweise `th.Properties["propertyname"].GetValue(th))`, um eine Eigenschaft des Objekts abzurufen. Zum Festlegen einer Eigenschaft nutzen Sie `th.Properties["propertyname"].SetValue(th, <value>);`.  
  
2.  Wandeln Sie das <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> des <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> in die Taskklasse um. Zum Umwandeln des Masseneinfügungstasks in einen <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>, nachdem er dem Paket als <xref:Microsoft.SqlServer.Dts.Runtime.Executable> hinzugefügt und anschließend in einen <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> umgewandelt wurde, verwenden Sie beispielsweise `BulkInsertTask myTask = th.InnerObject as BulkInsertTask;`.  
  
 Die Verwendung der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Klasse im Code anstelle der Umwandlung in die taskspezifische Klasse bietet folgende Vorteile:  
  
-   Der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost><xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>-Anbieter benötigt keinen Verweis auf die Assembly im Code.  
  
-   Sie können Code für generische Routinen schreiben, die für jeden Task anwendbar sind, da Sie bei der Kompilierung nicht über den Namen des Tasks verfügen müssen. Zu diesen generischen Routinen zählen Methoden, bei denen Sie den Tasknamen an die Methode übergeben und der Methodencode für alle Tasks geeignet ist. Diese Methode ist gut für das Schreiben von Testcode geeignet.  
  
 Die Umwandlung vom <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> in die taskspezifische Klasse bietet folgende Vorteile:  
  
-   Das Visual Studio-Projekt bietet Ihnen die Anweisungsvervollständigung (IntelliSense).  
  
-   Der Code wird möglicherweise schneller ausgeführt.  
  
-   Taskspezifische Objekte ermöglichen eine frühe Bindung und die daraus resultierenden Optimierungen. Weitere Informationen zur frühen und späten Bindung finden Sie im entsprechenden Thema der Visual Basic-Sprachkonzepte.  
  
 Im folgenden Codebeispiel wird das Konzept der Wiederverwendung von Taskcode veranschaulicht. Anstatt Tasks in ihre jeweiligen Klassenentsprechungen umzuwandeln, wird in diesem Codebeispiel die Umwandlung der ausführbaren Datei in einen <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> und die anschließende Verwendung der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> zum Schreiben von generischen Codes für alle Tasks gezeigt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
  
      string[] tasks = { "STOCK:SQLTask", "STOCK:ScriptTask",   
        "STOCK:ExecuteProcessTask", "STOCK:PipelineTask",   
        "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask" };  
  
      foreach (string s in tasks)  
      {  
        TaskHost taskhost = package.Executables.Add(s) as TaskHost;  
        DtsProperties props = taskhost.Properties;  
        Console.WriteLine("Enumerating properties on " + taskhost.Name);  
        Console.WriteLine(" TaskHost.InnerObject is " + taskhost.InnerObject.ToString());  
        Console.WriteLine();  
  
        foreach (DtsProperty prop in props)  
        {  
          Console.WriteLine("Properties for " + prop.Name);  
          Console.WriteLine("Name : " + prop.Name);  
          Console.WriteLine("Type : " + prop.Type.ToString());  
          Console.WriteLine("Readable : " + prop.Get.ToString());  
          Console.WriteLine("Writable : " + prop.Set.ToString());  
          Console.WriteLine();  
        }  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package = New Package()  
  
    Dim tasks() As String = New String() {"STOCK:SQLTask", "STOCK:ScriptTask", _  
              "STOCK:ExecuteProcessTask", "STOCK:PipelineTask", _  
              "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask"}  
  
    For Each s As String In tasks  
  
      Dim taskhost As TaskHost = CType(package.Executables.Add(s), TaskHost)  
      Dim props As DtsProperties = taskhost.Properties  
      Console.WriteLine("Enumerating properties on " & taskhost.Name)  
      Console.WriteLine(" TaskHost.InnerObject is " & taskhost.InnerObject.ToString())  
      Console.WriteLine()  
  
      For Each prop As DtsProperty In props  
        Console.WriteLine("Properties for " + prop.Name)  
        Console.WriteLine(" Name : " + prop.Name)  
        Console.WriteLine(" Type : " + prop.Type.ToString())  
        Console.WriteLine(" Readable : " + prop.Get.ToString())  
        Console.WriteLine(" Writable : " + prop.Set.ToString())  
        Console.WriteLine()  
      Next  
  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Blogbeitrag, [EzAPI – Updated for SQL Server 2012 (EzAPI: für SQL Server 2012 aktualisiert)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), auf blogs.msdn.com.  

## <a name="see-also"></a>Weitere Informationen  
 [Programmgesteuertes Verbinden von Tasks](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
  
  
