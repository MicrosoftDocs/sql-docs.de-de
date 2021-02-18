---
title: Überprüfen der Wiedergabeergebnisse
titleSuffix: SQL Server Distributed Replay
description: Nachdem das Distributed Replay-Feature von SQL Server eine verteilte Wiedergabe abgeschlossen hat, kann die Wiedergabeaktivität für jeden Client in Ablaufverfolgungsdateien auf jedem Client gespeichert werden.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: da999781-f0ff-47eb-ba7a-09c0ed8f61ad
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 5c2909a8f4c6f592781d94dbef6ab0288ff5980d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354504"
---
# <a name="review-the-replay-results"></a>Überprüfen der Wiedergabeergebnisse

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nachdem das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Feature eine verteilte Wiedergabe abgeschlossen hat, kann die Wiedergabeaktivität für jeden Client aufgezeichnet und in Ergebnis-Ablaufverfolgungsdateien auf jedem Client gespeichert werden. Um diese Aktivität aufzuzeichnen, müssen Sie beim Ausführen des Verwaltungstools mit der **replay** -Option den **-o** -Parameter verwenden. Weitere Informationen zur Wiedergabeoption finden Sie unter [Wiedergabeoption &#40;Verwaltungstool „Distributed Replay“&#41;](../../tools/distributed-replay/replay-option-distributed-replay-administration-tool.md).  
  
 Der Speicherort für die Ergebnisdateien der Ablaufverfolgung wird vom `<ResultDirectory>` -XML-Element in der Clientkonfigurationsdatei `DReplayClient.xml`, die sich auf jedem Client befindet, angegeben. Die Ablaufverfolgungsdateien im Clientergebnisverzeichnis werden bei jeder Wiedergabe überschrieben.  
  
 Um anzugeben, welche Art von Ausgabe in den Ergebnisdateien der Ablaufverfolgung aufgezeichnet werden soll, ändern Sie die Wiedergabekonfigurationsdatei `DReplay.exe.replay.config`. Sie können mit dem `<OutputOptions>` -XML-Element angeben, ob die Zeilenanzahl oder der Resultsetinhalt aufgezeichnet werden soll.  
  
 Weitere Informationen zu diesen Konfigurationseinstellungen finden Sie unter [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md).  
  
## <a name="event-classes-captured-in-result-trace-files"></a>In Ergebnisdateien der Ablaufverfolgung aufgezeichnete Ereignisklassen  
 In der folgenden Tabelle sind alle Ereignisklassen aufgeführt, die in den Ergebnisdaten der Ablaufverfolgung aufgezeichnet werden.  
  
|Category|EventClass-Name|Aufzeichnungshäufigkeit|Zeitpunkt der Aufzeichnung|  
|--------------|---------------------|-----------------------|----------------------|  
|Wiedergebbare Ereignisse|Audit Login|Einmal pro Audit Login-Ereignis in den ursprünglichen Ablaufverfolgungsdaten|Bei Fehlschlagen oder erfolgreichem Abschluss des Ereignisses|  
||Audit Logout|Einmal pro Audit Logout-Ereignis in den ursprünglichen Ablaufverfolgungsdaten|Bei Fehlschlagen oder erfolgreichem Abschluss des Ereignisses|  
||SQL:BatchCompleted|Einmal pro SQL:BatchStarting-Ereignis in den ursprünglichen Ablaufverfolgungsdaten|Bei Fehlschlagen oder erfolgreichem Abschluss des Ereignisses|  
||RPC:Completed|Einmal pro RPC:Starting-Ereignis in den ursprünglichen Ablaufverfolgungsdaten|Bei Fehlschlagen oder erfolgreichem Abschluss des Ereignisses|  
|Statistik und Ergebnisse|Replay Settings-Ereignis|Einmalig|Erstes Ereignis im Ergebnis der Ablaufverfolgung|  
||Replay Statistics-Ereignis|Einmalig|Letztes Ereignis im Ergebnis der Ablaufverfolgung|  
||Replay Result Set-Ereignis|Einmal pro SQL:BatchStarting-Ereignis und RPC:Starting-Ereignis<br /><br /> Nur aufgezeichnet, wenn der Wert der `<RecordResultSet>` -Option in der Wiedergabekonfigurationsdatei auf `Yes`festgelegt wurde.||  
||Replay Result Row-Ereignis|Einmal pro Zeile im Resultset für das SQL:BatchStarting-Ereignis und das RPC:Starting-Ereignis<br /><br /> Nur aufgezeichnet, wenn der Wert der `<RecordResultSet>` -Option in der Wiedergabekonfigurationsdatei auf `Yes`festgelegt wurde.||  
|Fehler und Warnungen|Replay Internal Error|Einmal für jeden internen Fehler|Bei interner Fehlerbedingung|  
||Replay Provider Error|Einmal pro Anbieterfehler|Bei Anbieterfehlerbedingung|  
  
 Beachten Sie Folgendes:  
  
-   Für jedes Ereignis, das erfolgreich auf dem Zielserver wiedergegeben wird, gibt es eine entsprechende Ausgabeereignisklasse.  
  
-   Für jeden Ereignisfehler oder -abbruch werden möglicherweise mehrere Fehler generiert.  
  
## <a name="event-class-column-mapping"></a>Zuordnung von Spalten zu Ereignisklassen  
 In der folgenden Abbildung wird gezeigt, welche Spalten im Ergebnis der Ablaufverfolgung für die einzelnen Typen von Ereignisklassen, die während der Wiedergabe aufgezeichnet werden, verfügbar sind.  
  
 ![Zuordnung von Spalten zu Ereignisklassen](../../tools/distributed-replay/media/eventclassmappings.gif "Zuordnung von Spalten zu Ereignisklassen")  
  
## <a name="column-descriptions-for-result-trace"></a>Beschreibungen der Spalten für das Ergebnis der Ablaufverfolgung  
 In der folgenden Tabelle werden die Spalten der Ergebnisdaten der Ablaufverfolgung beschrieben.  
  
|Name der Datenspalte|Datentyp|BESCHREIBUNG|Column ID|  
|----------------------|---------------|-----------------|---------------|  
|EventClass|**nvarchar**|Der Name der Ereignisklasse.|1|  
|EventSequence|**bigint**|Für Anbieterfehler sowie interne Fehler und Warnungen ist dies die Sequenz der Ereignisaufzeichnung, die dem Fehler bzw. der Warnung entspricht.<br /><br /> Für alle anderen Ereignisklassen ist dies die Sequenz des Ereignisses in den ursprünglichen Ablaufverfolgungsdaten.|2|  
|ReplaySequence|**bigint**|Für Anbieterfehler sowie interne Fehler und Warnungen ist dies die Sequenz der Ereigniswiedergabe, die dem Fehler bzw. der Warnung entspricht.<br /><br /> Für alle anderen Ereignisklassen ist dies die Sequenz des Ereignisses, die während der Wiedergabe zugewiesen wird.|3|  
|TextData|**ntext**|Der Inhalt von TextData hängt von der EventClass ab.<br /><br /> Für Audit Login und ExistingConnection sind dies die festgelegten Optionen für die Verbindung.<br /><br /> Für SQL:BatchStarting ist dies der Text der Batchanforderung.<br /><br /> Für RPC:Starting ist dies die gespeicherte Prozedur, die aufgerufen wurde.<br /><br /> Für das Replay Settings-Ereignis enthält diese Spalte die in der Wiedergabekonfigurationsdatei definierten Einstellungen.<br /><br /> Für das Replay Statistics-Ereignis enthält diese Spalte die folgenden Informationen:<br /><br /> – Die Zielinstanz von SQL Server für die Wiedergabe<br /><br /> – Die Gesamtanzahl der wiedergebbaren Ereignisse<br /><br /> – Die Anzahl der Anbieterfehler<br /><br /> – Die Anzahl der internen Fehler<br /><br /> – Interne Warnungen<br /><br /> – Gesamtzahl der Fehler<br /><br /> – Gesamterfolgsquote<br /><br /> – Die Wiedergabezeit (HH:MM:SS:MMM)<br /><br /> Für das Replay Result Set-Ereignis wird die Liste der Spaltenheader für die Rückgabeergebnisse angezeigt.<br /><br /> Für das Replay Result Row-Ereignis wird der Rückgabewert aller Spalten für diese Zeile angezeigt.<br /><br /> Für Replay Internal Warning und Replay Provider Error enthält diese Spalte die Anbieterwarnungen bzw. -fehler.|4|  
|Attention|**bigint**|Die Beachtungsdauer (in Millisekunden) für das Ereignis. Dies wird anhand des Attention-Ereignisses der Aufzeichnungsablaufverfolgung berechnet. Wenn kein Abfragetimeout für das Ereignis angegeben wurde, wird diese Spalte nicht aufgefüllt (NULL).|5|  
|SubmitTime|**datetime**|Der Zeitpunkt, zu dem das Ereignis an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gesendet wurde.|6|  
|IsSuccessful|**int**|Ein boolesches Flag, das angibt, ob ein bestimmtes Ereignis erfolgreich ausgeführt wurde und Resultsets an den Client zurückgegeben wurden.<br /><br /> Ein Ereignis, das eine Warnung generiert (wenn ein Ereignis etwa aufgrund von Attention oder eines vom Benutzer angegebenen Timeouts abgebrochen wird), gilt als erfolgreich.<br /><br /> IsSuccessful kann die folgenden Werte aufweisen:<br /><br /> 1 = erfolgreich<br /><br /> 0 = Fehler|7|  
|Duration [microsec]|**bigint**|Die Antwortzeitdauer (in Mikrosekunden) für das Ereignis. Die Messung beginnt, wenn das Anmelde-/Abmelde-/RPC-/Sprachereignis an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gesendet wurde.<br /><br /> Bei erfolgreichem Ereignis wird die Messung beendet, wenn das vollständige Resultset verarbeitet wurde.<br /><br /> Wenn das Ereignis nicht erfolgreich ist, endet die Messung zum Zeitpunkt des Fehlschlags oder Abbruchs des Ereignisses.|8|  
|RowCount|**bigint**|Wird abhängig vom Wert von `<RecordRowCount>` in der Wiedergabekonfigurationsdatei aufgefüllt:<br /><br /> Wenn `<RecordRowCount>` gleich Yes ist, enthält diese Zelle die Anzahl der Zeilen im Resultset, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zurückgegeben werden.<br /><br /> Wenn `<RecordRowCount>` gleich No ist, wird diese Zelle nicht aufgefüllt (NULL).|9|  
|CaptureSPID|**int**|Die ID der Aufzeichnungssitzung für das Ereignis.|10|  
|ConnectionID|**int**|Die ID der Aufzeichnungsverbindung für das Ereignis.|11|  
|ReplaySPID|**int**|Die ID der Wiedergabesitzung für das Ereignis.|12|  
|DatabaseName|**nvarchar**|Der Name der Datenbank, in der die Benutzeranweisung ausgeführt wird.|13|  
|LoginName|**nvarchar**|Der Benutzeranmeldename. Dabei kann es sich um eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsanmeldung oder die Microsoft Windows-Anmeldeinformationen im Format *Domänenname*\\*Benutzername* handeln.|14|  
|CaptureHostName|**nvarchar**|Der Name des Computers, auf dem der Clientdienst während der Aufzeichnung ausgeführt wird.|15|  
|ReplayHostName|**nvarchar**|Der Name des Computers, auf dem der Client während der Wiedergabe ausgeführt wird|16|  
|ApplicationName|**nvarchar**|Der Name der Clientanwendung, die während der Aufzeichnung die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verbindung hergestellt hat.|17|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Distributed Replay: Anforderungen](../../tools/distributed-replay/distributed-replay-requirements.md)   
 [Befehlszeilenoptionen für das Verwaltungstool &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)   
 [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)  
  
  
