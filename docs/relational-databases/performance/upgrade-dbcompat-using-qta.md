---
title: Aktualisieren von Datenbanken mithilfe des Abfrageoptimierungs-Assistenten
description: Erfahren Sie, wie der Abfrageoptimierungs-Assistent Sie durch den empfohlenen Workflow führt, um die Leistungsstabilität während Upgrades auf neuere SQL Server-Versionen aufrechtzuerhalten.
ms.custom: seo-dt-2019
ms.date: 02/13/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.querytuning.f1
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811e7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 1032f8e1ac32a08c9a6878c54fac0ee5e5c213e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015494"
---
# <a name="upgrading-databases-by-using-the-query-tuning-assistant"></a>Upgraden von Datenbanken mit dem Abfrageoptimierungs-Assistenten

[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Wenn Sie von einer älteren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version zu [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] oder neuer migrieren und ein Upgrade auf den aktuellen [Datenbankkompatibilitätsgrad](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) durchführen, kann es bei einer Workload womöglich zu Leistungseinbußen kommen. Dies ist in geringerem Ausmaß auch beim Upgrade von [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] auf eine neuere Version möglich.

Beginnend mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und mit jeder neuen Version werden alle Änderungen des Abfrageoptimierers auf den neuesten Datenbank-Kompatibilitätsgrad abgestimmt, sodass Ausführungspläne nicht direkt beim Upgrade geändert werden, sondern wenn ein Benutzer die `COMPATIBILITY_LEVEL`-Datenbankoption in die neueste verfügbare ändert. Weitere Informationen zu den in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] eingeführten Änderungen des Abfrageoptimierers finden Sie unter [Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md). Weitere Informationen zu Kompatibilitätsgraden und deren Auswirkungen auf Upgrades finden Sie unter [Kompatibilitätsgrade und Upgrades der Datenbank-Engine](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades).

Diese Gatingfunktion, die durch den Datenbank-Kompatibilitätsgrad bereitgestellt wird, bietet Ihnen in Kombination mit dem Abfragespeicher große Kontrolle über die Abfrageleistung im Upgradeprozess, wenn das Upgrade dem empfohlenen Workflow folgt, der unten gezeigt wird. Weitere Informationen zum empfohlenen Workflow für das Aktualisieren des Kompatibilitätsgrads finden Sie unter [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). 

![Empfohlener Datenbankupgradeworkflow mit dem Abfragespeicher](../../relational-databases/performance/media/query-store-usage-5.png "Empfohlener Datenbankupgradeworkflow mit dem Abfragespeicher") 

Diese Kontrolle über Upgrades wurde mit [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] weiter verbessert. In dieser Version wurde [Automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md) eingeführt und die Automatisierung des letzten Schritts im oben empfohlenen Workflow ermöglicht.

Beginnend mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 führt die neue Funktion **Abfrageoptimierungs-Assistent (Query Tuning Assistant, QTA)** Benutzer durch den empfohlenen Workflow, um die Leistungsstabilität bei Upgrades auf neuere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen aufrecht zu erhalten, wie im Abschnitt *Aufrechterhalten einer stabilen Leistung während des Upgrades auf eine neuere Version von SQL Server* der [Szenarien für die Verwendung des Abfragespeichers](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade) beschrieben. QTA führt allerdings kein Rollback auf einen bekannten funktionierenden Plan aus, wie im letzten Schritt des empfohlenen Workflows zu sehen. Stattdessen verfolgt QTA alle Regressionen nach, die in der Ansicht [Abfragespeicher – **Zurückgestellte Abfragen**](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed) gefunden wurden, und durchläuft mögliche Permutationen von anwendbaren Variationen des Optimierungsmodells, sodass ein neuer, besserer Plan erstellt werden kann.

> [!IMPORTANT]
> QTA generiert keine Benutzerworkload. Wenn Sie QTA in einer Umgebung ausführen, die nicht von Ihren Anwendungen verwendet wird, stellen Sie sicher, dass Sie weiterhin repräsentative Testworkloads auf dem [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Ziel mit anderen Mitteln ausführen können.

## <a name="the-query-tuning-assistant-workflow"></a>Workflow des Abfrageoptimierungs-Assistenten

Als Ausgangspunkt geht QTA davon aus, dass eine Datenbank aus einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (über [CREATE DATABASE... FOR ATTACH](../..//relational-databases/databases/attach-a-database.md) oder [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)) in eine neuere Version der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] verschoben wird, und der Kompatibilitätsgrad der Datenbank vor dem Upgrade wird nicht sofort geändert. QTA führt Benutzer durch die folgenden Schritte:

1. Konfigurieren des Abfragespeichers gemäß den empfohlenen Einstellungen für die Workloaddauer (in Tagen), die vom Benutzer festgelegt wird. Verwenden Sie eine Workloaddauer, die Ihrem normalen Geschäftszyklus entspricht.
2. Anfordern des Starts der erforderlichen Workload, damit der Abfragespeicher eine Baseline für Workloaddaten erfassen kann (wenn noch keine Baseline verfügbar ist).
3. Upgrade auf den vom Benutzer ausgewählten Zieldatenbank-Kompatibilitätsgrad.
4. Anfordern eines zweiten Durchgangs der Workloaddatenerfassung für Vergleichs- und Regressionserkennungszwecke.
5. Iterieren durch alle Regressionen, die auf Grundlage der Ansicht [Abfragespeicher **Zurückgestellte Abfragen**](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed) gefunden wurden, Experimentieren durch Erfassen von Laufzeitstatistiken für mögliche Permutationen anwendbarer Optimierungsmodellvariationen und Messen des Ergebnisses. 
6. Melden der gemessenen Optimierungen und optional Zulassen, dass diese Änderungen mit [Planhinweislisten](../../relational-databases/performance/plan-guides.md) persistent gespeichert werden.

Weitere Informationen zum Anfügen einer Datenbank finden Sie unter [Anfügen und Trennen einer Datenbank](../../relational-databases/databases/database-detach-and-attach-sql-server.md#AttachDb).

Im Folgenden erfahren Sie, wie QTA nur die letzten Schritte des empfohlenen Workflows zur Aktualisierung des Kompatibilitätsgrads mithilfe des Abfragespeichers wie oben beschrieben ändert. QTA bietet nicht nur die Möglichkeit, zwischen dem zurzeit ineffizienten Ausführungsplan und dem letzten bekannten funktionierenden Ausführungsplan zu wählen, sondern auch Optimierungsoptionen speziell für die ausgewählten zurückgestellten Abfragen, um einen neuen verbesserten Zustand mit optimierten Ausführungsplänen zu erstellen.

![Empfohlener Datenbankupgradeworkflow mit dem Abfrageoptimierungs-Assistenten](../../relational-databases/performance/media/qta-usage.png "Empfohlener Datenbankupgradeworkflow mit dem Abfrageoptimierungs-Assistenten")

### <a name="qta-tuning-internal-search-space"></a>Optimieren des internen Suchbereichs durch QTA

QTA bezieht sich nur auf `SELECT`-Abfragen, die aus dem Abfragespeicher ausgeführt werden können. Parametrisierte Abfragen sind zulässig, wenn der kompilierte Parameter bekannt ist. Abfragen, die von Laufzeitkonstrukten wie temporären Tabellen oder Tabellenvariablen abhängen, sind zurzeit nicht zulässig.

QTA zielt auf bekannte mögliche Muster von Anfrageregressionen aufgrund von Änderungen in [Kardinalitätsschätzungsversionen (Cardinality Estimator, CE)](../../relational-databases/performance/cardinality-estimation-sql-server.md). Wenn Sie beispielsweise eine Datenbank aus [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] mit dem Datenbank-Kompatibilitätsgrad 110 in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] mit dem Datenbank-Kompatibilitätsgrad 140 aktualisieren, können einige Abfragen Regression zeigen, da sie speziell für die Zusammenarbeit mit der CE-Version entworfen wurden, die in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] vorhanden war (CE 70). Dies bedeutet nicht, dass das Zurücksetzen von CE 140 auf CE 70 die einzige Option ist. Wenn nur eine bestimmte Änderung in der neueren Version zur Regression führt, dann ist es möglich, diese Abfrage mit einem Hinweis zu versehen, dass sie nur den relevanten Teil der vorherigen CE-Version verwendet, der für die spezifische Abfrage besser funktioniert hat, während sie gleichzeitig alle anderen Optimierungen der neueren CE-Versionen nutzt. Und auch andere Abfragen in der Workload, die keine Regression gezeigt haben, können von neueren CE-Optimierungen profitieren.

Die folgenden CE-Muster werden von QTA gesucht:

- **Unabhängigkeit im Vergleich zu Korrelation:** Wenn die Unabhängigkeitsannahme bessere Schätzungen für die spezifische Abfrage liefert, bewirkt der Abfragehinweis `USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES')`, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Ausführungsplan generiert, indem minimale Selektivität verwendet wird, wenn `AND`-Prädikate für Filter geschätzt werden, um die Korrelation zu berücksichtigen. Weitere Informationen finden Sie unter [USE HINT-Abfragehinweise](../../t-sql/queries/hints-transact-sql-query.md#use_hint) und [Versionen der Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Einfacher Einschluss im Vergleich zu Basiseinschluss:** Wenn ein anderer Joineinschluss bessere Schätzungen für die spezifische Abfrage liefert, bewirkt der Abfragehinweis `USE HINT ('ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS')`, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Ausführungsplan generiert, indem die Annahme eines einfachen Einschlusses anstelle der Standardannahme eines Basiseinschlusses verwendet wird. Weitere Informationen finden Sie unter [USE HINT-Abfragehinweise](../../t-sql/queries/hints-transact-sql-query.md#use_hint) und [Versionen der Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Feste Kardinalitätsschätzung der Tabellenwertfunktion mit mehreren Anweisungen (Multi-Statement Table-Valued Function, MSTVF)** von 100 Zeilen im Vergleich zu einer Zeile: Wenn die feste Standardschätzung für die Tabellenwertfunktion von 100 Zeilen nicht zu einem effizienteren Plan führt als die Verwendung der festen Schätzung einer Zeile (entsprechend dem Standard unter dem CE-Abfrageoptimierermodell von [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] und früheren Versionen), wird der Abfragehinweis `QUERYTRACEON 9488` verwendet, um einen Ausführungsplan zu erstellen. Weitere Informationen zu MSTVFs finden Sie unter [Erstellen benutzerdefinierter Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

> [!NOTE]
> Als letztes Mittel (wenn die Hinweise mit engem Bereich nicht ausreichend gute Ergebnisse für die zulässigen Abfragemuster liefern) wird auch die vollständige Verwendung von CE 70 in Betracht gezogen, indem der Abfragehinweis `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')` verwendet wird, um einen Ausführungsplan zu erstellen.

> [!IMPORTANT]
> Alle Hinweise erzwingen bestimmte Verhaltensweisen, die in zukünftigen Updates von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ggf. behoben werden. Es wird empfohlen, Hinweise nur dann anzuwenden, wenn keine andere Option besteht, und bei jedem neuen Upgrade den mit Hinweisen versehenen Code erneut zu überprüfen. Durch das Erzwingen von Verhaltensweisen verhindern Sie möglicherweise, dass Ihre Workload von Optimierungen profitieren kann, die in neueren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingeführt werden.

## <a name="starting-query-tuning-assistant-for-database-upgrades"></a>Starten des Abfrageoptimierungs-Assistenten für Datenbankupgrades

QTA ist eine sitzungsbasierte Funktion, die den Sitzungszustand im `msqta`-Schema der Benutzerdatenbank speichert, in der zum ersten Mal eine Sitzung erstellt wird. Mehrere Optimierungssitzungen können für eine einzelne Datenbank im Lauf der Zeit erstellt werden, aber nur eine aktive Sitzung kann für eine bestimmte Datenbank vorhanden sein.

### <a name="creating-a-database-upgrade-session"></a>Erstellen einer Datenbankupgradesitzung

1. Öffnen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] den Objekt-Explorer, und stellen Sie eine Verbindung mit [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.

2. Wählen Sie für die Datenbank, für die ein Upgrade des Datenbank-Kompatibilitätsgrads vorgesehen ist, mit der rechten Maustaste den Datenbanknamen aus, wählen Sie **Aufgaben** aus, wählen Sie **Datenbankupgrade** aus, und wählen Sie dann **Neue Datenbankupgradesitzung** aus.

3. Im Fenster des QTA-Assistenten sind zwei Schritte erforderlich, um eine Sitzung zu konfigurieren:

    1. Konfigurieren Sie im Fenster **Setup** den Abfragespeicher so, dass er das Äquivalent eines vollständigen Geschäftszyklus von Workloaddaten erfasst, die analysiert und optimiert werden sollen.
        - Geben Sie die erwartete Workloaddauer in Tagen ein (der Mindestwert ist 1 Tag). Diese Angabe wird verwendet, um empfohlene Abfragespeichereinstellungen vorzuschlagen, um vorläufig die gesamte Baseline zu erfassen. Die Erfassung einer guten Baseline ist wichtig, um sicherzustellen, dass alle zurückgestellten Abfragen, die nach einer Änderung des Kompatibilitätsgrads der Datenbank gefunden wurden, analysiert werden können. 
        - Legen Sie den vorgesehenen Zieldatenbank-Kompatibilitätsgrad fest, den die Benutzerdatenbank verwenden soll, nachdem der QTA-Workflow abgeschlossen wurde.
        Wählen Sie anschließend **Weiter** aus.

       ![Fenster zum Einrichten einer neuen Datenbankupgradesitzung](../../relational-databases/performance/media/qta-new-session-setup.png "Fenster zum Einrichten eines neuen Datenbankupgrades")  
  
    2. Im Fenster **Einstellungen** zeigen zwei Spalten den Status **Aktuell** des Abfragespeichers in der Zieldatenbank sowie die **empfohlenen** Einstellungen an. 
        - Die empfohlenen Einstellungen sind standardmäßig ausgewählt. Wenn Sie jedoch das Optionsfeld über der Spalte „Aktuell“ auswählen, werden die aktuellen Einstellungen akzeptiert, und Sie können auch die aktuelle Konfiguration des Abfragespeichers optimieren.
        - Die vorgeschlagene Einstellung *Schwellenwert für veraltete Abfragen* ist zwei Mal so groß wie die erwartete Workloaddauer in Tagen. Dies liegt daran, dass der Abfragespeicher Informationen zur Baselineworkload und zur Workload nach dem Datenbankupgrade speichern muss.
        Wählen Sie anschließend **Weiter** aus.

       ![Fenster für Einstellungen eines neuen Datenbankupgrades](../../relational-databases/performance/media/qta-new-session-settings.png "Fenster für Einstellungen eines neuen Datenbankupgrades")

       > [!IMPORTANT]
       > Die vorgeschlagene *Maximale Größe* ist ein beliebiger Wert, der ggf. für eine Workload mit kurzer Ausführungszeit geeignet ist.
       > Beachten Sie jedoch, dass es unter Umständen nicht ausreicht, Informationen zu den Baseline- und den Workloads nach dem Datenbankupgrade für sehr intensive Workloads zu speichern, insbesondere wenn viele verschiedene Pläne generiert werden können.
       > Wenn Sie davon ausgehen, dass dies der Fall sein wird, geben Sie einen höheren Wert ein, der angemessen ist.

4. Im Fenster **Optimierung** wird die Sitzungskonfiguration abgeschlossen, und die nächsten Schritte zum Öffnen und Fortsetzen der Sitzung werden eingeleitet. Wählen Sie nach Abschluss des Vorgangs **Fertig stellen** aus.

    ![Fenster zum Optimieren des neuen Datenbankupgrades](../../relational-databases/performance/media/qta-new-session-tuning.png "Fenster zum Optimieren des neuen Datenbankupgrades")

> [!NOTE]
> Ein mögliches alternatives Szenario beginnt damit, dass eine Datenbanksicherung vom Produktionsserver, auf dem eine Datenbank bereits den empfohlenen Workflow für das Upgrade der Datenbankkompatibilität durchlaufen hat, auf einem Testserver wiederhergestellt wird.

### <a name="executing-the-database-upgrade-workflow"></a>Ausführen des Workflows für das Datenbankupgrade

1. Wählen Sie für die Datenbank, für die ein Upgrade des Datenbank-Kompatibilitätsgrads vorgesehen ist, mit der rechten Maustaste den Datenbanknamen aus, wählen Sie **Aufgaben** aus, wählen Sie **Datenbankupgrade** aus, und wählen Sie dann **Sitzungen überwachen** aus.

2. Die Seite **Sitzungsverwaltung** listet die aktuellen und vergangenen Sitzungen für die Datenbank im Bereich auf. Wählen Sie die gewünschte Sitzung und dann **Details** aus.

    > [!NOTE]
    > Wenn die aktuelle Sitzung nicht vorhanden ist, wählen Sie die Schaltfläche **Aktualisieren** aus.

    Die Liste enthält die folgenden Informationen:
    - **Sitzungs-ID**
    - **Sitzungsname:** Vom System generierter Name, bestehend aus dem Namen der Datenbank sowie Datum und Uhrzeit der Sitzungserstellung.
    - **Status:** Status der Sitzung („Aktiv“ oder „Geschlossen“).
    - **Beschreibung**: Vom System generierte Beschreibung, bestehend aus dem vom Benutzer ausgewählten Kompatibilitätsgrad der Zieldatenbank und der Anzahl der Tage für die Geschäftszyklusworkload.
    - **Startzeit:** Das Datum und die Uhrzeit der Sitzungserstellung.

    ![Seite für Sitzungsverwaltung mit dem Abfrageoptimierungs-Assistenten](../../relational-databases/performance/media/qta-session-management.png "Seite für Sitzungsverwaltung mit dem Abfrageoptimierungs-Assistenten")

    > [!NOTE]
    > **Sitzung löschen** löscht alle Daten, die für die ausgewählte Sitzung gespeichert wurden.
    > Durch das Löschen einer geschlossenen Sitzung werden zuvor bereitgestellte Planhinweislisten jedoch **nicht** gelöscht.
    > Wenn Sie eine Sitzung löschen, für die Planhinweislisten bereitgestellt wurden, können Sie QTA nicht für ein Rollback verwenden.
    > Suchen Sie stattdessen mit der [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)-Systemtabelle nach Planhinweislisten, und löschen Sie diese mit [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md) manuell.
  
3. Der Einstiegspunkt für eine neue Sitzung ist der Schritt **Datenerfassung**.

    > [!NOTE]
    > Über die Schaltfläche **Sitzungen** kehren Sie zur Seite **Sitzungsverwaltung** zurück und behalten die aktive Sitzung in ihrem aktuellen Zustand bei.

    Dieser Schritt hat drei Teilschritte:

    1. **Baselinedatenerfassung** fordert den Benutzer auf, den repräsentativen Workloadzyklus auszuführen, damit der Abfragespeicher eine Baseline erfassen kann. Nachdem die Workload abgeschlossen wurde, überprüfen Sie **Workloadausführung abgeschlossen**, und wählen Sie dann **Weiter** aus.

        > [!NOTE]
        > Das QTA Fenster kann geschlossen werden, während die Workload ausgeführt wird. Die Rückkehr zur Sitzung (die im aktiven Zustand verbleibt) zu einem späteren Zeitpunkt wird mit dem gleichen Schritt fortgesetzt, bei dem sie unterbrochen wurde. 

        ![Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 1](../../relational-databases/performance/media/qta-step2-substep1.png "Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 1")

    2. **Upgrade der Datenbank** fordert zur Erteilung der Berechtigung auf, den Kompatibilitätsgrad der Datenbank in den gewünschten Zielwert zu aktualisieren. Um mit dem nächsten Teilschritt fortzufahren, wählen Sie **Ja** aus.

        ![Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 2: Upgrade des Datenbankkompatibilitätsgrads](../../relational-databases/performance/media/qta-step2-substep2-prompt.png "Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 2: Upgrade des Datenbankkompatibilitätsgrads")

        Die folgende Seite bestätigt, dass der Datenbank-Kompatibilitätsgrad erfolgreich aktualisiert wurde.

        ![Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 2](../../relational-databases/performance/media/qta-step2-substep2.png "Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 2")

    3. **Beobachtete Datenerfassung** fordert den Benutzer auf, den repräsentativen Workloadzyklus erneut auszuführen, sodass der Abfragespeicher eine vergleichende Baseline erfassen kann, die für die Suche nach Optimierungsmöglichkeiten verwendet wird. Verwenden Sie während der Ausführung der Workload die Schaltfläche **Aktualisieren**, um die Liste der zurückgestellten Abfragen zu aktualisieren, falls solche gefunden wurden. Ändern Sie den Wert für **Anzuzeigende Abfragen**, um die Anzahl der angezeigten Abfragen zu beschränken. Die Reihenfolge der Liste wird durch die **Metrik** (Dauer oder CpuTime) und die **Aggregation** („Mittelwert“ ist Standard) beeinflusst. Wählen Sie auch aus, wie viele **Abfragen angezeigt werden sollen**. Nachdem die Workload abgeschlossen wurde, überprüfen Sie **Workloadausführung abgeschlossen**, und wählen Sie dann **Weiter** aus.

        ![Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 3](../../relational-databases/performance/media/qta-step2-substep3.png "Abfrageoptimierungs-Assistent Schritt 2, Teilschritt 3")

        Die Liste enthält die folgenden Informationen:
        - **Abfrage-ID** 
        - **Abfragetext**: Eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, die durch Auswählen der Schaltfläche **...** erweitert werden kann.
        - **Ausführungen:** Zeigt die Anzahl der Ausführungen dieser Abfrage für die gesamte Workloadsammlung an.
        - **Baselinemetrik:** Die ausgewählte Metrik (Dauer oder CpuTime) in Millisekunden für die Baselinedatensammlung vor dem Upgrade des Datenbank-Kompatibilitätsgrads.
        - **Beobachtete Metrik:** Die ausgewählte Metrik (Dauer oder CpuTime) in Millisekunden für die Datensammlung nach dem Upgrade des Datenbank-Kompatibilitätsgrads.
        - **Änderung in %:** Prozentuale Änderung für die ausgewählte Metrik zwischen dem Zustand vor und nach dem Upgrade des Datenbank-Kompatibilitätsgrads. Ein negativer Wert stellt die Menge der gemessenen Regression für die Abfrage dar.
        - **Optimierbar:** *TRUE* oder *FALSE*, abhängig davon, ob die Abfrage für Experimente geeignet ist.

4. **Analyse anzeigen** ermöglicht die Auswahl, mit welchen Abfragen experimentiert werden soll, um Optimierungsmöglichkeiten zu ermitteln. Der Wert **Anzuzeigende Abfragen** wird zum Bereich der geeigneten Abfragen für Experimente. Nachdem die gewünschten Abfragen aktiviert wurden, wählen Sie **Weiter** aus, um die Experimente zu starten.

    > [!NOTE]
    > Abfragen mit „Optimierbar = FALSE“ können nicht für Experimente ausgewählt werden.

    > [!IMPORTANT]
    > Eine Eingabeaufforderung weist darauf hin, dass die Rückkehr zur Seite „Analyse anzeigen“ nicht möglich ist, sobald QTA in die Experimentierphase übergeht.   
    > Wenn Sie nicht alle geeigneten Abfragen auswählen, bevor Sie zur Experimentierphase übergehen, müssen Sie zu einem späteren Zeitpunkt eine neue Sitzung erstellen und den Workflow wiederholen. Dies erfordert das Zurücksetzen des Datenbank-Kompatibilitätsgrads auf den vorherigen Wert.

    ![Abfrageoptimierungs-Assistent Schritt 3](../../relational-databases/performance/media/qta-step3.png "Abfrageoptimierungs-Assistent Schritt 3")

5. **Ergebnisse anzeigen** ermöglicht die Auswahl, für welche Abfragen die vorgeschlagene Optimierung als Planhinweisliste bereitgestellt werden soll. 

    Die Liste enthält die folgenden Informationen:
    - **Abfrage-ID**
    - **Abfragetext**: Eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, die durch Auswählen der Schaltfläche **...** erweitert werden kann.
    - **Status:** Zeigt den aktuellen Experimentierstatus für die Abfrage an.
    - **Baselinemetrik**: Die ausgewählte Metrik (Dauer oder CpuTime) in Millisekunden für die Abfrage, die in **Schritt 2, Teilschritt 3** ausgeführt wurde. Diese Metrik stellt die zurückgestellte Abfrage nach dem Upgrade des Datenbank-Kompatibilitätsgrads dar.
    - **Beobachtete Metrik:** Die ausgewählte Metrik (Dauer oder CpuTime) für die Abfrage nach dem Experimentieren in Millisekunden, für die eine ausreichend gute Optimierung vorgeschlagen wurde.
    - **Änderung in %:** Prozentuale Änderung für die ausgewählte Metrik zwischen dem Zustand vor und nach dem Experimentieren. Sie stellt die Menge der gemessenen Optimierung für die Abfrage mit der vorgeschlagenen Optimierung dar.
    - **Abfrageoption:** Link zum vorgeschlagenen Hinweis, der die Abfrageausführungsmetrik verbessert.
    - **Bereitstellung möglich:** *TRUE* oder *FALSE*, abhängig davon, ob die vorgeschlagene Abfrageoptimierung als Planhinweisliste bereitgestellt werden kann.

    ![Abfrageoptimierungs-Assistent Schritt 4](../../relational-databases/performance/media/qta-step4.png "Abfrageoptimierungs-Assistent Schritt 4")

6. **Überprüfung** zeigt den Bereitstellungsstatus zuvor ausgewählter Abfragen für diese Sitzung an. Die Liste auf dieser Seite unterscheidet sich von der vorherigen Seite durch Ändern der Spalte **Kann bereitgestellt werden** in **Rollback kann ausgeführt werden**. Diese Spalte kann *TRUE* oder *FALSE* abhängig davon sein, ob für die bereitgestellte Abfrageoptimierung ein Rollback ausgeführt und ihre Planhinweisliste entfernt werden kann.

    ![Abfrageoptimierungs-Assistent Schritt 5](../../relational-databases/performance/media/qta-step5.png "Abfrageoptimierungs-Assistent Schritt 5")

    Wenn zu einem späteren Zeitpunkt ein Rollback für eine vorgeschlagene Optimierung erforderlich ist, wählen Sie die entsprechende Abfrage aus, und wählen Sie dann **Rollback** aus. Diese Abfrageplan-Hinweisliste wird entfernt und die Liste aktualisiert, um die Abfrage zu entfernen, für die ein Rollback ausgeführt wurde. Beachten Sie in der folgenden Abbildung, dass Abfrage 8 entfernt wurde.

    ![Abfrageoptimierungs-Assistent Schritt 5: Rollback](../../relational-databases/performance/media/qta-step5-rollback.png "Abfrageoptimierungs-Assistent Schritt 5: Rollback")

    > [!NOTE]
    > Durch das Löschen einer geschlossenen Sitzung werden zuvor bereitgestellte Planhinweislisten **nicht** gelöscht.
    > Wenn Sie eine Sitzung löschen, für die Planhinweislisten bereitgestellt wurden, können Sie QTA nicht für ein Rollback verwenden.
    > Suchen Sie stattdessen mit der [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)-Systemtabelle nach Planhinweislisten, und löschen Sie diese mit [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md) manuell.
  
## <a name="permissions"></a>Berechtigungen

Erfordert Mitgliedschaft in der Rolle **db_owner**.
  
## <a name="see-also"></a>Weitere Informationen

- [Kompatibilitätsgrade und Upgrades der Datenbank-Engine](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)
- [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [USE HINT-Abfragehinweise](../../t-sql/queries/hints-transact-sql-query.md#use_hint)
- [Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Automatic Tuning (Automatische Optimierung)](../../relational-databases/automatic-tuning/automatic-tuning.md)   
- [Verwenden des SQL Server-Abfrageoptimierungs-Assistenten](/learn/modules/use-sql-server-query-tuning-assistant/)