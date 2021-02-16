---
description: Codieren eines benutzerdefinierten Tasks
title: Codieren eines benutzerdefinierten Tasks | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- Validate method
- custom tasks [Integration Services], validating
- validation [Integration Services], design-time tasks
- SSIS custom tasks, validating
ms.assetid: dc224f4f-b339-4eb6-a008-1b4fe0ea4fd2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f336a5969e608d1f2647b02679e87da0a65c1bad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348224"
---
# <a name="coding-a-custom-task"></a>Codieren eines benutzerdefinierten Tasks

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Nachdem Sie eine Klasse erstellt haben, die von der Basisklasse [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) erbt, und das <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>-Attribut auf die Klasse angewendet haben, müssen Sie die Implementierung der Eigenschaften und Methoden der Basisklasse überschreiben, um die benutzerdefinierte Funktionalität bereitzustellen.  
  
## <a name="configuring-the-task"></a>Konfigurieren des Tasks  
  
### <a name="validating-the-task"></a>Überprüfen des Tasks  
 Wenn Sie ein [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Paket entwerfen, können Sie die Validierung verwenden, um Einstellungen für die einzelnen Tasks zu überprüfen, sodass falsche oder unpassende Einstellungen gleich nach dem Festlegen abgefangen werden können und nicht erst zur Laufzeit auffallen. Der Sinn und Zweck der Validierung besteht darin zu bestimmen, ob der Task ungültige Einstellungen oder Verbindungen umfasst, durch die das erfolgreiche Ausführen verhindert wird. Auf diese Weise wird sichergestellt, dass das Paket nur Tasks enthält, bei denen die Wahrscheinlichkeit groß ist, dass sie beim ersten Versuch korrekt ausgeführt werden.  
  
 Sie können die Validierung mithilfe der **Validate**-Methode in benutzerdefiniertem Code implementieren. Die Runtime-Engine überprüft einen Task, indem die **Validate**-Methode im Task aufgerufen wird. Der Taskentwickler ist dafür verantwortlich, die Kriterien zu definieren, die eine erfolgreiche oder fehlgeschlagene Taskvalidierung ausmachen, und eine Benachrichtigung an die Runtime-Engine mit dem Ergebnis dieser Auswertung auszugeben.  
  
#### <a name="task-abstract-base-class"></a>Abstrakte Basisklasse des Tasks  
 Durch die abstrakte [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task)-Basisklasse wird die **Validate**-Methode bereitgestellt, die jeder Task überschreibt, um seine eigenen Validierungskriterien zu definieren. Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer ruft die **Validate**-Methode automatisch mehrere Male während des Paketentwurfs auf und gibt dem Benutzer visuelle Hinweise, wenn Warnungen oder Fehler auftreten, sodass Probleme mit der Konfiguration des Tasks ermittelt werden können. Die Validierungsergebnisse von Tasks werden bereitgestellt, indem ein Wert aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Enumeration zurückgegeben wird und Warnungs- und Fehlerereignisse ausgelöst werden. Diese Ereignisse enthalten Informationen, die dem Benutzer im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer angezeigt werden.  
  
 Nachfolgend finden Sie einige Beispiele für Validierungen:  
  
-   Ein Verbindungs-Manager validiert den spezifischen Dateinamen.  
  
-   Ein Verbindungs-Manager überprüft, dass der Typ der Eingabe der erwartete Typ ist, beispielsweise eine XML-Datei.  
  
-   Ein Task, der eine Datenbankeingabe erwartet, überprüft, dass keine Daten von einer Nicht-Datenbankverbindung empfangen werden können.  
  
-   Ein Task garantiert, dass keine seiner Eigenschaften mit anderen Eigenschaften, die für den gleichen Task festgelegt wurden, in Konflikt steht.  
  
-   Ein Task garantiert, dass alle erforderlichen Ressourcen, die von dem Task bei der Ausführung verwendet werden, verfügbar sind.  
  
 Bei der Bestimmung, was validiert wird, spielt die Leistung eine wichtige Rolle. Die Eingabe für einen Task kann beispielsweise eine Verbindung über ein Netzwerk mit niedriger Bandbreite oder hohem Datenverkehr sein. Die Validierung kann möglicherweise mehrere Sekunden dauern, wenn Sie festlegen, dass überprüft werden soll, ob die Ressource verfügbar ist. Eine andere Validierung kann möglicherweise zu einem Roundtrip zu einem stark beanspruchten Server führen, und die Validierungsroutine kann langsam sein. Obwohl es viele Eigenschaften und Einstellungen gibt, die überprüft werden können, ist es nicht ratsam, alles zu überprüfen.  
  
-   Der Code in der **Validate**-Methode wird auch von <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> aufgerufen, bevor der Task ausgeführt wird, und <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> bricht die Ausführung ab, wenn die Validierung fehlschlägt.  
  
#### <a name="user-interface-considerations-during-validation"></a>Überlegungen zur Benutzeroberfläche während der Validierung  
 Die Klasse [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) enthält eine <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>-Schnittstelle als Parameter für die **Validate**-Methode. Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>-Schnittstelle umfasst die Methoden, die von dem Task aufgerufen werden, um Ereignisse für die Runtime-Engine auszulösen. Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A>-Methode und die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>-Methode werden aufgerufen, wenn eine Warnung oder eine Fehlerbedingung während der Validierung auftritt. Für beide Warnungsmethoden sind die gleichen Parameter erforderlich. Dazu gehören ein Fehlercode, eine Quellkomponente, eine Beschreibung, eine Hilfedatei sowie Hilfekontextinformationen. Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer verwendet diese Informationen, um visuelle Hinweise auf der Entwurfsoberfläche anzuzeigen. Die vom Designer bereitgestellten visuellen Hinweise enthalten ein Ausrufezeichen, das neben dem Task auf der Entwurfsoberfläche angezeigt wird. Dieser visuelle Hinweis signalisiert dem Benutzer, dass der Task zusätzliche Konfiguration erfordert, bevor die Ausführung fortgesetzt werden kann.  
  
 Mit dem Ausrufezeichen wird auch eine QuickInfo angezeigt, die eine Fehlermeldung enthält. Die Fehlermeldung wird vom Task im Beschreibungsparameter des Ereignisses bereitgestellt. Fehlermeldungen werden auch in der **Aufgabenliste** von [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] angezeigt, sodass der Benutzer einen zentralen Ort zur Anzeige aller Validierungsfehler erhält.  
  
#### <a name="validation-example"></a>Beispiel für die Validierung  
 Im folgenden Codebeispiel wird ein Task mit einer **UserName**-Eigenschaft veranschaulicht. Diese Eigenschaft wurde angegeben, da sie für eine erfolgreiche Validierung erforderlich ist. Wenn die Eigenschaft nicht festgelegt wird, gibt der Task einen Fehler aus und gibt einen <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Failure> aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Enumeration zurück. Die **Validate**-Methode wird in einem try/catch-Block umschlossen und führt zu einem Fehler bei der Validierung, wenn eine Ausnahme auftritt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string userName = "";  
  
  public override DTSExecResult Validate(Connections connections,  
     VariableDispenser variableDispenser, IDTSComponentEvents events,  
     IDTSLogging log)  
  {  
    try  
    {  
      if (this.userName == "")  
      {  
        //   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0);  
        //   Fail validation.  
        return DTSExecResult.Failure;  
      }  
      //   Return success.  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string UserName  
  {  
    get  
    {  
      return this.userName;  
    }  
    set  
    {  
      this.userName = value;  
    }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _userName As String = ""  
  
  Public Overrides Function Validate(ByVal connections As Connections, _  
     ByVal variableDispenser As VariableDispenser, _  
     ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging) As DTSExecResult  
  
    Try  
      If Me._userName = "" Then  
        '   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0)  
        '   Fail validation.  
        Return DTSExecResult.Failure  
      End If  
      '   Return success.  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property UserName() As String  
    Get  
      Return Me._userName  
    End Get  
    Set(ByVal Value As String)  
      Me._userName = Value  
    End Set  
  End Property  
  
End Class  
```  
  
### <a name="persisting-the-task"></a>Beibehalten des Tasks  
 In der Regel müssen Sie keine benutzerdefinierte Persistenz für einen Task implementieren. Die benutzerdefinierte Persistenz ist nur erforderlich, wenn die Eigenschaften eines Objekts komplexe Datentypen verwenden. Weitere Informationen finden Sie unter [Developing Custom Objects for Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md) (Entwickeln von benutzerdefinierten Objekten für Integration Services).  
  
## <a name="executing-the-task"></a>Ausführen des Tasks  
 In diesem Abschnitt wird beschrieben, wie die **Execute**-Methode verwendet wird, die von Tasks geerbt und überschrieben wird. In diesem Abschnitt werden auch verschiedene Methoden zum Bereitstellen von Informationen zu den Ergebnissen der Taskausführung erläutert.  
  
### <a name="execute-method"></a>Execute-Methode  
 In einem Paket enthaltene Tasks werden ausgeführt, wenn ihre **Execute**-Methode von der [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Runtime aufgerufen wird. Die grundlegende Geschäftslogik und Funktionalität von Tasks ist in dieser Methode implementiert. Die Ergebnisse der Ausführung werden durch Ausgeben von Meldungen, Zurückgeben eines Werts aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Enumeration und Überschreiben der **get**-Eigenschaft der **ExecutionValue**-Eigenschaft bereitgestellt.  
  
 Die Basisklasse [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) stellt eine Standardimplementierung der <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>-Methode bereit. Die benutzerdefinierten Tasks überschreiben diese Methode, um ihre Laufzeitfunktionalität zu definieren. Das <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt umschließt den Task, sodass dieser von der Runtime-Engine und den anderen Objekten in dem Paket isoliert wird. Aufgrund dieser Isolation erkennt der Task seine Stelle in dem Paket bezüglich der Ausführungsreihenfolge nicht und wird nur ausgeführt, wenn er durch die Laufzeit aufgerufen wird. Durch diese Architektur werden Probleme verhindert, die auftreten können, wenn Tasks während der Ausführung das Paket ändern. Der Task erhält nur durch die Objekte, die ihm in der <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>-Methode als Parameter bereitgestellt werden, Zugriff auf die anderen Objekte in dem Paket. Mithilfe dieser Parameter können Tasks Ereignisse auslösen, Einträge in das Ereignisprotokoll schreiben, auf die Variablenauflistung zugreifen und Verbindungen zu Datenquellen in Transaktionen eintragen, während gleichzeitig die Isolation beibehalten wird, die erforderlich ist, um die Stabilität und Zuverlässigkeit des Pakets zu garantieren.  
  
 In der folgenden Tabelle sind die für den Task in der <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>-Methode bereitgestellten Parameter aufgeführt.  
  
|Parameter|BESCHREIBUNG|  
|---------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Connections>|Enthält eine Auflistung von <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekten, die für den Task verfügbar sind.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.VariableDispenser>|Enthält die Variablen, die für den Task verfügbar sind. Die Tasks verwenden Variablen durch den VariableDispenser; die Tasks verwenden keine Variablen direkt. Der VariableDispenser sperrt und entsperrt Variablen und verhindert Deadlocks oder Überschreibungen.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>|Enthält die Methoden, die von dem Task zum Auslösen von Ereignissen in der Runtime-Engine aufgerufen werden.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSLogging>|Enthält Methoden und Eigenschaften, die von dem Task zum Schreiben von Ereignissen in das Ereignisprotokoll verwendet werden.|  
|Object|Enthält ggf. das Transaktionsobjekt, von dem der Container ein Teil ist. Dieser Wert wird als Parameter an die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A>-Methode eines <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>-Objekts übergeben.|  
  
### <a name="providing-execution-feedback"></a>Bereitstellen von Feedback zur Ausführung  
 Bei Tasks wird der Code in **try/catch**-Blöcke eingebunden, um zu verhindern, dass Ausnahmen in der Runtime-Engine ausgelöst werden. Dadurch wird sichergestellt, dass das Paket die Ausführung abschließt und nicht unerwartet beendet wird. Die Runtime-Engine bietet jedoch andere Mechanismen zur Behandlung von Fehlerbedingungen, die während der Ausführung eines Tasks auftreten können. Dazu gehört das Ausgeben von Fehler- und Warnmeldungen, das Zurückgeben eines Werts aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Struktur, das Ausgeben von Meldungen, das Zurückgeben des <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>-Werts sowie das Offenlegen von Informationen zu den Ergebnissen der Taskausführung durch die <xref:Microsoft.SqlServer.Dts.Runtime.Task.ExecutionValue%2A>-Eigenschaft.  
  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>-Schnittstelle enthält die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A>-Methode und die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>-Methode, die von dem Task aufgerufen werden können, um Fehler- und Warnmeldungen für die Runtime-Engine auszugeben. Für beide Methoden sind Parameter wie der Fehlercode, die Quellkomponente, die Beschreibung, die Hilfedatei sowie Hilfekontextinformationen erforderlich. Je nach der Konfiguration des Tasks reagiert die Laufzeit auf diese Meldungen, indem Ereignisse und Breakpoints ausgelöst oder Informationen in das Ereignisprotokoll geschrieben werden.  
  
 Das <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt stellt auch die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A>-Eigenschaft bereit, die verwendet werden kann, um zusätzliche Informationen zu den Ergebnissen der Ausführung bereitzustellen. Wenn ein Task beispielsweise Zeilen aus einer Tabelle als Teil seiner **Execute**-Methode löscht, wird möglicherweise die Anzahl von gelöschten Zeilen als Wert der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A>-Eigenschaft zurückgegeben. Darüber hinaus stellt das <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecValueVariable%2A>-Eigenschaft bereit. Mithilfe dieser Eigenschaft kann der Benutzer den von dem Task zurückgegebenen <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A>-Wert einer Variablen zuordnen, die für den Task sichtbar ist. Die angegebene Variable kann dann verwendet werden, um Rangfolgeneinschränkungen zwischen Tasks einzurichten.  
  
### <a name="execution-example"></a>Ausführungsbeispiel  
 Im folgenden Codebeispiel wird eine Implementierung der **Execute**-Methode veranschaulicht, und es wird eine überschriebene **ExecutionValue**-Eigenschaft gezeigt. Der Task löscht die Datei, die von der **fileName**-Eigenschaft des Tasks angegeben wird. Der Task gibt eine Warnung aus, wenn die Datei nicht vorhanden oder die **fileName**-Eigenschaft eine leere Zeichenfolge ist. Der Task gibt einen **booleschen** Wert in der <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A>-Eigenschaft zurück, um anzugeben, ob die Datei gelöscht wurde.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string fileName = "";  
  private bool fileDeleted = false;  
  
  public override DTSExecResult Execute(Connections cons,  
     VariableDispenser vars, IDTSComponentEvents events,  
     IDTSLogging log, Object txn)  
  {  
    try  
    {  
      if (this.fileName == "")  
      {  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0);  
        this.fileDeleted = false;  
      }  
      else  
      {  
        if (System.IO.File.Exists(this.fileName))  
        {  
          System.IO.File.Delete(this.fileName);  
          this.fileDeleted = true;  
        }  
        else  
          this.fileDeleted = false;  
      }  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string FileName  
  {  
    get { return this.fileName; }  
    set { this.fileName = value; }  
  }  
  public override object ExecutionValue  
  {  
    get { return this.fileDeleted; }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _fileName As String = ""  
  Private _fileDeleted As Boolean = False  
  
  Public Overrides Function Execute(ByVal cons As Connections, _  
     ByVal vars As VariableDispenser, ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging, ByVal txn As Object) As DTSExecResult  
  
    Try  
      If Me._fileName = "" Then  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0)  
        Me._fileDeleted = False  
      Else  
        If System.IO.File.Exists(Me._fileName) Then  
          System.IO.File.Delete(Me._fileName)  
          Me._fileDeleted = True  
        Else  
          Me._fileDeleted = False  
        End If  
      End If  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property FileName() As String  
    Get  
      Return Me._fileName  
    End Get  
    Set(ByVal Value As String)  
      Me._fileName = Value  
    End Set  
  End Property  
  
  Public Overrides ReadOnly Property ExecutionValue() As Object  
    Get  
      Return Me._fileDeleted  
    End Get  
  End Property  
  
End Class  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen eines benutzerdefinierten Tasks](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Codieren eines benutzerdefinierten Tasks](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Task](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
