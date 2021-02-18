---
title: Suchen und Ersetzen
description: Informieren Sie sich über die vier verschiedenen Möglichkeiten, Text zu suchen und zu ersetzen, bei denen jeweils eigene Versionen des Dialogfelds „Suchen und ersetzen“ angezeigt werden. Die Optionseinstellungen für „Suchen und ersetzen“ gelten für all diese Suchmöglichkeiten.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- match case [SQL Server]
- undo operations
- searches [SQL Server Management Studio]
- replacing text
- quick search and replaces [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], undo
- Query Editor [SQL Server Management Studio], searching
- regular expressions [SQL Server Management Studio]
- finding text [SQL Server Management Studio]
- text [SQL Server], search and replace operations
- finding [SQL Server Management Studio]
- locating text
- Query Editor [SQL Server Management Studio], replacing text
- Find and Replace dialog box
- wildcard options [SQL Server Management Studio]
- match whole word [SQL Server]
- searches [SQL Server Management Studio], replacing
ms.assetid: 3641c7b3-3e3e-4ddd-af82-c15b50004f94
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b723be5ceacacb587156ddad40837472ca696af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354577"
---
# <a name="search-and-replace"></a>Suchen und Ersetzen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Es gibt mehrere verschiedene Möglichkeiten, Text zu suchen und zu ersetzen. Im Menü **Bearbeiten** finden Sie unter **Suchen und Ersetzen** vier Auswahlmöglichkeiten: **Schnellsuche**, **Schnellersetzung**, **In Dateien suchen** oder **In Dateien ersetzen**. Durch jede dieser Optionen wird eine entsprechende Version des Dialogfelds **Suchen und Ersetzen** geöffnet. Mit Tastenkombinationen zur inkrementellen Suche können Sie auch einen Suchvorgang ohne Dialogfeld ausführen. Mit diesen Techniken können Sie den Bereich für den Such- und Ersetzungsvorgang steuern und die Methode zum Überprüfen von Suchübereinstimmungen und Ersetzungen auswählen.  
  
 Beim Suchen und Ersetzen von Text müssen Sie die folgenden Punkte beachten:  
  
-   Die im Dialogfeld **Suchen und Ersetzen** festgelegten Optionen betreffen alle Suchvorgänge. Zu diesen Optionen zählen **Groß-/Kleinschreibung beachten**, **Nur ganzes Wort suchen**, **Suchrichtung nach oben**, **Ausgeblendeten Text durchsuchen**, **Platzhalter**, **Reguläre Ausdrücke**, die Option zum **Suchen in allen geöffneten Dokumenten** und die Option zum **Suchen im aktuellen Projekt**. Diese Optionen sind nicht in allen Versionen des Dialogfelds **Suchen und Ersetzen** vollständig verfügbar.  
  
-   **Rückgängig** ist nur für Dokumente verfügbar, die nach einem Ersetzungsvorgang geöffnet bleiben.  
  
-   Wenn beim Vorgang **Alle ersetzen** , der sich über mehr als eine Datei erstreckt, die Option **Rückgängig** verwendet wird, wird sie als eine einzelne gesammelte Aktion über die betroffenen Dateien hinweg betrachtet. Das bedeutet, dass es nicht möglich ist, die Änderungen in einigen Dateien rückgängig zu machen und in anderen Dateien beizubehalten.  
  
 Im Allgemeinen können Sie keine Elemente mit grafischen Ansichten durchsuchen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Inkrementelles Durchsuchen eines aktiven Dokuments](./search-an-active-document-incrementally.md)   
 [Interaktives Durchsuchen von Dokumenten](./search-documents-interactively.md)   
 [Durchsuchen von Dokumenten mithilfe von Ergebnislisten](./search-documents-using-results-lists.md)   
 [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md)   
 [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md)  
  
