---
title: Verwalten der Codeformatierung
description: Hier erfahren Sie mehr über die drei Arten des Texteinzugs und deren Verwendung sowie das Erstellen klickbarer URLs.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- indenting code [SQL Server]
- displaying URLs
- code formatting [SQL Server Management Studio]
- collapsing text
- formats [SQL Server], code formatting in SQL Server Management Studio
- hiding text
- formats [SQL Server]
- text [SQL Server], code formats
- automatic indentation
- converting text to lower case
- Query Editor [SQL Server Management Studio], managing code formats
- URL displayed in code [SQL Server Management Studio]
- converting text to upper case
- text [SQL Server]
- unindenting code
ms.assetid: ddbac4d2-6bdc-4467-a352-e869ec880eed
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 550f34f022262b24cd7992c7b3ebe056b946f4cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354622"
---
# <a name="manage-code-formatting"></a>Verwalten der Codeformatierung

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Mit dem Editor können Sie Code formatieren, z. B. mit Einzug, ausgeblendetem Text, URLs usw. Außerdem können Sie mit dem intelligenten Einzug den Code automatisch bei der Eingabe formatieren.  
  
## <a name="indenting"></a>Einzug  
 Für den Texteinzug stehen drei verschiedene Stile zur Auswahl. Außerdem können Sie angeben, aus wie vielen Leerzeichen ein Einzug oder Tabstopp besteht und ob der Editor für den Einzug Tabulatoren oder Leerzeichen verwenden soll.  
  
#### <a name="to-choose-an-indenting-style"></a>So wählen Sie einen Einzugsstil aus  
  
1.  Klicken Sie im Menü **Extras** auf **Optionen**.  
  
2.  Klicken Sie auf **Text-Editor**.  
  
3.  Klicken Sie auf den Ordner, und wählen Sie die Option **Alle Sprachen** aus, um den Einzug für alle Sprachen festzulegen.  
  
4.  Klicken Sie auf **Tabstopps**.  
  
5.  Klicken Sie auf eine der folgenden Optionen:  
  
    -   **Keine**. Der Cursor wechselt zum Anfang der nächsten Zeile.  
  
    -   **Blockierung**: Der Cursor richtet die nächste Zeile an der vorherigen Zeile aus.  
  
    -   **Intelligent** (Standard). Der Sprachdienst bestimmt den passenden Einzugsstil.  
  
    > [!NOTE]  
    >  Für einige Sprachen sind nicht alle drei Einzugsoptionen verfügbar.  
  
#### <a name="to-change-indent-tab-settings"></a>So ändern Sie die Einstellungen für Tabulatoreinzug  
  
1.  Klicken Sie im Menü **Extras** auf **Optionen**.  
  
2.  Klicken Sie auf **Text-Editor**.  
  
3.  Wählen Sie den Ordner für **Alle Sprachen** aus, um den Einzug für alle Sprachen festzulegen.  
  
4.  Klicken Sie auf **Tabstopps**.  
  
5.  Zum Angeben von Tabulatorzeichen beim Verwenden von Tabstopps und Einzügen klicken Sie auf **Tabulatoren beibehalten**. Zum Angeben von Leerzeichen wählen Sie die Option **Leerzeichen einfügen** aus.  
  
     Wenn Sie **Leerzeichen einfügen** auswählen, geben Sie die Anzahl der Leerzeichen für die einzelnen Tabstopps oder Einzüge unter **Tabulatorgröße** bzw. **Einzugsgröße** an.  
  
#### <a name="to-indent-code"></a>So ziehen Sie Code ein  
  
1.  Wählen Sie den Text aus, den Sie einziehen möchten.  
  
2.  Drücken Sie die TAB-TASTE, oder klicken Sie auf der Standardsymbolleiste auf die Schaltfläche **Einzug** .  
  
#### <a name="to-unindent-code"></a>So heben Sie den Einzug für Code auf  
  
1.  Wählen Sie den Text aus, für den Sie den Einzug aufheben möchten.  
  
2.  Drücken Sie UMSCHALT+TAB, oder klicken Sie auf der Standardsymbolleiste auf die Schaltfläche **Einzug aufheben** .  
  
#### <a name="to-automatically-indent-all-of-your-code"></a>So ziehen Sie den gesamten Code automatisch ein  
  
1.  Klicken Sie im Menü **Extras** auf **Optionen**.  
  
2.  Klicken Sie auf **Text-Editor**.  
  
3.  Klicken Sie auf **Alle Sprachen**.  
  
4.  Klicken Sie auf **Tabstopps**.  
  
5.  Klicken Sie auf **Intelligent**.  
  
> [!NOTE]  
>  Die Option **Intelligent** ist für einige Sprachen nicht verfügbar.  
  
#### <a name="to-convert-white-space-to-tabs"></a>So konvertieren Sie Leerzeichen in Tabstopps  
  
1.  Wählen Sie den Text aus, dessen Leerzeichen in Tabstopps konvertiert werden sollen.  
  
2.  Zeigen Sie im Menü **Bearbeiten** auf **Erweitert**, und klicken Sie dann auf **Auswahl mit Tabstopps versehen**.  
  
#### <a name="to-convert-tabs-to-spaces"></a>So konvertieren Sie Tabstopps in Leerzeichen  
  
1.  Wählen Sie den Text aus, dessen Tabstopps in Leerzeichen konvertiert werden sollen.  
  
2.  Zeigen Sie im Menü **Bearbeiten** auf **Erweitert**, und klicken Sie dann auf **Tabstopps aus Auswahl entfernen**.  
  
 Das Verhalten dieser Befehle hängt von den Tabulatoreinstellungen im Dialogfeld **Optionen** ab. Wenn die Tabulatoreinstellung z. B. 4 lautet, wird durch den Befehl **Auswahl mit Tabstopps versehen** für jeweils vier aufeinander folgende Leerzeichen ein Tabstopp erstellt. Entsprechend werden durch den Befehl **Tabstopps aus Auswahl entfernen** für jeden Tabstopp vier Leerzeichen erstellt.  
  
## <a name="converting-text-to-upper-and-lower-case"></a>Konvertieren von Text in Groß- und Kleinschreibung  
 Sie können Text mit den entsprechenden Befehlen vollständig in Groß- oder Kleinbuchstaben konvertieren.  
  
#### <a name="to-switch-text-to-upper-or-lower-case"></a>So wandeln Sie Text in Groß- oder Kleinbuchstaben um  
  
1.  Wählen Sie den Text aus, den Sie konvertieren möchten.  
  
2.  Zum Konvertieren von Text in Großbuchstaben drücken Sie STRG+UMSCHALT+U, oder klicken Sie im Menü **Bearbeiten** im Untermenü **Erweitert** auf **In Großbuchstaben umwandeln** .  
  
3.  Zum Konvertieren von Text in Kleinbuchstaben drücken Sie STRG+UMSCHALT+L, oder klicken Sie im Menü **Bearbeiten** im Untermenü **Erweitert** auf **In Kleinbuchstaben umwandeln** .  
  
> [!NOTE]  
>  Eine vollständige Liste der Tastenkombinationen finden Sie unter [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="displaying-and-linking-to-urls"></a>Anzeigen von und Verknüpfen mit URLs  
 Sie können im Code klickbare URLs erstellen und anzeigen. Standardmäßig haben die URLs folgende Eigenschaften:  
  
-   Sie sind unterstrichen.  
  
-   Der Mauszeiger nimmt die Form einer Hand an, wenn Sie ihn über eine URL bewegen.  
  
-   Wenn die URL gültig ist, wird sie geöffnet, wenn Sie darauf klicken.  
  
#### <a name="to-display-a-clickable-url"></a>So zeigen Sie eine klickbare URL an  
  
1.  Klicken Sie im Menü **Extras** auf **Optionen**.  
  
2.  Klicken Sie auf **Text-Editor**.  
  
3.  Zum Ändern der Option für nur eine Sprache klicken Sie auf den entsprechenden Sprachordner, und klicken Sie dann auf **Allgemein**. Zum Ändern der Option für alle Sprachen klicken Sie auf **Alle Sprachen** , und klicken Sie dann auf **Allgemein**.  
  
4.  Wählen Sie die Option **Einfaches Klicken für URLs aktivieren** aus.  
  
  
