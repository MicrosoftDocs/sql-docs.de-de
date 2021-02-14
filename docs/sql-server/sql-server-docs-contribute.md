---
description: Mitwirken an der SQL Server-Dokumentation
title: Mitwirken an der SQL Server-Dokumentation | Microsoft-Dokumentation
ms.date: 08/13/2018
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: a0f3f665421c04208af1123e659aadcfe513d407
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080301"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>Mitwirken an der SQL Server-Dokumentation

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

An der SQL Server-Dokumentation kann jeder mitwirken. Beispielsweise können Rechtschreibfehler korrigiert, genauere Erklärungen vorgeschlagen und die technische Richtigkeit geprüft werden. In diesem Artikel werden die ersten Schritte hierfür beschrieben.

Wenn Sie zur Dokumentation beitragen möchten, stehen Ihnen zwei Hauptworkflows zur Verfügung:

|Workflow|BESCHREIBUNG|
|---|---|
| [Bearbeiten von Inhalten im Browser](#githubui) | Geeignet für kleinere Bearbeitungen eines Artikels |
| [Lokales Bearbeiten von Inhalten mit Tools](#tools) | Geeignet für komplexere Bearbeitungen in mehreren Artikeln und bei häufigen Beiträgen für docs.microsoft.com |

Alle öffentlichen Beiträge werden vom SQL-Inhaltsteam auf technische Richtigkeit und Konsistenz geprüft. 

## <a name="edit-in-your-browser"></a><a id="githubui"></a> Bearbeiten von Inhalten im Browser

Sie können im Browser einfache Änderungen an SQL Server-Inhalten vornehmen und diese dann an Microsoft übermitteln. Weitere Informationen finden Sie im [Leitfaden für Mitwirkende der Microsoft-Dokumentation: Übersicht](/contribute/#quick-edits-to-existing-documents). 

Die folgenden Schritte fassen den Prozess zusammen: 

1. Wählen Sie auf der Seite, zu der Sie Feedback übermitteln möchten, oben rechts den Link **Bearbeiten** aus.
1. Wählen Sie auf der nächsten Seite oben rechts das Symbol **Stift** aus.
1. Nehmen Sie auf der nächsten Seite im Textfenster **Datei bearbeiten** Ihre Änderungen direkt an dem Text vor, den Sie ändern möchten.
    Wenn Sie Hilfe beim Formatieren des neuen oder geänderten Texts benötigen, sehen Sie sich den [„Spickzettel“ zum Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) an.
1. Nachdem Sie Ihre Änderungen vorgenommen haben, gehen Sie unter **Änderungen vornehmen** folgendermaßen vor:
    1. Geben Sie im ersten Textfeld eine kurze Beschreibung der Änderung ein, die Sie vorgenommen haben.
    1. Geben Sie im Feld **Eine optionale erweiterte Beschreibung hinzufügen** eine kurze Erläuterung Ihrer Änderung an.
1. Wählen Sie **Dateiänderung vorschlagen** aus.
1. Wählen Sie auf der Seite **Änderungen werden verglichen** die Option **Pull Request erstellen** aus. 
1. Wählen Sie auf der Seite **Pull Request öffnen** die Option **Pull Request erstellen** aus. 

Die folgende GIF-Datei veranschaulicht den gesamten Ablauf zur Übermittlung von Änderungen in Ihrem Browser:

![SQL-Dokumentation bearbeiten](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="edit-locally-with-tools"></a><a id="tools"></a> Lokales Bearbeiten von Inhalten mit Tools

Eine weitere Bearbeitungsoption besteht darin, das **sql-docs**- oder **azure-docs**-Repository zu forken und es lokal auf dem Computer zu klonen. Anschließend können Sie mithilfe eines Markdown-Editors und eines Git-Clients die Änderungen übermitteln. Dieser Workflow eignet sich für Bearbeitungen, die komplexer sind oder mehrere Dateien betreffen. Außerdem ist er gut für Mitwirkende geeignet, die häufig Beiträge bei docs.microsoft.com einreichen.

Weitere Informationen zu dieser Bearbeitungsoption finden Sie in den folgenden Artikeln:

- [Einrichten eines GitHub-Kontos](/contribute/get-started-setup-github)
- [Install content authoring tools (Installieren von Erstellungstools für Inhalte)](/contribute/get-started-setup-tools)
- [Lokales Einrichten von Git für die Dokumentation](/contribute/get-started-setup-local)
- [Verwenden von Tools zum Einreichen von Änderungen](/contribute/how-to-write-workflows-major)

Wenn Sie einen Pull Request mit umfassenden Änderungen an der Dokumentation einreichen, wird in GitHub ein Kommentar angezeigt, in dem Sie aufgefordert werden, online eine **Lizenzvereinbarung für Mitwirkende** zu übermitteln. Sie müssen dieses Onlineformular ausfüllen, damit Ihr Pull Request akzeptiert wird.

## <a name="recognition"></a>Erkennung

Nachdem Ihre Änderungen akzeptiert wurden, werden Sie am Anfang des Artikels als Mitwirkender genannt.

![Nennung als Mitwirkender bei Einreichung von Inhaltsänderungen](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>Übersicht über sql-docs

Dieser Abschnitt enthält zusätzliche Hinweise zur Arbeit mit dem **sql-docs**-Repository.

> [!IMPORTANT]
> Diese beziehen sich ausschließlich auf **sql-docs**. Wenn Sie stattdessen einen SQL-Artikel der Azure-Dokumentation bearbeiten, finden Sie auf [GitHub in der Infodatei des azure-docs-Repositorys](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md) weitere Informationen.

Zur Organisation von Inhalten werden im [sql-docs](https://github.com/MicrosoftDocs/sql-docs)-Repository die folgenden Standardordner verwendet:

| Ordner | BESCHREIBUNG |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | Enthält alle veröffentlichten SQL Server-Inhalte. In Unterordnern werden verschiedene Inhaltsbereiche organisiert. |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | Enthält Includedateien. Diese Dateien bestehen aus Inhaltsblöcken, die in ein oder mehrere Artikel aufgenommen werden können. |
| **./media** | Jeder Ordner kann über einen **media**-Unterordner für Artikelbilder verfügen. Der **media**-Ordner enthält wiederum Unterordner mit denselben Namen der Artikel, in denen die Bilder angezeigt werden. Bei den Bildern sollte es sich um PNG-Dateien handeln, deren Dateinamen aus Kleinbuchstaben bestehen und keine Leerzeichen enthalten. |
| **TOC.MD** | Eine Inhaltsverzeichnisdatei. In jedem Unterordner kann eine TOC.MD-Datei verwendet werden. |

#### <a name="applies-to-includes"></a>applies-to-Includedateien

In jedem SQL Server-Artikel befindet sich nach dem Titel ein Verweis auf die **applies-to**-Includedatei. Damit wird angegeben, auf welche Bereiche oder Versionen von SQL Server sich der Artikel bezieht.

Im folgenden Markdown-Beispiel wird die Includedatei **appliesto-ss-asdb-asdw-pdw-md.md** abgerufen.

```Markdown
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
```

Dadurch wird der folgende Text am Anfang des Artikels angezeigt:

![applies-to-Text](./media/sql-server-docs-contribute/applies-to.png)

Mit den folgenden Tipps finden Sie die richtige applies-to-Includedatei für Ihren Artikel:

- Eine Liste häufig verwendeter Includedateien finden Sie unter [SQL Server-Includedateien für die Versionsverwaltung und „Applies-to“](applies-to-includes.md).
- Suchen Sie nach anderen Artikeln, in denen es um dasselbe Feature oder um eine vergleichbare Aufgabe geht. Wenn Sie diese Artikel bearbeiten, können Sie das Markdown-Element für den Link zur applies-to-Includedatei kopieren. Dabei können Sie die Bearbeitung abbrechen, ohne die Änderungen zu senden.
- Durchsuchen Sie das Verzeichnis [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) nach Dateien, die den Text „applies-to“ enthalten. Über die Schaltfläche **Find** (Suchen) können Sie in GitHub die Ergebnisse schnell filtern. Klicken Sie auf die Datei, um festzustellen, wie diese gerendert wird.
- Beachten Sie die Namenskonvention. Wenn der Name mehrmals den Buchstaben „x“ enthält, ist dieser vermutlich ein Platzhalter, der darauf hinweist, dass ein bestimmter Dienst nicht unterstützt wird. Durch **appliesto-xx-xxxx-asdw-xxx-md.md** wird beispielsweise angegeben, dass ausschließlich Azure Synapse Analytics unterstützt wird, da nur die Zeichenfolge **asdw** vorhanden ist. Die anderen Felder enthalten hingegen nur den Buchstaben „x“.
- In einigen Includedateien werden Versionsnummern wie **tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md** angegeben. Verwenden Sie diese Includedateien nur, wenn Sie wissen, dass dieses Feature mit einer bestimmten Version von SQL Server eingeführt wurde.

## <a name="contributor-resources"></a>Ressourcen für Mitwirkende

- [Leitfaden für Mitwirkende an der Dokumentation auf docs.microsoft.com](/contribute/)
- [Microsoft-Styleguide](/teamblog/style-guide)
- [Markdown-Grundlagen](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)

> [!TIP]
> Wenn Sie nicht Feedback zur Dokumentation, sondern zu einem SQL Server-Produkt geben möchten, können Sie die [hierfür eingerichtete Seite](https://feedback.azure.com/forums/908035-sql-server) nutzen.

## <a name="next-steps"></a>Nächste Schritte

Auf GitHub können Sie einen genaueren Blick auf das [sql-docs-Repository](https://github.com/MicrosoftDocs/sql-docs) werfen.

Außerdem haben Sie die Möglichkeit, Artikel zu suchen, Änderungen einzureichen und die SQL Server-Community zu unterstützen. 

Vielen Dank!