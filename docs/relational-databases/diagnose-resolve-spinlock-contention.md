---
title: 'Whitepaper: Diagnostizieren und Lösen von Spinlockkonflikten'
description: In diesem Artikel wird die Diagnose und Lösung von Spinlockkonflikten in SQL Server ausführlich behandelt. Dieser Artikel wurde ursprünglich vom SQLCAT (Microsoft SQL Server Customer Advisory Team, SQL Server-Kundenberatungsteam) von Microsoft veröffentlicht.
ms.date: 09/30/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: how-to
author: bluefooted
ms.author: pamela
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d7d68e5eec8ddef36970d073b96e54db80b2947
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074981"
---
# <a name="diagnose-and-resolve-spinlock-contention-on-sql-server"></a>Diagnostizieren und Lösen von Spinlockkonflikten in SQL Server

Dieser Artikel enthält ausführliche Informationen zum Identifizieren und Lösen von Problemen im Zusammenhang mit Spinlockkonflikten in SQL Server-Anwendungen auf Systemen mit hoher Parallelität.

> [!NOTE]
> Die hier dokumentierten Empfehlungen und bewährten Methoden basieren auf praktischen Erfahrungen bei der Entwicklung und Bereitstellung von realen OLTP-Systemen. Ursprünglich wurde dieser Artikel vom SQLCAT (Microsoft SQL Server Customer Advisory Team, Microsoft SQL Server-Kundenberatungsteam) veröffentlicht.

## <a name="background"></a>Hintergrund

Früher verwendeten Windows Server-Standardcomputer nur einen oder zwei Mikroprozessorchips/CPU-Chips, und CPUs verfügten über nur einen Prozessor oder „Kern“. Eine höhere Verarbeitungskapazität von Computern wurde durch die Verwendung von schnelleren CPUs erzielt, die größtenteils durch Verbesserungen in Bezug auf die Transistordichte möglich wurden. Seit der Entwicklung der ersten universellen Ein-Chip-CPU im Jahr 1971 hat sich die Transistordichte, sprich die Anzahl von Transistoren, die in einem integrierten Schaltkreis enthalten sein können, dem Mooreschen Gesetz entsprechend kontinuierlich alle zwei Jahre verdoppelt. In den letzten Jahren wurde der traditionelle Ansatz des Erhöhens der Verarbeitungskapazität von Computern durch schnellere CPUs um den Bau von Computern mit mehreren CPUs erweitert. Zum Zeitpunkt der Erstellung dieses Artikels umfasst die Nehalem-CPU-Architektur von Intel bis zu acht Kerne pro CPU, die bei Verwendung in einem 8-Sockelsystem dann durch die Nutzung der Hyperthreading-Technologie auf 128 logische Prozessoren verdoppelt werden können. Mit dem Steigen der Anzahl von logischen Prozessoren auf x86-kompatiblen Computern nehmen Probleme im Zusammenhang mit Parallelität zu, da die logischen Prozessoren um die Ressourcen konkurrieren. In diesem Leitfaden wird beschrieben, wie Sie bestimmte Probleme im Zusammenhang mit Ressourcenkonflikten identifizieren und lösen, die auftreten, wenn SQL Server-Anwendungen mit einigen Arbeitsauslastungen auf Systemen mit hoher Parallelität ausgeführt werden.

In diesem Abschnitt werden die vom SQLCAT bei der Diagnose und Lösung von Problemen im Zusammenhang mit Spinlockkonflikten gewonnenen Erkenntnisse analysiert. Spinlockkonflikte sind eine Art von Parallelitätsproblem, das bei realen Arbeitsauslastungen von Kunden in umfangreichen Systemen auftritt.

## <a name="symptoms-and-causes-of-spinlock-contention"></a>Symptome und Gründe für Spinlockkonflikte

In diesem Abschnitt wird beschrieben, wie Sie Probleme mit *Spinlockkonflikten* diagnostizieren, die sich negativ auf die Leistung von OLTP-Anwendungen in SQL Server auswirken. Die Diagnose und Problembehandlung im Zusammenhang mit Spinlocks sollte als Thema für Fortgeschrittene angesehen werden, da Wissen über Debugtools und Windows-Komponenten erforderlich ist.

Spinlocks sind einfache primitive Synchronisierungstypen, die zum Schutz des Zugriffs auf Datenstrukturen verwendet werden. Sie sind nichts SQL Server-Spezifisches. Das Betriebssystem verwendet sie, wenn der Zugriff auf eine bestimmte Datenstruktur nur für kurze Zeit benötigt wird. Wenn ein Thread, der versucht, einen Spinlock zu erlangen, keinen Zugriff erhält, wird er nicht sofort angehalten, sondern in einer Schleife ausgeführt, wobei regelmäßig überprüft wird, ob die Ressource verfügbar ist. Wartet ein Thread bereits seit einiger Zeit auf einen Spinlock, wird er angehalten, bevor er die Ressource abrufen kann. Durch dieses Anhalten können andere Threads auf derselben CPU ausgeführt werden. Dieses als „Backoff“ bezeichnete Verhalten wird weiter unten in diesem Artikel näher erläutert.

SQL Server verwendet Spinlocks zum Schutz des Zugriffs auf einige interne Datenstrukturen. Spinlocks werden in der Engine verwendet, um den Zugriff auf bestimmte Datenstrukturen ähnlich wie bei Latches zu serialisieren. Der Hauptunterschied zwischen einem Latch und einem Spinlock besteht darin, dass sich Threads bei Spinlocks eine Zeit lang „drehen“ (eine Schleife ausführen) und dabei die Verfügbarkeit der Datenstruktur immer wieder überprüft wird, während ein Thread, der versucht, Zugriff auf eine durch einen Latch geschützte Struktur zu erlangen, sofort angehalten wird, wenn die gewünschte Ressource nicht verfügbar ist. Das Anhalten erfordert einen Kontextwechsel des Threads nach außerhalb der CPU, damit ein anderer Thread ausgeführt werden kann. Dies ist ein relativ kostspieliger Vorgang, und bei Ressourcen, die nur kurz gesperrt werden, ist es im Allgemeinen effizienter, zuzulassen, dass ein Thread in einer Schleife ausgeführt wird, in der die Verfügbarkeit der Ressource regelmäßig überprüft wird.

## <a name="symptoms"></a>Symptome

Bei einem stark ausgelasteten System mit hoher Parallelität sind aktive Konflikte bei durch Spinlocks geschützten Strukturen, auf die häufig zugegriffen wird, normal. Diese Nutzung wird erst dann als problematisch angesehen, wenn Konflikte einen erheblichen CPU-Mehraufwand verursachen. Spinlockstatistiken können in SQL Server über die dynamische Verwaltungssicht (Dynamic Management View, DMV) *sys.dm_os_spinlock_stats* abgerufen werden. Diese Abfrage gibt z. B. die folgende Ausgabe zurück:

> [!NOTE]
> Weitere Informationen zur Interpretation der von dieser DMV zurückgegebenen Informationen finden Sie weiter unten in diesem Artikel.

```sql
select * from sys.dm_os_spinlock_stats
order by spins desc
```

![Ausgabe von sys.dm_os_spinlock_stats](./media/diagnose-resolve-spinlock-contention/image4.png)

Die von dieser Abfrage zur Verfügung gestellten Statistiken werden wie folgt beschrieben:

| Column | Beschreibung |
|---|---|
| **Collisions** (Kollisionen) | Dieser Wert erhöht sich immer dann, wenn der Zugriff eines Threads auf eine durch einen Spinlock geschützte Ressource blockiert wird. |
| **Spins** (Drehungen) | Dieser Wert erhöht sich immer dann, wenn ein Thread eine Schleife ausführt, während er darauf wartet, dass der Spinlock verfügbar wird. Er stellt ein Maß für den Arbeitsaufwand eines Threads dar, während dieser versucht, eine Ressource abzurufen. |
| **Spins_per_collision** (Drehungen_pro_Kollision) | Das Verhältnis von Drehungen zu Kollisionen |
| **Sleep time** (Ruhezeit) | Der Wert in dieser Spalte hängt mit Backoff-Ereignissen zusammen. Für die in diesem Artikel beschriebenen Methoden ist er jedoch nicht relevant. |
| **Backoffs** | Ein Backoff tritt auf, wenn ein sich „drehender“ Thread, der versucht, auf eine gesperrte Ressource zuzugreifen, festgestellt hat, dass er das Ausführen anderer Threads auf derselben CPU ermöglichen muss. |

Für die Erläuterungen in diesem Artikel ist die Anzahl von Kollisionen, Drehungen und Backoff-Ereignissen innerhalb eines bestimmten Zeitraums bei starker Auslastung des Systems besonders interessant. Wenn ein Thread versucht, auf eine durch einen Spinlock geschützte Ressource zuzugreifen, tritt eine Kollision auf. Wenn eine Kollision auftritt, erhöht sich die Anzahl von Kollisionen, und der Thread beginnt, sich in einer Schleife zu drehen und regelmäßig zu überprüfen, ob die Ressource verfügbar ist. Bei jeder Drehung des Threads (Schleife) erhöht sich die Anzahl von Drehungen.

Die Drehungen pro Kollision stellen ein Maß für die Anzahl von Drehungen dar, die ausgeführt werden, während ein Thread über einen Spinlock verfügt. Eine geringe Anzahl von Drehungen pro Kollision und eine hohe Anzahl von Kollisionen bedeutet z. B., dass bei einem Spinlock eine geringe Anzahl von Drehungen ausgeführt wird und viele Threads um den Spinlock konkurrieren. Eine hohe Anzahl von Drehungen bedeutet, dass der Thread sich im Spinlockcode relativ lang dreht (d. h., im Code wird eine hohe Anzahl von Einträgen in einem Hashbucket durchlaufen). Wenn die Anzahl von Konflikten steigt (und somit die Anzahl von Kollisionen), steigt auch die Anzahl von Drehungen.

Backoffs können Sie sich so ähnlich wie Drehungen vorstellen. Bei Spinlocks werden standardmäßig nicht unbegrenzt viele Drehungen ausgeführt, bis auf eine gesperrte Ressource zugegriffen werden kann, um eine übermäßig hohe verschwendete CPU-Auslastung zu vermeiden. Threads führen ein Backoff durch bzw. hören auf, sich zu drehen und „ruhen“, um sicherzustellen, dass der Spinlock die CPU-Ressourcen nicht übermäßig nutzt. Backoffs werden unabhängig davon durchgeführt, ob die Zielressource jemals abgerufen wird. Dies geschieht, um die Planung anderer Threads auf der CPU zu ermöglichen, in der Hoffnung, dass so mehr produktives Arbeiten möglich ist. Das Standardverhalten für die Engine ist, dass sich der Thread erst für einen immer gleich langen Zeitraum dreht und dann ein Backoff durchgeführt wird. Der Versuch, einen Spinlock zu erlangen, erfordert, dass der Zustand der Cacheparallelität beibehalten wird. Im Vergleich zum Drehen ist dies ein relativ CPU-intensiver Vorgang. Daher werden Versuche, einen Spinlock zu erlangen, möglichst selten ausgeführt und nicht jedes Mal, wenn ein Thread sich dreht. In SQL Server wurden bestimmte Spinlocktypen (z. B. LOCK_HASH) durch die Verwendung eines Zeitintervalls verbessert, das sich zwischen den einzelnen Versuchen, den Spinlock zu erlangen, (bis zu einer bestimmten Obergrenze) immer exponentiell verlängert, wodurch die Beeinträchtigung der CPU-Leistung oft reduziert wird.

Bei dem folgenden Diagramm handelt es sich um eine konzeptionelle Darstellung des Spinlockalgorithmus:

![Konzeptionelle Darstellung des Spinlockalgorithmus](./media/diagnose-resolve-spinlock-contention/image5.png)

## <a name="typical-scenarios"></a>Typische Szenarien

Spinlockkonflikte können aus vielen verschiedenen Gründen auftreten, die möglicherweise nichts mit Datenbank-Entwurfsentscheidungen zu tun haben. Da Spinlocks den Zugriff auf interne Datenstrukturen kontrollieren, äußern sich Spinlockkonflikte nicht auf dieselbe Weise wie Pufferlatchkonflikte, die direkt von Schemaentwurfsentscheidungen und Datenzugriffsmustern betroffen sind.

Das Symptom, das hauptsächlich mit Spinlockkonflikten assoziiert wird, ist eine hohe CPU-Auslastung aufgrund der hohen Anzahl von Drehungen und der vielen Threads, die versuchen, denselben Spinlock zu erlangen. Allgemein wurde dies auf Systemen mit \>= 24 CPU-Kernen und am häufigsten auf solchen mit \>= 32 CPU-Kernen beobachtet. Wie bereits erwähnt, ist eine gewisse Anzahl von Konflikten für Spinlocks bei OLTP-Systemen mit hoher Parallelität und erheblicher Auslastung normal, und die DMV *sys.dm_os_spinlock_stats* meldet auf Systemen, die bereits über einen längeren Zeitraum ausgeführt werden, oft eine hohe Anzahl von Drehungen (Milliarden/Billionen). Wie gesagt, reicht das Beobachten einer hohen Anzahl von Drehungen für einen bestimmten Spinlocktyp nicht aus, um zu erkennen, ob dies negative Auswirkungen auf die Arbeitsauslastungsleistung hat.

Eine Kombination aus mehreren der folgenden Symptome kann auf Spinlockkonflikte hindeuten:

* Für einen bestimmten Spinlocktyp ist eine hohe Anzahl von Drehungen und Backoffs zu beobachten.

* Das System verfügt über eine hohe CPU-Auslastung oder CPU-Auslastungsspitzen. Bei Szenarios mit hoher CPU-Auslastung wird eine (von der DMV *sys.dm_os_wait_stats* gemeldete) hohe Anzahl von Signalwartevorgängen für SOS_SCHEDULER_YIELD angezeigt.

* Das System verfügt über eine hohe Parallelität.

* Die CPU-Auslastung und die Anzahl von Drehungen steigen nicht proportional zum Durchsatz.

   > [!IMPORTANT]
   > Selbst wenn alle genannten Bedingungen vorliegen, ist es möglich, dass eine andere Grundursache für die hohe CPU-Auslastung verantwortlich ist. Tatsächlich ist es so, dass eine erhöhte CPU-Auslastung in den allermeisten Fällen auf andere Gründe als Spinlockkonflikte zurückzuführen ist. Häufigere Gründe für eine erhöhte CPU-Auslastung sind z. B. die folgenden:

* Abfragen, die im Laufe der Zeit aufgrund der steigenden Menge von zugrunde liegenden Daten ressourcenintensiver werden, was dazu führt, dass zusätzliche logische Lesevorgänge für speicherresidente Daten durchgeführt werden müssen

* Änderungen in Abfrageplänen, die eine suboptimale Ausführung zur Folge haben

Wenn alle diese Bedingungen zutreffen, führen Sie weitere Untersuchungen zu möglichen Problemen im Zusammenhang mit Spinlockkonflikten durch.

Ein häufiges und leicht zu diagnostizierendes Phänomen ist eine erhebliche Abweichung zwischen Durchsatz und CPU-Auslastung. Bei vielen OLTP-Arbeitsauslastungen besteht eine Beziehung zwischen (Durchsatz/Anzahl von Benutzern im System) und der CPU-Auslastung. Eine zu beobachtende hohe Anzahl von Drehungen in Verbindung mit einer erheblichen Abweichung zwischen CPU-Auslastung und Durchsatz kann ein Hinweis auf Spinlockkonflikte sein, die zu CPU-Mehraufwand führen. Ein wichtiger Aspekt in diesem Zusammenhang ist, dass diese Art von Abweichung häufig auch auf Systemen zu beobachten ist, wenn bestimmte Abfragen im Laufe der Zeit ressourcenintensiver werden. Beispielsweise führen Abfragen, die für Datasets ausgeführt werden, die im Laufe der Zeit immer mehr logische Lesevorgänge ausführen, möglicherweise zu ähnlichen Symptomen.

Das Ausschließen anderer häufigerer Ursachen für eine hohe CPU-Auslastung ist bei der Problembehandlung für diese Art von Problemen von entscheidender Bedeutung.

## <a name="example"></a>Beispiel

Im folgenden Beispiel besteht eine nahezu lineare Beziehung zwischen der CPU-Auslastung und dem in Transaktionen pro Sekunde gemessenen Durchsatz. Es ist normal, dass hier eine gewisse Abweichung zu beobachten ist, da eine steigende Arbeitsauslastung immer zu Mehraufwand führt. Wie hier gezeigt, wird diese Abweichung erheblich. Außerdem ist ein starker Rückgang beim Durchsatz zu beobachten, sobald die CPU-Auslastung 100 % erreicht.

![Rückgang der CPU-Auslastung im Systemmonitor](./media/diagnose-resolve-spinlock-contention/image7.png)

Beim Messen der Anzahl von Drehungen innerhalb von 3-Minuten-Intervallen ist eher ein exponentieller als ein linearer Anstieg zu beobachten, was zeigt, dass Spinlockkonflikte problematisch sein können.

![Diagramm mit Drehungen innerhalb von 3-Minuten-Intervallen](./media/diagnose-resolve-spinlock-contention/image8.png)

Wie bereits erwähnt, werden Spinlocks meist auf stark ausgelasteten Systemen mit hoher Parallelität verwendet.

Szenarios, in denen dieses Problem häufig auftritt, sind z. B. die folgenden:

* Es treten Probleme bei der Namensauflösung auf, die durch einen Fehler beim vollständigen Qualifizieren von Namen von Objekten verursacht wurden. Weitere Informationen finden Sie unter [Problembehandlung bei Blockierung aufgrund von Kompilierungssperren](https://support.microsoft.com/help/263889/how-to-troubleshoot-blocking-caused-by-compile-locks). Dieses spezifische Problem wird in diesem Artikel ausführlicher beschrieben.

* Im Sperren-Manager treten Konflikte für Sperrhashbuckets für Arbeitsauslastungen auf, die häufig auf dieselbe Sperre zugreifen (z. B. eine gemeinsame Sperre für eine häufig gelesene Zeile). Diese Art von Konflikt äußert sich in einem LOCK_HASH-Spinlock. In einem bestimmten Fall haben wir festgestellt, dass dieses Problem auf falsch modellierte Zugriffsmuster in einer Testumgebung zurückzuführen war. In dieser Umgebung griffen aufgrund von falsch konfigurierten Testparametern kontinuierlich mehr Threads als erwartet auf ein und dieselbe Zeile zu.

* Es liegt eine hohe Anzahl von DTC-Transaktionen bei einer hohen Latenz zwischen den MS DTC-Transaktionskoordinatoren vor. Dieses spezifische Problem ist im SQLCAT-Blogeintrag [Auflösen von DTC-bezogenen Wartevorgängen und Optimieren der Skalierbarkeit von DTC](https://techcommunity.microsoft.com/t5/datacat/resolving-dtc-related-waits-and-tuning-scalability-of-dtc/ba-p/305054) ausführlich dokumentiert.

## <a name="diagnosing-spinlock-contention"></a>Diagnostizieren von Spinlock-Konflikten

Dieser Abschnitt enthält Informationen zur Diagnose von Spinlockkonflikten in SQL Server. Die primären Tools für die Diagnose von Spinlockkonflikten sind:

| Tool | Verwendung |
|---|---|
| **Systemmonitor** | Suchen Sie nach Bedingungen mit hoher CPU-Auslastung oder Abweichungen zwischen Durchsatz und CPU-Auslastung. |
| **DMV „sys.dm_os_spinlock_stats“** | Suchen Sie nach einer hohen Anzahl von Drehungen und Backoff-Ereignissen im Zeitverlauf. |
| **Erweiterte Ereignisse von SQL Server** | Dieses Tool wird verwendet, um Aufruflisten für Spinlocks mit einer hohen Anzahl von Drehungen nachzuverfolgen. |
| **Arbeitsspeicherabbilder** | In einigen Fällen werden Arbeitsspeicherabbilder des SQL Server-Prozesses und die Windows-Debugtools verwendet. Im Allgemeinen wird eine solche Analyse durchgeführt, wenn die Microsoft SQL Server-Supportteams eingebunden werden. |

Der allgemeine technische Prozess für die Diagnose von Spinlockkonflikten in SQL Server sieht wie folgt aus:

1. **Schritt 1:** Ermitteln Sie, ob ein Konflikt vorliegt, der möglicherweise einen Zusammenhang mit Spinlocks aufweist.

2. **Schritt 2:** Erfassen Sie Statistiken von *sys.dm\_os_spinlock_stats*, um den Spinlocktyp mit den meisten Konflikten zu ermitteln.

3. **Schritt 3:** Rufen Sie die Debugsymbole für sqlservr.exe (sqlservr.pdb) ab, und speichern Sie sie im selben Verzeichnis wie die SQL Server-Dienst-EXE-Datei (sqlservr.exe) für die SQL Server-Instanz. Sie müssen über die Symbole für die jeweils ausgeführte SQL Server-Version verfügen, um die Aufruflisten für die Backoff-Ereignisse anzeigen zu können. Symbole für SQL Server werden auf dem Microsoft-Symbolserver bereitgestellt. Weitere Informationen zum Herunterladen von Symbolen über den Microsoft-Symbolserver finden Sie unter [Debuggen mit Symbolen](/windows/win32/dxtecharts/debugging-with-symbols).

4. **Schritt 4:** Verwenden Sie erweiterte Ereignisse von SQL Server, um die Backoff-Ereignisse für die gewünschten Spinlocktypen nachzuverfolgen.

Erweiterte Ereignisse bieten die Möglichkeit, das \"Backoff\"-Ereignis nachzuverfolgen und die Aufrufliste für die Vorgänge zu erfassen, die besonders häufig versuchen, den Spinlock zu erlangen. Durch die Analyse der Aufrufliste kann ermittelt werden, welche Art von Vorgang zu Konflikten bei einem bestimmten Spinlock beiträgt.

## <a name="diagnostic-walkthrough"></a>Exemplarische Vorgehensweise für die Diagnose

Im Rahmen der folgenden exemplarischen Vorgehensweise wird gezeigt, wie Sie die Tools und Methoden verwenden, um ein Problem mit Spinlockkonflikten in einem realen Szenario zu diagnostizieren. Sie basiert auf einem Kundenauftrag, bei dem ein Benchmarktest durchgeführt wurde, um ungefähr 6.500 gleichzeitige Benutzer auf einem Server mit 8 Sockeln, 64 physischen Kernen und 1 TB Arbeitsspeicher zu simulieren.

### <a name="symptoms"></a>Symptome

Es wurden regelmäßige CPU-Auslastungsspitzen beobachtet, die zu einer CPU-Auslastung von fast 100 % führten. Darüber hinaus wurde eine Abweichung zwischen Durchsatz und CPU-Auslastung festgestellt, die zu diesem Problem führte. Zu dem Zeitpunkt, zu dem die große CPU-Spitze auftrat, trat bereits bei hoher CPU-Auslastung in bestimmten Abständen eine hohe Anzahl von Drehungen auf.

Dies war ein Extremfall, bei dem durch den Konflikt ein Spinlockkonvoi entstand. Ein Konvoi tritt auf, wenn Threads die Arbeitsauslastung nicht mehr weiter ausführen können, sondern stattdessen alle Verarbeitungsressourcen dafür verwenden, zu versuchen, auf die Sperre zuzugreifen. Das Systemmonitorprotokoll zeigt diese Abweichung zwischen dem Durchsatz im Transaktionsprotokoll und der CPU-Auslastung sowie schließlich die große CPU-Auslastungsspitze.

![CPU-Auslastungsspitze im Systemmonitor](./media/diagnose-resolve-spinlock-contention/image9.png)

Nach dem Abfragen von *sys.dm_os_spinlock_stats*, um festzustellen, ob erhebliche Konflikte für SOS_CACHESTORE vorliegen, wurde ein Skript für erweiterte Ereignisse verwendet, um die Anzahl von Backoff-Ereignissen für die gewünschten Spinlocktypen zu messen.

| name | Kollisionen | Drehungen | Drehungen pro Kollision | Backoffs |
|---|---|---|---|---|
| **SOS_CACHESTORE** |       14.752.117 |   942.869.471.526 |   63.914 |                67.900.620 |
| **SOS_SUSPEND_QUEUE** |   69.267.367 |   473.760.338.765 |   6\.840  |                2\.167.281 |
| **LOCK_HASH** |           5\.765.761 |    260.885.816.584 |   45.247 |                3\.739.208 |
| **MUTEX** |               2\.802.773 |    9\.767.503.682 |     3\.485  |                350.997 |
| **SOS_SCHEDULER** |       1\.207.007 |    3\.692.845.572 |     3\.060  |                109.746 |

Die einfachste Möglichkeit der Quantifizierung der Auswirkungen der Drehungen ist die Betrachtung der Anzahl von Backoff-Ereignissen gemäß *sys.dm_os_spinlock_stats* im selben 1-Minuten-Intervall für die Spinlocktypen mit der höchsten Anzahl von Drehungen. Diese Methode ist am besten geeignet, um erhebliche Konflikte zu erkennen, da sie anzeigt, wenn Threads die Drehungsobergrenze erreichen, während sie auf das Erlangen des Spinlocks warten. Das folgende Skript veranschaulicht eine komplexe Methode, bei der erweiterte Ereignisse verwendet werden, um verwandte Backoff-Ereignisse zu messen und die spezifischen Codepfade zu identifizieren, bei denen die Konflikte auftreten.

Weitere Informationen zu erweiterten Ereignissen von SQL Server finden Sie unter [Übersicht über erweiterte Ereignisse](./extended-events/extended-events.md).

**Skript**

```sql
/*
This Scriptis provided "AS IS" with no warranties, and confers no rights.

This script will monitor for backoff events over a given period of time and
capture the code paths (callstacks) for those.

--Find the spinlock types
select map_value, map_key, name from sys.dm_xe_map_values
where name = 'spinlock_types'
order by map_value asc

--Example: Get the type value for any given spinlock type
select map_value, map_key, name from sys.dm_xe_map_values
where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')

Examples:
61LOCK_HASH
144 SOS_CACHESTORE
08MUTEX

*/

--create the even session that will capture the callstacks to a bucketizer
--more information is available in this reference: http://msdn.microsoft.com/en-us/library/bb630354.aspx
create event session spin_lock_backoff on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
where 
type = 61--LOCK_HASH
or type = 144--SOS_CACHESTORE
or type = 8--MUTEX
)
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

--Ensure the session was created
select * from sys.dm_xe_sessions
where name = 'spin_lock_backoff'

--Run this section to measure the contention 
alter event session spin_lock_backoff on server state=start

--wait to measure the number of backoffs over a 1 minute period
waitfor delay '00:01:00'

--To view the data
--1. Ensure the sqlservr.pdb is in the same directory as the sqlservr.exe
--2. Enable this trace flag to turn on symbol resolution 
DBCC traceon (3656, -1)

--Get the callstacks from the bucketize target
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spin_lock_backoff'

--clean up the session 
alter event session spin_lock_backoff on server state=stop
drop event session spin_lock_backoff on server
```

Wenn Sie die Ausgabe analysieren, sehen Sie die Aufruflisten für die häufigsten Codepfade für die SOS_CACHESTORE-Drehungen. Während die CPU-Auslastung hoch war, wurde das Skript mehrmals ausgeführt, um die Konsistenz der zurückgegeben Aufruflisten zu überprüfen. Beachten Sie, dass die Aufruflisten mit der höchsten Slotbucketanzahl in den beiden Ausgaben gleich sind (35.668 und 8.506). Diese Aufruflisten verfügen über eine „Slotanzahl“, die zwei Größenordnungen größer als der Eintrag mit der nächstniedrigeren Anzahl ist. Dies ist ein Hinweis auf einen relevanten Codepfad.

> [!NOTE]
> Es ist nicht ungewöhnlich, dass vom obigen Skript Aufruflisten zurückgegeben werden. Wenn das Skript für eine Minute ausgeführt wurde, haben wir beobachtet, dass eine Aufrufliste mit einer Slotanzahl von > 1000 problematisch war, jedoch die Slotanzahl von > 10.000 noch problematischer war, da diese höher war.

> [!NOTE]
> Die Formatierung der folgenden Ausgabe wurde zur besseren Lesbarkeit bereinigt.

**Output 1**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="35668" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid 
      CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey
      CMEDCatalogOwner::GetProxyOwnerBySID
      CMEDProxyDatabase::GetOwnerBySID
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
  </value> 
</Slot>
<Slot count="752" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey             CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
  </value>
  </Slot>
```

**Output 2**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="8506" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep+c7 [ @ 0+0x0 SpinlockBase::Backoff Spinlock<144,1,0>::SpinToAcquireOptimistic
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
</value> 
 </Slot>
<Slot count="190" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep
       SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
   </value> 
 </Slot>
```

Im vorherigen Beispiel verfügten die interessantesten Listen über die höchste Anzahl von Slots (35.668 und 8.506), bei der es sich auch um eine Slotanzahl \> 1.000 handelt.

Nun fragen Sie sich möglicherweise, inwiefern diese Information Ihnen weiterhilft. Im Allgemeinen sind umfassende Kenntnisse in Bezug auf die SQL Server-Engine erforderlich, um die Informationen aus den Aufruflisten nutzen zu können. Daher betritt der Problembehandlungsprozess nun eine Grauzone. In diesem speziellen Fall ist anhand der Aufruflisten erkennbar, dass der Codepfad, in dem das Problem auftritt, mit Sicherheits- und Metadatensuchvorgängen verknüpft ist. Dies geht aus den folgenden Listenrahmen hervor: **„CMEDCatalogOwner::GetProxyOwnerBySID“ und „CMEDProxyDatabase::GetOwnerBySID“** .

Ausschließlich auf diesen Informationen basierend ist es schwierig, das Problem zu beheben. Sie geben jedoch Hinweise in Bezug darauf, worauf Sie sich bei der weiteren Problembehandlung konzentrieren sollten, um das Problem weiter einzugrenzen.

Da dieses Problem mit Codepfaden in Zusammenhang zu stehen schien, die sicherheitsbezogene Überprüfungen durchführten, wurde entschieden, einen Test durchzuführen, bei dem Anwendungsbenutzern, die eine Verbindung mit der Datenbank herstellten, SysAdmin-Berechtigungen erteilt wurden. In einer Produktionsumgebung ist dieses Vorgehen zwar nie zu empfehlen, in unserer Testumgebung erwies es sich jedoch als nützlicher Schritt bei der Problembehandlung. Wenn die Sitzungen mit erhöhten Rechten (SysAdmin) ausgeführt wurden, verschwanden die CPU-Spitzen im Zusammenhang mit Konflikten.

## <a name="options-and-workarounds"></a>Optionen und Problemumgehungen

Die Problembehandlung bei Spinlockkonflikten kann auf jeden Fall eine nicht triviale Aufgabe sein. Es gibt keinen allgemeinen optimalen Ansatz dafür. Der erste Schritt bei der Problembehandlung und der Behebung von Leistungsproblemen besteht darin, die Grundursache zu ermitteln. Die Verwendung der in diesem Artikel beschriebenen Methoden und Tools ist der erste Schritt der Durchführung der zum Verstehen der Konfliktpunkte im Zusammenhang mit Spinlocks erforderlichen Analyse.

Mit der Entwicklung neuer Versionen von SQL Server wird die Skalierbarkeit in der Engine durch das Implementieren von Code, der für Systeme mit hoher Parallelität besser optimiert ist, immer weiter verbessert. SQL Server hat viele Optimierungen für Systeme mit hoher Parallelität eingeführt. Eine davon ist das exponentielle Backoff für die häufigsten Konfliktpunkte. Durch bestimmte Optimierungen ab SQL Server 2012 wurde speziell dieser Bereich durch die Verwendung exponentieller Backoffalgorithmen für alle Spinlocks innerhalb der Engine verbessert.

Wenn Sie High-End-Anwendungen entwickeln, für die eine äußerst hohe Leistung und ein sehr großer Umfang erforderlich sind, sollten Sie überlegen, wie Sie den benötigten Codepfad in SQL Server so kurz wie möglich halten. Ein kürzerer Codepfad bedeutet weniger Arbeit für die Datenbank-Engine und vermeidet natürlich Konfliktpunkte. Eine Nebenwirkung bei vielen bewährten Methoden ist, dass die erforderliche Arbeit der Engine reduziert und somit die Arbeitsauslastungsleistung optimiert wird.

Einige der in diesem Artikel bereits erwähnten bewährten Methoden sind Beispiele hierfür:

* **Vollqualifizierte Namen**: Vollqualifizierte Namen für alle Objekte führen dazu, dass SQL Server keine für die Namensauflösung erforderlichen Codepfade mehr ausführen muss. Beim Spinlocktyp SOS_CACHESTORE waren Konfliktpunkte auch zu beobachten, wenn beim Aufrufen gespeicherter Prozeduren keine vollqualifizierten Namen verwendet wurden. Wenn diese Namen nicht vollqualifiziert sind, muss SQL Server das Standardschema für den Benutzer suchen, was dazu führt, dass ein längerer Codepfad zum Ausführen des SQL-Codes erforderlich ist.

* **Parametrisierte Abfragen**: Ein weiteres Beispiel ist die Verwendung von parametrisierten Abfragen und Aufrufen gespeicherter Prozeduren, um die für das Generieren von Ausführungsplänen erforderliche Arbeit zu reduzieren. Dies führt ebenfalls zu einem kürzeren Codepfad für die Ausführung.

* **LOCK_HASH-Konflikte**: In einigen Fällen sind Konflikte bei bestimmten Sperrstruktur- oder Hashbucketkollisionen unvermeidlich. Auch wenn die SQL Server-Engine den Großteil der Sperrstrukturen partitioniert, gibt es dennoch Fälle, in denen das Erlangen einer Sperre zum Zugriff auf denselben Hashbucket führt. Dies ist z. B. bei einer Anwendung der Fall, bei der mehrere Threads gleichzeitig auf dieselbe Zeile zugreifen (also bei Verweisdaten). Diese Arten von Problemen können mit Methoden behandelt werden, bei denen entweder diese Verweisdaten innerhalb des Datenbankschemas aufskaliert oder nach Möglichkeit NOLOCK-Hinweise verwendet werden.

Der erste Ansatz bei der Optimierung von SQL Server-Arbeitsauslastungen sind immer die Standardoptimierungsmethoden (z. B. Indizierung, Abfrageoptimierung und E/A-Optimierung). Neben den standardmäßig durchgeführten Optimierungsmaßnahmen stellt jedoch auch die Verwendung von Methoden zur Reduzierung der für das Ausführen von Vorgängen erforderlichen Codemenge einen wichtigen Ansatz dar. Auch wenn bewährte Methoden verwendet werden, besteht auf stark ausgelasteten Systemen mit hoher Parallelität dennoch die Möglichkeit, dass Spinlockkonflikte auftreten. Die Verwendung der in diesem Artikel beschriebenen Tools und Methoden kann dabei helfen, diese Arten von Problemen einzugrenzen oder auszuschließen und zu ermitteln, wann die entsprechenden Microsoft-Ressourcen eingebunden werden müssen.

Diese Vorgehensweisen bieten hoffentlich sowohl eine nützliche Methodik für diese Art der Problembehandlung als auch Einblicke in einige der mit SQL Server möglichen fortgeschritteneren Methoden zur Leistungsprofilerstellung.

## <a name="appendix-automate-memory-dump-capture"></a>Anhang: Automatisieren der Erfassung von Arbeitsspeicherabbildern

Das folgende Skript für erweiterte Ereignisse hat sich für die Automatisierung der Erfassung von Arbeitsspeicherabbildern, wenn Spinlockkonflikte erheblich werden, als nützlich erwiesen. In einigen Fällen sind Arbeitsspeicherabbilder für eine vollständige Diagnose des Problems erforderlich, oder sie werden von Microsoft-Supportteams für die Durchführung einer ausführlichen Analyse verlangt. In SQL Server 2008 gibt es eine Obergrenze von 16 Frames für vom Bucketizer erfasste Aufruflisten. Dies ist möglicherweise nicht detailliert genug, um festzustellen, wo genau in der Engine der Einstieg in die Aufrufliste erfolgt. In SQL Server 2012 wurden Verbesserungen eingeführt, indem die Anzahl der Frames in vom Bucketizer erfassten Aufruflisten auf 32 erhöht wurde.

Das folgende SQL-Skript kann verwendet werden, um den Erfassungsprozess für Arbeitsspeicherabbilder als Hilfe bei der Analyse von Spinlockkonflikten zu automatisieren:

```sql
/*
This script is provided "AS IS" with no warranties, and confers no rights.

Use:    This procedure will monitor for spinlocks with a high number of backoff events
        over a defined time period which would indicate that there is likely significant
        spin lock contention.
        
        Modify the variables noted below before running.


Requires:
        xp_cmdshell to be enabled
            sp_configure 'xp_cmd', 1
            go 
            reconfigure 
            go
        
*********************************************************************************************************/
use tempdb
go 
if object_id('sp_xevent_dump_on_backoffs') is not null
    drop proc sp_xevent_dump_on_backoffs
go 
create proc sp_xevent_dump_on_backoffs
(
    @sqldumper_path                       nvarchar(max)      = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
    ,@dump_threshold                      int                = 500           --capture mini dump when the slot count for the top bucket exceeds this
    ,@total_delay_time_seconds            int                = 60            --poll for 60 seconds
    ,@PID                                 int                = 0
    ,@output_path                         nvarchar(max)      = 'c:\'
    ,@dump_captured_flag                  int = 0 OUTPUT
    
)
as
/* 
    --Find the spinlock types
    select map_value, map_key, name from sys.dm_xe_map_values
    where name = 'spinlock_types'
    order by map_value asc

    --Example: Get the type value for any given spinlock type
    select map_value, map_key, name from sys.dm_xe_map_values
    where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')
*/
if exists (select * from sys.dm_xe_session_targets xst
                inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
                where xs.name = 'spinlock_backoff_with_dump')
    drop event session spinlock_backoff_with_dump on server

create event session spinlock_backoff_with_dump  on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
            where
                type = 61                 --LOCK_HASH
                --or type = 144           --SOS_CACHESTORE
                --or type = 8             --MUTEX
                --or type = 53            --LOGCACHE_ACCESS
                --or type = 41            --LOGFLUSHQ
                --or type = 25            --SQL_MGR
                --or type = 39            --XDESMGR
                )
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

alter event session spinlock_backoff_with_dump  on server state=start


declare @instance_name            nvarchar(max) = @@SERVICENAME
declare @loop_count               int = 1
declare @xml_result               xml 
declare @slot_count               bigint 
declare @xp_cmdshell              nvarchar(max) = null

--start polling for the backoffs
print 'Polling for: ' + convert(varchar(32), @total_delay_time_seconds) + ' seconds'
while (@loop_count < CAST (@total_delay_time_seconds/1 as int))
begin 
    waitfor delay '00:00:01'

    --get the xml from the bucketizer for the session
    select @xml_result= CAST(target_data as xml)
    from sys.dm_xe_session_targets xst
        inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
    where xs.name = 'spinlock_backoff_with_dump'
    
    --get the highest slot count from the bucketizer
    select @slot_count = @xml_result.value(N'(//Slot/@count)[1]', 'int')

    --if the slot count is higher than the threshold in the one minute period
    --dump the process and clean up session
    if (@slot_count > @dump_threshold)
    begin 
        print 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 c:\ '''
        select @xp_cmdshell = 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 ' + @output_path + ' '''
        exec sp_executesql @xp_cmdshell
        print 'loop count: ' + convert (varchar(128), @loop_count)
        print 'slot count: ' + convert (varchar(128), @slot_count)
        set @dump_captured_flag = 1
        break
    end 

    --otherwise loop 
    set @loop_count = @loop_count + 1

end

--see what was collected then clean up
DBCC traceon (3656, -1)
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
    inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spinlock_backoff_with_dump'

alter event session spinlock_backoff_with_dump  on server state=stop
drop event session spinlock_backoff_with_dump  on server
go

/* CAPTURE THE DUMPS 
******************************************************************/
--Example: This will run continuously until a dump is created. 
declare @sqldumper_path                nvarchar(max)        = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
declare @dump_threshold                int                  = 300            --capture mini dump when the slot count for the top bucket exceeds this
declare @total_delay_time_seconds      int                  = 60             --poll for 60 seconds 
declare @PID                           int                  = 0
declare @flag                          tinyint              = 0
declare @dump_count                    tinyint              = 0
declare @max_dumps                     tinyint              = 3              --stop after collecting this many dumps
declare @output_path                   nvarchar(max)        = 'c:\'          --no spaces in the path please :)


--Get the process id for sql server 
declare @error_log table (LogDate datetime,
    ProcessInfo varchar(255),
    Text varchar(max)
    )
insert into @error_log
    exec ('xp_readerrorlog 0, 1, ''Server Process ID''')
select @PID = convert(int, (REPLACE(REPLACE(Text, 'Server Process ID is ', ''), '.', '')))
    from @error_log where Text like ('Server Process ID is%')
print 'SQL Server PID: ' + convert (varchar(6), @PID)

--Loop to monitor the spinlocks and capture dumps. while (@dump_count < @max_dumps)
begin 

    exec sp_xevent_dump_on_backoffs @sqldumper_path             = @sqldumper_path,
                                    @dump_threshold             = @dump_threshold,
                                    @total_delay_time_seconds   = @total_delay_time_seconds,
                                    @PID                        = @PID,
                                    @output_path                = @output_path,
                                    @dump_captured_flag         = @flag OUTPUT
    if (@flag > 0) 
        set @dump_count=@dump_count + 1
    print 'Dump Count: ' + convert(varchar(2), @dump_count)
    waitfor delay '00:00:02'

end
```

## <a name="appendix-capture-spinlock-statistics-over-time"></a>Anhang: Erfassen von Spinlockstatistiken im Zeitverlauf

Das folgende Skript kann verwendet werden, um die Spinlockstatistiken für einen bestimmten Zeitraum anzuzeigen. Immer wenn es ausgeführt wird, wird die Differenz zwischen den aktuellen Werten und den zuvor erfassten Werten zurückgegeben.

```sql
/* Snapshot the current spinlock stats and store so that this can be compared over a time period
   Return the statistics between this point in time and the last collection point in time.

   **This data is maintained in tempdb so the connection must persist between each execution**
   **alternatively this could be modified to use a persisted table in tempdb. if that
   is changed code should be included to clean up the table at some point.**
*/

use tempdb
go

declare @current_snap_time    datetime
declare @previous_snap_time   datetime

set @current_snap_time = GETDATE()

if not exists(select name from tempdb.sys.sysobjects where name like '#_spin_waits%')
    create table #_spin_waits
    (
        lock_name    varchar(128)
        ,collisions  bigint
        ,spins       bigint
        ,sleep_time  bigint
        ,backoffs    bigint
        ,snap_time   datetime
    )

--capture the current stats
insert into #_spin_waits
    (
        lock_name
        ,collisions
        ,spins
        ,sleep_time
        ,backoffs
        ,snap_time
        )
        select  name
                ,collisions
                ,spins
                ,sleep_time
                ,backoffs
                ,@current_snap_time
        from sys.dm_os_spinlock_stats

select top 1 @previous_snap_time = snap_time from #_spin_waits
                where snap_time < (select max(snap_time) from #_spin_waits)
                order by snap_time desc

--get delta in the spin locks stats   
select top 10
        spins_current.lock_name
        , (spins_current.collisions - spins_previous.collisions) as collisions
        , (spins_current.spins - spins_previous.spins) as spins
        , (spins_current.sleep_time - spins_previous.sleep_time) as sleep_time
        , (spins_current.backoffs - spins_previous.backoffs) as backoffs
        , spins_previous.snap_time as [start_time]
        , spins_current.snap_time as [end_time]
        , DATEDIFF(ss, @previous_snap_time, @current_snap_time) as [seconds_in_sample]
    from #_spin_waits spins_current
    inner join (
        select * from #_spin_waits
          where snap_time = @previous_snap_time
        ) spins_previous on (spins_previous.lock_name = spins_current.lock_name)
    where
        spins_current.snap_time = @current_snap_time
        and spins_previous.snap_time = @previous_snap_time
        and spins_current.spins > 0
    order by (spins_current.spins - spins_previous.spins) desc

--clean up table
delete from #_spin_waits
where snap_time = @previous_snap_time
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Tools für die Leistungsüberwachung finden Sie unter [Tools für die Leistungsüberwachung und -optimierung](./performance/performance-monitoring-and-tuning-tools.md).
