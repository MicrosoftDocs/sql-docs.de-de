---
title: Analysieren der Skriptleistung
description: In diesem Artikel erfahren Sie, wie Clientstatistiken, Ausführungspläne und Code Analysis für SQL Server Data Tools die Leistung von Abfragen, gespeicherten Prozeduren und Skripts verbessern kann.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f4bbdd31-12a5-4c57-b0fe-1c6683820f11
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5f8d17fa156cd144a87b2dc387564cd93556d8da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077191"
---
# <a name="analyze-script-performance"></a>Analysieren der Skriptleistung

Sie können mit den von SQL Server Data Tools bereitgestellten Tools bestimmen, ob die Leistung von Abfragen, gespeicherten Prozeduren oder Skripts verbessert werden kann. Indem Sie beispielsweise Clientstatistiken wie die Antwortzeiten für häufig verwendete Abfragen überwachen, können Sie ermitteln, ob Änderungen an der Abfrage oder den Indizes in den Tabellen erforderlich sind. Solche Statistiken können die Clientausführungszeit, das Abfrageprofil sowie gesendete und empfangene Pakete/Bytes enthalten.  
  
Außerdem ist für gewisse Leistungsprobleme ein besserer Lösungsansatz eine Analyse der Anwendungsabfragen sowie der durch die Anwendung an die Datenbank gesendeten Updates und deren Interaktion mit den in der Datenbank und im Datenbankschema enthaltenen Daten. In Ausführungsplänen werden die vom SQL Server-Abfrageoptimierer ausgewählten Datenabrufmethoden grafisch dargestellt und die Ausführungskosten für bestimmte Anweisungen und Abfragen angezeigt. Auf diese Weise ist erkennbar, wie die SQL-Abfrage von SQL Server verarbeitet wird und wodurch Leistungseinbußen verursacht werden.  
  
## <a name="using-client-statistics"></a>Verwenden von Clientstatistiken  
Wenn Sie im Transact\-SQL-Editor ein Skript oder eine Abfrage ausführen, können Sie festlegen, dass Clientstatistiken wie Anwendungsprofil-, Netzwerk- und Zeitstatistiken zur Ausführung gesammelt werden sollen. Mit solchen Metriken können Sie die Effizienz von Skripts messen und Vergleichstests mit anderen Skripts durchführen.  
  
Um die Erfassung von Clientstatistiken zu aktivieren oder zu deaktivieren, zeigen Sie im geöffneten Transact\-SQL-Editor im Menü **Daten** auf **Transact\-SQL-Editor** und klicken auf **Ausführungseinstellungen** und dann auf **Clientstatistiken einschließen**. Sie können auch auf der Symbolleiste des Transact\-SQL-Editors auf die Schaltfläche **Clientstatistiken einschließen** (die fünfte Schaltfläche von rechts) klicken oder mit der rechten Maustaste in den Transact\-SQL-Editor klicken und anschließend auf **Ausführungseinstellungen** und **Clientstatistiken einschließen** klicken. Beachten Sie, dass Sie zum Erfassen von Statistiken für eine Abfrage diese Funktion vor der Ausführung der Abfrage aktivieren müssen.  
  
Wenn Sie Clientstatistiken aktiviert haben, wird nach Ausführen der Abfrage die Registerkarte **Statistik** neben der Registerkarte **Meldung** angezeigt. Wenn Sie Clientstatistiken deaktiviert haben, wird die Registerkarte **Statistik** nicht angezeigt. Statistiken von aufeinander ausgeführten Abfragen werden mit Durchschnittswerten aufgelistet.  
  
Weitere Informationen zu den gesammelten Statistiken finden Sie unter [Statistikbereich im Abfragefenster](https://msdn.microsoft.com/library/aa216969(SQL.80).aspx) und im Abschnitt zur [Registerkarte „Clientstatistiken“ in diesem Artikel](/previous-versions/visualstudio/visual-studio-2010/aa833205(v=vs.100)).  
  
## <a name="using-execution-plans"></a>Verwenden von Ausführungsplänen  
In Ausführungsplänen wird angezeigt, wie die Datenbank-Engine in Tabellen navigiert und Indizes verwendet, um auf die Daten für eine Abfrage oder eine andere DML-Anweisung wie ein Update zuzugreifen oder diese zu verarbeiten. Durch diese grafische Darstellung sind die Leistungsmerkmale einer Abfrage wesentlich leichter zu verstehen.  
  
Öffnen Sie ein Transact\-SQL-Skript, das die Abfragen enthält, die im Transact\-SQL-Editor analysiert werden sollen. Sie können anschließend den zu überprüfenden Code markieren und festlegen, dass ein geschätzter Ausführungsplan angezeigt werden soll. Klicken Sie dazu auf der Symbolleiste des Editors auf die Schaltfläche **Geschätzten Ausführungsplan anzeigen**. Wenn Sie auf **Geschätzten Ausführungsplan anzeigen** klicken, werden die Transact\-SQL-Abfragen bzw. -Batches nicht ausgeführt. Stattdessen wird das Skript analysiert, und der Abfrageausführungsplan wird angezeigt, den die Datenbank-Engine bei tatsächlicher Ausführung der Abfragen mit größter Wahrscheinlichkeit verwenden würde.  
  
Nachdem das Skript analysiert oder ausgeführt wurde, klicken Sie auf die Registerkarte **Ausführungsplan**, um eine grafische Darstellung der Ausführungsplanausgabe anzuzeigen.  
  
Die Ausgabe des grafischen Ausführungsplans wird von rechts nach links und von oben nach unten gelesen. Jede Abfrage im Batch, die analysiert wird, wird einschließlich des prozentualen Kostenanteils der Abfrage an den Batchgesamtkosten angezeigt. Um weitere Informationen wie Kosten und Operation für die einzelnen Schritte anzuzeigen, platzieren Sie den Mauszeiger über den [Symbolen der logischen und physischen Operatoren](/previous-versions/sql/sql-server-2008-r2/ms175913(v=sql.105)) im grafischen Plan.  
  
Um die Anzeige des Ausführungsplans zu ändern, klicken Sie mit der rechten Maustaste auf den **Ausführungsplan**, und klicken Sie auf **Vergrößern**, **Verkleinern**, **Vergrößern/Verkleinern** oder **Mit Zoom anpassen**. Mit **Vergrößern** und **Verkleinern** können Sie den Ausführungsplan in festgelegten Schritten vergrößern oder verkleinern. **Vergrößern/Verkleinern** ermöglicht Ihnen, die Anzeigevergrößerung nach Wunsch festzulegen, etwa auf 80 Prozent.  **Mit Zoom anpassen** passt den Ausführungsplan an den Ergebnisbereich an.  
  
Ausführungspläne können gespeichert und für eine spätere Untersuchung erneut geöffnet werden. Klicken Sie hierzu mit der rechten Maustaste auf den **Ausführungsplan**, und wählen Sie **Ausführungsplan speichern unter** aus. Anschließend können Sie den Plan in Visual Studio wie jede andere Datei öffnen.  
  
## <a name="using-code-analysis"></a>Verwenden der Codeanalyse  
Mithilfe der Codeanalyse können Sie potenzielle Probleme in den Skripts ermitteln, z. B. Entwurfs-, Benennungs- und Leistungsprobleme.  Regeln für Datenbankprojekte sind in vordefinierten Regelsätzen organisiert, die auf bestimmte Bereiche abzielen. Sie können die einzelnen Regeln auf der Registerkarte **Codeanalyse** der Eigenschaftenseite **Projekteigenschaften** aktivieren oder deaktivieren. Auf derselben Registerkarte können Sie festlegen, dass die Codeanalyse bei jedem Erstellen eines Projekts automatisch ausgeführt werden soll. Zudem können Sie angeben, ob Warnungen als Fehler behandelt werden sollen.  
  
Um die Codeanalyse manuell auszuführen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Codeanalyse ausführen** aus. Warnungen der Codeanalyse werden im Fenster **Fehlerliste** aufgelistet. Sie können auf eine Warnung doppelklicken, um zum Quellcode zu navigieren, der das betreffende Problem enthält. Außerdem können Sie über das Kontextmenü **Hilfe zu Fehlern anzeigen** weitere Informationen und mögliche Korrekturen für eine Warnung aufrufen.  
  
Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren von Datenbankcode zum Verbessern der Codequalität](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
