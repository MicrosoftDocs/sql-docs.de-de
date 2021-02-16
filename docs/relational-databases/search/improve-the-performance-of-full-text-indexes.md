---
description: Verbessern der Leistung von Volltextindizes
title: Verbessern der Leistung von Volltextindizes | Microsoft-Dokumentation
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- performance [SQL Server], full-text search
- full-text queries [SQL Server], performance
- crawls [full-text search]
- full-text indexes [SQL Server], performance
- full-text search [SQL Server], performance
- batches [SQL Server], full-text search
ms.assetid: ef39ef1f-f0b7-4582-8e9c-31d4bd0ad35d
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d834493bac55e71e00fff11a9d27d7a0ee47387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348632"
---
# <a name="improve-the-performance-of-full-text-indexes"></a>Verbessern der Leistung von Volltextindizes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
In diesem Thema werden einige häufige Ursachen für eine geringe Leistung für Volltextindizes und -Abfragen beschrieben. Darüber hinaus werden einige Vorschläge für das Verringern dieser Probleme und das Verbessern der Leistung gemacht.
  
##  <a name="common-causes-of-performance-issues"></a><a name="causes"></a> Common causes of performance issues
### <a name="hardware-resource-issues"></a>Probleme mit Hardware
Die Leistung für Volltextindizes und Volltextabfragen wird von den Hardwareressourcen wie Arbeitsspeicher, Datenträgergeschwindigkeit, CPU-Geschwindigkeit und Computerarchitektur beeinflusst.  

Der Hauptgrund für eine eingeschränkte Leistung beim Erstellen von Volltextindizes sind Einschränkungen bei den Hardwareressourcen:  
  
-   **CPU** Wenn die CPU-Nutzung des Filterdaemon-Hostprozesses (fdhost.exe) oder des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Prozesses (sqlservr.exe) fast 100 % beträgt, dann ist die CPU der Engpass.  
  
-   **Speicher** Wenn ein Mangel an physischem Speicher besteht, kann ggf. der Arbeitsspeicher der Engpass sein.  

-   **Datenträger**. Wenn die durchschnittliche Warteschlangenlänge des Datenträgers zweimal so groß wie die Anzahl von Leseköpfen ist, dann besteht ein Engpass auf dem Datenträger. Die erste Problemumgehung ist das Erstellen von Volltextkatalogen, die getrennt von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankendateien und -protokollen sind. Platzieren Sie Protokolle, die Datenbankdateien und Volltextkataloge auf getrennten Datenträgern. Installieren Sie schnellere Datenträger, und verwenden Sie RAID, um die Indexleistung zu verbessern.  
  
    > [!NOTE]  
    > Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] kann die Volltext-Engine den AWE-Speicher verwenden, weil die Volltext-Engine Teil von sqlservr.exe ist. Weitere Informationen finden Sie unter [Architektur der Volltextsuche](../../relational-databases/search/full-text-search.md#architecture).  

### <a name="full-text-batching-issues"></a>Volltext-Batchverarbeitungs Probleme
 Wenn auf dem System keine Hardwareengpässe vorhanden sind, hängt die Indizierungsleistung der Volltextsuche vor allem von folgenden Faktoren ab:  
  
-   Wie lange das Erstellen von Volltextbatches durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dauert.  
  
-   Wie schnell der Filterdaemon diese Batches verarbeiten kann.  

### <a name="full-text-index-population-issues"></a>Volltextindex-Auffüllungsprobleme
-   **Typ der Auffüllung.** Im Gegensatz zur vollständigen Auffüllung eignet sich die inkrementelle, manuelle und automatische Änderungsnachverfolgung der Auffüllung nicht zum Maximieren von Hardwareressourcen, um schnellere Geschwindigkeiten zu erzielen. Aus diesem Grund können die Optimierungsvorschläge in diesem Thema die Leistung für Volltextindizierung nicht verbessern, wenn sie inkrementelle, manuelle oder automatische Änderungsüberwachungsauffüllung verwenden.  
  
-   **Mastermerge** Nach dem Ende einer Auffüllung wird ein abschließender Mergeprozess ausgelöst, der die Indexfragmente zu einem Mastervolltextindex zusammenführt. Dies ermöglicht eine verbesserte Abfrageleistung, da statt mehrerer Indexfragmente nur der Masterindex abgefragt werden muss. Zudem können bessere Bewertungsstatistiken zum Erstellen der Relevanzrangfolge verwendet werden. Der Mastermergeprozess kann E/A-intensiv sein, da beim Zusammenführen der Indexfragmente umfangreiche Daten geschrieben und gelesen werden müssen. Eingehende Abfragen werden dadurch jedoch nicht blockiert.  
  
    Die Masterzusammenführung einer großen Datenmenge kann eine Transaktion mit langer Ausführungszeit erzeugen und das Abschneiden des Transaktionsprotokolls während des Prüfpunkts verzögern. In diesem Fall kann das Transaktionsprotokoll unter dem vollständigen Wiederherstellungsmodell erheblich anwachsen. Sie sollten sicherstellen, dass das Transaktionsprotokoll vor dem Reorganisieren eines großen Volltextindexes in einer Datenbank, die das vollständige Wiederherstellungsmodell verwendet, genügend Speicherplatz für eine Transaktion mit langer Laufzeit bietet. Weitere Informationen finden Sie unter [Verwalten der Größe der Transaktionsprotokolldatei](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
##  <a name="tune-the-performance-of-full-text-indexes"></a><a name="tuning"></a> Optimieren der Leistung von Volltextindizes  
Sie können die Leistung Ihrer Volltextindizes mit den folgenden bewährten Methoden maximieren:  
  
-   Um alle Prozessoren oder Kerne maximal zu nutzen, setzen Sie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) '**max full-text crawl ranges**' auf die Anzahl der CPUs des Systems fest. Informationen zu dieser Konfigurationsoption finden Sie unter [Max. Bereich für Volltextdurchforstung (Serverkonfigurationsoption)](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md).  
  
-   Stellen Sie sicher, dass die Basistabelle einen gruppierten Index besitzt. Verwenden Sie einen ganzzahligen Datentyp für die erste Spalte des gruppierten Index. Vermeiden Sie das Verwenden von GUIDs in der ersten Spalte des gruppierten Index. Eine Mehrbereichsauffüllung für einen gruppierten Index kann die höchste Auffüllungsgeschwindigkeit erzielen. Es ist ratsam, für die Spalte, die als Volltextschlüssel dient, einen ganzzahligen Datentyp zu verwenden.  
  
-   Aktualisieren Sie die Statistiken der Basistabelle mithilfe der [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) -Anweisung. Noch wichtiger ist das Aktualisieren der Statistik für den gruppierten Index bzw. des Volltextschlüssels für eine vollständige Auffüllung. Dies unterstützt eine Mehrbereichsauffüllung beim Erzeugen guter Partitionen in der Tabelle.  
  
-   Bevor Sie eine vollständige Auffüllung auf einem großen Computer mit mehreren CPUs ausführen, sollten Sie die Größe des Pufferpools vorübergehend einschränken, indem Sie den **max server memory** -Wert so festlegen, dass noch genug Speicher für den fdhost.exe-Prozess und die Betriebssystemprozesse verfügbar ist. Weitere Informationen finden Sie unter [Schätzen der Arbeitsspeicheranforderungen des Filterdaemon-Hostprozesses (fdhost.exe)](#estimate) weiter unten in diesem Artikel.

-   Wenn Sie eine inkrementelle Auffüllung basierend auf einer timestamp-Spalte verwenden, erstellen Sie einen zweiten Index in einer **timestamp**-Spalte, wenn Sie die Leistung der inkrementellen Auffüllung verbessern möchten.  
  
##  <a name="troubleshoot-the-performance-of-full-populations"></a><a name="full"></a> Beheben von Leistungsproblemen bei vollständiger Auffüllung  
### <a name="review-the-full-text-crawl-logs"></a>Überprüfen Sie die Volltextdurchforstungsprotokolle
 Leistungsprobleme diagnostizieren Sie, indem Sie die Volltextdurchforstungsprotokolle überprüfen.
 
Tritt während eines Durchforstungsvorgangs ein Fehler auf, wird von der Durchforstungsprotokollfunktion der Volltextsuche ein Durchforstungsprotokoll erstellt und gewartet. Dabei handelt es sich um eine Nur-Text-Datei. Jedes Durchforstungsprotokoll gehört zu einem bestimmten Volltextkatalog. Standardmäßig befinden sich Durchforstungsprotokolle für eine bestimmte Instanz (in diesem Beispiel die Standardinstanz) im Ordner `%ProgramFiles%\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\LOG`.
 
Das Benennungsschema für Durchforstungsprotokolldateien lautet folgendermaßen:  
  
`SQLFT<DatabaseID\><FullTextCatalogID\>.LOG[<n\>]`
  
Die variablen Teile des Durchforstungsprotokolldatei-Namens sind die folgenden.
-   \<**DatabaseID**>: Die ID einer Datenbank. <**dbid**> ist eine fünfstellige Zahl mit führenden Nullen.  
-   <**Volltext-Katalog-ID**>: Die ID eines Volltextkatalogs. \<**catid**> ist eine fünfstellige Zahl mit führenden Nullen.  
-   <**n**>: Ist eine ganze Zahl, die angibt, dass mindestens ein Durchforstungsprotokoll desselben Volltextkatalogs vorhanden ist.  
  
 `SQLFT0000500008.2` ist z.B. die Durchforstungsprotokolldatei für eine Datenbank mit der Datenbank-ID = 5 und der Volltextkatalog-ID = 8. Die 2 am Ende des Dateinamens gibt an, dass zwei Durchforstungsprotokolldateien für dieses Datenbank-Katalog-Paar vorhanden sind.  

### <a name="check-physical-memory-usage"></a>Überprüfen Sie die Verwendung des physischen Speichers  
 Während einer Volltextauffüllung ist es möglich, dass `fdhost.exe` oder `sqlservr.exe` viel Arbeitsspeicher beansprucht oder nicht genügend Arbeitsspeicher vorhanden ist.
-   Wenn das Protokoll der Volltextdurchforstung zeigt, dass `fdhost.exe` häufig neu gestartet oder dass Fehlercode 8007008 zurückgegeben wird, bedeutet dies, dass für einen der Prozesse kein Speicher mehr verfügbar ist.
-   Wenn `fdhost.exe` Dumps erzeugt, insbesondere auf großen Multi-CPU-Computern, steht möglicherweise nicht genügend Arbeitsspeicher zur Verfügung.  
-   Informationen zu Arbeitsspeicherpuffern, die von einer Volltextdurchforstung verwendet werden, finden Sie unter [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md).  
  
 Die möglichen Ursachen für geringen Speicherplatz oder Problemen mit nicht genügend Arbeitsspeicher sind folgende:  
  
-   **Der Arbeitsspeicher reicht nicht aus**. Wenn der physische Speicher, der während einer vollständigen Auffüllung verfügbar ist, 0 ist, kann es sein, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Pufferpool den größten Teil des physischen Speichers des Systems belegt.  
  
     Der Prozess `sqlservr.exe` versucht, den gesamten verfügbaren Speicher für den Pufferpool bis zum konfigurierten maximalen Serverarbeitsspeicher für sich zu beanspruchen. Wenn die **max server memory** -Zuordnung zu groß ist, können Probleme aufgrund ungenügenden Arbeitsspeichers und Fehler bei der Zuordnung von gemeinsam genutzten Speicherbereich für den Prozess „fdhost.exe“ auftreten.  
  
     Sie können dieses Problem beheben, indem Sie den Wert **max server memory** des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Pufferpools entsprechend anpassen. Weitere Informationen finden Sie unter [Schätzen der Arbeitsspeicheranforderungen des Filterdaemon-Hostprozesses (fdhost.exe)](#estimate) weiter unten in diesem Artikel. Möglicherweise ist es auch hilfreich, die verwendete Batchgröße für die Volltextindizierung zu reduzieren.  

-   **Speicherkonflikte**. Während einer Volltextauffüllung können auf Multi-CPU-Computern Konflikte zwischen fdhost.exe oder sqlservr.exe um den Pufferpoolarbeitsspeicher auftreten. Der daraus resultierende Mangel an gemeinsam genutztem Speicherbereich verursacht Batchwiederholungen, Arbeitsspeicherüberlastung und Dumps durch den Prozess "fdhost.exe".  

-   **Auslagerungsprobleme** Eine zu kleine Auslagerungsdatei, z. B. wenn ein System über eine kleine Auslagerungsdatei mit eingeschränkter Vergrößerung verfügt, kann ebenfalls dazu führen, dass fdhost.exe oder sqlservr.exe nicht mehr auf genügend Arbeitsspeicher zugreifen können. Wenn die Crawlprotokolle keine speicherbezogenen Fehler anzeigen, ist die Leistung wahrscheinlich aufgrund zu vieler Auslagerungen beeinträchtigt.  
  
### <a name="estimate-the-memory-requirements-of-the-filter-daemon-host-process-fdhostexe"></a><a name="estimate"></a> Schätzen der Arbeitsspeicheranforderungen des Filterdaemon-Hostprozesses (fdhost.exe)  
 Der vom fdhost.exe-Prozess für eine Auffüllung benötigte Arbeitsspeicher hängt hauptsächlich von den verwendeten Volltext-Crawlbereichen, der Größe des Inbound Shared Memory (ISM) und der maximalen Anzahl von ISM-Instanzen ab.  
  
 Der vom Filterdaemonhost verwendete Arbeitsspeicher (in Bytes) kann mit der folgenden Formel ungefähr geschätzt werden:  
  
`number_of_crawl_ranges * ism_size * max_outstanding_isms * 2`  
  
 Die Standardwerte der Variablen in der vorangehenden Formel lauten wie folgt:  
  
|**Variable**|**Standardwert**|  
|------------------|-----------------------|  
|*number_of_crawl_ranges*|Die Anzahl der CPUs|  
|*ism_size*|1 MB für x86-Computer<br /><br /> 4 MB, 8 MB oder 16 MB für x64-Computer, abhängig vom gesamten physischen Arbeitsspeicher|  
|*max_outstanding_isms*|25 für x86-Computer<br /><br /> 5 für x64-Computer|  
  
 Die folgende Tabelle enthält Richtlinien zum Schätzen der Arbeitsspeicheranforderungen von fdhost.exe. Die Formeln in dieser Tabelle verwenden die folgenden Werte:  
  
-   *F*, eine Schätzung des Arbeitsspeichers, den „fdhost.exe“ (in MB) benötigt.  
  
-   *T*, der gesamte physische Speicher, der für das System (in MB) verfügbar ist.  
  
-   *M*, die optimale **max server memory** -Einstellung.  
  
Wichtige Informationen zu den folgenden Formeln finden Sie in den Notizen unter der Tabelle.  
  
|Plattform|Schätzung des von „fdhost.exe“ benötigten Arbeitsspeichers in MB: *F*^1|Formel zum Berechnen des „max server memory“-Werts: *M*^2|  
|--------------|-----------------------------------------------------------|-----------------------------------------------------|  
|x86|*F* = *Anzahl der Durchforstungsbereiche* \* 50|*M* = Minimum (*T*, 2000) - F - 500|  
|x64|*F* = *Anzahl der Durchforstungsbereiche* \* 10 \* 8|*M* = *T* - *F* - 500|  

**Hinweise zu den Formeln**
1.  Wenn mehrere vollständige Auffüllungen ausgeführt werden, berechnen Sie die Arbeitsspeicheranforderungen für „fdhost.exe“ separat, also *F1*, *F2*, usw. Berechnen Sie anschließend *M* als _T_ **-** sigma **(** _F_i **)** .  
2.  500 MB ist eine Schätzung des erforderlichen Speichers, der von den anderen Prozessen im System benötigt wird. Wenn das System noch weitere Aufgaben durchführt, sollten Sie diesen Wert entsprechend erhöhen.  
3.  .*ism_size* wird 8 MB für x64-Plattformen angenommen.  
  
 #### <a name="example-estimate-the-memory-requirements-of-fdhostexe"></a>Beispiel: Schätzen der Arbeitsspeicheranforderungen von fdhost.exe  
  
 Dieses Beispiel gilt für einen 64-Bit-Computer mit 8 GB RAM und 4 Dual-Core-Prozessoren. Die erste Berechnung schätzt den Speicher, der von „fdhost.exe“ benötigt wird: *F*. Die Anzahl der Durchforstungsbereiche beträgt `8`.  
  
 `F = 8*10*8 = 640`  
  
 Die nächste Berechnung ergibt den optimalen Wert für **max server memory** -*M*. *T*, der gesamte physische Speicher, der auf diesem System verfügbar ist, beträgt `8192` (in MB).  
  
 `M = 8192-640-500 = 7052`  
  
 #### <a name="example-setting-max-server-memory"></a>Beispiel: Festlegen des maximalen Serverarbeitsspeichers  
  
 In diesem Beispiel werden die Anweisungen [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) und [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) von [!INCLUDE[tsql](../../includes/tsql-md.md)] verwendet, um **maximalen Serverarbeitsspeicher** auf den Wert festzulegen, der im vorherigen Beispiel für *M* berechnet wurde, also `7052`:  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'max server memory', 7052;  
GO  
RECONFIGURE;  
GO  
```  
  
Weitere Informationen zur Serverarbeitsspeicher-Option finden Sie unter [Serverkonfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
### <a name="check-cpu-usage"></a>Überprüfen der CPU-Verwendung  
Es ist wahrscheinlich, dass die Leistung der vollständigen Auffüllungen nicht optimal ist, wenn der mittlere CPU-Verbrauch weniger als 30 Prozent beträgt. In diesem Abschnitt werden einige Faktoren behandelt, die sich auf den CPU-Verbrauch auswirken.  
  
-   Lange Wartezeiten für Seiten  
  
     Um herauszufinden, ob die Wartezeit für Seiten hoch ist, führen Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung aus:  
  
    ```sql  
    SELECT TOP 10 * FROM sys.dm_os_wait_stats ORDER BY wait_time_ms DESC;  
    ```  
  
     In der folgenden Tabelle sind die relevanten Wartetypen aufgeführt.  
  
    |Wartetyp|BESCHREIBUNG|Mögliche Lösung|  
    |---------------|-----------------|-------------------------|  
    |PAGEIO_LATCH_SH (_EX oder _UP)|Dies kann auf einen E/A-Engpass hinweisen. In diesem Fall ist normalerweise auch eine hohe durchschnittliche Warteschlangenlänge des Datenträgers zu erkennen.|Sie können den E/A-Engpass ggf. reduzieren, indem Sie den Volltextindex in eine andere Dateigruppe auf einem anderen Datenträger verschieben.|  
    |PAGELATCH_EX (oder _UP)|Dies kann auf eine hohe Zahl von Konflikten zwischen Threads hinweisen, die versuchen, in dieselbe Datenbankdatei zu schreiben.|Diese Konflikte können ggf. verringert werden, indem Sie Dateien der Dateigruppe hinzufügen, auf der sich der Volltextindex befindet.|  
  
     Weitere Informationen finden Sie unter [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
-   Ineffizienzen beim Durchsuchen der Basistabelle  
  
     Eine vollständige Auffüllung durchsucht die Basistabelle, um Batches zu erzeugen. Dieses Durchsuchen der Tabelle kann in den folgenden Szenarios ineffizient sein:  
  
    -   Wenn die Basistabelle über einen hohen Prozentsatz an Außerhalb-Spalten (out-of-row) verfügt, für die eine Volltextindizierung durchgeführt wird, kann das Durchsuchen der Basistabelle zum Erzeugen von Batches den Engpass bewirken. In diesem Fall kann es helfen, die kleineren Daten mithilfe von **varchar(max)** oder **nvarchar(max)** in die Zeilen zu verschieben.  
  
    -   Wenn die Basistabelle stark fragmentiert ist, kann das Durchsuchen ggf. ineffizient sein. Informationen zur Berechnung von Außerhalb-Spalten-Daten und zur Indexfragmentierung finden Sie unter [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) und [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
         Um die Fragmentierung zu reduzieren, können Sie den gruppierten Index neu organisieren oder neu erstellen. Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
##  <a name="troubleshoot-slow-indexing-of-documents"></a><a name="filters"></a> Problembehandlung bei langsamer Indizierung von Dokumenten

> [!NOTE]
> Dieser Abschnitt beschreibt ein Problem, das nur Kunden betrifft, die Dokumente indizieren (z.B. Microsoft Word-Dokumente), in die andere Dokumenttypen eingebettet sind.

Beim Auffüllen eines Volltextindexes werden von der Volltext-Engine zwei Arten von Filtern verwendet, Multithread-Filter und Filter mit einem einzigen Thread.
-   Einige Dokumente, wie z. B. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word-Dokumente, werden mit einem Multithreadfilter gefiltert.
-   Andere Dokumente, wie z. B. Adobe Acrobat PDF-Dokumente (Portable Document Format), werden hingegen mit einem Filter mit einem einzigen Thread gefiltert.  
  
 Aus Sicherheitsgründen werden Filter mit Filterdaemon-Hostprozessen geladen. Eine Serverinstanz verwendet einen Multithreadprozess für alle Multithreadfilter und einen Singlethreadprozess für alle Filter mit einem einzigen Thread. Wenn in einem Dokument, für das ein Multithreadfilter verwendet wird, ein Dokument eingebettet ist, für das ein Filter mit einem einzigen Thread verwendet wird, startet die Volltext-Engine einen Singlethreadprozess für das eingebettete Dokument. Beispiel: Bei einem Word-Dokument, das ein PDF-Dokument enthält, verwendet die Volltext-Engine einen Multithreadprozess für den Inhalt des Word-Dokuments und einen Singlethreadprozess für den Inhalt des PDF-Dokuments. Ein Filter mit einem einzigen Thread funktioniert in dieser Umgebung jedoch möglicherweise nicht ordnungsgemäß und kann die Stabilität des Filterprozesses gefährden. Unter bestimmten Umständen mit vielen eingebetteten Dokumenten, kann Destabilisierung zum Absturz des Prozesses führen. In diesem Fall verbindet die Volltext-Engine alle Dokumente, bei denen Fehler auftraten (z.B. ein Word-Dokument mit eingebettetem PDF-Inhalt), erneut mit dem Singlethread-Filterprozess. Kommt dies häufig vor, hat das eine Leistungsminderung des Volltextindizierungsprozesses zur Folge.  
  
Sie müssen den Filter für das Containerdokument (hier das Word-Dokument) als Filter mit einem einzigen Thread kennzeichnen, um dieses Problem zu umgehen. Hierzu müssen Sie den Registrierungswert **ThreadingModel** für den Filter auf **Apartment Threaded** festlegen. Informationen zu Singlethreadapartments finden Sie im Whitepaper [Understanding and Using COM Threading Models](/previous-versions/ms809971(v=msdn.10))(Grundlegendes zur Verwendung von COM-Threadingmodellen).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Serverkonfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [Max. Bereich für Volltextdurchforstung (Serverkonfigurationsoption)](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md)   
 [Auffüllen von Volltextindizes](../../relational-databases/search/populate-full-text-indexes.md)   
 [Erstellen und Verwalten von Volltextindizes](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)   
 [Behandeln von Problemen mit der Volltextindizierung](../../relational-databases/search/troubleshoot-full-text-indexing.md)  
 [Architektur der Volltextsuche](../../relational-databases/search/full-text-search.md#architecture) 
