---
title: Durchsuchen von Dokumenten mithilfe von Ergebnislisten
description: Hier erfahren Sie, wie Sie Text in allen Dateien suchen und ersetzen, die in einem Projekt, einer Projektmappe oder einem Dateisystemordner enthalten sind. Übereinstimmungen werden im Fenster „Suchergebnisse 1“ angezeigt, und Sie können auf eine Übereinstimmung doppelklicken, um die Zeile anzuzeigen, in der sie enthalten ist.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], result lists
- result list searches [SQL Server Management Studio]
- searches [SQL Server Management Studio], multiple files
- Query Editor [SQL Server Management Studio], search multiple files
ms.assetid: 275e1b6c-fbd0-4408-af77-35903f90657c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 79bd3ff3e7998c92e1c3f6474480cb36c352a98a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354585"
---
# <a name="search-documents-using-results-lists"></a>Durchsuchen von Dokumenten mithilfe von Ergebnislisten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
   Mit dem Dialogfeld **Suchen und Ersetzen** können Sie Text in allen Dateien innerhalb eines Projekts bzw. einer Projektmappe oder in einem Dateisystemordner durchsuchen und ersetzen, auch wenn diese nicht in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] geöffnet sind. Übereinstimmungen aus Suchvorgängen, die mit dem Dialogfeld **Suchen und Ersetzen** ausgeführt wurden, werden in den Fenstern Suchergebnisse 1 und Suchergebnisse 2 angezeigt. Im Fenster Suchergebnisse 2 können Sie den genauen Text aus der Zeile anzeigen, in der sich die Übereinstimmung befindet.  
  
### <a name="to-search-in-multiple-files"></a>So durchsuchen Sie mehrere Dateien  
  
1.  Zeigen Sie im Menü **Bearbeiten** auf **Suchen und Ersetzen** , und klicken Sie dann auf **In Dateien suchen**.  
  
2.  Geben Sie in das Textfeld **Suchen nach** den Text ein, nach dem Sie suchen möchten.  
  
3.  Klicken Sie in der Liste **Suchen in** auf **Alle geöffneten Dokumente**, **Aktuelles Projekt**, **Gesamte Projektmappe**, oder geben Sie einen Verzeichnispfad ein.  
  
4.  Wählen Sie in der Liste **Dateitypen** eine der aufgeführten Dateierweiterungsgruppen aus, oder geben Sie die Dateierweiterungen für die Dateitypen ein, nach denen gesucht werden soll, getrennt durch Semikolons. Verwenden Sie \*geöffnet sind.\* um alle Dateien in dem in der Dropdownliste **Suchen in** angegebenen Verzeichnis zu durchsuchen.  
  
5.  Treffen Sie aus den übrigen Suchoptionen eine Auswahl, um die Genauigkeit der Suche zu verbessern.  
  
6.  Klicken Sie auf **Suchen** , um die Suche zu starten.  
  
 Standardmäßig werden die Übereinstimmungen für den Suchvorgang im Fenster Suchergebnisse 1 angezeigt. Sie können die Suchübereinstimmungen durchsuchen, indem Sie im Fenster Suchergebnisse 1 auf die einzelnen Einträge doppelklicken. Um die Suchergebnisse in einem neuen Fenster anzuzeigen, wählen Sie die Option zum **Anzeigen in Suche 2** aus.  
  
#### <a name="to-replace-across-multiple-files-or-folders"></a>So ersetzen Sie Elemente in mehreren Dateien oder Ordnern  
  
1.  Zeigen Sie im Menü **Bearbeiten** auf **Suchen und Ersetzen** , und klicken Sie dann auf **In Dateien ersetzen**.  
  
2.  Geben Sie in das Textfeld **Suchen nach** den Text ein, nach dem Sie suchen möchten.  
  
3.  Geben Sie in das Feld **Ersetzen durch** den Text ein, durch den der Suchtext ersetzt werden soll.  
  
4.  Klicken Sie in der Liste **Suchen in** auf **Alle geöffneten Dokumente**, **Aktuelles Projekt**, **Gesamte Projektmappe**, oder geben Sie einen Verzeichnispfad ein.  
  
5.  Klicken Sie auf **Ersetzen** , um die gefundene Suchübereinstimmung durch den Text im Feld **Ersetzen durch** zu ersetzen. Wenn Sie auf **Weitersuchen** klicken, können Sie einzelne Übereinstimmungen auslassen. Wenn Sie auf **Datei überspringen** klicken, können Sie eine Datei auslassen.  
  
     \- oder –  
  
     Klicken Sie auf **Alle ersetzen** , um alle gefundenen Suchübereinstimmungen durch den Text im Feld **Ersetzen durch** zu ersetzen. Wählen Sie **Nach dem Ersetzen, geänderte Dateien geöffnet lassen** aus, wenn Sie einige der vorgenommenen Ersetzungen zu einem späteren Zeitpunkt rückgängig machen möchten.  
  
    > [!NOTE]  
    >  Durch **Alle ersetzen** werden alle gefundenen Suchübereinstimmungen ersetzt, einschließlich jener in den Dateien, die Sie mit **Datei überspringen** oder **Weitersuchen** ausgelassen haben. Sie können die Option **Rückgängig** nur für Ersetzungen verwenden, die in Dateien vorgenommen wurden, die nach dem Ersetzungsvorgang geöffnet bleiben.  
  
 Standardmäßig werden die Ersetzungsinformationen im Fenster Suchergebnisse 1 angezeigt. Sie können die Ersetzungen durchsuchen, indem Sie im Fenster Suchergebnisse 1 auf die einzelnen Einträge doppelklicken.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Suchen und Ersetzen](./search-and-replace.md)   
 [Interaktives Durchsuchen von Dokumenten](./search-documents-interactively.md)   
 [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md)   
 [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md)  
  
