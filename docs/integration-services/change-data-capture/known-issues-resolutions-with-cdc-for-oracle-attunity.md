---
description: Bekannte Fehler und Lösungen bei Change Data Capture für Oracle von Attunity
title: Bekannte Fehler und Lösungen bei Change Data Capture für Oracle von Attunity | Microsoft-Dokumentation
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a898b268de4f339d26a6720c53356723ad56b285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062120"
---
# <a name="known-errors-and-resolutions-with-change-data-capture-for-oracle-by-attunity"></a>Bekannte Fehler und Lösungen bei Change Data Capture für Oracle von Attunity
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]

In diesem Thema werden die wichtigsten Probleme und bekannten Lösungen beim Anzeigen einer CDC-Instanz (Change Data Capture) im Oracle CDC Designer-Konfigurationstool aufgelistet. Dieses Tool ist Bestandteil von Change Data Capture für Oracle von Attunity, das seit SQL Server 2012 im Lieferumfang enthalten ist. 

## <a name="bug-fixes"></a>Behebung von Programmfehlern
Bevor Sie allzu viel Zeit für Problembehandlung aufwenden, achten Sie darauf, die neuesten Builds von CDC für Oracle von Attunity zu verwenden, um bekannte Probleme wie die folgenden zu vermeiden:

### <a name="sql-server-2017"></a>SQL Server 2017

**Version 5.0.0.111** enthält diese Korrekturen:
- Fehlerbehebung: Beim Hinzufügen einer Oracle-Tabelle tritt im Oracle CDC-Designer der Fehler „Incorrect syntax near the keyword ‚KEY‘“ (Syntaxfehler in der Nähe des Schlüsselworts ‚KEY‘) auf. 
- Verbesserung: verbesserte Unterstützung für RAC. Dies schließt eine bessere Behandlung beim Neustart eines RAC-Knotens ein. 
- Fehlerbehebung: CDC funktioniert nicht mit Oracle 10.2 aufgrund der Anforderung von NEXT_CHANGE# aus dem v$log. 


**Version 5.0.0.93** enthält diese Korrekturen: 
- Fehler „Incorrect syntax near the word ‚KEY‘“ (Syntaxfehler in der Nähe des Worts ‚KEY‘) in Microsoft CDC für Oracle von Attunity Designer beim Hinzufügen einer Oracle-Tabelle. 

 
### <a name="sql-server-2016"></a>SQL Server 2016

**Version 4.0.107** enthält diese Korrekturen:
- Fehlerbehebungen: Beim Hinzufügen einer Oracle-Tabelle tritt im Oracle CDC-Designer der Fehler „Incorrect syntax near the keyword ‚KEY‘“ (Syntaxfehler in der Nähe des Schlüsselworts ‚KEY‘) auf.
- Verbesserung: verbesserte Unterstützung für RAC. Dies schließt ein besseres Neustartverhalten von RAC-Knoten ein.
- Fehlerbehebungen: CDC funktioniert nicht mit Oracle 10.2 aufgrund der Anforderung von NEXT_CHANGE# aus dem v$log.

**Version 4.0.0.95** enthält diese Korrekturen: 
- Fehlerbehebungen: Beim Hinzufügen einer Oracle-Tabelle tritt im Oracle CDC-Designer der Fehler „Incorrect syntax near the keyword ‚KEY‘“ (Syntaxfehler in der Nähe des Schlüsselworts ‚KEY‘) auf.

**Version 4.0.0.88** enthält diese Korrekturen:
-  In den erweiterten Optionen der Attunity CDC-Instanz hinzugefügte Eigenschaften werden entfernt, wenn eine Tabelle zu CDC hinzugefügt oder daraus entfernt wird. 
- Attunity CDC funktioniert nach dem Anwenden einer SQL-Korrektur, die „__$command_id column“ hinzufügt, nicht mehr

### <a name="sql-server-2014"></a>SQL Server 2014 

**Version 2.0.0.114** enthält diese Korrekturen:
- Fehlerbehebungen: Beim Hinzufügen einer Oracle-Tabelle tritt im Oracle CDC-Designer der Fehler „Incorrect syntax near the keyword ‚KEY‘“ (Syntaxfehler in der Nähe des Schlüsselworts ‚KEY‘) auf.
- Verbesserung: verbesserte Unterstützung für RAC. Dies schließt ein besseres Neustartverhalten von RAC-Knoten ein.
- Fehlerbehebungen: CDC funktioniert nicht mit Oracle 10.2 aufgrund der Anforderung von NEXT_CHANGE# aus dem v$log.

**Version 2.0.0.92** enthält diese Korrekturen: 
- In den erweiterten Optionen der Attunity CDC-Instanz hinzugefügte Eigenschaften werden entfernt, wenn eine Tabelle zu CDC hinzugefügt oder daraus entfernt wird. Attunity CDC funktioniert nach dem Anwenden einer SQL-Korrektur, die „__$command_id column“ hinzufügt, nicht mehr
- Fehler bei der Überprüfung der Metadaten für die Oracle-Tabelle „cdc.table_name“. Der column_name-Spaltenindex liegt außerhalb des zulässigen Bereichs.  Ferner dieses Problem: Der Oracle CDC-Dienst zeigt den Status „abgebrochen“ an, wenn CDC für Oracle von Attunity verwendet wird
    - Behoben in _Kumulatives Update 1 für SQL Server 2014 RTM_, wie in KB [2894025](https://support.microsoft.com/kb/2894025) beschrieben.
- Einige Änderungen fehlen und werden nicht in die SQL Server-Datenbank repliziert. Dieses Problem tritt auf, wenn eine Tabelle mehr als ein CLOB (Character Large Binary Object) enthält und eins der CLOBs einen großen Wert aufweist. 
    - Behoben in _Kumulatives Update 1 für SQL Server 2014 SP1_ und _Kumulatives Update 8 für SQL Server 2014 RTM_, wie in KB [3029096](https://support.microsoft.com/kb/3029096) beschrieben. 
- Change Data Capture für Oracle von Attunity funktioniert nicht mehr, wenn Oracle-Tabellen Spalten mit Long-Datentyp aufweisen.
    - Behoben in _Kumulatives Update 5 für SQL Server 2014 SP1_ und _Kumulatives Update 12 für SQL Server 2014 RTM_, wie in KB [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d) beschrieben.

### <a name="sql-server-2012"></a>SQL Server 2012

**Version 1.1.0.102** enthält diese Korrekturen: 
 
- In den erweiterten Optionen der Attunity CDC-Instanz hinzugefügte Eigenschaften werden entfernt, wenn eine Tabelle zu CDC hinzugefügt oder daraus entfernt wird. Attunity CDC funktioniert nach dem Anwenden einer SQL-Korrektur, die „__$command_id column“ hinzufügt, nicht mehr
- Die CDC für Oracle-Instanz hängt beim Start und erfasst keine Änderungen. Der Oracle-Serverarbeitsspeicher vergrößert sich, bis kein Arbeitsspeicher mehr vorhanden ist oder ein Absturz auftritt.
- [2672759](https://support.microsoft.com/kb/2672759): Fehlermeldung beim Verwenden des Change Data Capture Service für Oracle von Attunity: „ORA-00600: internal error code“ („ORA-00600: interner Fehlercode“). Fügen Sie Nachverfolgung auf der SOURCE-Ebene hinzu, und überprüfen Sie, ob der gleiche ORA-00600-Fehler auftritt. Behoben durch den Download eines Oracle-Patches.
- Mehrere Partitionen
    - Wenn Sie mehr als 10 Partitionen in einer Oracle-Tabelle verwenden, kann die CDC-Instanz nicht alle Änderungen in der Tabelle erfassen. Wenn die Oracle-Tabelle mit mehr als 10 Partitionen definiert wurde, werden nur die Änderungen aus den letzten 10 Partitionen erfasst. Korrigiert in der _Service Pack 1-Version für SQL Server 2012_. Weitere Informationen finden Sie auf der [Downloadseite für das SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575). 
- Änderungen gehen verloren
    - Die Ereigniserfassung kann in einer Endlosschleife münden und die Erfassung neuer Datenänderungen beenden (zusammenhängend mit Oracle-Fehler 5623813). Wenn die CDC-Instanz in einer Oracle RAC-Umgebung beendet oder fortgesetzt wird, können Änderungen übersprungen/verloren werden. Dies bedeutet, dass in der SQL Server Change Data Capture wichtige Zeilen fehlen, sodass es zu Datenverlust im Data Warehouse oder dem abonnierenden System kommt. Korrigiert in der _Service Pack 1-Version für SQL Server 2012_. Weitere Informationen finden Sie auf der [Downloadseite für das SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)
- Doppelte Breite für Spalten in SQL
    - Beim Erstellen einer CDC für Oracle-Instanz in den Skripts, die für SQL Server ausgeführt werden, wird die Länge einer Spalte mit einem Datentyp variabler Breite in SQL Server-Tabellen, die im Skript erstellt werden, verdoppelt. Wenn Sie beispielsweise versuchen, Änderungen in einer VARCHAR2(10)-Spalte in einer Oracle-Tabelle nachzuverfolgen, ist die entsprechende SQL Server-Tabelle im Bereitstellungsskript NVARCHAR(20). Behoben in _Kumulatives Update 2 für SQL Server 2012 SP1_ und _Kumulatives Update 5 für SQL Server 2012_, wie in KB [2769673](https://support.microsoft.com/kb/2769673) beschrieben. 
- DDL-Daten werden abgeschnitten
    - Wenn Sie eine DDL-Anweisung (Data Definition Language) mit einer Größe von mehr als 4.000 Byte für eine Oracle-Datenbank ausführen, die nicht dem lateinischen Alphabet entstammende Zeichen enthält, tritt ein Fehler bei CDC für Oracle von Attunity auf. Außerdem wird die Fehlermeldung `ORA-01406: fetched column value was truncated.` angezeigt. Behoben in _Kumulatives Update 4 für SQL Server 2012 SP1_, wie in KB [2839806](https://support.microsoft.com/kb/2839806) beschrieben. 
- Die Änderungen in den letzten zwei Spalten gehen verloren
    - Behoben in _Kumulatives Update 6 für SQL Server 2012 SP1_, wie in KB [2874879](https://support.microsoft.com/kb/2874879) beschrieben. 
- Das SQL-Transaktionsprotokoll wächst, wenn Sie CDC für Oracle verwenden
     - Wenn Instanzen von Change Data Capture für Oracle konfiguriert werden, weist die SQL-Datenbank, die die geänderten Daten empfängt, gespiegelte Tabellen mit für die Replikation gekennzeichneten Transaktionen auf. Dieses Verhalten tritt auf, weil CDC für Oracle auf zugrundeliegenden gespeicherten Systemprozeduren aufbaut, die denen ähneln, die in CDC für SQL Server verwendet werden. Da aber keine SQL-CDC-Replikation stattfindet, wenn CDC für Oracle eigenständig verwendet wird, gibt es keinen Protokollleser zum Löschen der für die Replikation gekennzeichneten Transaktionen. Da die Transaktion nicht in SQL Server repliziert werden muss, kann sie ohne Bedenken mithilfe der später in diesem Artikel beschriebenen Problemumgehung manuell als verteilt gekennzeichnet werden. Weitere Informationen finden Sie in KB [2871474](https://support.microsoft.com/kb/2871474). 
- Fehler `ORACDC000T: Error encountered at position to change event - SCN not found - EOF simulated`. Behoben in _Kumulatives Update 7 für SQL Server 2012 SP1_, wie in KB [2883524](https://support.microsoft.com/kb/2883524) beschrieben. 
- Fehler bei der Überprüfung der Metadaten für die Oracle-Tabelle „cdc.table_name“. Der column_name-Spaltenindex liegt außerhalb des zulässigen Bereichs. Behoben in _Kumulatives Update 7 für SQL Server 2012 SP1_, wie in KB [2883524](https://support.microsoft.com/kb/2883524) beschrieben.
- Der Oracle CDC-Dienst zeigt den Status „abgebrochen“ an, wenn CDC für Oracle von Attunity in SQL Server 2012 verwendet wird. Behoben in _Kumulatives Update 8 für SQL Server 2012 SP1_, wie in KB [2923839](https://support.microsoft.com/kb/2923839) beschrieben.  
- Einige Änderungen fehlen und werden nicht in die SQL Server-Datenbanken repliziert. Dieses Problem tritt auf, wenn eine Tabelle mehr als ein CLOB (Character Large Binary Object) enthält und eins der CLOBs einen großen Wert aufweist. Behoben in _Kumulatives Update 8 für SQL Server 2012 SP1_, wie in KB [2923839](https://support.microsoft.com/kb/2923839) beschrieben.   
- Change Data Capture für Oracle von Attunity funktioniert nicht mehr, wenn Oracle-Tabellen Spalten mit Long-Datentyp aufweisen. Behoben in _Kumulatives Update 2 für SQL Server 2012 SP3_ und _Kumulatives Update 11 für SQL Server 2012 SP2_, wie in KB [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d) beschrieben. 

## <a name="collect-detailed-logs"></a>Erfassen ausführlicher Protokolle 

In diesem Abschnitt wird erläutert, wie Fehler und Ereignisse aus der CDC-Instanz erfasst werden. 

### <a name="management-console"></a>Verwaltungskonsole

Wenn im linken Bereich eine CDC-Instanz hervorgehoben ist, können Sie Fehler in den **Statusmeldungen** sehen, die in der Verwaltungskonsole des Oracle Change Data Capture-Designers abgelegt wurden. 

### <a name="query-trace-table"></a>Abfragen der Ablauftabelle

Sie können die Ablauftabelle in der CDC-Datenbank innerhalb von SQL Server abfragen, um protokollierte Fehler anzuzeigen. 

### <a name="save-output-from-basic-logging"></a>Speichern der Ausgabe der Standardprotokollierung 

Wählen Sie zum Erfassen von Diagnosedaten auf der Statusregisterkarte in der Verwaltungskonsole von Oracle Change Data Capture **Collect Diagnostics** (Diagnosedaten sammeln) aus. 

![Screenshot: Registerkarte „Status“ in der Verwaltungskonsole von Oracle Change Data Capture mit umrandeter Option „Diagnosedaten sammeln“](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

Wählen Sie eine Anfangszeit und einen Speicherort für die Protokolldatei aus. Wählen Sie anschließend **Erstellen** aus, um die Erfassung von Diagnosedaten zu starten. 

![Screenshot: Dialogfeld „Collect Diagnostics for testTA“ (Diagnosedaten für testTA sammeln)](media/known-issues-resolutions-with-cdc-for-oracle-attunity/start-diagnostics.png)

### <a name="detailed-errors"></a>Detaillierte Fehler

Sie können den Grad der von der Instanz erfassten Ablaufverfolgung heraufsetzen und das Szenario wiederholen, um detailliertere Protokollierung zu erfassen. Wählen Sie hierzu unter **Aktionen** **Eigenschafen** aus, und fügen Sie dann auf der Registerkarte **Erweitert** eine neue Eigenschaft **Erweiterte Eigenschaften** hinzu. Legen Sie den Namen der Eigenschaft auf `trace` und dann den Wert auf `SOURCE` fest. 

![Screenshot: Option „Eigenschaften“ unter „Aktionen“](media/known-issues-resolutions-with-cdc-for-oracle-attunity/properties.png)

Reproduzieren Sie den Fehler, und aktivieren Sie dann die Option **Diagnosedaten sammeln**, um Protokolle zu erfassen. 

![Weiterer Screenshot der Registerkarte „Status“ in der Verwaltungskonsole von Oracle Change Data Capture mit umrandeter Option „Diagnosedaten sammeln“](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

## <a name="ora-00942-table-of-view-does-not-exist"></a>ORA-00942 Table of view does not exist (ORA-00942 Ansichtstabelle ist nicht vorhanden) 

Dies ist ein häufiger Fehler, der im Feld **Statusmeldung** der CDC-Instanz angezeigt wird. Die Instanz führt mehrere Wiederholungsversuche aus, sodass das Statussymbol vorübergehend zu grün wechselt, schließlich tritt aber ein Fehler mit rotem Ausrufezeichen und dem Status UNEXPECTED (UNERWARTET) auf. 

![Screenshot: Im Feld „Statusmeldung“ der CDC-Instanz angezeigter häufiger Fehler](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-error.png)

```
"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC508E:Oracle method OCIStmtExecute failed with error: ORA-00942: table or view does not exist ","source","",""

"ERROR","computername","RUNNING","IDLE",
"ORACDC518E:Failed to verify archive log mode.","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC517E:Oracle Call Interface (OCI) method failed: ORA-00942: table or view does not exist .","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC414E:The Engine component failed with return code 1.","engine","",""
```

Dies tritt auf, wenn das Oracle-Konto, das die Verbindung der CDC-Instanz mit dem Oracle-Server herstellt, keine Berechtigung zum Anzeigen von Systemprotokollansichten besitzt. 

### <a name="resolution"></a>Lösung

Um diesen Fehler zu beheben, erteilen Sie entweder dem aktuell konfigurierten Benutzer angemessene Berechtigungen innerhalb des Oracle-Datenbanksystems, oder ändern Sie das für Verbindungen von der CDC-Instanz mit dem Oracle-Server verwendete Konto. 

Die Liste aller erforderlichen Berechtigungen ist ausführlich in der Hilfedatei aufgeführt, die im Installationsordner der Programmdateien enthalten ist `C:\Program Files\Change Data Capture for Oracle by Attunity\Attunity.SqlServer.XdbCdcDesigner.chm`.  Die vollständige Liste finden Sie auf der Seite „Connect to an Oracle Source Database“ (Herstellen einer Verbindung mit einer Oracle-Quelldatenbank) in der CHM-Datei.

Sie können das Benutzerkonto festlegen, indem Sie die CDCInstance im linken Bereich und dann die Schaltfläche „Eigenschaften“ im Aktionsbereich ganz rechts im **Change Data Capture Designer**-Fenster auswählen. Sie können das Konto für die Oracle Log Mining Authentication auf der Dialogseite „Eigenschaften“ ändern.

![Screenshot: Registerkarte „Oracle“ des Dialogfelds „testTA Properties“ (testTA-Eigenschaften)](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-connection.png)


  
## <a name="see-also"></a>Siehe auch  
 [Nachverfolgen von Datenänderungen &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Über Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Arbeiten mit Änderungsdaten &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Verwalten und Überwachen von Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
