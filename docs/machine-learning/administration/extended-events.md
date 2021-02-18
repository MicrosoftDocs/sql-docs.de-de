---
title: Überwachen von Skripts mit erweiterten Ereignissen
description: Erfahren Sie, wie Sie erweiterte Ereignisse verwenden, um Vorgänge im Zusammenhang mit SQL Server Machine Learning Services, dem SQL Server-Launchpad und externen Skripts für Python- oder R-Aufträge zu überwachen und Probleme zu beheben.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/04/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: d8324e49ce4de61096880275a07df5a20e288a93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337160"
---
# <a name="monitor-python-and-r-scripts-with-extended-events-in-sql-server-machine-learning-services"></a>Überwachen von Python- und R-Skripts mit erweiterten Ereignissen in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Erfahren Sie, wie Sie erweiterte Ereignisse verwenden, um Vorgänge im Zusammenhang mit SQL Server Machine Learning Services, dem SQL Server-Launchpad und externen Skripts für Python- oder R-Aufträge zu überwachen und Probleme zu beheben.

## <a name="extended-events-for-sql-server-machine-learning-services"></a>Erweiterte Ereignisse für SQL Server Machine Learning Services

Führen Sie die folgende Abfrage über Azure Data Studio oder SQL Server Management Studio aus, um eine Liste der Ereignisse abzurufen, die im Zusammenhang mit SQL Server Machine Learning Services stehen.

```sql
SELECT o.name AS event_name, o.description
FROM sys.dm_xe_objects o
JOIN sys.dm_xe_packages p
ON o.package_guid = p.guid
WHERE o.object_type = 'event'
AND p.name = 'SQLSatellite';
```

Weitere Informationen zu erweiterten Ereignissen finden Sie unter [Tools für erweiterte Ereignisse](../../relational-databases/extended-events/extended-events-tools.md).

## <a name="additional-events-specific-to-machine-learning-services"></a>Zusätzliche für Machine Learning Services spezifische Ereignisse

Zusätzliche erweiterte Ereignisse sind für Komponenten verfügbar, die mit SQL Server Machine Learning Services verknüpft sind und von dieser Engine verwendet werden, z. B. das [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] und BXLServer sowie der Satellitenprozess, der die Python- oder R-Runtime startet. Diese zusätzlichen erweiterten Ereignisse werden von externen Prozessen ausgelöst und müssen daher mithilfe eines externen Hilfsprogramms erfasst werden.

Weitere Informationen zum Erfassen dieser Ereignisse finden Sie im Abschnitt [Sammeln von Ereignissen aus externen Prozessen](#bkmk_externalevents).

<a name="bkmk_xeventtable"></a> 

## <a name="table-of-extended-events"></a>Tabelle erweiterter Ereignisse

|Ereignis|Beschreibung|Notizen|  
|-----------|-----------------|---------|  
|connection_accept|Tritt auf, wenn eine neue Verbindung akzeptiert wird. Dieses Ereignis dient dazu, alle Verbindungsversuche zu protokollieren.||  
|failed_launching|Fehler beim Starten.|Gibt einen Fehler an.|  
|satellite_abort_connection|Eintrag über einen Verbindungsabbruch||  
|satellite_abort_received|Wird ausgelöst, wenn eine Abbruchnachricht über eine Satellitenverbindung empfangen wird.||  
|satellite_abort_sent|Wird ausgelöst, wenn eine Abbruchnachricht über eine Satellitenverbindung gesendet wird.||  
|satellite_authentication_completion|Wird ausgelöst, wenn die Authentifizierung für eine Verbindung über TCP oder Named Pipe abgeschlossen wurde.||  
|satellite_authorization_completion|Wird ausgelöst, wenn die Autorisierung für eine Verbindung über TCP oder Named Pipe abgeschlossen wurde.||  
|satellite_cleanup|Wird ausgelöst, wenn ein Satellit Cleanup aufruft.|Nur aus externem Prozess ausgelöst. Lesen Sie die Anleitung für das Sammeln von Ereignissen aus externen Prozessen.|  
|satellite_data_chunk_sent|Wird ausgelöst, wenn die Satellitenverbindung das Senden eines einzelnen Datensegments abschließt.|Das Ereignis gibt die Anzahl der gesendeten Zeilen, Spalten, der verwendeten SNI-Pakete und die beim Senden des Blocks verstrichene Zeit in Millisekunden an. Anhand dieser Informationen können Sie nachvollziehen, wie viel Zeit für das Übergeben von verschiedene Datentypen benötigt wird und wie viele Pakete verwendet werden.|  
|satellite_data_receive_completion|Wird ausgelöst, wenn alle erforderlichen Abfragedaten über die Satellitenverbindung empfangen wurden.|Nur aus externem Prozess ausgelöst. Lesen Sie die Anleitung für das Sammeln von Ereignissen aus externen Prozessen.|  
|satellite_data_send_completion|Wird ausgelöst, wenn alle erforderlichen Abfragedaten über die Satellitenverbindung gesendet wurden.||  
|satellite_data_send_start|Wird ausgelöst, wenn die Datenübertragung gestartet wird.| Wird ausgelöst, wenn die Datenübertragung beginnt (unmittelbar bevor der erste Datenblock gesendet wird).|  
|satellite_error|Wird verwendet, um einen SQL-Satellitenfehler nachzuverfolgen||  
|satellite_invalid_sized_message|Länge dieser Nachricht ist ungültig.||  
|satellite_message_coalesced|Wird verwendet, um das Zusammenfügen von Nachrichten auf Netzwerkebene nachzuverfolgen||  
|satellite_message_ring_buffer_record|Eintrag über Nachrichtenringpuffer||  
|satellite_message_summary|zusammenfassende Informationen zur Nachrichtenübermittlung||  
|satellite_message_version_mismatch|Versionsfeld der Nachricht stimmt nicht überein||  
|satellite_messaging|Wird verwendet, um ein Nachrichtenereignis (Binden, Bindung aufheben usw.) nachzuverfolgen.||  
|satellite_partial_message|Wird verwendet, um eine Teilnachricht auf Netzwerkebene nachzuverfolgen.||  
|satellite_schema_received|Wird ausgelöst, wenn die Schemanachricht empfangen und von SQL gelesen wird.||  
|satellite_schema_sent|Wird ausgelöst, wenn der Satellit eine Schemanachricht sendet.|Nur aus externem Prozess ausgelöst. Lesen Sie die Anleitung für das Sammeln von Ereignissen aus externen Prozessen.|  
|satellite_service_start_posted|Wird ausgelöst, wenn die Startnachricht des Diensts an Launchpad gesendet wird.|Dies erteilt Launchpad den Auftrag, den externen Prozess zu starten. Außerdem enthält das Ereignis eine ID für die neue Sitzung.|  
|satellite_unexpected_message_received|Wird ausgelöst, wenn eine unerwartete Nachricht empfangen wird.|Gibt einen Fehler an.|  
|stack_trace|Tritt auf, wenn ein Speicherabbild des Prozesses angefordert wird.|Gibt einen Fehler an.|  
|trace_event|Wird zu Protokollierungszwecken verwendet|Diese Ereignisse können Ablaufverfolgungsmeldungen von SQL Server, Launchpad und externen Prozessen enthalten. Dies schließt die Ausgabe an „stdout“ und „stderr“ aus R ein.|  
|launchpad_launch_start|Wird ausgelöst, wenn Launchpad einen Satelliten startet.|Wird nur aus Launchpad ausgelöst. Lesen Sie die Anleitung für das Sammeln von Ereignissen aus launchpad.exe.|  
|launchpad_resume_sent|Wird ausgelöst, wenn Launchpad den Satelliten gestartet und eine Fortsetzungsmeldung an SQL Server gesendet hat.|Wird nur aus Launchpad ausgelöst. Lesen Sie die Anleitung für das Sammeln von Ereignissen aus launchpad.exe.|  
|satellite_data_chunk_sent|Wird ausgelöst, wenn die Satellitenverbindung das Senden eines einzelnen Datensegments abschließt.|Enthält Informationen über die Anzahl der Spalten, Zeilen und Pakete sowie über die zum Versenden des Segments benötigten Zeit.|  
|satellite_sessionId_mismatch|Sitzungs-ID der Nachricht wird nicht erwartet||  

<a name="bkmk_externalevents"></a>

### <a name="collecting-events-from-external-processes"></a>Sammeln von Ereignissen aus externen Prozessen

SQL Server Machine Learning Services startet einige Dienste, die außerhalb des SQL Server-Prozesses ausgeführt werden. Sie können Ereignisse erfassen, die im Zusammenhang mit diesen externen Prozessen auftreten, indem Sie eine Konfigurationsdatei zur Ereignisnachverfolgung erstellen. Diese Datei müssen Sie im gleichen Verzeichnis wie die ausführbare Datei für den Prozess ablegen.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> In SQL Server 2019 wurde der Isolationsmechanismus geändert. Daher müssen Sie dem Verzeichnis, in dem die Konfigurationsdatei für die Ereignisablaufverfolgung gespeichert ist, geeignete Berechtigungen erteilen. Informationen zum Festlegen dieser Berechtigungen finden Sie im [Abschnitt zu Dateiberechtigungen unter „SQL Server 2019 unter Windows: Isolationsänderungen für Machine Learning Services“](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

+ **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**   
  
    Legen Sie die *.xml*-Datei im Binn-Verzeichnis für die SQL Server-Instanz ab, um Ereignisse zu erfassen, die im Zusammenhang mit dem Launchpad auftreten. In einer Standardinstallation wäre dies:

    `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\MSSQL\Binn`.  
  
+ **BXLServer** ist der Satellitenprozess, der die Erweiterbarkeit von SQL auf externe Skriptsprachen wie R oder Python unterstützt. Für jede externe Sprachinstanz wird eine separate Instanz von BXLServer gestartet.
  
    Sie können Ereignisse erfassen, die im Zusammenhang mit BXLServer auftreten, indem Sie die *.xml*-Datei im Installationsverzeichnis von R oder Python ablegen. In einer Standardinstallation wäre dies:
     
    **R:** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64`.  

    **Python:** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\rxLibs`.

Der Name der Konfigurationsdatei muss identisch mit dem der ausführbaren Datei sein und dem Format „[Name].xevents.xml“ entsprechen. Mit anderen Worten: Die Dateien müssen folgendermaßen benannt werden:

+ `Launchpad.xevents.xml`
+ `bxlserver.xevents.xml`

Die Konfigurationsdatei weist das folgende Format auf:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="[session name]" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="you">Xevent for launchpad or bxl server.</description>  
    <event package="SQLSatellite" name="[XEvent Name 1]" />  
    <event package="SQLSatellite" name="[XEvent Name 2]" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="[SessionName].xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Bearbeiten Sie zum Konfigurieren der Verfolgung den Platzhalter für den Namen der Sitzung (*session name*), den Platzhalter für den Dateinamen (`[SessionName].xel`) und die Namen der Ereignisse, die Sie erfassen möchten (z. B. `[XEvent Name 1]` oder `[XEvent Name 1]`).  
+ Eine beliebige Anzahl von Tags für Ereignispakete kann angezeigt werden und wird gesammelt, solange das Namensattribut korrekt ist.

### <a name="example-capturing-launchpad-events"></a>Beispiel: Erfassen von Launchpad-Ereignissen

Das folgende Beispiel zeigt die Definition einer Ereignisnachverfolgung für den Launchpad-Dienst:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="launchpad_launch_start" />  
    <event package="SQLSatellite" name="launchpad_resume_sent" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="launchpad_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Legen Sie die *.xml*-Datei im Binn-Verzeichnis für die SQL Server-Instanz ab.
+ Diese Datei muss den Namen `Launchpad.xevents.xml` haben.

### <a name="example-capturing-bxlserver-events"></a>Beispiel: Erfassen von BXLServer-Ereignissen  

Das folgende Beispiel zeigt die Definition einer Ereignisablaufverfolgung für die ausführbare Datei von BXLServer.
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
 <event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="satellite_abort_received" />  
    <event package="SQLSatellite" name="satellite_authentication_completion" />  
    <event package="SQLSatellite" name="satellite_cleanup" />  
    <event package="SQLSatellite" name="satellite_data_receive_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_start" />  
    <event package="SQLSatellite" name="satellite_schema_sent" />   
    <event package="SQLSatellite" name="satellite_unexpected_message_received" />    
    <event package="SQLSatellite" name="satellite_data_chunk_sent" />   
    <target package="package0" name="event_file">  
      <parameter name="filename" value="satellite_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Legen Sie die *.xml*-Datei im selben Verzeichnis ab wie die ausführbare BXLServer-Datei.
+ Diese Datei muss den Namen `bxlserver.xevents.xml` haben.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen der Python- und R-Skriptausführung mithilfe von benutzerdefinierten Berichten in SQL Server Management Studio](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
- [Überwachen von SQL Server Machine Learning Services mithilfe von dynamischen Verwaltungssichten](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)