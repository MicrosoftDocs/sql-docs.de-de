---
description: MSSQLSERVER_17890
title: MSSQLSERVER_17890
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17890 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 121ec18d62a79a10df015d26c0e2c2885c8cd0b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340758"
---
# <a name="mssqlserver_17890"></a>MSSQLSERVER_17890
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|17890|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|SRV_WS_TRIMMED|
|Meldungstext|Ein erheblicher Teil des SQL Server-Prozessarbeitsspeichers wurde ausgelagert. Dies kann die Leistung beeinträchtigen. Dauer: %d Sekunden. Workingset (KB): %I64d; Commit ausgeführt (KB): %I64d; Arbeitsspeichernutzung: %d%%.|
||

## <a name="explanation"></a>Erklärung

Möglicherweise wird im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll oder im Windows-Anwendungsereignisprotokoll die folgende Fehlermeldung angezeigt.

> Ein erheblicher Teil des SQL Server-Prozessarbeitsspeichers wurde ausgelagert. Dies kann die Leistung beeinträchtigen. Duration (Dauer): 0 Sekunden. Arbeitssatz (KB): 3383250, zugesichert (KB): 9112480, Arbeitsspeicherauslastung: 37 %.

Möglicherweise bemerken Sie zudem eine plötzliche Leistungsbeeinträchtigung bei der Abfrageausführung und allen anderen Vorgängen auf der SQL Server-Instanz.

## <a name="cause"></a>Ursache

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überwacht die verschiedenen arbeitsspeicherbezogenen Informationen zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess. In diesem Fall wurde festgestellt, dass der Arbeitssatz des Prozesses weniger als 50 % des zugesicherten Prozessarbeitsspeichers beträgt. Aus diesem Grund wurde diese Warnung ausgegeben. Nachfolgend sind die üblichen Ursachen dieser Warnung aufgeführt:

- Das Betriebssystem lagert große Teile des zugesicherten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsspeichers in die Auslagerungsdatei aus.
- Dies könnte daran liegen, dass der Bedarf an Arbeitsspeicher durch andere Anwendungen oder Betriebssystemanforderungen plötzlich zunimmt.
- Eine weitere mögliche Ursache ist, dass bestimmte Gerätetreiber zusammenhängende Arbeitsspeicherzuweisungen benötigen und anfordern.

## <a name="user-action"></a>Benutzeraktion

Um zu verhindern, dass das Windows-Betriebssystem den Pufferpoolarbeitsspeicher des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses auslagert, können Sie den Arbeitsspeicher sperren, der im physischen Arbeitsspeicher für den Pufferpool zugeordnet ist. Zum Sperren des Arbeitsspeichers weisen Sie die Benutzerberechtigung „Sperren von Seiten im Speicher“ dem Benutzerkonto zu, das als Startkonto des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Diensts verwendet wird. Bevor Sie diese Lösung implementieren, sollten Sie die [Gründe für das Auslagern von SQL Server-Arbeitsspeicher](#what-causes-sql-server-memory-to-be-paged-out) sowie die wichtigen Überlegungen vor dem Zuweisen der Benutzerberechtigung „Sperren von Seiten im Speicher“ für eine SQL Server-Instanz durchlesen.

> [!NOTE]
> Bei Verwendung von „Sperren von Seiten im Speicher“ wird sichergestellt, dass der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwaltete Arbeitsspeicher nicht ausgelagert wird. Threadstapel, EXE, DLL-Images, Heapspeicher und CLR-Speicher können jedoch weiterhin vom Betriebssystem ausgelagert werden.
>
> Ab [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 SP1 (kumulatives Update 2) kann die Benutzerberechtigung „Sperren von Seiten im Speicher“ sowohl in der Standard- als auch in der Enterprise-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Weitere Informationen zur Unterstützung von gesperrten Seiten finden Sie hier: [KB970070 – Unterstützung für gesperrte Seiten bei SQL Server Standard Edition (64 Bit)](https://support.microsoft.com/help/970070).

Führen Sie folgende Schritte aus, um die Benutzerberechtigung „Sperren von Seiten im Speicher“ zuzuweisen:

1. Klicken Sie auf **Start** und dann auf **Ausführen**, geben Sie *gpedit.msc* ein, und klicken Sie dann auf **OK**.
1. Das Dialogfeld „Gruppenrichtlinie“ wird geöffnet.
1. Erweitern Sie nacheinander **Computerkonfiguration** und dann **Windows-Einstellungen**.
1. Erweitern Sie **Sicherheitseinstellungen** und dann **Lokale Richtlinien**.
1. Klicken Sie auf „Zuweisen von Benutzerrechten“, und doppelklicken Sie dann auf **Sperren von Seiten im Speicher**.
1. Klicken Sie im Dialogfeld **Lokale Sicherheitsrichtlinie einstellen** auf **Benutzer hinzufügen** oder **Gruppe hinzufügen**.
1. Fügen Sie im Dialogfeld **Benutzer auswählen** oder **Gruppen auswählen** das Konto hinzu, das zum Ausführen der Datei „Sqlservr.exe“ berechtigt ist, und klicken Sie dann auf **OK**.
1. Schließen Sie das Dialogfeld **Gruppenrichtlinie**.
1. Starten Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst neu.

Nachdem Sie die Benutzerberechtigung „Sperren von Seiten im Speicher“ zugewiesen und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst neu gestartet haben, lagert das Windows-Betriebssystem den Pufferpoolarbeitsspeicher innerhalb des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses nicht mehr aus. Das Windows-Betriebssystem kann anderen Arbeitsspeicher innerhalb des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses jedoch weiterhin auslagern.

Sie können überprüfen, ob die Benutzerberechtigung von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verwendet wird, indem Sie sicherstellen, dass beim Starten die folgende Meldung in das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll geschrieben wird: „Für den Pufferpool werden gesperrte Seiten verwendet“

Diese Meldung gilt nur für SQL Server. Weitere Informationen zu dieser Meldung im Fehlerprotokoll finden Sie hier: [Muss ich im lokalen System die Berechtigung „Sperren von Seiten im Speicher“ zuweisen](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426)

Auch wenn das Windows-Betriebssystem Speicher auslagert, der nicht im Zusammenhang mit dem Pufferpool steht, können Leistungsprobleme auftreten. Die im Abschnitt „Erklärung“ erwähnten Fehlermeldungen werden jedoch nicht im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll protokolliert.

## <a name="what-causes-sql-server-memory-to-be-paged-out"></a>Gründe für das Auslagern von SQL Server-Arbeitsspeicher

Die Gründe für dieses Problem lassen sich in drei allgemeine Kategorien untergliedern:

- Anwendungsbezogene Probleme: Der verfügbare physische Speicher ist durch alle Anwendungen gemeinsam belegt, und das Betriebssystem muss Speicher für neue Anwendungsanforderungen für Ressourcen freigeben. In dieser Situation sollten üblicherweise die Anwendungen ermittelt werden, die den Arbeitsspeicher auslasten. Anschließend führen Sie die erforderlichen Schritte aus, um die Speicherkapazität so auf die Anwendungen zu verteilen, dass es nicht zu einer RAM-Auslastung kommt.
- Gerätetreiberprobleme: Gerätetreiber können zu einer Arbeitssatzauslagerung aller Prozesse führen, wenn der Treiber eine Speicherbelegungsfunktion nicht korrekt aufruft.
- Betriebssystemprobleme

Nachfolgend finden Sie Informationen zu diesen Kategorien

- **Anwendungsbezogene Probleme**: Anwendungen können den gesamten Arbeitsspeicher im System belegen. Wenn neuer Speicher angefordert wird, versucht das Betriebssystem, diese Anforderung zu erfüllen. Sollte kein freier Speicher verfügbar sein, wird der Arbeitssatz ausgeführter Anwendungen verkleinert, um die Speicheranforderungen zu erfüllen. In diesen Fällen macht sich möglicherweise für die meisten, wenn nicht sogar für alle Anwendungen eine signifikante Verkleinerung des Arbeitssatzes bemerkbar. Um dieses Verhalten zu kontrollieren, können Sie die folgenden Zähler des Leistungsmonitors für alle Anwendungen innerhalb des Systems überwachen:

  - Leistungsobjekt: Prozess
  - Leistungsindikator: Arbeitssatz
  
  Überwachen Sie außerdem den folgenden Zähler, um die verfügbare Größe des physischen Speichers innerhalb des Systems zu ermitteln.
  
  - Leistungsobjekt: Arbeitsspeicher
  - Leistungsindikator: Verfügbarer Arbeitsspeicher (MB)
  
  Das typische Verhalten ist ein Rückgang des verfügbaren Speichers auf fast 0 MB, während gleichzeitig die Arbeitssatzzähler für die meisten oder alle Prozesse des Systems plötzlich abfallen. Wenn Sie dieses Verhalten beobachten, müssen Sie möglicherweise die Speicherauslastung im System senken. Senken Sie dazu z. B. die maximale Serverarbeitsspeichergröße für SQL Server.
  
    In einigen Fällen wird auch zu viel Systemcache durch Anwendungen verwendet, sodass der Systemcache sehr groß wird. Als Reaktion auf dieses Wachstum lagert das System den Arbeitssatz des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses oder anderer Anwendungen aus. Wenn dieses Problem auftritt, können Sie einige der Speicherverwaltungsfunktionen in der Anwendung nutzen. Mit diesen Funktionen wird die Systemcachegröße gesteuert, die Datei-E/A-Vorgänge in der Anwendung verwenden können. Mit den Funktionen „SetSystemFileCacheSize“ und „GetSystemFileCacheSize“ legen Sie z. B. die Systemcachegröße fest, die von Datei-E/A-Vorgängen verwendet werden kann.
  
    Mithilfe des Leistungsobjekts „Speicher“ können Sie die Werte verschiedener Zähler in diesem Objekt anzeigen, um zu ermitteln, ob der Systemcache-Arbeitssatz zu viel Speicher belegt. Zeigen Sie z. B. die Zähler „Cachebytes“ und „Systemcache: Residente Bytes“ an. Weitere Informationen zu diesem Thema finden Sie hier:
  
    - [Zu viel Cache](/archive/blogs/ntdebugging/too-much-cache)
    - [Microsoft Windows Dynamic Cache Service](/archive/blogs/ntdebugging/microsoft-windows-dynamic-cache-service)
    - [Leistungsprobleme bei Anwendungen und Diensten, wenn der Systemdateicache den Großteil des physischen Speichers belegt](https://support.microsoft.com/help/976618)
  
    Sie können den Microsoft Windows Dynamic Cache Service herunterladen und bereitstellen, um die Größe des Speichers festzulegen, die der Systemcache nutzt.

- **Gerätetreiberprobleme**: Wenn ein Gerätetreiber die Funktion `MmAllocateContiguousMemory` verwendet und den Wert des Parameters „HighestAcceptableAddress“ auf weniger als 4 GB festlegt, lagert das Windows-Betriebssystem möglicherweise den Arbeitssatz der Prozesse innerhalb des Systems (einschließlich des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses) aus. Zum Behandeln dieses Problems kontaktieren Sie den Anbieter des Gerätetreibers, um Treiberupdates zu erhalten.

    Wenn ein Gerätetreiber versucht, Speicher zu belegen, lagert das Windows-Betriebssystem möglicherweise den Arbeitssatz anderer Anwendungen aus. Mit diesem Windows-Hotfix können Sie die Ereignisablaufverfolgung verwenden, um den Gerätetreiber zu ermitteln, der das Problem verursacht. Weitere Informationen zum Treiber, der das Verkleinern des Arbeitssatzes verursacht, finden Sie unter [Ermitteln von Treibern, die zusammenhängenden Speicher belegen](/previous-versions/windows/desktop/xperf/identifying-drivers-that-allocate-contiguous-memory).

- **Betriebssystemprobleme**: Zum Behandeln der bekannten Probleme, aufgrund derer das Windows-Betriebssystem den Arbeitssatz des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses auslagert, verwenden Sie die in den folgenden Microsoft Knowledge Base-Artikeln beschriebenen Hotfixes.

  > [!NOTE]
  > Hotfixes sind kumulativ. Eine spätere Version eines Hotfixes umfasst die früheren Versionen des jeweiligen Hotfixes.

  - Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Satz wird möglicherweise verkleinert, wenn das System erweiterte TCP-Features nutzt. Weitere Informationen finden Sie unter [Problembehandlung für erweiterte Netzwerkleistungsfeatures wie RSS und NetDMA](/troubleshoot/windows-server/networking/troubleshoot-network-performance-features-rss-netdma).

  - Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Windows Server 2008 ausführen, müssen Sie Fixes für bekannte Probleme anwenden, die zur Verkleinerung von Arbeitssätzen oder einer unnötigen übermäßigen Speicherauslastung durch andere Betriebssystemkomponenten führen können. Weitere Informationen finden Sie im Artikel [Der Berichtgenerierungsprozess reagiert möglicherweise nicht mehr, wenn Sie Perfmon.exe mit der Active Directory-Diagnosevorlage ausführen, um einen Bericht auf einem Windows Server 2008-basierten Domänencontroller zu generieren](/troubleshoot/windows-server/performance/report-generation-process-stops-responding).

  - Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter Windows Server 2008 R2 ausführen, müssen Sie Fixes für bekannte Probleme anwenden, die zur Verkleinerung von Arbeitssätzen führen können. Weitere Informationen finden Sie in den folgenden Artikeln:

    - [Computer unter Windows 7 oder Windows Server 2008 R2 reagieren beim Ausführen einer großen Anwendung nicht mehr](https://support.microsoft.com/help/979149)
    - [Schlechte Leistung auf Computern mit NUMA-basierten Prozessoren unter Windows Server 2008 R2 oder Windows 7, wenn ein Thread eine große Speichermenge innerhalb der ersten 4 GB Speicher anfordert](https://support.microsoft.com/help/2155311)
    - [Computer weisen zeitweilig eine schlechte Leistung auf oder reagieren nicht mehr, wenn der Storport-Treiber in Windows Server 2008 R2 verwendet wird](https://support.microsoft.com/help/2468345)

## <a name="important-considerations-before-you-assign-the-lock-pages-in-memory-user-right"></a>Wichtige Überlegungen vor dem Zuweisen der Benutzerberechtigung „Sperren von Seiten im Speicher“

Bevor Sie die Benutzerberechtigung „Sperren von Seiten im Speicher“ zuweisen, sollten Sie einige Aspekte berücksichtigen. Wenn diese Benutzerberechtigung in Systemen zugewiesen wird, die nicht ordnungsgemäß konfiguriert sind, kann das System instabil oder die Leistung des gesamten Systems beeinträchtigt werden. Darüber hinaus wird möglicherweise die Ereignis-ID 333 im Ereignisprotokoll protokolliert.

Wenn Sie sich bei diesen Problemen an den Microsoft-Kundensupport wenden, werden Sie möglicherweise dazu aufgefordert, diese Benutzerberechtigung für das Benutzerkonto aufzuheben, das als Startkonto des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Diensts verwendet wird. Dieser Schritt kann zum Erfassen wichtiger Leistungsdaten erforderlich sein, die die Supportmitarbeiter für die erforderliche Konfiguration der verschiedenen Optionen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und andere Anwendungen benötigen, die innerhalb des Systems ausgeführt werden. Nachdem die Supportmitarbeiter die Leistungsdaten erfasst haben, können Sie die Benutzerberechtigung „Sperren von Seiten im Speicher“ erneut dem Startkonto des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Diensts zuweisen.

Stellen Sie vor dem Zuweisen der Benutzerberechtigung „Sperren von Seiten im Speicher“ sicher, dass Sie ein Leistungsmonitorprotokoll erstellen, anhand dessen Sie die Speicheranforderungen verschiedener Anwendungen und Dienste ermitteln können, die im System installiert sind. Zu diesen Anwendungen zählt auch SQL Server. Erfassen Sie die folgenden Baselineinformationen, um die Speicheranforderungen zu ermitteln:

- Stellen Sie sicher, dass die Optionen „Max. Serverarbeitsspeicher“ und „Min. Serverarbeitsspeicher“ ordnungsgemäß konfiguriert sind. Diese Optionen spiegeln lediglich die Speicheranforderung des Pufferpools des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses wider. Sie umfassen nicht den Speicher, der für andere Komponenten innerhalb des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses zugeordnet ist. Dazu zählen die folgenden Komponenten:

  - Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsthreads
  - Verschiedene DLLs und Komponenten, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess innerhalb des Adressraums des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses lädt
  - Sicherungs- und Wiederherstellungsvorgänge

- Die DLLs und Komponenten umfassen verschiedene OLE DB-Anbieter, erweiterte gespeicherte Prozeduren, Microsoft COM-Objekte für die gespeicherte Prozedur „sp_OACreate“, Verbindungsserver und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR. Der für diese Komponenten zugeordnete Speicher gehört nicht zum Pufferpoolbereich des Adressraums des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses. Um die ideale maximale Speichergröße zu bestimmen, die der gesamte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess verwenden kann, gehen Sie wie folgt vor: Subtrahieren Sie den Speicher, der für Komponenten zugeordnet ist, die den Pufferpool nicht verwenden, von der Gesamtspeichergröße, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess verwenden soll. Den verbleibenden Wert können Sie dann verwenden, um die Option „Max. Serverarbeitsspeicher“ festzulegen. Vor dem Festlegen der Optionen „Max. Serverarbeitsspeicher“ und „Min. Serverarbeitsspeicher“ sollten Sie das Thema zum manuellen Festlegen der Speicheroptionen auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Books Online-Website sorgfältig durchlesen.

- Bestimmen Sie die Arbeitsspeicheranforderungen anderer Anwendungen und der Komponenten des Windows-Betriebssystems. Anwendungen können weitere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten umfassen, z. B. den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Replikations-Agents, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Reporting Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Integration Services und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Volltextsuche. Anwendungen, die Sicherungs- und Dateikopiervorgänge ausführen, können eine große Menge an Arbeitsspeicher belegen. Berücksichtigen Sie Vorgänge wie das Massenkopieren und den Momentaufnahmen-Agent, durch die Datei-E/A generiert wird. Wenn Sie die Werte für „Max. Serverarbeitsspeicher“ und „Min. Serverarbeitsspeicher“ festlegen, müssen die Speicheranforderungen all dieser Anwendungen berücksichtigt werden. Um die Speicheranforderung für einen bestimmten Prozess zu bestimmen, können Sie für jeden Prozess die Zähler „Private Bytes“ und „Arbeitssatz“ unter dem Objekt „Prozess“ verwenden.

- Standardmäßig wurde die Benutzerberechtigung „Sperren von Seiten im Speicher“ dem integrierten Konto „Lokales System“ bereits zugewiesen. Weitere Informationen finden Sie auf der folgenden Microsoft-Website: [Muss ich für das lokale System die Berechtigung „Sperren von Seiten im Speicher“ zuweisen?](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426?advanced=false&collapse_discussion=true&search_type=thread)

- Wenn Sie ein Windows-Benutzerkonto global für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesse in einer Domäne verwenden, bestimmen Sie die Benutzerberechtigungen, die zugewiesen werden, über die Konfiguration einer Gruppenrichtlinie. 32-Bit-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesse verwenden dieses Konto möglicherweise als Startkonto. Für dieses Konto ist die Benutzerberechtigung „Sperren von Seiten im Speicher“ jedoch erforderlich, um das `Address Windowing Extensions`-Feature (AWE) zu aktivieren. Weitere Informationen finden Sie im Thema zum Bereitstellen der maximalen Menge von Arbeitsspeicher für SQL Server auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Books Online-Website.

- Berücksichtigen Sie vor dem Konfigurieren der Optionen „Max. Serverarbeitsspeicher“ und „Min. Serverarbeitsspeicher“ für mehrere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen die Speicheranforderungen des Nicht-Pufferpools der einzelnen Instanzen von SQL Server. Konfigurieren Sie diese Optionen dann für jede Instanz von SQL Server.

Idealerweise erfassen Sie diese Baselineinformationen zu Spitzenlastzeiten. Auf diese Weise können Sie die Speicheranforderungen für verschiedene Anwendungen und Komponenten zur Unterstützung der Spitzenlast bestimmen. Die Arbeitsspeicheranforderungen variieren abhängig von den Aktivitäten und Anwendungen, die auf einem System ausgeführt werden. Sie können die Informationen abrufen, die in der dynamischen Verwaltungssicht „sys.dm_os_process_memory“ bereitgestellt werden, um zu ermitteln, ob es innerhalb des Systems zu einer hohen Speicherauslastung kommt. Weitere Informationen finden Sie unter [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md).

## <a name="improvements-added-in-windows-server-2008-and-r2-version"></a>In Windows Server 2008 und R2 hinzugefügte Verbesserungen

Mit Windows Server 2008 und Windows Server 2008 R2 wurde der Mechanismus für die Zuordnung von zusammenhängendem Speicher verbessert. Durch diese Verbesserung lassen sich in Windows Server 2008 und Windows Server 2008 R2 die Auswirkungen reduzieren, die mit dem Auslagern der Arbeitssätze von Anwendungen einhergehen, wenn neue Speicheranforderungen eingehen.

Nachfolgend werden die Verbesserungen aus dem Microsoft-Whitepaper zu den Fortschritten bei der Speicherverwaltung in Windows beschrieben:

*In Windows Server 2008 wurde die Zuordnung von physisch zusammenhängendem Speicher erheblich verbessert. Anforderungen zum Zuordnen von zusammenhängendem Speicher werden nun mit deutlich höherer Wahrscheinlichkeit erfolgreich ausgeführt. Der Grund dafür ist, dass der Speicher-Manager Seiten jetzt dynamisch ersetzt und dabei üblicherweise den Arbeitssatz nicht verkleinert und keine E/A-Vorgänge ausführt. Darüber hinaus können nun deutlich mehr Seitentypen (darunter u. a. Kernelstapel und Dateisystem-Metadatenseiten) ersetzt werden. Folglich ist üblicherweise eine größere Menge an zusammenhängendem Speicher verfügbar. Außerdem wurden die Kosten für diese Zuordnungen deutlich reduziert.*

Weitere Informationen finden Sie unter [Probleme beim Verkleinern von SQL Server-Arbeitssätzen](https://techcommunity.microsoft.com/t5/sql-server-support/sql-server-working-set-trim-problems-consider/ba-p/315462?advanced=false&collapse_discussion=true&search_type=thread).

Die in diesem Artikel beschriebenen Drittanbieterprodukte werden von Unternehmen hergestellt, die von Microsoft unabhängig sind. Microsoft übernimmt keine Garantie, weder ausdrücklich noch konkludent, für die Leistungsfähigkeit oder Zuverlässigkeit dieser Produkte.