---
title: Aktivieren der Ereignisablaufverfolgung in SqlClient
description: In diesem Artikel wird erläutert, wie Sie die Ereignisablaufverfolgung in SqlClient aktivieren, indem Sie einen Ereignislistener implementieren. Außerdem erfahren Sie, wie Sie auf Ereignisdaten zugreifen.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: c13942dc5633deec4759ebd4c8a5661654c0952b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837087"
---
# <a name="enable-event-tracing-in-sqlclient"></a>Aktivieren der Ereignisablaufverfolgung in SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

[Ereignisablaufverfolgung für Windows-Ereignisse (ETW)](/windows/win32/etw/event-tracing-portal) ist eine effiziente Funktion für Ablaufverfolgung auf Kernelebene, mit der Sie vom Treiber definierte Ereignisse zu Debug- und Testzwecken protokollieren können. SqlClient unterstützt das Erfassen von ETW-Ereignissen auf verschiedenen Informationsebenen. Clientanwendungen sollten auf Ereignisse aus der EventSource-Implementierung von SqlClient lauschen, um die Erfassung von Ereignisablaufverfolgungen zu starten.

```
Microsoft.Data.SqlClient.EventSource
```

Die aktuelle Implementierung unterstützt die folgenden Schlüsselwörter für Ereignisse:

| Name des Schlüsselworts | Wert | BESCHREIBUNG |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | Dieses Schlüsselwort aktiviert die Erfassung von Start/Stop-Ereignissen vor und nach der Befehlsausführung. |
| Trace | 2 | Dieses Schlüsselwort aktiviert die Erfassung einfacher Ereignisse der Anwendungsflowablaufverfolgung. |
| `Scope` | 4 | Dieses Schlüsselwort aktiviert die Erfassung von Enter- und Exit-Ereignissen. |
| NotificationTrace | 8 | Dieses Schlüsselwort aktiviert die Erfassung von `SqlNotification`-Ablaufverfolgungsereignissen. |
| NotificationScope | 16 | Dieses Schlüsselwort aktiviert Enter- und Exit-Ereignisse im `SqlNotification`-Bereich. |
| PoolerTrace | 32 | Dieses Schlüsselwort aktiviert Ablaufverfolgungsereignisse des Verbindungspoolingflows. |
| PoolerScope | 64 | Dieses Schlüsselwort aktiviert Ablaufverfolgungsereignisse des Verbindungspoolingbereichs. |
| AdvancedTrace | 128 | Dieses Schlüsselwort aktiviert die Erfassung erweiterter Flowablaufverfolgungsereignisse. |
| AdvancedTraceBin  | 256 | Dieses Schlüsselwort aktiviert die Erfassung erweiterter Flowablaufverfolgungsereignisse mit zusätzlichen Informationen. |
| CorrelationTrace | 512 | Dieses Schlüsselwort aktiviert die Erfassung von Ablaufverfolgungsereignissen für Korrelationsflows. |
| StateDump | 1024 | Dieses Schlüsselwort aktiviert die Erfassung einer vollständigen Statussicherung von `SqlConnection`. |
| SNITrace | 2048 | Dieses Schlüsselwort aktiviert die Erfassung von Flowablaufverfolgungsereignissen in der Implementierung von verwalteten Netzwerken (gilt nur für .NET Core). |
| SNIScope | 4096 | Dieses Schlüsselwort aktiviert die Erfassung von Bereichsablaufverfolgungsereignissen in der Implementierung von verwalteten Netzwerken (gilt nur für .NET Core). |
|||

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird die Ereignisablaufverfolgung für einen Datenvorgang für die **AdventureWorks**-Beispieldatenbank aktiviert, und die Ereignisse werden im Konsolenfenster angezeigt.

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>Unterstützung der Ereignisablaufverfolgung in nativer SNI-Datei

**Microsoft.Data.SqlClient** 2.1.0 erweitert die Unterstützung der Ereignisablaufverfolgung in **Microsoft.Data.SqlClient.SNI** und **Microsoft.Data.SqlClient.SNI.runtime**. Durch Senden eines Ereignisbefehls (EventCommand) an `SqlClientEventSource` können Ereignisse in der nativen Datei „SNI.dll“ mit den Tools [Xperf](/windows-hardware/test/wpt/) und [PerfView](https://github.com/microsoft/perfview) gesammelt werden. Die gültigen EventCommand-Werte sind nachstehend aufgeführt:

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

Im folgenden Beispiel wird die Ereignisablaufverfolgung in der nativen Datei „SNI.dll“ aktiviert, wenn die Anwendung auf .NET Framework abzielt. 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>Erfassen der Ablaufverfolgung mit Xperf

1. Starten Sie die Ablaufverfolgung über die folgende Befehlszeile.

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```

2. Führen Sie das Beispiel der nativen SNI-Ablaufverfolgung aus, um eine Verbindung mit SQL Server herzustellen.

3. Beenden Sie die Ablaufverfolgung über die folgende Befehlszeile.

   ```
   xperf -stop trace
   ```

4. Öffnen Sie mithilfe von PerfView die in Schritt 1 angegebene Datei „myTrace.etl“. Das SNI-Ablaufverfolgungsprotokoll kann anhand der Ereignisnamen `Microsoft.Data.SqlClient.EventSource/SNIScope` und `Microsoft.Data.SqlClient.EventSource/SNITrace` gefunden werden.

   ![Anzeigen der SNI-Ablaufverfolgungsdatei mit PerfView](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>Erfassen der Ablaufverfolgung mit PerfView

1. Starten Sie PerfView, und führen Sie über die Menüleiste `Collect > Collect` aus.

2. Konfigurieren Sie den Namen der Ablaufverfolgungsdatei, Ausgabepfad und Anbieternamen.

   ![Konfigurieren von PerfView vor der Sammlung](media/collect-event-trace-native-sni.png)

3. Starten Sie die Sammlung.

4. Führen Sie das Beispiel der nativen SNI-Ablaufverfolgung aus, um eine Verbindung mit SQL Server herzustellen.

5. Beenden Sie die Sammlung in PerfView. Das Generieren der Datei „PerfViewData.etl“ wird je nach Konfiguration in Schritt 2 eine Weile dauern.

6. Öffnen Sie die `etl`-Datei in PerfView. Das SNI-Ablaufverfolgungsprotokoll kann anhand der Ereignisnamen `Microsoft.Data.SqlClient.EventSource/SNIScope` und `Microsoft.Data.SqlClient.EventSource/SNITrace` gefunden werden.

## <a name="external-resources"></a>Externe Ressourcen  

Weitere Informationen finden Sie in den folgenden Ressourcen.  
  
|Resource|BESCHREIBUNG|  
|--------------|-----------------|  
|[EventSource-Klasse](/dotnet/api/system.diagnostics.tracing.eventsource)|Dient zum Erstellen von ETW-Ereignissen.|
|[EventListener-Klasse](/dotnet/api/system.diagnostics.tracing.eventlistener)|Diese Klasse stellt Methoden zum Aktivieren und Deaktivieren von Ereignissen aus Ereignisquellen bereit.|
