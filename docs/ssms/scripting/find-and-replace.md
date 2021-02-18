---
title: Suchen und Ersetzen
description: In diesem Artikel erfahren Sie, wie Sie das Dialogfeld „Suchen und Ersetzen“ nutzen, um Text zu suchen und zu ersetzen.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find and Replace dialog box
ms.assetid: 09297893-d80b-4c88-86b4-52bfb639e521
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd45603e4254be161b6e80a59e969f61784cc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354665"
---
# <a name="find-and-replace"></a>Suchen und Ersetzen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Mithilfe des Dialogfelds **Suchen und Ersetzen** können Sie Texte innerhalb von Dateien finden und ggf. ersetzen. Es können verschiedene Versionen des Dialogfelds **Suchen und Ersetzen** mit leicht unterschiedlichen Optionen angezeigt werden, abhängig davon, auf welche Weise das Dialogfeld geöffnet wurde. Zeigen Sie im Menü **Bearbeiten** auf **Suchen und Ersetzen**, und klicken Sie auf **Schnellsuche** , um das Dialogfeld mit Such- aber ohne Ersetzungsoptionen zu öffnen. Zeigen Sie im Menü **Bearbeiten** auf **Suchen und Ersetzen**, und klicken Sie auf **Schnellersetzung** , um das Dialogfeld mit Such- und Ersetzungsoptionen zu öffnen.  
  
 Das Dialogfeld **Suchen und Ersetzen** kann auch über Symbolleistenschaltflächen und Tastenkombinationen geöffnet werden.  
  
## <a name="find-what"></a>Suchen nach  
 Diese Steuerelemente ermöglichen Ihnen, die Zeichenfolge oder den Ausdruck anzugeben, nach dem gesucht werden soll.  
  
 **Suchen nach**  
 Geben Sie den Suchtext ein. Im Dialogfeld wird ein wahrscheinlicher Suchtext eingetragen. Dabei wird entweder auf den Text zurückgegriffen, der vor dem Öffnen des Dialogfelds mit dem Cursor markiert wurde, oder auf in der Nähe befindlichen Text oder auf zuvor verwendeten Suchtext. Sie können aus der Dropdownliste eine der letzten 20 Suchzeichenfolgen zur Wiederverwendung auswählen.  
  
 **[Zeichenfolge mit Platzhaltern]**  
 Wenn Sie Platzhalter wie Sternchen (`*`) und Fragezeichen (`?`) in den Suchzeichenfolgen verwenden möchten, aktivieren Sie unter **Suchoptionen** das Kontrollkästchen **Mit** , und klicken Sie dann auf **Platzhalter**.  
  
 **[regulärer Ausdruck]**  
 Wenn die Such-Engine die Suchzeichenfolge als regulären Ausdruck interpretieren soll, aktivieren Sie unter **Suchoptionen** das Kontrollkästchen **Mit**, und klicken Sie anschließend auf **Reguläre Ausdrücke**.  
  
 **Ausdrucks-Generator**  
 Die dreieckige Schaltfläche neben dem Feld **Suchen nach** steht zur Verfügung, nachdem das Kontrollkästchen **Mit** in den **Suchoptionen** aktiviert wurde. Klicken Sie auf diese Schaltfläche, um in Abhängigkeit von der für **Mit** gewählten Option eine Liste von Platzhaltern oder regulären Ausdrücken anzuzeigen. Ein aus dieser Liste ausgewähltes Element wird der Zeichenfolge hinzugefügt, die unter **Suchen nach** angegeben wurde.  
  
## <a name="replace-with"></a>Ersetzen durch  
 Mithilfe dieser Steuerelemente können Sie angeben, was anstelle der übereinstimmenden Zeichenfolge bzw. des übereinstimmenden Ausdrucks eingefügt werden soll.  
  
 **Replace with**  
 Um Instanzen der in **Suchen nach** angegebenen Zeichenfolge mit einer anderen zu ersetzen, geben Sie in diesem Feld die Zeichenfolge ein, die sie ersetzen soll. Um Instanzen des in **Suche nach** angegebenen Texts zu löschen, lassen Sie dieses Feld leer. Wählen Sie die Dropdownliste aus, um die letzten 20 Eingaben anzuzeigen. Um reguläre Ausdrücke in die im Feld **Ersetzen durch** angegebene Zeichenfolge einzubeziehen, aktivieren Sie das Kontrollkästchen **Mit** , und klicken Sie auf die Option **Reguläre Ausdrücke**. Dieses Feld wird nur angezeigt, wenn das Dialogfeld durch Klicken auf **Schnellersetzung** geöffnet wurde.  
  
 **Replace with**  
 Um Instanzen der in **Suchen nach** angegebenen Zeichenfolge mit einer anderen zu ersetzen, geben Sie in diesem Feld die Zeichenfolge ein, die sie ersetzen soll. Um Instanzen der im Feld **Suche nach** angegebenen Zeichenfolge zu löschen, lassen Sie dieses Feld leer. Wählen Sie die Dropdownliste aus, um die letzten 20 Eingaben anzuzeigen. Um reguläre Ausdrücke in die im Feld **Ersetzen durch** angegebene Zeichenfolge einzubeziehen, aktivieren Sie das Kontrollkästchen **Mit** , und klicken Sie auf die Option **Reguläre Ausdrücke**.  
  
 **Ausdrucks-Generator**  
 Die dreieckige Schaltfläche neben dem Feld **Ersetzen durch** steht zur Verfügung, nachdem das Kontrollkästchen **Mit** in den **Suchoptionen** aktiviert wurde. Klicken Sie auf diese Schaltfläche, um in Abhängigkeit von der für **Mit** gewählten Option eine Liste von Platzhaltern oder regulären Ausdrücken anzuzeigen. Ein aus dieser Liste ausgewähltes Element wird der Zeichenfolge hinzugefügt, die unter **Ersetzen durch** angegeben wurde.  
  
 **Replace**  
 Klicken Sie auf diese Schaltfläche, um die aktuelle Instanz der im Feld **Suchen nach** angegebenen Zeichenfolge mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist, und um die nächste Instanz innerhalb des Bereichs zu suchen, der im Feld **Suchen in** angegeben ist.  
  
 **Alle ersetzen**  
 Klicken Sie auf diese Schaltfläche, um alle Instanzen der im Feld **Suchen nach** angegebenen Zeichenfolge in allen Dateien innerhalb des unter **Suchen in** angegebenen Bereichs mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist.  
  
> [!CAUTION]  
>  Stellen Sie sicher, dass der unter **Suchen in** festgelegte Bereich nur die Dateien umfasst, die Sie ändern möchten.  
  
 Es wird eine Erinnerung mit der Option **Geänderte Dateien geöffnet lassen** angezeigt. Um die Option **Rückgängig** beizubehalten, müssen Sie diese Option auswählen. Die Option **Rückgängig** ist nur in Dateien verfügbar, die nach einer Änderung geöffnet bleiben.  
  
 **Datei überspringen**  
 Wird verfügbar, wenn der unter **Suchen in** angegebene Wert mehrere Dateien einschließt. Klicken Sie auf diese Schaltfläche, wenn Sie die aktuelle Datei nicht durchsuchen oder ändern möchten. Die Suche wird dann in der nächsten Datei auf der Liste unter **Suchen in** fortgesetzt.  
  
## <a name="look-in"></a>Suchen in  
 **Look in**  
 Wählen Sie den Speicherort aus, in dem der unter **Suchen nach** angegebene Text gesucht werden soll. Mit der Option **Aktuelles Dokument** wird das Dokumentfenster durchsucht, das beim Öffnen des Dialogfelds fokussiert war, und mit der Option **Alle geöffneten Dokumentfenster** werden alle Dokumentfenster durchsucht, die aktuell in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]geöffnet sind.  
  
## <a name="find-options"></a>Mit  
 Sie können den Abschnitt **Suchoptionen** reduzieren oder erweitern. Die folgenden Optionen können aktiviert oder deaktiviert werden.  
  
 **Groß-/Kleinschreibung beachten**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden im Suchergebnisfenster nur Instanzen der unter **Suchen nach** angegebenen Zeichenfolge angezeigt, bei denen neben der inhaltlichen Übereinstimmung auch die Groß-/Kleinschreibung identisch ist. Eine Suche nach "**MyObject**" bei aktiviertem Kontrollkästchen **Groß-/Kleinschreibung beachten** gibt "MyObject" nicht aber "myobject" oder "MYOBJECT" zurück.  
  
 **Nur ganzes Wort suchen**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden im Suchergebnisfenster nur Instanzen der unter **Suchen nach** angegebenen Zeichenfolge angezeigt, bei denen jeweils das ganze Wort übereinstimmt. Eine Suche nach **MyObject** gibt beispielsweise "MyObject", nicht aber "CMyObject" oder "MyObjectC" zurück.  
  
 **Suchrichtung nach oben**  
 Sucht von der Cursorposition bis zum Anfang des Dokuments.  
  
 **Ausgeblendeten Text durchsuchen**  
 Sucht Instanzen des Texts in Textbereichen, die ausgeblendet oder reduziert sind.  
  
 **Verwenden Sie**  
 Gibt an, wie in den Textfeldern **Suchen nach** oder **Ersetzen durch** eingegebene Sonderzeichen interpretiert werden sollen. Zur Auswahl stehen **Platzhalter** und **Reguläre Ausdrücke**.  
  
 **Regular Expressions**  
 Mithilfe spezieller Notationen werden zu suchende Textmuster definiert. Eine Liste finden Sie unter [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md).  
  
 **Platzhalter**  
 Sonderzeichen, wie Sternchen (`*`) und Fragezeichen (`?`), stellen ein oder mehrere Zeichen dar. Eine Liste finden Sie unter [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md).  
  
 **Weitersuchen**  
 Beginnt die Suche nach dem im Feld **Suchen nach** angegebenen Text.  
  
 **Replace**  
 Klicken Sie auf diese Schaltfläche, um die aktuelle Instanz der in **Suchen nach** angegebenen Zeichenfolge mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist, und um die nächste Instanz innerhalb des Bereichs zu suchen, der im Feld **Suchen in** angegeben ist.  
  
 **Replace All**  
 Klicken Sie auf diese Schaltfläche, um alle Instanzen der in **Suchen nach** angegebenen Zeichenfolge in allen Dateien innerhalb des unter **Suchen in** angegebenen Bereichs mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist.  
  
> [!CAUTION]  
>  Stellen Sie sicher, dass der unter **Suchen in** festgelegte Bereich nur die Dateien umfasst, die Sie ändern möchten.  
  
## <a name="find-and-replace-views"></a>Sichten beim Suchen und Ersetzen  
 Die Registerkarten oben im Fenster Suchen und Ersetzen beinhalten Menüs, die als **Sichten** bezeichnet werden. Mithilfe dieser Menüs können Sie eine Gruppe von Feldern auswählen, die im aktiven Bereich angezeigt werden. Sie können das Fenster **Suchen und Ersetzen** an einem für Sie geeigneten Platz angedockt lassen und dann zwischen den Registerkarten und Sichten wechseln, um einen beliebigen Typ eines Such- oder Ersetzungsvorgangs auszuführen.  
  
 **Schnellsuche**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **Schnellsuche** .  
  
 **In Dateien suchen**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **In Dateien suchen** .  
  
 **Schnellersetzung**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **Schnellersetzung** .  
  
 **In Dateien ersetzen**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **In Dateien ersetzen** .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)