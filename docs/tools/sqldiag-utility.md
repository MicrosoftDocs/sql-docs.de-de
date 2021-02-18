---
title: SQLdiag (Hilfsprogramm)
description: Mit dem Hilfsprogramm SQLdiag können Sie Protokolle und Datendateien von SQL Server und anderen Servertypen sammeln und Server langfristig überwachen oder Probleme beheben.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], SQLdiag
- stopping diagnostic collection
- storing diagnostic information
- performance [SQL Server], diagnostic collection
- diagnostic records [SQL Server]
- scripts [SQL Server], diagnostic collection
- logs [SQL Server], diagnostic collection
- starting diagnostic collection
- clustered instance of SQL Server
- monitoring performance [SQL Server], diagnostic collection
- security [SQL Server], diagnostic collection
- SQLDIAG service
- space [SQL Server], diagnostic collection
- SQLdiag utility
- disk space [SQL Server], diagnostic collection
- configuration files [SQL Server]
- automatic diagnostic collection
- clusters [SQL Server], diagnostic collection
ms.assetid: 45ba1307-33d1-431e-872c-a6e4556f5ff2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c9f3528ac4f377e379afd26e6583df3fe63fb2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353335"
---
# <a name="sqldiag-utility"></a>SQLdiag (Hilfsprogramm)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Das Hilfsprogramm **SQLDiag** listet allgemeine Diagnoseinformationen auf und kann als Konsolenanwendung oder Dienst ausgeführt werden. Mithilfe von **SQLDiag** können Sie Protokolle und Datendateien von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und anderen Servertypen sammeln. Dies kann hilfreich sein, um Server für eine gewisse Zeit zu überwachen oder bestimmte Serverprobleme zu behandeln. **SQLDiag** dient dazu, das Sammeln von Diagnoseinformationen für [!INCLUDE[msCoName](../includes/msconame-md.md)] Support Services zu beschleunigen und zu vereinfachen.  
  
> [!NOTE]  
>  Dieses Hilfsprogramm kann weiter geändert werden, und es besteht die Möglichkeit, dass Anwendungen oder Skripts, die auf den Befehlszeilenargumenten und dem entsprechenden Verhalten dieses Hilfsprogramms aufbauen, in zukünftigen Versionen nicht ordnungsgemäß ausgeführt werden.  
  
 Mit **SQLDiag** können die folgenden Arten von Diagnoseinformationen gesammelt werden:  
  
-   Windows-Leistungsprotokolle  
  
-   Windows-Ereignisprotokolle  
  
-   [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] Ablaufverfolgungen  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Blockierungsinformationen  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Konfigurationsinformationen  
  
 Sie können angeben, welche Informationen **SQLDiag** sammeln soll, indem Sie die Konfigurationsdatei „SQLDiag.xml“ bearbeiten. Dies wird im nächsten Abschnitt weiter beschrieben.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqldiag   
     { [/?] }  
     |  
     { [/I configuration_file]  
       [/O output_folder_path]  
       [/P support_folder_path]  
       [/N output_folder_management_option]  
       [/M machine1 [ machine2 machineN]| @machinelistfile]  
       [/C file_compression_type]  
       [/B [+]start_time]  
       [/E [+]stop_time]  
       [/A SQLdiag_application_name]  
       [/T { tcp [ ,port ] | np | lpc } ]  
       [/Q] [/G] [/R] [/U] [/L] [/X] }  
     |  
     { [START | STOP | STOP_ABORT] }  
     |  
     { [START | STOP | STOP_ABORT] /A SQLdiag_application_name }  
```  
  
## <a name="arguments"></a>Argumente  
 **/?**  
 Zeigt Informationen zur Nutzung an.  
  
 **/I** _configuration_file_  
 Legt die Konfigurationsdatei fest, die **SQLDiag** verwenden soll. Standardmäßig wird mit **/I** die Datei SQLDiag.Xml festgelegt.  
  
 **/O** _output_folder_path_  
 Leitet die Ausgabe von **SQLDiag** in den angegebenen Ordner um. Wenn die Option **/O** nicht angegeben wird, wird die Ausgabe von **SQLDiag** in den Unterordner SQLDIAG unterhalb des Startordners **SQLDiag** geschrieben. Wenn der Ordner SQLDIAG nicht vorhanden ist, versucht **SQLDiag** , ihn zu erstellen.  
  
> [!NOTE]  
>  Der Speicherort des Ausgabeordners ist relativ zum Speicherort des Unterstützungsordners, der mit **/P** angegeben werden kann. Geben Sie den vollständigen Verzeichnispfad für **/O** an, um einen gänzlich anderen Speicherort für den Ausgabeordner festzulegen.  
  
 **/P** _support_folder_path_  
 Legt den Pfad für den Unterstützungsordner fest. Standardmäßig wird für **/P** der Ordner festgelegt, in dem sich die ausführbaren Dateien von **SQLDiag** befinden. Der Unterstützungsordner enthält **SQLDiag** -Unterstützungsdateien, wie die XML-Konfigurationsdatei, Transact-SQL-Skripts und andere Dateien, die das Hilfsprogramm zum Auflisten von Diagnoseinformationen verwendet. Wenn Sie mithilfe dieser Option einen anderen Pfad für die Unterstützungsdateien angeben, kopiert **SQLDiag** automatisch die erforderlichen Unterstützungsdateien in den angegebenen Ordner, sofern sie noch nicht vorhanden sind.  
  
> [!NOTE]  
>  Soll der aktuelle Ordner als Unterstützungspfad festgelegt werden, geben Sie **%cd%** folgendermaßen in der Befehlszeile an:  
>   
>  **SQLDIAG /P %cd%**  
  
 **/N** _output_folder_management_option_  
 Legt fest, ob **SQLDiag** den Ausgabeordner beim Starten überschreibt oder umbenennt. Verfügbare Optionen:  
  
 1 = Überschreibt den Ausgabeordner (Standardeinstellung)  
  
 2 = Beim Starten von **SQLDiag** wird der Ausgabeordner in SQLDIAG_00001, SQLDIAG_00002 usw. umbenannt. Nachdem der aktuelle Ausgabeordner umbenannt wurde, schreibt **SQLDiag** die Ausgabe in den Standardausgabeordner SQLDIAG.  
  
> [!NOTE]  
>  **SQLDiag** fügt beim Starten die Ausgabe nicht an den aktuellen Ausgabeordner an. Das Programm kann nur den Standardausgabeordner überschreiben (Option 1) oder den Ordner umbenennen (Option 2) und dann die Ausgabe in den neuen Standardausgabeordner namens SQLDIAG schreiben.  
  
 **/M** _machine1_ [ *machine2* *machineN*] | *\@machinelistfile*  
 Überschreibt die in der Konfigurationsdatei angegebenen Computer. Standardmäßig ist die Konfigurationsdatei SQLDiag.Xml oder wird mit dem **/I** -Parameter festgelegt. Wenn Sie mehr als einen Computer angeben möchten, verwenden Sie Leerzeichen zum Trennen der Computernamen.  
  
 Durch Verwenden von *\@machinelistfile* wird ein Computerlisten-Dateiname angegeben, der in der Konfigurationsdatei gespeichert werden muss.  
  
 **/C** _file_compression_type_  
 Legt den Typ der Dateikompression fest, die für die Dateien im Ausgabeordner von **SQLDiag** verwendet wird. Verfügbare Optionen:  
  
 0 = keine Komprimierung (Standardeinstellung)  
  
 1 = NTFS-Komprimierung  
  
 **/B** [ **+** ]*start_time*  
 Gibt das Datum und die Uhrzeit des Zeitpunkts, an dem die Sammlung von Diagnoseinformationen gestartet werden soll, im folgenden Format an:  
  
 YYYYMMDD_HH:MM:SS  
  
 Die Uhrzeit wird in 24-Stunden-Notation angegeben. Beispiel: 2 Uhr nachmittags sollte für **14:00:00** angegeben werden.  
  
 Verwenden Sie **+** ohne Datumsangabe (also nur HH:MM:SS), um eine Uhrzeit relativ zum aktuellen Datum und zur aktuellen Uhrzeit anzugeben. Wenn Sie beispielsweise **/B +02:00:00** angeben, wartet **SQLDiag** 2 Stunden, bevor die Sammlung von Informationen gestartet wird.  
  
 Fügen Sie kein Leerzeichen zwischen **+** und dem angegebenen Wert für *start_time* ein.  
  
 Wenn Sie eine Startzeit angeben, die in der Vergangenheit liegt, erzwingt **SQLDiag** eine Änderung des Startdatums, sodass Startdatum und -zeit in der Zukunft liegen. Wenn Sie beispielsweise **/B 01:00:00** angeben und es schon 08:00:00 Uhr ist, ändert **SQLDiag** das Startdatum und legt den nächsten Tag als Startdatum fest.  
  
 Beachten Sie, dass **SQLDiag** die lokale Zeit auf dem Computer verwendet, auf dem das Hilfsprogramm ausgeführt wird.  
  
 **/E** [ **+** ]*stop_time*  
 Gibt das Datum und die Uhrzeit des Zeitpunkts, an dem die Sammlung von Diagnoseinformationen beendet werden soll, im folgenden Format an:  
  
 YYYYMMDD_HH:MM:SS  
  
 Die Uhrzeit wird in 24-Stunden-Notation angegeben. Beispiel: 2 Uhr nachmittags sollte für **14:00:00** angegeben werden.  
  
 Verwenden Sie **+** ohne Datumsangabe (also nur HH:MM:SS), um eine Uhrzeit relativ zum aktuellen Datum und zur aktuellen Uhrzeit anzugeben. Wenn Sie beispielsweise **/B +02:00:00 /E +03:00:00** angeben, um eine Start- und eine Beendigungszeit festzulegen, wartet **SQLDiag** 2 Stunden, bevor die Sammlung von Informationen gestartet wird. Daraufhin werden 3 Stunden lang Informationen gesammelt, bevor der Vorgang endet und das Hilfsprogramm beendet wird. Wenn **/B** nicht angegeben wird, beginnt **SQLDiag** sofort mit dem Sammeln von Diagnoseinformationen und beendet den Vorgang an dem Datum und der Uhrzeit, das bzw. die durch **/E** angegeben wurde.  
  
 Fügen Sie kein Leerzeichen zwischen **+** und dem angegebenen Wert für *start_time* oder *end_time* ein.  
  
 Beachten Sie, dass **SQLDiag** die lokale Zeit auf dem Computer verwendet, auf dem das Hilfsprogramm ausgeführt wird.  
  
 **/A** _SQLdiag_application_name_  
 Ermöglicht das Ausführen mehrerer Instanzen des Hilfsprogramms **SQLDiag** für dieselbe Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Jeder *SQLDiag_application_name* identifiziert eine andere Instanz von **SQLDiag**. Zwischen einer Instanz von *SQLDiag_application_name* und einem Instanznamen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] besteht keine Beziehung.  
  
 *SQLDiag_application_name* kann zum Starten oder Beenden einer bestimmten Instanz des **SQLDiag** -Diensts verwendet werden.  
  
 Beispiel:  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
 Das Argument kann zusammen mit der Option **/R** auch zum Registrieren einer bestimmten Instanz von **SQLDiag** als Dienst verwendet werden. Beispiel:  
  
 **SQLDIAG /R /A** _SQLdiag_application_name_  
  
> [!NOTE]  
>  **SQLDiag** setzt vor den für *SQLDiag_application_name* angegebenen Instanznamen automatisch das Präfix DIAG$. Hierdurch kann beim Registrieren von **SQLDiag** als Dienst ein aussagefähiger Dienstname bereitgestellt werden.  
  
 /T { tcp [ ,*port* ] | np | lpc }  
 Stellt eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Verwendung des angegebenen Protokolls her.  
  
 tcp [,*port*]  
 Transmission Control Protocol/Internet Protocol (TCP/IP). Sie können optional eine Portnummer für die Verbindung angeben.  
  
 np  
 Named Pipes. Standardmäßig überwacht die Standardinstanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die Named Pipe `\\.\pipe\sql\query` und `\\.\pipe\MSSQL$<instancename>\sql\query` auf eine benannte Instanz. Sie können keine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Verwendung eines alternativen Pipenamens herstellen.  
  
 lpc  
 Lokaler Prozeduraufruf. Dieses Shared Memory-Protokoll ist verfügbar, wenn der Client eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf demselben Computer herstellt.  
  
 **/Q**  
 Führt **SQLDiag** im stillen Modus aus. **/Q** unterdrückt alle Aufforderungen, z.B. Aufforderungen zur Angabe eines Kennworts.  
  
 **/G**  
 Führt **SQLDiag** im generischen Modus aus. Wenn **/G** angegeben wird, wird beim Starten von **SQLDiag** weder eine [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konnektivitätsprüfung erzwungen, noch wird überprüft, ob der Benutzer Mitglied der festen Serverrolle **sysadmin** ist. Stattdessen orientiert sich **SQLDiag** an Windows, um zu bestimmen, ob ein Benutzer die erforderlichen Rechte zum Sammeln der angeforderten Diagnoseinformationen besitzt.  
  
 Wenn **/G** nicht angegeben wird, führt **SQLDiag** eine Überprüfung durch, um zu bestimmen, ob der Benutzer ein Mitglied der Windows-Gruppe **Administratoren** ist. Gehört der Benutzer nicht der Gruppe [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Administratoren **an, werden keine** -Diagnoseinformationen gesammelt.  
  
 **/R**  
 Registriert **SQLDiag** als Dienst. Alle Befehlszeilenargumente, die angegeben werden, wenn Sie **SQLDiag** als Dienst registrieren, werden für die künftige Ausführung des Diensts beibehalten.  
  
 Wenn **SQLDiag** als Dienst registriert wird, lautet der Standarddienstname SQLDIAG. Sie können den Dienstnamen mithilfe des **/A** -Arguments ändern.  
  
 Starten Sie den Dienst mit dem Befehlszeilenargument **START** :  
  
 **SQLDIAG START**  
  
 Sie können auch den Befehl **net start** verwenden, um den Dienst zu starten.  
  
 **net  start SQLDIAG**  
  
 **/U**  
 Hebt die Registrierung von **SQLDiag** als Dienst auf.  
  
 Verwenden Sie das **/A** -Argument auch zum Aufheben der Registrierung einer benannten Instanz von **SQLDiag** .  
  
 **/L**  
 Führt **SQLDiag** im fortlaufenden Modus aus, wenn mit den Argumenten **/B** oder **/E** auch eine Startzeit bzw. Beendigungszeit angegeben wird. **SQLDiag** wird automatisch neu gestartet, nachdem die Sammlung von Diagnoseinformationen aufgrund eines geplanten Herunterfahrens beendet wurde. Dies ist beispielsweise beim Verwenden des Arguments **/E** oder **/X** der Fall.  
  
> [!NOTE]  
>  **SQLDiag** ignoriert das **/L** -Argument, wenn mit den Befehlszeilenargumenten **/B** und **/E** keine Startzeit bzw. Beendigungszeit angegeben wird.  
  
 Das Verwenden von **/L** schließt den Dienstmodus nicht automatisch ein. Wenn Sie **/L** beim Ausführen von **SQLDiag** als Dienst verwenden möchten, müssen Sie die Option in der Befehlszeile angeben, wenn Sie den Dienst registrieren.  
  
 **/X**  
 Führt **SQLDiag** im Momentaufnahmemodus aus. **SQLDiag** erstellt eine Momentaufnahme aller konfigurierten Diagnosesammlungen und wird dann automatisch heruntergefahren.  
  
 **START** | **STOP** | **STOP_ABORT**  
 Startet oder beendet den **SQLDiag** -Dienst. **STOP_ABORT** zwingt den Dienst, so schnell wie möglich herunterzufahren, ohne die aktuell durchgeführte Sammlung von Diagnoseinformationen abzuschließen.  
  
 Wenn diese Dienstkontrollargumente verwendet werden, müssen sie als erstes Argument in der Befehlszeile angegeben werden. Beispiel:  
  
 **SQLDIAG START**  
  
 Nur das **/A** -Argument, das eine benannte Instanz von **SQLDiag** angibt, kann zusammen mit **START**, **STOP** oder **STOP_ABORT** verwendet werden, um eine bestimmte Instanz des **SQLDiag** -Diensts zu steuern. Beispiel:  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
## <a name="security-requirements"></a>Sicherheitsanforderungen  
 Sofern **SQLDiag** nicht im generischen Modus (durch Angeben des Befehlszeilenarguments **/G** ) ausgeführt wird, muss der Benutzer, der **SQLDiag** ausführt, Mitglied der Windows-Gruppe **Administratoren** und der festen [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Serverrolle **sysadmin** sein. Standardmäßig verwendet **SQLDiag** die Windows-Authentifizierung für die Verbindung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierung wird jedoch ebenfalls unterstützt.  
  
## <a name="performance-considerations"></a>Überlegungen zur Leistung  
 Inwiefern sich die Ausführung von **SQLDiag** auf die Leistung auswirkt, hängt von der Art der Diagnosedaten ab, deren Sammlung konfiguriert wurde. Wenn Sie **SQLDiag** z.B. so konfiguriert haben, dass [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsdaten gesammelt werden, wirkt sich dies umso stärker auf die Serverleistung aus, je mehr Ereignisklassen Sie für die Ablaufverfolgung ausgewählt haben.  
  
 Die Leistungseinbußen, die sich durch das Ausführen von **SQLDiag** ergeben, entsprechen ungefähr der Summe der Kosten, die sich durch die getrennte Sammlung der konfigurierten Diagnosedaten ergeben. So entstehen z.B. durch die Sammlung von Ablaufverfolgungsdaten mithilfe von **SQLDiag** dieselben Leistungskosten wie durch die Sammlung mit [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. Die Leistungseinbußen bei Verwendung von **SQLDiag** sind unerheblich.  
  
## <a name="required-disk-space"></a>Erforderlicher Speicherplatz  
 Da **SQLDiag** verschiedene Arten von Diagnoseinformationen sammeln kann, variiert auch der freie Speicherplatz, der für das Ausführen von **SQLDiag** erforderlich ist. Die Menge der gesammelten Diagnoseinformationen hängt von der Eigenart und dem Umfang der Arbeitsauslastung ab, die der Server verarbeitet, und kann von einigen Megabytes bis hin zu vielen Gigabytes reichen.  
  
## <a name="configuration-files"></a>Konfigurationsdateien  
 Beim Start liest **SQLDiag** die Konfigurationsdatei und die angegebenen Befehlszeilenargumente. In der Konfigurationsdatei geben Sie die Art der Diagnoseinformationen, die mit **SQLDiag** gesammelt werden. Standardmäßig verwendet **SQLDiag** die Konfigurationsdatei SQLDiag.Xml, die sich im Startordner des Hilfsprogramms **SQLDiag** befindet und bei jeder Ausführung des Tools extrahiert wird. Die Konfigurationsdatei verwendet das XML-Schema, SQLDiag_schema.xsd, das ebenfalls bei jedem Ausführen von **SQLDiag** aus der ausführbaren Datei in das Startverzeichnis des Hilfsprogramms extrahiert wird.  
  
### <a name="editing-the-configuration-files"></a>Bearbeiten der Konfigurationsdateien  
 Sie können SQLDiag.Xml kopieren und bearbeiten, um die Art der von **SQLDiag** gesammelten Diagnosedaten zu ändern. Bearbeiten Sie die Konfigurationsdatei immer mit einem XML-Editor, der die Konfigurationsdatei anhand des XML-Schemas überprüfen kann (z. B. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]). SQLDiag.Xml sollte nicht direkt bearbeitet werden. Erstellen Sie stattdessen eine Kopie von SQLDiag.Xml, und weisen Sie der Datei in demselben Ordner einen neuen Namen zu. Bearbeiten Sie dann die neue Datei, und verwenden Sie das Argument **/I** , um sie an **SQLDiag** zu übergeben.  
  
#### <a name="editing-the-configuration-file-when-sqldiag-runs-as-a-service"></a>Bearbeiten der Konfigurationsdatei, wenn SQLdiag als Dienst ausgeführt wird  
 Wenn Sie **SQLDiag** bereits als Dienst ausgeführt haben und die Konfigurationsdatei nun bearbeiten müssen, müssen Sie die Registrierung des SQLDIAG-Diensts aufheben, indem Sie das Befehlszeilenargument **/U** angeben und den Dienst anschließend mithilfe des Befehlszeilenarguments **/R** erneut registrieren. Durch das Aufheben der Registrierung und die erneute Registrierung des Diensts werden alle Konfigurationsinformationen entfernt, die in der Windows-Registrierung zwischengespeichert waren.  
  
## <a name="output-folder"></a>Ausgabeordner  
 Wenn Sie keinen Ausgabeordner mithilfe von **/O** angeben, erstellt **SQLDiag** den Unterordner SQLDIAG unterhalb des Startordners von **SQLDiag** . Beim Sammeln von Diagnoseinformationen, die große Mengen an Ablaufverfolgungsdaten einschließen, z.B. mit [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , müssen Sie sicherstellen, dass sich der Ausgabeordner auf einem lokalen Laufwerk befindet, das über ausreichend Speicherplatz zum Speichern der angeforderten Diagnoseausgabe verfügt.  
  
 Bei einem Neustart von **SQLDiag** wird der Inhalt des Ausgabeordners überschrieben. Geben Sie **/N 2** in der Befehlszeile an, um dies zu vermeiden.  
  
## <a name="data-collection-process"></a>Datensammlung  
 Beim Starten von **SQLDiag** werden die Initialisierungsprüfungen ausgeführt, die notwendig sind, um die in „SQLDiag.Xml“ angegebenen Diagnosedaten zu sammeln. Dieser Vorgang kann mehrere Sekunden dauern. Wenn das Hilfsprogramm als Konsolenanwendung ausgeführt wird, wird eine Meldung angezeigt, sobald **SQLDiag** mit der Sammlung von Diagnosedaten begonnen hat. Diese Meldung informiert Sie darüber, dass die Sammlung von Daten mithilfe von **SQLDiag** gestartet wurde dass Sie diesen Vorgang durch Drücken von STRG+C beenden können. Wenn **SQLDiag** als Dienst ausgeführt wird, wird eine vergleichbare Meldung in das Windows-Ereignisprotokoll geschrieben.  
  
 Wenn Sie **SQLDiag** zum Diagnostizieren eines reproduzierbaren Problems verwenden, sollten Sie warten, bis diese Meldung angezeigt wird, bevor Sie das Problem auf dem Server reproduzieren.  
  
 **SQLDiag** sammelt die meisten Diagnosedaten parallel. Alle Diagnoseinformationen werden gesammelt, indem Verbindungen mit Tools wie dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Hilfsprogramm **sqlcmd** oder dem Windows-Befehlsprozessor hergestellt werden, es sei denn die Informationen werden über Windows-Leistungs- und Ereignisprotokolle gesammelt. **SQLDiag** verwendet einen Arbeitsthread je Computer, um die Sammlung der Diagnosedaten mithilfe dieser anderen Tools zu überwachen; hierbei wird oftmals gleichzeitig auf die Beendigung der Ausführung mehrerer Tools gewartet. Während des Sammelns leitet **SQLDiag** die Ausgabe jeder Diagnose an den Ausgabeordner weiter.  
  
## <a name="stopping-data-collection"></a>Beenden der Datensammlung  
 Nachdem **SQLDiag** mit dem Sammeln von Diagnosedaten begonnen hat, wird dieser Vorgang fortgesetzt, bis Sie ihn beenden oder er konfigurationsgemäß zu einer bestimmten Zeit beendet wird. Sie können **SQLDiag** so konfigurieren, dass die Datensammlung zu einem bestimmten Zeitpunkt beendet wird, indem Sie **/E** verwenden, um eine bestimmte Beendigungszeit anzugeben, oder **/X** angeben, um **SQLDiag** im Momentaufnahmemodus auszuführen.  
  
 Wenn **SQLDiag** beendet wird, werden alle gestarteten Diagnosen beendet. So werden beispielsweise [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] -Ablaufverfolgungen, die gesammelt wurden, und das Ausführen von [!INCLUDE[tsql](../includes/tsql-md.md)] -Skripts beendet. Weiterhin werden alle Unterprozesse beendet, die während der Datensammlung erzeugt wurden. Nachdem die Sammlung von Diagnosedaten abgeschlossen ist, wird **SQLDiag** beendet.  
  
> [!NOTE]  
>  Das Anhalten des **SQLDiag** -Diensts wird nicht unterstützt. Wenn Sie versuchen, den **SQLDiag** -Dienst anzuhalten, wird der Dienst nach Abschluss der Sammlung von Diagnoseinformationen beendet, die beim Anhalten des Diensts gerade durchgeführt wurde. Wenn Sie **SQLDiag** nach Beendigung neu starten, wird beim Starten der Anwendung der Inhalt des Ausgabeordners überschrieben. Soll der Ausgabeordner nicht überschrieben werden, müssen Sie **/N 2** in der Befehlszeile angeben.  
  
 **So beenden Sie SQLdiag, wenn das Hilfsprogramm als Konsolenanwendung ausgeführt wird**  
  
 Wenn Sie **SQLDiag** als Konsolenanwendung ausführen, können Sie das Ausführen beenden, indem Sie im Konsolenfenster, in dem **SQLDiag** ausgeführt wird, STRG+C drücken. Nach dem Drücken von STRG+C wird eine Meldung im Konsolenfenster angezeigt, die Sie darüber informiert, dass die Datensammlung durch **SQLDiag** beendet wird und Sie warten sollten, bis der Prozess heruntergefahren wird, was einige Minuten in Anspruch nehmen kann.  
  
 Drücken Sie STRG+C zweimal, um alle untergeordneten Diagnoseprozesse zu beenden und die Anwendung sofort zu beenden.  
  
 **So beenden Sie SQLdiag, wenn das Hilfsprogramm als Dienst ausgeführt wird**  
  
 Wenn Sie **SQLDiag** als Dienst ausführen, führen Sie **SQLDiag STOP** im Startordner von **SQLDiag** aus, um das Programm zu beenden.  
  
 Wenn Sie mehrere Instanzen von **SQLDiag** auf demselben Computer ausführen, können Sie beim Beenden des Diensts auch den Namen der jeweiligen Instanz von **SQLDiag** an die Befehlszeile übergeben. Verwenden Sie die folgende Syntax, um z.B. eine Instanz von **SQLDiag** mit dem Namen „Instance1“ zu beenden:  
  
```  
SQLDIAG STOP /A Instance1  
```  
  
> [!NOTE]  
>  **/A** ist das einzige Befehlszeilenargument, das mit **START**, **STOP** oder **STOP_ABORT** verwendet werden kann. Wenn Sie eine benannte Instanz von **SQLDiag** mit einem der Dienstkontrollverben angeben müssen, müssen Sie **/A** wie im vorherigen Syntaxbeispiel nach dem Kontrollverb in der Befehlszeile angeben. Werden Kontrollverben verwendet, müssen diese als erstes Argument in der Befehlszeile angegeben werden.  
  
 Soll der Dienst so schnell wie möglich beendet werden, führen Sie **SQLDIAG STOP_ABORT** im Startordner des Hilfsprogramms aus. Mit diesem Befehl werden alle aktuell ausgeführten Sammlungen von Diagnoseinformationen abgebrochen, ohne auf ihre Beendigung zu warten.  
  
> [!NOTE]  
>  Verwenden Sie **SQLDiag STOP** oder **SQLDIAG STOP_ABORT** , um den **SQLDiag** -Dienst zu beenden. Verwenden Sie die zum Beenden von **SQLDiag** oder anderen [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Diensten nicht die Windows-Dienstkonsole.  
  
## <a name="automatically-starting-and-stopping-sqldiag"></a>Automatisches Starten und Beenden von SQLdiag  
 Wenn Sie die Sammlung von Diagnosedaten zu einem bestimmten Zeitpunkt automatisch starten und beenden möchten, können Sie die Argumente **/B**_start\_time_ und **/E**_stop\_time_ (die Angabe erfolgt in 24-Stunden-Notation) verwenden. Wenn Sie beispielsweise ein Problem behandeln möchten, das immer gegen ca. 02:00:00 Uhr auftritt, können Sie **SQLDiag** so konfigurieren, dass die Sammlung von Diagnosedaten automatisch um 01:00 Uhr beginnt und automatisch um 03:00:00 Uhr endet. Geben Sie die Start- und Beendigungszeit mit dem **/B** -Argument und dem **/E** -Argument an. Verwenden Sie die 24-Stunden-Notation, um das genau Start- und Beendigungsdatum und die Uhrzeit im Format YYYYMMDD_HH:MM:SS anzugeben. Zum Angeben einer relativen Start- und Beendigungszeit geben Sie **+** vor der Start- und der Beendigungszeit an, wobei jedoch der Datumsabschnitt (YYYYMMDD_) wie im folgenden Beispiel gezeigt, entfällt. Diese Angabe bewirkt, dass **SQLDiag** 1 Stunde wartet, bevor mit dem Sammeln von Informationen begonnen wird. Die Datensammlung wird dann für 3 Stunden fortgeführt und danach beendet:  
  
```  
sqldiag /B +01:00:00 /E +03:00:00  
```  
  
 Wenn ein relativer *start_time* -Wert angegeben wird, wird **SQLDiag** zu einem Zeitpunkt gestartet, der relativ zum aktuellen Datum und zur aktuellen Uhrzeit ist. Wenn ein relativer *end_time* -Wert angegeben wird, wird **SQLDiag** zu einem Zeitpunkt beendet, der relativ zum angegebenen *start_time*-Wert ist. Wenn die angegebenen Start- oder Beendigungszeitpunkte in der Vergangenheit liegen, erzwingt **SQLDiag** eine Änderung des Startdatums, sodass Startdatum und -uhrzeit in der Zukunft liegen.  
  
 Dies hat erhebliche Auswirkungen auf die von Ihnen ausgewählten Start- und Beendigungsdaten. Betrachten Sie das folgenden Beispiel:  
  
```  
sqldiag /B +01:00:00 /E 08:30:00  
```  
  
 Wenn es derzeit 08:00 Uhr, ist die Beendigungszeit schon vorbei, bevor die Sammlung der Diagnosedaten überhaupt begonnen hat. Da **SQLDiag** die Start- und Beendigungsdaten automatisch anpasst und auf den nächsten Tag verschiebt, wenn die Zeitpunkte in der Vergangenheit liegen, beginnt die Sammlung der Diagnosedaten in diesem Beispiel heute um 09:00 Uhr (mit **+** wurde eine relative Startzeit angegeben) und wird bis zum folgenden Morgen um 08:30 Uhr fortgeführt.  
  
### <a name="stopping-and-restarting-sqldiag-to-collect-daily-diagnostics"></a>Beenden und erneutes Starten von SQLdiag zum Sammeln täglicher Diagnoseinformationen  
 Mithilfe von **/L** können Sie jeden Tag einen angegebenen Satz von Diagnosedaten sammeln, ohne **SQLDiag** manuell starten und beenden zu müssen. Durch **/L** wird **SQLDiag** fortlaufend ausgeführt, da dieses Argument bewirkt, dass sich das Hilfsprogramm nach einem geplanten Herunterfahren automatisch neu startet. Wenn **/L** angegeben wurde und **SQLDiag** beendet wird, weil die mit **/E** angegebene Beendigungszeit erreicht wurde oder weil das Hilfsprogramm durch die Angabe von **/X** im Momentaufnahmemodus ausgeführt wird, wird **SQLDiag** nicht vollständig beendet, sondern erneut gestartet.  
  
 Im folgenden Beispiel wird angegeben, dass **SQLDiag** im fortlaufenden Modus ausgeführt wird, damit das Hilfsprogramm automatisch neu gestartet wird, nachdem zwischen 03:00:00 und 05:00:00 Uhr die Sammlung von Diagnosedaten erfolgt ist.  
  
```  
sqldiag /B 03:00:00 /E 05:00:00 /L  
```  
  
 Im folgenden Beispiel wird angegeben, dass **SQLDiag** im fortlaufenden Modus ausgeführt wird, damit das Hilfsprogramm automatisch neu gestartet wird, nachdem um 03:00:00 Uhr eine Momentaufnahme der Diagnosedaten erstellt wurde.  
  
```  
sqldiag /B 03:00:00 /X /L  
```  
  
## <a name="running-sqldiag-as-a-service"></a>Ausführen von SQLdiag als Dienst  
 Wenn Sie **SQLDiag** verwenden möchten, um Diagnosedaten über längere Zeiträume zu sammeln, in deren Verlauf Sie sich jedoch möglicherweise vom Computer abmelden müssen, auf dem **SQLDiag** ausgeführt wird, können Sie es als Dienst ausführen.  
  
 **So registrieren Sie SQLDiag für das Ausführen als Dienst**  
  
 Sie können **SQLDiag** für das Ausführen als Dienst registrieren, indem Sie **/R** in der Befehlszeile angeben. Hierdurch wird **SQLDiag** für das Ausführen als Dienst registriert. Der Dienstname von **SQLDiag** ist SQLDIAG. Wenn Sie beim Registrieren von **SQLDiag** als Dienst weitere Argumente in der Befehlszeile angeben, werden diese Argumente beibehalten und erneut verwendet, wenn der Dienst gestartet wird.  
  
 Wenn Sie den Standarddienstnamen SQLDIAG ändern möchten, geben Sie mit dem Befehlszeilenargument **/A** einen anderen Namen an. **SQLDiag** setzt vor jeden mit **/A** angegebenen Instanznamen von **SQLDiag** automatisch das Präfix DIAG$, um aussagefähige Dienstnamen zu erstellen.  
  
 **So heben Sie die Registrierung des SQLDIAG-Diensts auf**  
  
 Zum Aufheben der Dienstregistrierung geben Sie das Argument **/U** an. Wird die Registrierung von **SQLDiag** als Dienst aufgehoben, wird hierdurch auch der Windows-Registrierungsschlüssel für den SQLDIAG-Dienst gelöscht.  
  
 **So starten Sie den SQLDIAG-Dienst bzw. führen einen Neustart des Diensts durch**  
  
 Führen Sie zum Starten oder Neustarten des SQLDIAG-Diensts **SQLDiag START** an der Befehlszeile aus.  
  
 Falls Sie mithilfe des **/A** -Arguments mehrere Instanzen von **SQLDiag** ausführen, können Sie beim Starten des Diensts auch den Instanznamen von **SQLDiag** an der Befehlszeile übergeben. Verwenden Sie die folgende Syntax, um z.B. eine Instanz von **SQLDiag** mit dem Namen „Instance1“ zu starten:  
  
```  
SQLDIAG START /A Instance1  
```  
  
 Sie können auch den Befehl **net start** verwenden, um den SQLDIAG-Dienst zu starten.  
  
 Wenn Sie **SQLDiag** neu starten, wird der Inhalt des aktuellen Ausgabeordners überschrieben. Soll dies verhindert werden, geben Sie **/N 2** in der Befehlszeile an, damit der Ausgabeordner beim Starten des Hilfsprogramms umbenannt wird.  
  
 Das Anhalten des **SQLDiag** -Diensts wird nicht unterstützt.  
  
## <a name="running-multiple-instances-of-sqldiag"></a>Ausführen mehrerer Instanzen von SQLdiag  
 Sie können mehrere Instanzen von **SQLDiag** auf demselben Computer ausführen, indem Sie **/A**_SQLDiag\_application\_name_ in der Befehlszeile angeben. Dies ist sinnvoll, wenn von derselben Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verschiedene Sätze von Diagnosedaten gleichzeitig gesammelt werden sollen. Sie können beispielsweise eine benannte Instanz von **SQLDiag** konfigurieren, um fortlaufend eine geringe Menge von Daten zu sammeln. Falls anschließend ein bestimmtes Problem in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]auftritt, können Sie die Standardinstanz von **SQLDiag** ausführen, um Diagnosedaten für dieses Problem zu sammeln, oder um auf Anforderung vom [!INCLUDE[msCoName](../includes/msconame-md.md)] -Kundenservice und -support hin einen Satz von Diagnosedaten zu sammeln, die für die Diagnose eines Problems erforderlich sind.  
  
## <a name="collecting-diagnostic-data-from-clustered-sql-server-instances"></a>Sammeln von Diagnosedaten von gruppierten SQL Server-Instanzen  
 **SQLDiag** unterstützt das Sammeln von Diagnosedaten von gruppierten Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Zum Sammeln von Diagnosedaten von gruppierten Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] müssen Sie sicherstellen, dass **"."** für das **name**-Attribut des **\<Machine>** -Elements in der Konfigurationsdatei „SQLDiag.Xml“ angegeben ist. Das Argument **/G** darf in der Befehlszeile nicht angegeben werden. Standardmäßig ist **"."** für das **name** -Attribut in der Konfigurationsdatei angegeben und das Argument **/G** deaktiviert. Normalerweise ist es nicht nötig, die Konfigurationsdatei zu bearbeiten oder die Befehlszeilenargumente zu ändern, wenn Sie Diagnosedaten von einer gruppierten Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sammeln möchten.  
  
 Wenn **"."** als Computername angegeben ist, erkennt **SQLDiag** , dass es in einem Cluster ausgeführt wird, und ruft daher gleichzeitig Diagnoseinformationen von allen virtuellen Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] im Cluster ab. Wenn die Diagnosedaten nur für eine der virtuellen Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] gesammelt werden sollen, die auf einem Computer ausgeführt werden, müssen Sie den Namen dieser virtuellen Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für das **name**-Attribut des **\<Machine>** -Elements in „SQLDiag.Xml“ angeben.  
  
> [!NOTE]  
>  Zum Sammeln von [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsinformationen aus gruppierten Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] müssen auf dem Cluster administrative Freigaben (ADMIN$) aktiviert sein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Referenz zum Eingabeaufforderungs-Hilfsprogramm &#40;Datenbank-Engine&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
