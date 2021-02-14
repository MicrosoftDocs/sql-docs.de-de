---
description: Neue GUI-Features in SSMA für MySQL (MySqlToSql)
title: Neue GUI-Features in SSMA für MySQL (mysqlto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 0e59e2dc-1e4a-47c0-a5c3-ae7b5f5e469c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: dea94421878b6ae3e81c3c5b81825f6e47c5a711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069955"
---
# <a name="new-gui-features-in-ssma-for-mysql-mysqltosql"></a>Neue GUI-Features in SSMA für MySQL (MySqlToSql)
In diesem Kapitel werden neue Funktionen der SSMA-Benutzeroberfläche beschrieben.  
  
## <a name="layouts"></a>Layouts  
Diese Funktion ermöglicht es Ihnen, eine der beiden vordefinierten Windows-Layouts auszuwählen oder Ihr eigenes Layout zu erstellen. Um auf das layoutuntermenü zuzugreifen, zeigen Sie im Menü Ansicht auf Layouts. Dort können Sie eines der vorhandenen Layouts auswählen, das aktuelle Layout hinzufügen oder Layouts verwalten.  
  
### <a name="add-current-layout"></a>Aktuelles Layout hinzufügen  
Zum Speichern des aktuellen Windows-Layouts zeigen Sie im Menü Ansicht auf Layouts, und klicken Sie dann auf Aktuelles Layout hinzufügen.  
  
### <a name="choose-predefined-layout"></a>Vordefiniertes Layout auswählen  
Um eines der vordefinierten Layouts auszuwählen, zeigen Sie im Menü Ansicht auf Layouts, und klicken Sie dann auf Standard Layout oder ohne Explorer. Sie können auch die Tastenkombination STRG + ALT + 1 oder STRG + ALT + 2 für vordefinierte Layouts verwenden.  
  
### <a name="choose-user-defined-layout"></a>Benutzerdefiniertes Layout auswählen  
Um benutzerdefiniertes Layout auszuwählen, zeigen Sie im Menü Ansicht auf Layouts, und klicken Sie dann auf eines der benutzerdefinierten Layouts. Sie können auch Verknüpfungen verwenden, die für die Layouts definiert sind.  
  
### <a name="manage-layouts"></a>Layouts verwalten  
Zeigen Sie zum Öffnen des Dialog Felds Layouts verwalten im Menü Ansicht auf Layouts, und klicken Sie dann auf Layouts verwalten. Im Dialogfeld "Layouts verwalten" finden Sie eine Liste der vorhandenen Layouts auf der linken Seite des Dialog Felds. Dort können Sie das Layout auswählen, um die Einstellungen zu ändern. Außerdem können Sie die Reihenfolge der Layouts in der Liste ändern oder das Layout mithilfe der Schaltflächen oben in der Liste löschen. Auf der rechten Seite des Dialog Felds können Sie die folgenden Layouteinstellungen ändern:  
  
-   Layoutname  
  
-   Synchronisierung von Metadaten-Explorers  
  
-   Sichtbarkeit und Breite des Quell-und zielmetadatenexplorers  
  
-   Sichtbarkeit der Quell-oder Zielfenster und ihrer Größen  
  
-   Sichtbarkeit und Höhe von hilffenstern  
  
## <a name="bookmarks"></a>Lesezeichen  
Diese Funktion ermöglicht es Ihnen, ein oder mehrere Lesezeichen im Quell-oder Zielcode festzulegen, mithilfe von Verknüpfungen schnell ein Lesezeichen zu finden und die Lesezeichen mit einem benutzerfreundlichen Dialogfeld zu verwalten.  
  
### <a name="toggle-bookmark"></a>Lesezeichen umschalten  
Sie können ein Lesezeichen wie folgt festlegen/entfernen:  
  
-   Lesezeichen mit Schaltfläche im oberen Bereich des Quell-oder Ziel-SQL-Fensters verwenden  
  
-   Klicken Sie auf den grauen Bereich auf der linken Seite des SQL-Fensters.  
  
-   Verwenden Sie STRG + UMSCHALT + &lt; 0.. 9 &gt; zum Festlegen des nummerierten Lesezeichens.  
  
### <a name="bookmark-navigation"></a>Lesezeichen Navigation  
Die Lesezeichen können wie folgt durchlaufen werden:  
  
-   Schaltflächen "Nächstes Lesezeichen", Vorheriges Lesezeichen oben im SQL-Fenster verwenden  
  
-   Verwenden von STRG + &lt; 0.. 9 &gt; zum Suchen des nummerierten Lesezeichens  
  
-   Verwenden von Schaltflächen gehe zu oder Quelle anzeigen im Dialogfeld Lesezeichen verwalten  
  
### <a name="removing-bookmark"></a>Entfernen von Lesezeichen  
Sie können ein Lesezeichen wie folgt entfernen:  
  
-   Löschen Sie alle Lesezeichen im aktuellen Dokument mithilfe der Schaltfläche Löschen am oberen Rand des SQL-Fensters.  
  
-   Schaltflächen entfernen oder alle entfernen im Dialogfeld "Lesezeichen verwalten"  
  
### <a name="manage-bookmarks"></a>Verwalten von Textmarken  
Um das Dialogfeld Lesezeichen verwalten zu öffnen, klicken Sie im Menü Bearbeiten auf Lesezeichen verwalten. Im Dialogfeld wird eine Liste vorhandener Lesezeichen angezeigt. Sie können die Schaltflächen auf der rechten Seite des Dialog Felds verwenden, um die Lesezeichen zu verwalten.  
  
## <a name="object-history"></a>Objekt Verlauf  
Der GUI-Objekt Verlauf bietet Ihnen die folgenden Vorteile beim Navigieren in Objekten:  
  
-   Sie können die Schaltflächen "zurück" und "Vorwärts" verwenden, um die bereits besuchten Objekte zu navigieren.  
  
-   Wenn Sie zurück zum Objekt sind, kehren Sie zurück zur gleichen Registerkarte, die Sie verlassen haben.  
  
-   Wenn Sie zurück zum Objekt sind und die Registerkarte SQL ist, kehren Sie zurück zur gleichen Cursorposition, die Sie verlassen haben.  
  
## <a name="advanced-search-capabilities"></a>Erweiterte Suchfunktionen  
Erweiterte Suchfunktionen stellen die leistungsstarken und flexiblen Suchfunktionen bereit und ermöglichen Ihnen das Auffinden von Objekt Deklaration, das Abrufen von Objektinformationen, das Durchführen einer Schnellsuche, das Ausführen erweiterter Objekt Suchvorgänge in Kategorien mit Mustern  
  
### <a name="get-quick-information"></a>Schnelle Informationen  
Sie können schnell Informationen zum Objekt an der Cursorposition auf folgende Weise erhalten:  
  
-   Klicken Sie oben im SQL-Fenster auf die QuickInfo-Schaltfläche.  
  
-   Klicken Sie im Kontextmenü mit der rechten Maustaste auf Quick Info.  
  
-   Drücken Sie STRG + UMSCHALT + LEERTASTE.  
  
### <a name="find-declaration"></a>Deklaration suchen  
Sie können auf folgende Weise zur Deklaration des Objekts an der Cursorposition wechseln:  
  
-   Klicken Sie oben im SQL-Fenster auf die Schaltfläche Gehe zu Deklaration.  
  
-   Klicken Sie im Kontextmenü mit der rechten Maustaste auf Gehe zu Deklaration.  
  
-   Drücken Sie F12.  
  
### <a name="quick-search"></a>Schnellsuche  
Sie können die Schnellsuche mithilfe der folgenden Funktionen ausführen:  
  
-   Sie können die Suche mit der Tastenkombination STRG + F starten.  
  
-   Sie können den letzten Suchvorgang mit F3 wiederholen.  
  
-   Sie können die letzte Suche rückwärts mit UMSCHALT + F3 wiederholen.  
  
-   Sie können das nächste Vorkommen des Worts an der Cursorposition mithilfe von Strg + F3 suchen.  
  
-   Sie können das vorherige Vorkommen des Worts an der Cursorposition mithilfe von STRG + UMSCHALT + F3 suchen.  
  
-   Diese Aktionen können auch mit Menü Elementen durchgeführt werden.  
  
### <a name="advanced-search"></a>Erweiterte Suche  
Um das Dialogfeld "Erweiterte Suche" zu öffnen, klicken Sie auf der Menüleiste "Bearbeiten" auf Erweiterte Suche. Im Dialogfeld können Sie ein beliebiges Objekt mithilfe eines Musters suchen. Am oberen Rand des Dialog Felds können Sie Suchbereich und Objektkategorien auswählen.  
  
