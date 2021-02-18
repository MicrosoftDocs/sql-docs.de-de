---
title: Fenster Suchergebnisse
description: Zwei Suchergebnisfenster enthalten Übereinstimmungen, die über die Vorgänge „In Dateien suchen“ und „In Dateien ersetzen“ gefunden wurden. Hier erfahren Sie, wie Sie diese Fenster anzeigen und wie Sie die Codedatei und Zeile anzeigen können, die einer Übereinstimmung entspricht.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vs.findresults2
helpviewer_keywords:
- Find Results Windows dialog box
ms.assetid: 3b68dbb7-26d6-4bc9-bd2c-c27e5dc385c3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2baef28ceee31cd1a6fc3aa59a2a8c55f64d210b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271643"
---
# <a name="find-results-windows"></a>Fenster Suchergebnisse
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In den beiden Fenstern mit der Bezeichnung Suchergebnisse werden Übereinstimmungen angezeigt, die mithilfe der Registerkarten **In Dateien suchen** oder **In Dateien ersetzen** des Dialogfelds **Suchen und Ersetzen** gefunden wurden. Mit dem Befehl **Ergebnisoptionen** für **In Dateien suchen** und **In Dateien ersetzen** können Sie das Fenster Suchergebnisse auswählen, in dem gefundene Übereinstimmungen aufgeführt werden.  
  
 Wenn Übereinstimmungen gefunden werden, wird das ausgewählte Fenster Suchergebnisse immer automatisch geöffnet. Wenn Sie ein Fenster mit der Bezeichnung Suchergebnisse manuell öffnen möchten, klicken Sie im Menü **Ansicht** auf **Weitere Fenster** , und klicken Sie anschließend auf **Suchergebnisse 1** oder **Suchergebnisse 2**.  
  
 Sie können die Codedatei anzeigen und zur Zeile springen, in der eine Übereinstimmung auftritt, indem Sie in der Ergebnisliste auf eine beliebige Zeile doppelklicken. Die Quelldatei wird im Code Editor angezeigt, wobei sich die Einfügemarke an der Stelle befindet, an der der übereinstimmende Text beginnt. Im Indikatorrand des Editors wird ein Symbol angezeigt, mit der die Zeile markiert wird, die die Übereinstimmung enthält, und in der Statusleiste wird der vollständige Text angezeigt.  
  
## <a name="toolbar-buttons"></a>Symbolleistenschaltflächen  
 Mithilfe der folgenden Symbolleistenschaltfläche können Sie die Ergebnisliste durchsuchen und zu den Zeilen im Code springen, in denen Übereinstimmungen gefunden wurden.  
  
 **Seitenflag + Pfeil nach oben**  
 Wechselt zur Zeile, in der die ausgewählte Übereinstimmung gefunden wurde.  
  
 **Seite + Pfeil nach links**  
 Wechselt zur Zeile mit der vorherigen Übereinstimmung.  
  
 **Seite + Pfeil nach rechts**  
 Wechselt zur Zeile mit der nächsten Übereinstimmung.  
  
 **Auswahl aufheben**  
 Entfernt alle Übereinstimmungen aus der Liste der **Ergebnisse** .  
  
## <a name="shortcut-keys"></a>Tastenkombinationen  
 Mithilfe der folgenden verfügbaren Tastenkombinationen können Sie schnell durch die gefundenen Übereinstimmungen navigieren.  
  
 STRG+POS1  
 Bildlauf zum Anfang des Fensters Suchergebnisse.  
  
 STRG+ENDE  
 Bildlauf zum Ende des Fensters Suchergebnisse.  
  
 BILD-AUF  
 Bildlauf nach oben zur nächsten Gruppe von Übereinstimmungen.  
  
 BILD-AB  
 Bildlauf nach unten zur nächsten Gruppe von Übereinstimmungen.  
  
 NACH-OBEN-TASTE  
 Auswählen der vorherigen Übereinstimmung.  
  
 NACH-UNTEN-TASTE  
 Auswählen der nächsten Übereinstimmung.  
  
## <a name="search-result-entries"></a>Suchergebniseinträge  
 Jeder Eintrag in der Ergebnisliste enthält die folgenden Informationen:  
  
-   Vollständiger Pfad  
  
-   Dateiname  
  
-   Zeilennummer  
  
-   Den vollständigen Text der Zeile mit der Übereinstimmung  
  
> [!TIP]  
>  Sie können die **Schnellsuche** verwenden, um längere Ergebnislisten zu durchsuchen. Öffnen Sie das Fenster Suchergebnisse, und docken Sie es an. Klicken Sie anschließend auf die dreieckige Schaltfläche **Ansicht** auf der Registerkarte **Suchen** , und wechseln Sie zur **Schnellsuche**. Legen Sie das Feld **Suchen in** der Suche auf **Aktives Fenster** fest, geben Sie eine **Suchen nach** -Zeichenfolge ein, und klicken Sie anschließend auf **Weitersuchen**. Hierdurch können Sie die Ergebnisliste nach Übereinstimmungen durchsuchen, die in bestimmten Ordnern oder Dateien gefunden wurden, oder nach Übereinstimmungen in Codezeilen, in denen auch andere wesentliche Begriffe auftreten.  
  
## <a name="summary-lines"></a>Zusammenfassungszeilen  
 Jede Gruppe von Suchergebnissen beginnt mit einer Zeile, in der die Suchparameter angegeben werden, und endet mit einer Statistikzeile. Die Ergebnisliste einer **In Dateien suchen** -Suche in allen geöffneten Dokumenten nach Zeichenfolgen, die mit dem regulären Ausdruck "`var[1-3]&par`" übereinstimmen, kann beispielsweise mit der folgenden Zeile von Suchparametern beginnen:  
  
 `Find all "var[1-3]&par" Regular Expression, All Open Documents`  
  
 und könnte mit der folgenden Statistikzeile enden:  
  
 `Total found: 57  Matching files: 23  Total files searched: 59`  
  
 Die Ergebnisliste einer in allen geöffneten Dokumenten durchgeführten Suche **In Dateien ersetzen** , durch die mit dem regulären Ausdruck "`var[1-3]&par`" übereinstimmende Zeichenfolgen durch die Zeichenfolge "`sample`" ersetzt werden, kann beispielsweise mit der folgenden Zeile von Suchparametern beginnen:  
  
 `Replace "var[1-3]&par", "sample", Regular Expression, All Open Documents`  
  
 und könnte mit der folgenden Statistikzeile enden:  
  
 `Total replaced: 57  Matching files: 23  Total files searched: 59`  
