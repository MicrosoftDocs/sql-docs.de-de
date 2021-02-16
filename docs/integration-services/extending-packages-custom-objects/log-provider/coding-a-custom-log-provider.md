---
description: Codieren eines benutzerdefinierten Protokollanbieters
title: Codieren eines benutzerdefinierten Protokollanbieters | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom log providers [Integration Services], coding
ms.assetid: 979a29ca-956e-4fdd-ab47-f06e84cead7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93ddf518405b44b5abb6956ee5da063b98ece1e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348895"
---
# <a name="coding-a-custom-log-provider"></a>Codieren eines benutzerdefinierten Protokollanbieters

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Nachdem Sie eine Klasse erstellt haben, die von der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>-Basisklasse erbt, und das <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>-Attribut auf die Klasse angewendet haben, müssen Sie die Implementierung der Eigenschaften und Methoden der Basisklasse überschreiben, um die benutzerdefinierte Funktionalität bereitzustellen.  
  
 Funktionstüchtige Beispiele benutzerdefinierter Protokollanbieter finden Sie unter [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Protokollanbieter](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md).  
  
## <a name="configuring-the-log-provider"></a>Konfigurieren des Protokollanbieters  
  
### <a name="initializing-the-log-provider"></a>Initialisieren des Protokollanbieters  
 Sie überschreiben die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.InitializeLogProvider%2A>-Methode, um Verweise auf die Connections-Auflistung und die Ereignisschnittstelle zwischenzuspeichern. Sie können diese zwischengespeicherten Verweise später in anderen Methoden des Protokollanbieters verwenden.  
  
### <a name="using-the-configstring-property"></a>Verwenden der ConfigString-Eigenschaft  
 Zur Entwurfszeit erhält ein Protokollanbieter Konfigurationsinformationen aus der Spalte **Konfiguration**. Diese Konfigurationsinformationen entsprechen der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>-Eigenschaft des Protokollanbieters. Standardmäßig enthält diese Spalte ein Textfeld, aus dem Sie alle Zeichenfolgeninformationen abrufen können. Die meisten in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] integrierten Protokollanbieter verwenden diese Eigenschaft, um den Namen des Verbindungs-Managers zu speichern, den der Anbieter zur Verbindung mit einer externen Datenquelle verwendet. Falls Ihr Protokollanbieter die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>-Eigenschaft verwendet, wenden Sie die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>-Methode an, um diese Eigenschaft zu überprüfen und sicherzustellen, dass sie richtig eingestellt ist.  
  
### <a name="validating-the-log-provider"></a>Überprüfen des Protokollanbieters  
 Sie überschreiben die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>-Methode, um sicherzustellen, dass der Anbieter ordnungsgemäß konfiguriert wurde und zum Ausführen bereit ist. In der Regel genügt eine minimale Überprüfung, um sich zu vergewissern, dass <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> richtig eingestellt ist. Die Ausführung kann erst fortgesetzt werden, wenn der Protokollanbieter <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> von der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>-Methode zurückgibt.  
  
 Im folgenden Beispiel wird eine Implementierung von <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> gezeigt, die sicherstellt, dass der Name eines Verbindungs-Managers angegeben ist, dass der Verbindungs-Manager im Paket enthalten ist und dass er einen Dateinamen in der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>-Eigenschaft zurückgibt.  
  
```csharp  
public override DTSExecResult Validate(IDTSInfoEvents infoEvents)  
{  
    if (this.ConfigString.Length == 0 || connections.Contains(ConfigString) == false)  
    {  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
        return DTSExecResult.Failure;  
    }  
    else  
    {  
        string fileName = connections[ConfigString].AcquireConnection(null) as string;  
  
        if (fileName == null || fileName.Length == 0)  
        {  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
            return DTSExecResult.Failure;  
        }  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As IDTSInfoEvents) As DTSExecResult  
    If Me.ConfigString.Length = 0 Or connections.Contains(ConfigString) = False Then  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
        Return DTSExecResult.Failure  
    Else   
        Dim fileName As String =  connections(ConfigString).AcquireConnectionCType(as string, Nothing)  
  
        If fileName = Nothing Or fileName.Length = 0 Then  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
            Return DTSExecResult.Failure  
        End If  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
### <a name="persisting-the-log-provider"></a>Beibehalten des Protokollanbieters  
 In der Regel müssen Sie keine benutzerdefinierte Persistenz für einen Verbindungs-Manager implementieren. Die benutzerdefinierte Persistenz ist nur erforderlich, wenn die Eigenschaften eines Objekts komplexe Datentypen verwenden. Weitere Informationen finden Sie unter [Developing Custom Objects for Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md) (Entwickeln von benutzerdefinierten Objekten für Integration Services).  
  
## <a name="logging-with-the-log-provider"></a>Protokollieren mit dem Protokollanbieter  
 Es gibt drei Laufzeitmethoden, die von allen Protokollanbietern überschrieben werden müssen: <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> und <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
> [!IMPORTANT]  
>  Während der Überprüfung und Ausführung eines einzelnen Pakets werden die Methoden <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> und <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> mehrmals aufgerufen. Stellen Sie sicher, dass mit Ihrem benutzerdefinierten Code keine früheren Protokolleinträge beim nächsten Öffnen und Schließen des Protokolls überschrieben werden. Falls Sie die Protokollierung von Überprüfungsereignissen in Ihrem Testpaket festgelegt haben, sollte als erstes protokolliertes Ereignis OnPreValidate angezeigt werden. Falls als erstes Ereignis PackageStart angezeigt wird, wurden die anfänglichen Überprüfungsereignisse überschrieben.  
  
### <a name="opening-the-log"></a>Öffnen des Protokolls  
 Die meisten Protokollanbieter stellen eine Verbindung mit einer externen Datenquelle, wie z. B. einer Datei oder einer Datenbank her, um die Ereignisinformationen zu speichern, die während der Paketausführung gesammelt werden. Wie bei jedem anderen Objekt erfolgt die Verbindung mit der externen Datenquelle zur Laufzeit normalerweise über die Verwendung von Verbindungs-Manager-Objekten.  
  
 Die Methode <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> wird zu Beginn der Paketausführung aufgerufen. Überschreiben Sie diese Methode, um eine Verbindung mit der externen Datenquelle herzustellen.  
  
 Im folgenden Beispielcode wird ein Protokollanbieter gezeigt, der während <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> eine Textdatei zum Schreiben öffnet. Die Datei wird durch Aufrufen der Methode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> des Verbindungs-Managers geöffnet, der in der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>-Eigenschaft angegeben wurde.  
  
```csharp  
public override void OpenLog()  
{  
    if(!this.connections.Contains(this.ConfigString))  
        throw new Exception("The ConnectionManager " + this.ConfigString + " does not exist in the Connections collection.");  
  
    this.connectionManager = connections[ConfigString];  
    string filePath = this.connectionManager.AcquireConnection(null) as string;  
  
    if(filePath == null || filePath.Length == 0)  
        throw new Exception("The ConnectionManager " + this.ConfigString + " is not a valid FILE ConnectionManager");  
  
    //  Create a StreamWriter to append to.  
    sw = new StreamWriter(filePath,true);  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString());  
}  
```  
  
```vb  
Public Overrides  Sub OpenLog()  
    If Not Me.connections.Contains(Me.ConfigString) Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " does not exist in the Connections collection.")  
    End If  
  
    Me.connectionManager = connections(ConfigString)  
    Dim filePath As String =  Me.connectionManager.AcquireConnectionCType(as string, Nothing)  
  
    If filePath = Nothing Or filePath.Length = 0 Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " is not a valid FILE ConnectionManager")  
    End If  
  
    '  Create a StreamWriter to append to.  
    sw = New StreamWriter(filePath,True)  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString())  
End Sub  
```  
  
### <a name="writing-log-entries"></a>Schreiben von Protokolleinträgen  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>-Methode wird immer dann aufgerufen, wenn ein Objekt im Paket ein Ereignis auslöst, indem es eine Fire\<event>-Methode für eine der Ereignisschnittstellen aufruft. Jedes Ereignis wird mit Informationen über seinen Kontext ausgelöst und enthält normalerweise eine erklärende Meldung. Aber nicht jeder Aufruf der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>-Methode enthält Informationen für jeden Methodenparameter. Beispielsweise liefern einige Standardereignisse, deren Namen selbsterklärend sind, keinen Meldungstext, und Datencode und Datenbytes sind für optionale Zusatzinformationen gedacht.  
  
 Im folgenden Codebeispiel wird die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>-Methode implementiert und die Ereignisse in den Datenstrom geschrieben, der im vorhergehenden Abschnitt geöffnet wurde.  
  
```csharp  
public override void Log(string logEntryName, string computerName, string operatorName, string sourceName, string sourceID, string executionID, string messageText, DateTime startTime, DateTime endTime, int dataCode, byte[] dataBytes)  
{  
    sw.Write(logEntryName + ",");  
    sw.Write(computerName + ",");  
    sw.Write(operatorName + ",");  
    sw.Write(sourceName + ",");  
    sw.Write(sourceID + ",");  
    sw.Write(messageText + ",");  
    sw.Write(dataBytes + ",");  
    sw.WriteLine("");  
}  
```  
  
```vb  
Public Overrides  Sub Log(ByVal logEnTryName As String, ByVal computerName As String, ByVal operatorName As String, ByVal sourceName As String, ByVal sourceID As String, ByVal executionID As String, ByVal messageText As String, ByVal startTime As DateTime, ByVal endTime As DateTime, ByVal dataCode As Integer, ByVal dataBytes() As Byte)  
    sw.Write(logEnTryName + ",")  
    sw.Write(computerName + ",")  
    sw.Write(operatorName + ",")  
    sw.Write(sourceName + ",")  
    sw.Write(sourceID + ",")  
    sw.Write(messageText + ",")  
    sw.Write(dataBytes + ",")  
    sw.WriteLine("")  
End Sub  
```  
  
### <a name="closing-the-log"></a>Schließen des Protokolls  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>-Methode wird am Ende der Paketausführung aufgerufen, nachdem die Ausführung aller Objekte im Paket abgeschlossen ist oder wenn das Paket aufgrund eines Fehlers gestoppt wird.  
  
 Im folgenden Codebeispiel wird die Implementierung der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>-Methode veranschaulicht, die den Dateidatenstrom schließt, der von der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>-Methode geöffnet wurde.  
  
```csharp  
public override void CloseLog()  
{  
    if (sw != null)  
    {  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString());  
        sw.Close();  
    }  
}  
```  
  
```vb  
Public Overrides  Sub CloseLog()  
    If Not sw Is Nothing Then  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString())  
        sw.Close()  
    End If  
End Sub  
```  
 
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen eines benutzerdefinierten Protokollanbieters](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Protokollanbieter](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
