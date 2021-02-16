---
description: Programmgesteuertes Aktivieren der Protokollierung
title: Programmgesteuertes Aktivieren der Protokollierung | Microsoft-Dokumentation
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
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- Integration Services packages, logs
- SSIS logging
- log providers [Integration Services]
- logs [Integration Services], enabling
- LoggingMode property
- LogProvider object
- packages [Integration Services], logs
ms.assetid: 3222a1ed-83eb-421c-b299-a53b67bba740
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 35b731cdb854d24ee2c4759c9ce3f611dc98c104
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349172"
---
# <a name="enabling-logging-programmatically"></a>Programmgesteuertes Aktivieren der Protokollierung

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Runtime-Engine stellt eine Auflistung von <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider>-Objekten bereit, mit deren Hilfe ereignisspezifische Informationen während der Paketüberprüfung und -ausführung aufgezeichnet werden können. <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider>-Objekte sind für <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer>-Objekte verfügbar; hierzu zählen auch die Objekte <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, <xref:Microsoft.SqlServer.Dts.Runtime.Package>, <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop> und <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>. Die Protokollierung wird für einzelne Container oder das gesamte Paket aktiviert.  
  
 Es gibt mehrere Typen von Protokollanbietern, die für einen zu verwendenden Container verfügbar sind. Dies bietet die Flexibilität, Protokollinformationen in vielen verschiedenen Formaten erstellen und speichern zu können. Das Eintragen eines Containerobjekts zur Protokollierung umfasst zwei Schritte. Zuerst wird die Protokollierung aktiviert, und im zweiten Schritt wird ein Protokollanbieter ausgewählt. Die Eigenschaften <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingOptions%2A> und <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A> des Containers werden zum Angeben der protokollierten Ereignisse und zum Auswählen des Protokollanbieters verwendet.  
  
## <a name="enabling-logging"></a>Aktivieren der Protokollierung  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A>-Eigenschaft ist in jedem Container verfügbar, der eine Protokollierung ausführen kann, und legt fest, ob die Ereignisinformationen des Containers im Ereignisprotokoll aufgezeichnet werden. Der Eigenschaft wird ein Wert aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSLoggingMode>-Struktur zugewiesen, sie wird standardmäßig vom übergeordneten Container geerbt. Wenn der Container ein Paket ist und deshalb keinen übergeordneten Container hat, verwendet die Eigenschaft die <xref:Microsoft.SqlServer.Dts.Runtime.DTSLoggingMode.UseParentSetting>, die standardmäßig auf **Deaktiviert** festgelegt ist.  
  
### <a name="selecting-a-log-provider"></a>Auswählen eines Protokollanbieters  
 Nachdem die <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A>-Eigenschaft auf **Aktiviert** festgelegt wurde, wird der <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders>-Auflistung des Containers ein Protokollanbieter hinzugefügt, um den Prozess abzuschließen. Die <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders>-Auflistung steht im <xref:Microsoft.SqlServer.Dts.Runtime.LoggingOptions>-Objekt zur Verfügung und enthält die für den Container ausgewählten Protokollanbieter. Zum Erstellen eines Anbieters und Hinzufügen dieses Anbieters zu einer Auflistung wird die <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders.Add%2A>-Methode aufgerufen. Die Methode gibt daraufhin den Protokollanbieter zurück, der der Auflistung hinzugefügt wurde. Jeder Anbieter verfügt über eindeutige Konfigurationseinstellungen, und diese Eigenschaften werden mithilfe der <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider.ConfigString%2A>-Eigenschaft festgelegt.  
  
 In der folgenden Tabelle werden die verfügbaren Protokollanbieter, ihre Beschreibung und ihre <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider.ConfigString%2A>-Informationen aufgeführt.  
  
|Anbieter|Beschreibung|ConfigString-Eigenschaft|  
|--------------|-----------------|---------------------------|  
|SQL Server Profiler|Generiert SQL-Ablaufverfolgungen, die aufgezeichnet und im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Profiler angezeigt werden können. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist TRC.|Es ist keine Konfiguration erforderlich.|  
|SQL Server|Schreibt in allen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken Ereignisprotokolleinträge in die **sysssislog**-Tabelle.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anbieter erfordert eine angegebene Verbindung zur Datenbank sowie den Namen der Zieldatenbank.|  
|Textdatei|Schreibt Ereignisprotokolleinträge im durch Trennzeichen getrennten CSV-Format in ASCII-Textdateien. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist LOG.|Der Name eines Dateiverbindungs-Managers.|  
|Windows-Ereignisprotokoll|Schreibt Protokolle in das Anwendungsprotokoll im standardmäßigen Windows-Ereignisprotokoll auf dem lokalen Computer.|Es ist keine Konfiguration erforderlich.|  
|XML-Datei|Schreibt Ereignisprotokolleinträge in Dateien im XML-Format. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist XML.|Der Name eines Dateiverbindungs-Managers.|  
  
 Ob Ereignisse in das Ereignisprotokoll aufgenommen werden oder nicht, wird über die **EventFilterKind**-Eigenschaft und die **EventFilter**-Eigenschaft des Containers festgelegt. Die **EventFilterKind**-Struktur enthält zwei Werte, **ExclusionFilter** und **InclusionFilter**, die angeben, ob die Ereignisse, die **EventFilter** hinzugefügt werden, im Ereignisprotokoll enthalten sind. Der **EventFilter**-Eigenschaft wird daraufhin ein Zeichenfolgenarray mit den Namen der Ereignisse zugewiesen, nach denen gefiltert wurde.  
  
 Mit dem folgenden Code wird die Protokollfunktion eines Pakets aktiviert, der Protokollanbieter für Textdateien zur <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders>-Auflistung hinzugefügt und eine Liste der Ereignisse angegeben, die in die Protokollausgabe aufgenommen werden sollen.  
  
## <a name="sample"></a>Beispiel  
  
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
  
      ConnectionManager loggingConnection = p.Connections.Add("FILE");  
      loggingConnection.ConnectionString = @"C:\SSISPackageLog.txt";  
  
      LogProvider provider = p.LogProviders.Add("DTS.LogProviderTextFile.2");  
      provider.ConfigString = loggingConnection.Name;  
      p.LoggingOptions.SelectedLogProviders.Add(provider);  
      p.LoggingOptions.EventFilterKind = DTSEventFilterKind.Inclusion;  
      p.LoggingOptions.EventFilter = new String[] { "OnPreExecute",   
         "OnPostExecute", "OnError", "OnWarning", "OnInformation" };  
      p.LoggingMode = DTSLoggingMode.Enabled;  
  
      // Add tasks and other objects to the package.  
  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
  
    Dim loggingConnection As ConnectionManager = p.Connections.Add("FILE")  
    loggingConnection.ConnectionString = "C:\SSISPackageLog.txt"  
  
    Dim provider As LogProvider = p.LogProviders.Add("DTS.LogProviderTextFile.2")  
    provider.ConfigString = loggingConnection.Name  
    p.LoggingOptions.SelectedLogProviders.Add(provider)  
    p.LoggingOptions.EventFilterKind = DTSEventFilterKind.Inclusion  
    p.LoggingOptions.EventFilter = New String() {"OnPreExecute", _  
       "OnPostExecute", "OnError", "OnWarning", "OnInformation"}  
    p.LoggingMode = DTSLoggingMode.Enabled  
  
    ' Add tasks and other objects to the package.  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
