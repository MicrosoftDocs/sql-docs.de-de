---
description: Debuggen des Datenflusses
title: Debuggen des Datenflusses | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- progress reporting [Integration Services]
- data viewers [Integration Services]
- data flow [Integration Services], debugging
- debugging [Integration Services], data flow
- counting rows
ms.assetid: 1c574f1b-54f7-4c05-8e42-8620e2c1df0f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c69bed5fbec76fab380f91515827630fa60938a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342162"
---
# <a name="debugging-data-flow"></a>Debuggen des Datenflusses

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer enthalten Features und Tools, mit denen Sie Probleme in den Datenflüssen einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paket behandeln können.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer stellt Daten-Viewer bereit.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer und [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Transformationen stellen die Zeilenanzahl bereit.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer stellt zur Laufzeit Fortschrittsberichte bereit.  
  
## <a name="data-viewers"></a>Daten-Viewer  
 Daten-Viewer zeigen Daten zwischen zwei Komponenten in einem Datenfluss an. Mit Daten-Viewern können Daten angezeigt werden, wenn die Daten von einer Datenquelle extrahiert werden und an einen Datenfluss weitergegeben werden, vor und nach dem Update der Daten durch eine Transformation sowie vor dem Laden der Daten in das Ziel.  
  
 Um die Daten anzuzeigen, fügen Sie dem Pfad, der zwei Datenflusskomponenten verbindet, Daten-Viewer hinzu. Durch die Möglichkeit, Daten zwischen Datenflusskomponenten anzuzeigen, können Sie auf einfache Weise unerwartete Datenwerte identifizieren, die Änderung von Spaltenwerten durch eine Transformation anzeigen sowie die Ursache für einen Fehler bei einer Transformation ermitteln. Beispielsweise kann es sein, dass bei einer Suche in einer Verweistabelle ein Fehler auftritt. Um dieses Problem zu beseitigen, können Sie eine Transformation hinzufügen, die Standarddaten für leere Spalten bereitstellt.  
  
 Ein Daten-Viewer kann Daten in einem Raster anzeigen. Bei einem Raster wählen Sie die Spalten aus, die angezeigt werden sollen. Die Werte für die ausgewählten Spalten werden im Tabellenformat angezeigt.  
  
 Sie können auch mehrere Daten-Viewer in einen Pfad einschließen. Daten können in unterschiedlichen Formaten angezeigt werden (erstellen Sie z.B. eine Diagrammansicht und eine Rasteransicht der Daten). Außerdem können Sie unterschiedliche Daten-Viewer für verschiedene Datenspalten erstellen.  
  
 Wenn Sie einem Pfad einen Daten-Viewer hinzufügen, fügt der [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer der Entwurfsoberfläche der Registerkarte **Datenfluss** neben dem Pfad ein Daten-Viewer-Symbol hinzu. Transformationen mit mehreren Ausgaben, wie z. B. die Transformation für bedingtes Teilen, können in jedem Pfad einen Daten-Viewer enthalten.  
  
 Zur Laufzeit wird ein **Daten-Viewer** -Fenster geöffnet, in dem die vom Daten-Viewer-Format definierten Informationen angezeigt werden. Beispielsweise zeigt ein Daten-Viewer, der das Rasterformat verwendet, Daten für die ausgewählten Spalten, die Anzahl von an die Datenflusskomponente übergebenen Ausgabezeilen sowie die Anzahl dargestellter Zeilen an. Die Informationen werden pufferweise angezeigt und, ein Puffer kann, abhängig von der Zeilenbreite im Datenfluss, mehr oder weniger Zeilen enthalten.  
  
 Im Dialogfeld **Daten-Viewer** können Sie die Daten in die Zwischenablage kopieren, alle Daten aus der Tabelle löschen, den Daten-Viewer neu konfigurieren, den Datenfluss fortsetzen und den Daten-Viewer anfügen oder trennen.  
  
#### <a name="to-add-a-data-viewer"></a>So fügen Sie einen Daten-Viewer hinzu  
  
-   [Hinzufügen eines Daten-Viewers zu einem Datenfluss](#add_viewer)  
  
## <a name="row-counts"></a>Zeilenanzahl  
 Die Anzahl von Zeilen, die über einen Pfad verschoben wurden, werden in der Entwurfsoberfläche der Registerkarte **Datenfluss** in [!INCLUDE[ssIS](../../includes/ssis-md.md)] neben dem Pfad angezeigt. Dieser Wert wird regelmäßig aktualisiert, während die Daten über den Pfad verschoben werden.  
  
 Sie können dem Datenfluss auch eine Transformation für Zeilenanzahl hinzufügen, um die endgültige Zeilenanzahl in einer Variablen aufzuzeichnen. Weitere Informationen finden Sie unter [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
## <a name="progress-reporting"></a>Fortschrittsberichte  
 Wenn Sie ein Paket ausführen, stellt der [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer den Fortschritt in der Entwurfsoberfläche der Registerkarte **Datenfluss** dar, indem jede Datenflusskomponente in der entsprechenden Statusfarbe dargestellt wird. Wenn eine Komponente mit der Arbeit beginnt, wird die Farbe von keiner Farbe in gelb geändert. Wenn die Komponente erfolgreich abgeschlossen ist, wird sie in grün geändert. Rot bedeutet, dass bei der Komponente ein Fehler aufgetreten ist.  
  
 In der folgenden Tabelle wird die Farbcodierung beschrieben.  
  
|Color|BESCHREIBUNG|  
|-----------|-----------------|  
|Keine Farbe|Wartet auf den Aufruf durch die Datenfluss-Engine.|  
|Gelb|Führt eine Transformation aus, extrahiert Daten oder lädt Daten.|  
|Grün|Wurde erfolgreich ausgeführt.|  
|Rot|Wurde mit Fehlern ausgeführt.|  

## <a name="analysis-of-data-flow"></a>Analyse des Datenflusses
  Mit der [SSISDB](../../integration-services/system-views/catalog-execution-data-statistics.md)-Datenbanksicht **catalog.execution_data_statistics** lässt sich der Datenfluss von Paketen analysieren. In dieser Sicht wird immer dann eine Zeile angezeigt, wenn eine Datenflusskomponente Daten an eine Downstreamkomponente sendet. Anhand der Informationen können Sie ein tieferes Verständnis der Zeilen erhalten, die an jede Komponente gesendet werden.  
  
> [!NOTE]  
>  Der Protokollierungsgrad muss auf **Ausführlich** festgelegt werden, um Informationen mit der catalog.execution_data_statistics-Sicht aufzuzeichnen.  
  
 Im folgenden Beispiel wird die Anzahl der Zeilen angezeigt, die zwischen Komponenten eines Pakets gesendet wurden.  
  
```sql
use SSISDB  
select package_name, task_name, source_component_name, destination_component_name, rows_sent  
from catalog.execution_data_statistics  
where execution_id = 132  
order by source_component_name, destination_component_name   
```  
  
 Im folgenden Beispiel wird die Anzahl der Zeilen pro Millisekunde berechnet, die von jeder Komponente für eine bestimmte Ausführung gesendet wurden. Die berechneten Werte lauten wie folgt:  
  
-   **total_rows** : Die Summe aller von der Komponente gesendeten Zeilen  
  
-   **wall_clock_time_ms:** Die insgesamt verstrichene Ausführungszeit, in Millisekunden, für jede Komponente  
  
-   **num_rows_per_millisecond:** Die Anzahl der pro Millisekunde von jeder Komponente gesendeten Zeilen  
  
 Mit der **HAVING** -Klausel wird ein Fehler aufgrund einer Division durch 0 in den Berechnungen verhindert.  
  
```sql  
use SSISDB  
select source_component_name, destination_component_name,  
    sum(rows_sent) as total_rows,  
    DATEDIFF(ms,min(created_time),max(created_time)) as wall_clock_time_ms,  
    ((0.0+sum(rows_sent)) / (datediff(ms,min(created_time),max(created_time)))) as [num_rows_per_millisecond]  
from [catalog].[execution_data_statistics]  
where execution_id = 132  
group by source_component_name, destination_component_name  
having (datediff(ms,min(created_time),max(created_time))) > 0  
order by source_component_name desc  
```  

## <a name="configure-an-error-output-in-a-data-flow-component"></a>Konfigurieren einer Fehlerausgabe in einer Datenflusskomponente
  Viele Datenflusskomponenten unterstützen Fehlerausgaben, und [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer bietet je nach Komponente unterschiedliche Möglichkeiten für die Konfiguration einer Fehlerausgabe. Sie können nicht nur eine Fehlerausgabe, sondern auch die Spalten einer Fehlerausgabe konfigurieren. Dies schließt das Konfigurieren der von der Komponente hinzugefügten Spalten **ErrorCode** und **ErrorColumn** ein.  
  
### <a name="configuring-an-error-output"></a>Konfigurieren einer Fehlerausgabe  
 Zum Konfigurieren einer Fehlerausgabe stehen Ihnen zwei Optionen zur Verfügung:  
  
-   Verwenden des Dialogfelds **Fehlerausgabe konfigurieren** . Sie können dieses Dialogfeld zum Konfigurieren einer Fehlerausgabe für jede Datenflusskomponente verwenden, die eine Fehlerausgabe unterstützt.  
  
-   Verwenden des Editordialogfelds für die Komponente. Die Fehlerausgaben können bei einigen Komponenten direkt in ihrem Editordialogfeld konfiguriert werden. Es ist jedoch nicht möglich, Fehlerausgaben im Editordialogfeld für die ADO NET-Quelle, die Transformation für das Importieren von Spalten, die Transformation für OLE DB-Befehl oder das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Ziel zu konfigurieren.  
  
 Im Folgenden wird die Verwendung dieser Dialogfelder zum Konfigurieren von Fehlerausgaben beschrieben.  
  
#### <a name="to-configure-an-error-output-using-the-configure-error-output-dialog-box"></a>So konfigurieren Sie eine Fehlerausgabe mit dem Dialogfeld "Fehlerausgabe konfigurieren"  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer auf die Registerkarte **Datenfluss** .  
  
4.  Ziehen Sie die Fehlerausgabe, dargestellt durch einen roten Pfeil, von der Komponente, die die Fehlerquelle darstellt, zu einer anderen Komponente im Datenfluss.  
  
5.  Wählen Sie im Dialogfeld **Fehlerausgabe konfigurieren** eine Aktion in den Spalten **Fehler** und **Abschneiden** für jede Spalte der Komponenteneingabe.  
  
6.  Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern**, um das aktualisierte Paket zu speichern.  
  
#### <a name="to-add-an-error-output-using-the-editor-dialog-box-for-the-component"></a>So fügen Sie eine Fehlerausgabe mit dem Editordialogfeld für die Komponente hinzu  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer auf die Registerkarte **Datenfluss** .  
  
4.  Doppelklicken Sie auf die Datenflusskomponenten, für die Sie eine Fehlerausgabe konfigurieren möchten, und führen Sie je nach Komponente einen der folgenden Schritte durch:  
  
    -   Klicken Sie auf **Fehlerausgabe konfigurieren**.  
  
    -   Klicken Sie auf **Fehlerausgabe**.  
  
5.  Legen Sie für jede Spalte die Option **Fehler** fest.  
  
6.  Legen Sie für jede Spalte die Option **Abschneiden** fest.  
  
7.  Klicken Sie auf **OK**.  
  
8.  Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern**, um das aktualisierte Paket zu speichern.  
  
### <a name="configuring-error-output-columns"></a>Konfigurieren von Fehlerausgabespalten  
 Verwenden Sie die Registerkarte **Eingabe- und Ausgabeeigenschaften** im Dialogfeld **Erweiterter Editor** zum Konfigurieren von Fehlerausgabespalten.  
  
#### <a name="to-configure-error-output-columns"></a>So konfigurieren Sie Fehlerausgabespalten  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer auf die Registerkarte **Datenfluss** .  
  
4.  Klicken Sie mit der rechten Maustaste auf die Komponente, deren Fehlerausgabespalten Sie konfigurieren möchten, und klicken Sie auf **Erweiterten Editor anzeigen**.  
  
5.  Klicken Sie auf die Registerkarte **Eingabe- und Ausgabeeigenschaften**, und erweitern Sie **\<component name>-Fehlerausgabe** und dann **Ausgabespalten**.  
  
6.  Klicken Sie auf eine Spalte, und aktualisieren Sie ihre Eigenschaften.  
  
    > [!NOTE]  
    >  Die Liste mit den Spalten enthält die Spalten in der Komponenteneingabe, die durch vorherige Fehlerausgaben hinzugefügten Spalten **ErrorCode** und **ErrorColumn** sowie die von dieser Komponente hinzugefügten Spalten **ErrorCode** und **ErrorColumn** .  
  
7.  Klicken Sie auf **OK**.  
  
8.  Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern**, um das aktualisierte Paket zu speichern.  

## <a name="add-a-data-viewer-to-a-data-flow"></a><a name="add_viewer"></a> Hinzufügen eines Daten-Viewers zu einem Datenfluss
  In diesem Thema wird beschrieben, wie ein Daten-Viewer in einem Datenfluss hinzugefügt und konfiguriert wird. Ein Daten-Viewer zeigt Daten an, die sich zwischen zwei Datenflusskomponenten bewegen. Ein Daten-Viewer kann z. B. die Daten anzeigen, die von einer Datenquelle extrahiert werden, bevor die Daten von einer Transformation im Datenfluss geändert werden.  
  
 Ein Pfad verbindet Komponenten in einem Datenfluss, indem die Ausgabe einer Datenflusskomponente mit der Eingabe einer anderen Komponente verbunden wird.  
  
 Bevor Sie einem Paket Daten-Viewer hinzufügen können, muss das Paket einen Datenfluss-Task enthalten und mindestens zwei verbundene Datenflusskomponenten.  
  
 Fügen Sie einer Fehlerausgabe einen Daten-Viewer hinzu, um die Beschreibung des Fehlers und den Namen der Spalte anzuzeigen, in der der Fehler aufgetreten ist. Standardmäßig enthält die Fehlerausgabe nur numerische Bezeichner für den Fehler und die Spalte.  
  
### <a name="to-add-a-data-viewer-to-a-data-flow"></a>So fügen Sie einem Datenfluss einen Daten-Viewer hinzu  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie auf die Registerkarte **Ablaufsteuerung** , falls sie nicht bereits aktiviert ist.  
  
4.  Klicken Sie auf den Datenflusstask, an dessen Datenfluss Sie einen Daten-Viewer anfügen möchten, und klicken Sie dann auf die Registerkarte **Datenfluss** .  
  
5.  Klicken Sie mit der rechten Maustaste auf einen Pfad zwischen zwei Datenflusskomponenten, und klicken Sie auf **Bearbeiten**.  
  
6.  Auf der Seite **Allgemein** können Sie Pfadeigenschaften anzeigen und bearbeiten. Beispielsweise können Sie aus der Dropdownliste **PathAnnotation** die Anmerkung auswählen, die neben dem Pfad angezeigt wird.  
  
7.  Auf der Seite **Metadaten** können Sie die Spaltenmetadaten anzeigen und die Metadaten in die Zwischenablage kopieren.  
  
8.  Klicken Sie auf der Seite **Daten-Viewer** auf **Daten-Viewer aktivieren**.  
  
9. Wählen Sie im Bereich Anzuzeigende Spalten die Spalten aus, die im Daten-Viewer angezeigt werden sollen. Standardmäßig werden alle verfügbaren Spalten ausgewählt und in der Liste **Angezeigte Spalten** aufgeführt. Verschieben Sie Spalten, die Sie nicht verwenden möchten, in die Liste **Nicht verwendete Spalten** , indem Sie diese auswählen und dann auf den linken Pfeil klicken.  
  
    > [!NOTE]  
    >  Im Raster werden Werte, die die Datentypen DT_DATE, DT_DBTIME2, DT_FILETIME, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 und DT_DBTIMESTAMPOFFSET darstellen, als ISO 8601-formatierte Zeichenfolgen angezeigt, und das **T** -Trennzeichen wird durch ein Leerzeichen ersetzt. Werte, die den DT_DATE-Datentyp und den DT_FILETIME-Datentyp darstellen, enthalten sieben Ziffern für Sekundenbruchteile. Da der DT_FILETIME-Datentyp nur drei Ziffern von Sekundenbruchteilen speichert, zeigt das Raster Nullen für die restlichen vier Ziffern an. Werte, die den DT_DBTIMESTAMP-Datentyp darstellen, enthalten drei Ziffern für Sekundenbruchteile. Für Werte, die die Datentypen DT_DBTIME2, DT_DBTIMESTAMP2 und DT_DBTIMESTAMPOFFSET darstellen, entspricht die Anzahl der Ziffern für Sekundenbruchteile der für den Datentyp der Spalte festgelegten Skala. Weitere Informationen zu ISO 8601-Formaten finden Sie unter [Date and Time Formats](../data-flow/parsing-data.md). Weitere Informationen zu Datentypen finden Sie unter [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
10. Klicken Sie auf **OK**.  

## <a name="data-flow-taps"></a>Datenflussabzweigungen
 Sie können dem Datenflusspfad eines Pakets zur Laufzeit eine Datenabzweigung hinzufügen und die Ausgabe von der Datenabzweigung an eine externe Datei weiterleiten. Um diese Funktion verwenden zu können, müssen Sie das SSIS-Projekt mithilfe des Projektbereitstellungsmodells auf einem SSIS-Server bereitstellen. Nachdem Sie das Paket auf dem Server bereitgestellt haben, müssen Sie T-SQL-Skripts für die SSISDB-Datenbank ausführen, um vor der Paketausführung Datenabzweigungen hinzuzufügen. Beispielszenario:  
  
1.  Erstellen Sie mithilfe der gespeicherten Prozedur [catalog.create_execution &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) eine Paketausführungsinstanz.  
  
2.  Fügen Sie mithilfe einer der gespeicherten Prozeduren [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md) bzw. [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md) eine Datenabzweigung hinzu.  
  
3.  Starten Sie die Paketausführungsinstanz mithilfe von [catalog.start_execution &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).  
  
 Im Folgenden ein SQL-Beispielskript, durch das die im vorangehenden Szenario beschriebenen Schritte ausgeführt werden:  
  
```sql
Declare @execid bigint  
EXEC [SSISDB].[catalog].[create_execution] @folder_name=N'ETL Folder', @project_name=N'ETL Project', @package_name=N'Package.dtsx', @execution_id=@execid OUTPUT  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt'  
EXEC [SSISDB].[catalog].[start_execution] @execid  
```  
  
 Die Parameter für die Ordner-, Projekt- und Paketnamen der gespeicherten Prozedur create_execution entsprechen den Ordner-, Projekt- und Paketnamen im Integration Services-Katalog. Sie können die Ordner-, Projekt- und Paketnamen, die Sie im Aufruf von create_execution verwenden möchten, wie in der folgenden Abbildung dargestellt, aus SQL Server Management Studio abrufen. Wenn das SSIS-Projekt hier nicht angezeigt wird, wurde es möglicherweise noch nicht auf dem SSIS-Server bereitgestellt. Klicken Sie in Visual Studio mit der rechten Maustaste auf das SSIS-Projekt, und klicken Sie auf Bereitstellen, um das Projekt auf dem erwarteten SSIS-Server bereitzustellen.  
  
 Anstatt die SQL-Anweisungen einzugeben, können Sie das Skript zur Paketausführung anhand der folgenden Schritte generieren:  
  
1.  Klicken Sie mit der rechten Maustaste auf **Package.dtsx** , und klicken Sie auf **Ausführen**.  
  
2.  Klicken Sie auf die Symbolleistenschaltfläche **Skript** , um das Skript zu generieren.  
  
3.  Fügen Sie dann die Anweisung add_data_tap vor dem Aufruf von start_execution hinzu.  
  
 Der Parameter task_package_path der gespeicherten Prozedur add_data_tap entspricht der Eigenschaft PackagePath des Datenflusstasks in Visual Studio. Klicken Sie in Visual Studio mit der rechten Maustaste auf **Datenflusstask**, und klicken Sie auf **Eigenschaften** , um das Eigenschaftenfenster zu öffnen.  Notieren Sie sich den Wert der Eigenschaft **PackagePath** , um sie im Aufruf der gespeicherten Prozedur „add_data_tap“ als Wert für den Parameter „task_package_path“ zu verwenden.  
  
 Der Parameter dataflow_path_id_string der gespeicherten Prozedur add_data_tap entspricht der Eigenschaft IdentificationString des Datenflusspfads, dem Sie eine Datenabzweigung hinzufügen möchten. Klicken Sie auf den Datenflusspfad (Pfeil zwischen den Tasks im Datenfluss), und notieren Sie sich den Wert der Eigenschaft **IdentificationString** im Eigenschaftenfenster, um „dataflow_path_id_string“ abzurufen.  
  
 Wenn Sie das Skript ausführen, wird die Ausgabedatei unter „\<Program Files>\Microsoft SQL Server\110\DTS\DataDumps“ gespeichert. Wenn bereits eine Datei mit diesem Namen vorhanden ist, wird eine neue Datei mit einem Suffix erstellt (z. B. "output[1].txt").  
  
 Wie oben erwähnt, können Sie anstelle der gespeicherten Prozedur „add_data_tap“ auch die gespeicherte Prozedur [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)verwenden. Diese gespeicherte Prozedur verwendet anstelle von task_package_path die ID des Datenflusstasks als Parameter. Die ID des Datenflusstasks finden Sie im Eigenschaftenfenster in Visual Studio.  
  
### <a name="removing-a-data-tap"></a>Entfernen einer Datenabzweigung  
 Bevor Sie die Ausführung starten, können Sie eine Datenabzweigung mithilfe der gespeicherten Prozedur [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md) entfernen. Diese gespeicherte Prozedur verwendet die ID der Datenabzweigung als Parameter, die Sie als Ausgabe der gespeicherten Prozedur add_data_tap abrufen können.  
  
```sql
DECLARE @tap_id bigint  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt' @data_tap_id=@tap_id OUTPUT  
EXEC [SSISDB].[catalog].remove_data_tap @tap_id  
```  
  
### <a name="listing-all-data-taps"></a>Auflisten aller Datenabzweigungen  
 Sie können auch alle Datenabzweigungen mithilfe der Sicht catalog.execution_data_taps auflisten. Im folgenden Beispiel werden Datenabzweigungen für die Instanz einer Spezifikationsausführung (ID: 54) extrahiert.  
  
```sql 
select * from [SSISDB].[catalog].execution_data_taps where execution_id=@execid  
```  
  
### <a name="performance-consideration"></a>Leistungsaspekte  
 Wenn Sie den ausführlichen Protokolliergrad aktivieren und Datenabzweigungen hinzufügen, erhöhen sich die von der Datenintegrationslösung ausgeführten E/A-Vorgänge. Daher wird empfohlen, Datenabzweigungen nur zur Problembehandlung hinzuzufügen.  
  
### <a name="video"></a>Video  
 Das [Video auf TechNet](../../sql-server/index.yml) zeigt, wie Sie Datenabzweigungen im SQL Server 2012 SSISDB-Katalog hinzufügen bzw. verwenden, die das programmgesteuerte Debuggen von Paketen und die Erfassung von Teilergebnissen zur Laufzeit unterstützen. Das Video zeigt außerdem, wie Sie diese Datenabzweigungen auflisten bzw. entfernen, und welche bewährten Methoden für Datenabzweigungen in SSIS-Paketen empfohlen werden.  
 
## <a name="see-also"></a>Weitere Informationen  
 [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md)  
  
