---
title: dta
description: Das dta-Hilfsprogramm ist eine Eingabeaufforderungsversion des Datenbankoptimierungsratgebers, die Ihnen die Verwendung der Funktionen in Anwendungen und Skripts ermöglicht.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: b05279df08153efab4257a74a8389ea33b38b966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338741"
---
# <a name="dta-utility"></a>dta

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das **dta** -Hilfsprogramm ist die Eingabeaufforderungsversion des Datenbankoptimierungsratgebers. Mit dem Hilfsprogramm **dta** soll es Ihnen ermöglicht werden, die Funktionalität des Datenbankoptimierungsratgebers in Anwendungen und Skripts zu verwenden.  

Wie der Datenbankoptimierungsratgeber dient auch das **dta** -Hilfsprogramm dazu, eine Arbeitsauslastung zu analysieren und Empfehlungen zu den physischen Entwurfsstrukturen auszugeben, um die Serverleistung für diese Arbeitsauslastung zu verbessern. Bei der Arbeitsauslastung kann es sich um einen Plancache, eine [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsdatei oder -tabelle oder ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript handeln. Zu den physischen Entwurfsstrukturen zählen Indizes, indizierte Sichten und die Partitionierung. Nach der Analyse einer Arbeitsauslastung erstellt das **dta** -Hilfsprogramm eine Empfehlung für den physischen Datenbankentwurf und kann das notwendige Skript zum Implementieren der Empfehlung generieren. Arbeitsauslastungen können über die Eingabeaufforderung mithilfe der Argumente **-if** oder **-it** angegeben werden. Mithilfe von **-ix** können Sie zudem eine XML-Eingabedatei über die Eingabeaufforderung angeben. In diesem Fall wird die Arbeitsauslastung in der XML-Eingabedatei angegeben.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -I time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>Argumente  
 **-?**  
 Zeigt Informationen zur Nutzung an.  
  
 **-A** _time_for_tuning_in_minutes_  
 Gibt das Zeitlimit für die Optimierung in Minuten an. Das **dta** -Hilfsprogramm verwendet die angegebene Zeitspanne, um die Arbeitsauslastung zu optimieren und ein Skript mit den empfohlenen Änderungen des physischen Entwurfs zu generieren. Standardmäßig geht **dta** von einer Optimierungszeit von 8 Stunden aus. Durch den Wert 0 wird eine unbegrenzte Optimierungszeit festgelegt. Das **dta** -Hilfsprogramm beendet die Optimierung der gesamten Arbeitsauslastung möglicherweise bereits vor Ablauf des Zeitlimits. Wenn sichergestellt werden soll, dass die gesamte Arbeitsauslastung optimiert wird, empfiehlt es sich jedoch, eine unbegrenzte Optimierungszeit anzugeben (-A 0).  
  
 **-a**  
 Optimiert die Arbeitsauslastung und wendet die Empfehlung an, ohne Sie zur Bestätigung aufzufordern.  
  
 **-B** _storage_size_  
 Gibt den maximalen Speicherplatz (in MB) an, der durch den empfohlenen Index und die Partitionierung beansprucht werden kann. Wenn mehrere Datenbanken optimiert werden, werden bei der Berechnung des Speicherplatzes Empfehlungen für alle Datenbanken berücksichtigt. Standardmäßig geht **dta** von der kleineren der folgenden Speichergrößen aus:  
  
-   Das Dreifache der aktuellen Rohdatengröße, einschließlich der Gesamtgröße der Heaps und gruppierten Indizes der Tabellen in der Datenbank.  
  
-   Der freie Speicherplatz auf allen angefügten Laufwerken plus die Rohdatengröße.  
  
 Nicht im Standardspeicherplatz enthalten sind nicht gruppierte Indizes und indizierte Sichten.  
  
 **-C** _max_columns_in_index_  
 Gibt die maximale Anzahl von Spalten in Indizes an, die von **dta** vorgeschlagen werden. Der Maximalwert ist 1024. Standardmäßig ist dieses Argument auf 16 festgelegt.  
  
 **-c** _max_key_columns_in_index_  
 Gibt die maximale Anzahl von Schlüsselspalten in Indizes an, die von **dta** vorgeschlagen werden. Der Standardwert ist 16, was dem maximal zulässigen Wert entspricht. Das **dta** -Hilfsprogramm berücksichtigt auch die Erstellung von Indizes mit integrierten Spalten. Bei Indizes, für die eingeschlossene Spalten empfohlen werden, kann die in diesem Argument angegebene Anzahl von Spalten überschritten werden.  
  
 **-D** _database_name_  
 Gibt den Namen jeder zu optimierenden Datenbank an. Die erste Datenbank ist die Standarddatenbank. Sie können mehrere Datenbanken angeben, indem Sie Kommas als Trennzeichen zwischen den Datenbanknamen verwenden. Beispiel:  
  
```  
dta -D database_name1, database_name2...  
```  
  
 Alternativ dazu können Sie auch mehrere Datenbanken angeben, indem Sie das Argument **-D** für jeden einzelnen Datenbanknamen verwenden. Beispiel:  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 Das Argument **-D** ist verbindlich. Wenn das Argument **-d** nicht angegeben wurde, stellt **dta** zunächst eine Verbindung mit der Datenbank her, die in der ersten `USE database_name` -Klausel in der Arbeitsauslastung angegeben ist. Enthält die Arbeitsauslastung keine explizite `USE database_name` -Klausel, müssen Sie das Argument **-d** verwenden.  
  
 Angenommen, eine Arbeitsauslastung enthält keine explizite `USE database_name` -Klausel. Wenn Sie nun den folgenden **dta** -Befehl verwenden, wird keine Empfehlung generiert:  
  
```  
dta -D db_name1, db_name2...  
```  
  
 Wenn Sie dieselbe Arbeitsauslastung verwenden, dieses Mal jedoch den folgenden **dta** -Befehl mit dem Argument **-d** angeben, wird eine Empfehlung generiert:  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 Gibt die erste Datenbank an, mit der **dta** eine Verbindung herstellt, wenn eine Arbeitsauslastung optimiert wird. Für dieses Argument kann nur eine einzige Datenbank angegeben werden. Beispiel:  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 Wenn mehrere Datenbanknamen angegeben werden, gibt **dta** einen Fehler zurück. Das Argument **-d** ist optional.  
  
 Wenn Sie eine XML-Eingabedatei verwenden, können Sie die erste Datenbank, mit der **dta** eine Verbindung herstellt, mithilfe des **DatabaseToConnect** -Elements angeben, das sich unterhalb des **TuningOptions** -Elements befindet. Weitere Informationen finden Sie unter [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).  
  
 Wenn Sie nur eine einzige Datenbank optimieren, wird mit dem Argument **-d** eine Funktionalität bereitgestellt, die der des Arguments **-d** im Hilfsprogramm **sqlcmd** ähnlich ist. Die USE *database_name* -Anweisung wird jedoch nicht ausgeführt. Weitere Informationen finden Sie unter [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
 **-E**  
 Verwendet eine vertrauenswürdige Verbindung, statt ein Kennwort anzufordern. Sie müssen entweder das Argument **-E** oder das Argument **-U** verwenden, das eine Anmelde-ID angibt.  
  
 **-e** _tuning_log_name_  
 Gibt den Namen der Tabelle oder der Datei an, in der **dta** Ereignisse aufzeichnet, die nicht optimiert werden konnten. Die Tabelle wird auf dem Server erstellt, auf dem die Optimierung ausgeführt wird.  
  
 Wenn eine Tabelle verwendet wird, geben Sie den Namen im folgenden Format an: *[database_name].[owner_name].table_name*. In der folgenden Tabelle ist für jeden Parameter der zugehörige Standardwert aufgeführt:  
  
|Parameter|Standardwert|Details|  
|---------------|-------------------|-------------|  
|*database_name*|Der mit der Option *-D* angegebene Wert für **database_name** .||  
|*owner_name*|**dbo**|*owner_name* muss **dbo** lauten. Wenn ein anderer Wert angegeben wird, schlägt die Ausführung von **dta** fehl, und es wird ein Fehler ausgegeben.|  
|*table_name*|Keine||  
  
 Wenn eine Datei verwendet wird, geben Sie als Dateierweiterung .xml an. Beispiel: TuningLog.xml.  
  
> [!NOTE]  
>  Das Hilfsprogramm **dta** löscht den Inhalt benutzerdefinierter Optimierungsprotokolltabellen nicht, wenn die Sitzung gelöscht wird. Beim Optimieren sehr großer Arbeitsauslastungen wird empfohlen, für das Optimierungsprotokoll eine Tabelle anzugeben. Die Optimierung großer Arbeitsauslastungen kann große Optimierungsprotokolle zur Folge haben. Daher lassen sich die Sitzungen deutlich schneller löschen, wenn eine Tabelle verwendet wird.  
  
 **-F**  
 Ermöglicht **dta** das Überschreiben einer vorhandenen Ausgabedatei. Wenn bereits eine Ausgabedatei mit demselben Namen vorhanden ist und **-F** nicht angegeben wird, gibt **dta** einen Fehler zurück. Sie können **-F** mit **-of**, **-or** oder **-ox** verwenden.  
  
 **-fa** _physical_design_structures_to_add_  
 Gibt an, welche Arten physischer Entwurfsstruktur **dta** in die Empfehlung aufnehmen soll. In der folgenden Tabelle sind die Werte, die für dieses Argument angegeben werden können, sowie die zugehörigen Beschreibungen aufgeführt. Wenn kein Wert angegeben wird, verwendet **dta** den Standardwert **-fa IDX**.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|IDX_IV|Indizes und indizierte Sichten.|  
|IDX|Nur Indizes.|  
|IV|Nur indizierte Sichten.|  
|NCL_IDX|Nur nicht gruppierte Indizes.|  
  
 **-fi**  
 Gibt an, dass gefilterte Indizes für neue Empfehlungen berücksichtigt werden. Weitere Informationen finden Sie unter [erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
**-fc**  
 Gibt an, dass Columnstore-Indizes für neue Empfehlungen berücksichtigt werden sollen. DTA unterstützt gruppierte und nicht gruppierte Columnstore-Indizes. Weitere Informationen finden Sie unter    
[Empfehlungen für den Columnstore-Index im Datenbankoptimierungsratgeber (DTA)](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md).
 ||  
|-|  
|**Gilt für**:  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.|  

  
 **-fk** _keep_existing_option_  
 Gibt an, welche vorhandenen physischen Entwurfsstrukturen **dta** beim Generieren der Empfehlung beibehalten muss. In der folgenden Tabelle sind die Werte, die für dieses Argument angegeben werden können, sowie die zugehörigen Beschreibungen aufgeführt:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|Keine|Keine vorhandenen Strukturen|  
|ALL|Alle vorhandenen Strukturen|  
|ALIGNED|Alle Strukturen mit ausgerichteten Partitionen.|  
|CL_IDX|Alle gruppierten Indizes für Tabellen|  
|IDX|Alle gruppierten und nicht gruppierten Indizes für Tabellen|  
  
 **-fp** _partitioning_strategy_  
 Gibt an, ob neue physische Entwurfsstrukturen (Indizes und indizierte Sichten), die von **dta** vorgeschlagen werden, partitioniert werden sollen und wie diese Partitionierung ggf. erfolgen soll. In der folgenden Tabelle sind die Werte, die für dieses Argument angegeben werden können, sowie die zugehörigen Beschreibungen aufgeführt:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|Keine|Keine Partitionierung|  
|FULL|Vollständige Partitionierung (zur Verbesserung der Leistung)|  
|ALIGNED|Nur ausgerichtete Partitionierung (zur Verbesserung der Verwaltbarkeit)|  
  
 ALIGNED bedeutet, dass jeder vorgeschlagene Index in der von **dta** generierten Empfehlung ganz genau so wie die zugrunde liegende Tabelle partitioniert wird, für die der Index definiert wurde. Nicht gruppierte Indizes für eine indizierte Sicht werden mit der indizierten Sicht ausgerichtet. Für dieses Argument kann nur ein Wert angegeben werden. Der Standardwert ist **-fp NONE**.  
  
 **-fx** _drop_only_mode_  
 Gibt an, dass **dta** nur das Löschen vorhandener physischer Entwurfsstrukturen in Erwägung zieht. Das Erstellen neuer physischer Entwurfsstrukturen wird nicht in Erwägung gezogen. Wenn diese Option angegeben wird, bewertet **dta** die Zweckmäßigkeit vorhandener physischer Entwurfsstrukturen und empfiehlt das Löschen selten verwendeter Strukturen. Für dieses Argument werden keinen Werte angegeben. Es kann nicht mit den Argumenten **-fa**, **-fp** oder **-fk ALL** verwendet werden.  
  
 **-ID** _session_ID_  
 Gibt einen numerischen Bezeichner für die Optimierungssitzung an. Wird dieses Argument nicht angegeben, generiert **dta** eine ID-Nummer. Sie können diesen Bezeichner zum Anzeigen von Informationen zu vorhandenen Optimierungssitzungen verwenden. Wenn Sie keinen Wert für **-ID** angeben, muss mit **-s** ein Sitzungsname angegeben werden.  
  
 **-ip**  
 Gibt an, dass der Plancache als Arbeitsauslastung verwendet wird. Die ersten 1.000 Plancacheereignisse für explizit ausgewählte Datenbanken werden analysiert. Dieser Wert kann mit der Option **–n** geändert werden.  

**-iq**  
 Gibt an, dass der Abfragespeicher als Arbeitsauslastung verwendet wird. Die wichtigsten 1.000 Ereignisse aus dem Abfragespeicher für explizit ausgewählte Datenbanken werden analysiert. Dieser Wert kann mit der Option **–n** geändert werden.  Weitere Informationen finden Sie unter [Abfragespeicher](../../relational-databases/performance/how-query-store-collects-data.md) und [Datenbankoptimierung mithilfe der Arbeitsauslastung aus dem Abfragespeicher](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
 ||  
|-|  
|**Gilt für**:  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.|  
     
 **-if** _workload_file_  
 Gibt den Pfad und den Namen der Arbeitsauslastungsdatei an, die als Eingabe für die Optimierung verwendet werden soll. Die Datei muss eines der folgenden Formate aufweisen: TRC (SQL Server Profiler-Ablaufverfolgungsdatei), SQL (SQL-Datei) oder LOG ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ablaufverfolgungsdatei). Es muss entweder eine Arbeitsauslastungsdatei oder eine Arbeitsauslastungstabelle angegeben werden.  
  
 **-it** _workload_trace_table_name_  
 Gibt den Namen einer Tabelle an, die die Arbeitsauslastungs-Ablaufverfolgung für das Optimieren enthält. Der Name wird im folgenden Format angegeben: [*database_name*] **.** [*owner_name*] **.** _table_name_.  
  
 In der folgenden Tabelle ist für jeden Parameter der zugehörige Standardwert aufgeführt:  
  
|Parameter|Standardwert|  
|---------------|-------------------|  
|*database_name*|Der mit der Option *-D* angegebene Wert für **database_name**.|  
|*owner_name*|**dbo**|  
|*table_name*|Keine.|  
  
> [!NOTE]  
>  *owner_name* muss **dbo** lauten. Wenn ein anderer Wert angegeben wird, schlägt die Ausführung von **dta** fehl, und es wird ein Fehler zurückgegeben. Beachten Sie zudem, dass entweder eine Arbeitsauslastungstabelle oder eine Arbeitsauslastungsdatei angegeben werden muss.  
  
 **-ix** _input_XML_file_name_  
 Gibt den Namen der XML-Datei an, die die Eingabeinformationen für **dta** enthält. Dieser muss einem gültigen XML-Dokument entsprechen, das mit "DTASchema.xsd" übereinstimmt. Widersprüchliche Argumente, die an der Eingabeaufforderung für Optimierungsoptionen angegeben werden, überschreiben den entsprechenden Wert in dieser XML-Datei. Die einzige Ausnahme hiervon sind benutzerdefinierte Konfigurationen, die im Auswertungsmodus in der XML-Eingabedatei eingegeben werden. Wenn beispielsweise eine Konfiguration im **Configuration** -Element der XML-Eingabedatei eingegeben wird und das **EvaluateConfiguration** -Element ebenfalls als Optimierungsoption angegeben wird, überschreibt die in der XML-Eingabedatei angegebene Optimierungsoption die an der Eingabeaufforderung eingegebene Option.  

 **-k** _maxtotalindexes_  
 Die maximale Anzahl von Indizes in der Empfehlung  

 **-K** _maxtotalindexes_  
 Die maximale Anzahl von Indizes pro Tabelle

 **-m** _minimum_improvement_  
 Gibt den Mindestprozentsatz der Verbesserung an, die durch eine empfohlene Konfiguration erreicht werden muss.  
  
 **-N** _online_option_  
 Gibt an, ob physische Entwurfsstrukturen online erstellt werden. In der folgenden Tabelle sind die Werte, die Sie für dieses Argument angeben können, sowie die zugehörigen Beschreibungen aufgeführt:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|OFF|Es können keine empfohlenen physischen Entwurfsstrukturen online erstellt werden.|  
|EIN|Alle empfohlenen physischen Entwurfsstrukturen können online erstellt werden.|  
|MIXED|Der Datenbankoptimierungsratgeber versucht, sofern möglich physische Entwurfsstrukturen zu empfehlen, die online erstellt werden können.|  
  
 Wenn Indizes online erstellt werden, wird ONLINE = ON an die Objektdefinition angefügt.  
  
 **-n** _number_of_events_  
 Gibt die Anzahl von Ereignissen in der Arbeitsauslastung an, die **dta** optimieren soll. Wenn dieses Argument angegeben wird und es sich bei der Arbeitsauslastung um eine Ablaufverfolgungsdatei handelt, die Informationen zur Dauer enthält, optimiert **dta** die Ereignisse dabei Anhand ihrer Dauer in abnehmender Reihenfolge. Dieses Argument ist hilfreich, um zwei Konfigurationen mit physischen Entwurfsstrukturen miteinander zu vergleichen. Für den Vergleich von zwei Konfigurationen geben Sie wie folgt für beide Konfigurationen dieselbe Anzahl von zu optimierenden Ereignissen und dann, ebenfalls für beide Konfigurationen, eine unbegrenzte Dauer an:  
  
```  
dta -n number_of_events -A 0  
```  
  
 In diesem Fall ist es wichtig, eine unbegrenzte Optimierungszeit anzugeben (`-A 0`). Andernfalls geht der Datenbankoptimierungsratgeber standardmäßig von einer achtstündigen Optimierungsdauer aus.
 
 **-I** _time_window_in_hours_   
   Gibt das Zeitfenster (in Stunden) an, in dem eine Abfrage ausgeführt werden muss, damit sie von DTA bei der Verwendung der Option **-iq** (Arbeitsauslastung aus dem Abfragespeicher) für die Optimierung berücksichtigt wird. 
```  
dta -iq -I 48  
```  
In diesem Fall verwendet DTA Abfragespeicher als Quelle der Arbeitsauslastung und berücksichtigt nur Abfragen, die in den letzten 48 Stunden ausgeführt wurden.  
  ||  
|-|  
|**Gilt für**:  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.|  


  
 **-of** _output_script_file_name_  
 Gibt an, dass **dta** die Empfehlung in Form eines [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts in die Datei mit dem angegebenen Dateinamen im angegebenen Zielverzeichnis schreibt.  
  
 Sie können **-F** mit dieser Option verwenden. Stellen Sie sicher, dass der Dateiname eindeutig ist, insbesondere wenn Sie außerdem **-or** und **-ox** verwenden.  
  
 **-or** _output_xml_report_file_name_  
 Gibt an, dass **dta** die Empfehlung in einen Ausgabebericht im XML-Format schreibt. Wenn ein Dateiname angegeben wird, werden die Empfehlungen in diese Datei im angegebenen Zielverzeichnis geschrieben. Andernfalls verwendet **dta** den Sitzungsnamen, um den Dateinamen zu generieren, und schreibt diese Datei in das aktuelle Verzeichnis.  
  
 Sie können **-F** mit dieser Option verwenden. Stellen Sie sicher, dass der Dateiname eindeutig ist, insbesondere wenn Sie außerdem **-of** und **-ox** verwenden.  
  
 **-ox** _output_XML_file_name_  
 Gibt an, dass **dta** die Empfehlung in Form einer XML-Datei mit dem angegebenen Dateinamen im angegebenen Zielverzeichnis schreibt. Stellen Sie sicher, dass der Datenbankoptimierungsratgeber über die Berechtigungen zum Schreiben in das Zielverzeichnis verfügt.  
  
 Sie können **-F** mit dieser Option verwenden. Stellen Sie sicher, dass der Dateiname eindeutig ist, insbesondere wenn Sie außerdem **-of** und **-or** verwenden.  
  
 **-P** _password_  
 Gibt das Kennwort für die Anmelde-ID an. Wenn diese Option nicht verwendet wird, fordert **dta** zur Eingabe eines Kennworts auf.  
  
 **-q**  
 Legt den stillen Modus fest. Es werden keine Informationen an der Konsole ausgegeben. Das gilt auch für Status- und Headerinformationen.  
  
 **-rl** _analysis_report_list_  
 Gibt die Liste der zu generierenden Analyseberichte an. In der folgenden Tabelle sind die Werte aufgeführt, die für dieses Argument angegeben werden können:  
  
|Wert|Bericht|  
|-----------|------------|  
|ALL|Alle Analyseberichte|  
|STMT_COST|Anweisungskostenbericht|  
|EVT_FREQ|Ereignishäufigkeitsbericht|  
|STMT_DET|Anweisungsdetailbericht|  
|CUR_STMT_IDX|Beziehungsbericht Anweisung – Index (aktuelle Konfiguration)|  
|REC_STMT_IDX|Beziehungsbericht Anweisung – Index (empfohlene Konfiguration)|  
|STMT_COSTRANGE|Anweisungskostenumfangs-Bericht|  
|CUR_IDX_USAGE|Indexverwendungsbericht (aktuelle Konfiguration)|  
|REC_IDX_USAGE|Indexverwendungsbericht (empfohlene Konfiguration)|  
|CUR_IDX_DET|Indexdetailbericht (aktuelle Konfiguration)|  
|REC_IDX_DET|Indexdetailbericht (empfohlene Konfiguration)|  
|VIW_TAB|Beziehungsbericht Sicht – Tabelle|  
|WKLD_ANL|Arbeitsauslastungsanalyse-Bericht|  
|DB_ACCESS|Datenbankzugriffsbericht|  
|TAB_ACCESS|Tabellenzugriffsbericht|  
|COL_ACCESS|Spaltenzugriffsbericht|  
  
 Sie können mehrere Berichte angeben, indem Sie die einzelnen Werte durch Kommas voneinander trennen. Beispiel:  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 Gibt den Namen des Computers und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz an, mit der eine Verbindung hergestellt werden soll. Wenn *server_name* nicht angegeben wird, stellt **dta** eine Verbindung mit der Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem lokalen Computer her. Diese Option ist erforderlich, wenn eine Verbindung mit einer benannten Instanz hergestellt wird oder wenn **dta** von einem Remotecomputer im Netzwerk aus ausgeführt wird.  
  
 **-s** _session_name_  
 Gibt den Namen der Optimierungssitzung an. Der Sitzungsname ist erforderlich, wenn **-ID** nicht angegeben wird.  
  
 **-Tf** _table_list_file_  
 Gibt den Namen einer Datei an, die eine Liste der Tabellen enthält, die optimiert werden sollen. Jede Tabelle, die in der Datei aufgelistet wird, sollte in einer neuen Zeile beginnen. Tabellennamen sollten mit einer dreiteiligen Benennung qualifiziert werden, z.B. **AdventureWorks2012.HumanResources.Department**. Zum Aufrufen der Tabellenskalierungsfunktion kann hinter dem Namen einer vorhandenen Tabelle eine Zahl angegeben werden, die die voraussichtliche Anzahl von Zeilen in der Tabelle anzeigt. Der Datenbankoptimierungsratgeber berücksichtigt die voraussichtliche Anzahl von Zeilen bei der Optimierung oder Bewertung von Anweisungen in der Arbeitsauslastung, die auf diese Tabellen verweisen. Beachten Sie, dass zwischen der Anzahl in *number_of_rows* und dem Wert für *table_name* ein oder mehrere Leerzeichen stehen können.  
  
 Eine für *table_list_file* angegebene Datei muss folgendes Format aufweisen:  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 Die Verwendung dieses Arguments ist eine Alternative zur Eingabe einer Tabellenliste an der Eingabeaufforderung ( **-Tl**). Verwenden Sie keine Tabellenlistendatei ( **-Tf**), wenn Sie **-Tl** verwenden. Wenn beide Argumente verwendet werden, schlägt die Ausführung von **dta** fehl und es wird ein Fehler zurückgegeben.  
  
 Wenn die Argumente **-Tf** und **-Tl** nicht angegeben werden, werden alle Benutzertabellen in den angegebenen Datenbanken bei der Optimierung berücksichtigt.  
  
 **-Tl** _table_list_  
 Gibt eine Liste mit zu optimierenden Tabellen an der Eingabeaufforderung an. Geben Sie zwischen Tabellennamen ein Komma als Trennzeichen ein. Wenn nur eine Datenbank mit dem Argument **-D** angegeben wird, müssen die Tabellennamen nicht mit einem Datenbanknamen qualifiziert werden. Andernfalls ist für jede Tabelle ihr vollqualifizierter Name im Format *database_name.schema_name.table_name* erforderlich.  
  
 Die Verwendung dieses Arguments ist eine Alternative zum Verwenden einer Tabellenlistendatei ( **-Tf**). Wenn sowohl **-Tl** als auch **-Tf** verwendet werden, schlägt die Ausführung von **dta** fehl und es wird ein Fehler zurückgegeben.  
  
 **-U** _login_id_  
 Gibt die Anmelde-ID an, die zum Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet wird.  
  
 **-u**  
 Startet die Benutzeroberfläche des Datenbankoptimierungsratgebers. Alle Parameter werden als Starteinstellungen für die Benutzeroberfläche interpretiert.  
  
 **-x**  
 Startet eine Optimierungssitzung und beendet das Hilfsprogramm.  
  
## <a name="remarks"></a>Bemerkungen  
 Drücken Sie einmal STRG+C, um die Optimierungssitzung zu beenden und Empfehlungen basierend auf der Analyse zu generieren, die **dta** bis zu diesem Zeitpunkt abgeschlossen hat. Daraufhin werden Sie aufgefordert zu entscheiden, ob Sie Empfehlungen generieren möchten. Drücken Sie erneut STRG+C, um die Optimierungssitzung zu beenden, ohne Empfehlungen zu generieren.  
  
## <a name="examples"></a>Beispiele  
 **A. Optimieren einer Arbeitsauslastung mit einer Empfehlung, die Indizes und indizierte Sichten einschließt**  
  
 In diesem Beispiel wird eine sichere Verbindung (`-E`) verwendet, um eine Verbindung mit der **tpcd1G** -Datenbank auf dem Server „MyServer“ herzustellen und dann eine Arbeitsauslastung zu analysieren und Empfehlungen zu erstellen. Die Ausgabe wird in die Skriptdatei "script.sql" geschrieben. Wenn „script.sql“ bereits vorhanden ist, wird die Datei von **dta** überschrieben, da das Argument `-F` angegeben wurde. Die Dauer der Optimierungssitzung wird nicht begrenzt, um sicherzustellen, dass die Arbeitsauslastung vollständig analysiert wird (`-A 0`). Die Empfehlung muss eine Verbesserung von mindestens 5 % (`-m 5`) ergeben. In der endgültigen Empfehlung von **dta** sollten Indizes und indizierte Sichten enthalten sein (`-fa IDX_IV`).  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **B. Begrenzen des verwendeten Speicherplatzes**  
  
 In diesem Beispiel wird die Gesamtgröße der Datenbank, einschließlich Rohdaten und zusätzlicher Indizes, auf 3 GB begrenzt (`-B 3000`) und die Ausgabe in die Datei d:\result_dir\script1.sql geschrieben. Die Optimierung wird maximal 1 Stunde ausgeführt (`-A 60`).  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **C. Begrenzen der Anzahl optimierter Abfragen**  
  
 In diesem Beispiel wird die Optimierung beendet, sobald die Anzahl der Abfragen, die aus der Datei orders_wkld.sql gelesen werden, den Wert 10 erreicht hat (`-n 10`) oder die Optimierung für eine Dauer von 15 Minuten ausgeführt wurde (`-A 15`), je nachdem, welches Ereignis zuerst eintritt. Wenn Sie die Optimierung aller 10 Abfragen sicherstellen möchten, geben Sie mithilfe von `-A 0` eine unbegrenzte Optimierungszeit an. Wenn die Zeit ein wichtiger Faktor ist, geben Sie ein geeignetes Zeitlimit an, indem Sie wie im folgenden Beispiel gezeigt mit dem Argument `-A` die Anzahl der Minuten angeben, die für die Optimierung zur Verfügung stehen.  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **D. Optimieren bestimmter Tabellen, die in einer Datei aufgeführt sind**  

 In diesem Beispiel wird die Verwendung von *table_list_file* (das Argument **-Tf** ) veranschaulicht. Die Datei table_list.txt enthält Folgendes:  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

Der Inhalt der Datei table_list.txt gibt Folgendes an:  

- Es sollen nur die Datenbanktabellen **Customer**, **Store** und **Product** optimiert werden.  
  
- Es wird angenommen, dass die Tabellen **Customer** und **Product** 100.000 bzw. 2.000.000 Zeilen enthalten.  
  
- Es wird angenommen, dass die Anzahl von Zeilen in **Store** der aktuellen Anzahl von Zeilen in der Tabelle entspricht.  

    Beachten Sie, dass sich in *table_list_file* zwischen dem Wert für die Anzahl von Zeilen und dem davor aufgeführten Tabellennamen ein oder mehrere Leerzeichen befinden können.  
    
    Die Optimierungszeit beträgt 2 Stunden (`-A 120`), und die Ausgabe wird in eine XML-Datei geschrieben (`-ox XMLTune.xml`).  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>Weitere Informationen

- [Referenz zum Eingabeaufforderungs-Hilfsprogramm &#40;Datenbank-Engine&#41;](../../tools/command-prompt-utility-reference-database-engine.md)
- [Datenbankoptimierungsratgeber](../../relational-databases/performance/database-engine-tuning-advisor.md)