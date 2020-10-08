---
title: Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse
description: Erfahren Sie, wie Sie auf erweiterte Ereignisse auf dem Server im Zusammenhang mit Ereignissen für den Microsoft JDBC-Treiber für SQL Server zugreifen können.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a79e9468-2257-4536-91f1-73b008c376c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d9b75ea8c722ca753e831811226b8128df15266
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725506"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  In [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] wurde die Ablaufverfolgung ([Ablaufverfolgung für Treibervorgänge](../../connect/jdbc/tracing-driver-operation.md)) aktualisiert, damit Clientereignisse einfacher mit Diagnoseinformationen (z.B. Verbindungsfehlern) aus dem Konnektivitätsringpuffer des Servers und den anwendungsbezogenen Leistungsinformationen in den erweiterten Ereignisprotokollen korreliert werden können. Informationen dazu, wie Sie das Protokoll für erweiterte Ereignisse lesen, finden Sie unter [Übersicht über erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md).  
  
## <a name="details"></a>Details  
 Bei Verbindungsvorgängen sendet [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] eine Clientverbindungs-ID. Wenn die Verbindung nicht hergestellt werden kann, können Sie auf den Konnektivitätsringpuffer zugreifen ([Behandlung von Konnektivitätsproblemen in SQL Server 2008 mit dem Konnektivitätsringpuffer](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)), das Feld **ClientConnectionID** suchen und Diagnoseinformationen zum Verbindungsfehler abrufen. Clientverbindungs-IDs werden nur im Ringpuffer protokolliert, wenn ein Fehler auftritt. (Wenn vor dem Senden des prelogin-Pakets keine Verbindung hergestellt werden kann, wird keine Clientverbindungs-ID generiert.) Die Clientverbindungs-ID ist eine 16-Byte-GUID. Sie können auch die Clientverbindungs-ID in der erweiterten Ereignisse-Zielausgabe suchen, wenn Ereignissen in einer Sitzung für erweiterte Ereignisse die Aktion **client_connection_id** hinzugefügt wird. Wenn Sie weitere Unterstützung zur Clienttreiberdiagnose benötigen, können Sie die Ablaufverfolgung aktivieren und den Verbindungsbefehl erneut ausführen. Beobachten Sie dabei das Feld **ClientConnectionID** in der Ablaufverfolgung.  
  
 Sie können die Clientverbindungs-ID programmgesteuert abrufen, und zwar mit der [ISQLServerConnection-Schnittstelle](../../connect/jdbc/reference/isqlserverconnection-interface.md). Die Verbindungs-ID ist auch in verbindungsbezogenen Ausnahmen enthalten.  
  
 Bei einem Verbindungsfehler kann die Clientverbindungs-ID in den BID-Ablaufverfolgungsinformationen (integrierte Diagnose) des Servers und im Konnektivitätsringpuffer nützlich sein, um die Clientverbindungen mit Verbindungen auf dem Server zu korrelieren. Weitere Informationen zur BID-Ablaufverfolgung auf dem Server finden Sie unter [Data Access Tracing (Datenzugriffsablaufverfolgung)](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Der Artikel zur Datenzugriffsablaufverfolgung enthält auch Informationen zu Datenzugriffsablaufverfolgungen, die sich nicht auf [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] beziehen. Informationen zum Ausführen einer Datenzugriffsablaufverfolgung unter Verwendung von [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] finden Sie unter [Ablaufverfolgung für Treibervorgänge](../../connect/jdbc/tracing-driver-operation.md).  
  
 Der JDBC-Treiber sendet außerdem eine threadspezifische Aktivitäts-ID. Die Aktivitäts-ID wird in den Sitzungen für erweiterte Ereignisse aufgezeichnet, wenn die Sitzungen bei aktivierter TRACK_CAUSAILITY-Option gestartet werden. Bei Leistungsproblemen mit einer aktiven Verbindung können Sie die Aktivitäts-ID aus der Ablaufverfolgung des Clients (Feld ActivityID) abrufen und dann in der Ausgabe der erweiterten Ereignisse nach der Aktivitäts-ID suchen. Die Aktivitäts-ID in erweiterten Ereignissen ist eine 16-Byte-GUID (entspricht nicht der GUID für die Clientverbindungs-ID), an die eine 4-Byte-Sequenznummer angehängt ist. Die Sequenznummer stellt die Reihenfolge einer Anforderung in einem Thread dar. Die ActivityId wird bei SQL-Stapelanweisungen und RPC-Anforderungen gesendet. Zum Aktivieren des Sendens der ActivityId an den Server müssen Sie zunächst das folgende Schlüssel-Wert-Par in der Datei „Logging.Properties“ angeben:  
  
```
com.microsoft.sqlserver.jdbc.traceactivity = on  
```  
  
 Jeder andere Wert als `on` (unter Beachtung der Groß-/Kleinschreibung) deaktiviert das Senden der ActivityId.  
  
 Weitere Informationen finden Sie unter [Ablaufverfolgung für Treibervorgänge](../../connect/jdbc/tracing-driver-operation.md). Dieses Ablaufverfolgungsflag wird mit den entsprechenden JDBC-Objektprotokollierungen verwendet, um zu bestimmen, ob im JDBC-Treiber eine Ablaufverfolgung der ActivityId ausgeführt und diese gesendet werden soll. Zusätzlich zum Aktualisieren der Datei „Logging.Properties“ muss die Protokollierung „com.microsoft.sqlserver.jdbc“ mit dem Wert FINER oder höher aktiviert werden. Wenn Sie die ActivityId an den Server für Anforderungen einer bestimmten Klasse senden möchten, muss die entsprechende Klassenprotokollierung mit dem Wert FINER oder FINEST aktiviert werden. Wenn die Klasse beispielsweise SQLServerStatement lautet, aktivieren Sie die Protokollierung com.microsoft.sqlserver.jdbc.SQLServerStatement.  
  
 Im folgenden Beispiel wird [!INCLUDE[tsql](../../includes/tsql-md.md)] zum Starten einer Sitzung für erweiterte Ereignisse verwendet, die in einem Ringpuffer gespeichert werden und die von einem Client in RPC- und Batch-Vorgängen gesendete Aktivitäts-ID aufzeichnen:  
  
```sql
create event session MySession on server  
add event connectivity_ring_buffer_recorded,  
add event sql_statement_starting (action (client_connection_id)),  
add event sql_statement_completed (action (client_connection_id)),  
add event rpc_starting (action (client_connection_id)),  
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
```  
  
## <a name="see-also"></a>Weitere Informationen

[Diagnostizieren von Problemen mit dem JDBC-Treiber](../../connect/jdbc/diagnosing-problems-with-the-jdbc-driver.md)