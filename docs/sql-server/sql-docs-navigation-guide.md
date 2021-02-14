---
title: Navigationstipps zur SQL Server-Dokumentation
description: 'Tipps und Tricks für die Navigation durch die technische Dokumentation von SQL Server: Erläuterungen z. B. zur Hubseite, zum Inhaltsverzeichnis, zur Überschrift sowie zur Verwendung der Brotkrümelnavigation und zur Verwendung des Versionsfilters.'
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: fc749f636a4d1b7b2e737e54d9ecd9418321c548
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082554"
---
# <a name="sql-server-docs-navigation-guide"></a>Navigationsleitfaden zur SQL Server-Dokumentation

Dieses Thema enthält einige Tipps und Tricks zum Navigieren im technischen Bereich der SQL Server-Dokumentation.  

## <a name="hub-page"></a>Hubseite

Die Hubseite von SQL Server finden Sie unter [https://aka.ms/sqldocs](../index.yml?WT.mc_id=akams). Sie ist der Einstiegspunkt für die Suche nach relevanten SQL Server-Inhalten.

Sie können jederzeit zurück zu dieser Seite navigieren, indem Sie **SQL-Dokumentation** in der Überschrift am oberen Rand jeder Seite in der technischen SQL Server-Dokumentation auswählen: 

![SQL-Dokumentation in der Überschrift](media/sql-server-docs-navigation-guide/sql-docs-in-header.png)

## <a name="offline-documentation"></a>Offlinedokumentation

Wenn Sie die SQL Server-Dokumentation auf einem Offlinesystem anzeigen möchten, haben Sie zwei Möglichkeiten. Sie können entweder eine PDF-Datei erstellen, wo immer Sie sich in der technischen Dokumentation zu SQL Server befinden, oder Sie können die Offlineinhalte mit dem [SQL Server Offline Help Viewer](./sql-server-offline-documentation.md) herunterladen. 

Wenn Sie eine PDF-Datei erstellen möchten, wählen Sie den Link **PDF herunterladen** aus, der sich unten in jedem Inhaltsverzeichnis befindet.


![PDF herunterladen](media/sql-server-docs-navigation-guide/download-pdf.png)

## <a name="toc-symbols"></a>Symbole im Inhaltsverzeichnis 

Inhaltsverzeichniseinträge mit einem `>` am Ende weisen darauf hin, dass Sie zur technischer Dokumentation mit einem anderen Inhaltsverzeichnis geleitet werden. 

![Einzelnes Größer-als-Zeichen im Inhaltsverzeichnis](media/sql-server-docs-navigation-guide/single-carrots-in-sql-docs-toc.png)

Inhaltsverzeichniseinträge mit einem `>>` am Ende weisen darauf hin, dass Sie docs.microsoft.com verlassen. 

![Navigationsmarker im Inhaltsverzeichnis](media/sql-server-docs-navigation-guide/double-carrots-in-sql-docs-toc.png)

Wenn Sie zu einer dieser Seiten navigieren, können Sie zur technischen Hauptseite von SQL Server und zum Inhaltsverzeichnis zurückkehren, indem Sie den Eintrag „Willkommen bei SQL Server >“ auswählen, der sich oben in jedem dieser Inhaltsverzeichnisse befindet. 

![Navigieren zurück zum SQL-Inhaltsverzeichnis](media/sql-server-docs-navigation-guide/navigate-back-to-sql-toc.png)

## <a name="toc-search"></a>Suchen im Inhaltsverzeichnis 
Auf docs.microsoft.com können Sie den Inhalt des Inhaltsverzeichnisses über das Filtersuchfeld oben durchsuchen: 

![Filterfeld verwenden](media/sql-server-docs-navigation-guide/sql-docs-toc-filter.gif)

## <a name="version-filter"></a>Versionsfilter
Die technische SQL Server-Dokumentation enthält Inhalte für mehrere unterstützte Versionen und Varianten von SQL Server. Die Funktionen können sich zwischen Versionen und Varianten von SQL Server unterscheiden. Daher kann der Inhalt manchmal unterschiedlich sein. 

Sie können den [Versionsfilter](versioning-system-monikers-ui-sql-server.md) verwenden, um sicherzustellen, dass der Inhalt für die entsprechende Version und Variante von SQL Server angezeigt wird: 

![Versionsfilter der SQL-Dokumentation](media/sql-server-docs-navigation-guide/sql-docs-version-filter.gif)

Wenn Sie **All SQL** \> **Hide nothing** (Alle SQL-Produkte > Nichts ausblenden) auswählen, wird sichergestellt, dass alle Inhalte sichtbar sind und nichts durch den Versionsfilter verborgen wird. Über die Option **Nichts ausblenden** können für verschiedene Versionen von SQL Server relevante Inhalte innerhalb desselben Artikels angezeigt werden. Dies kann widersprüchlich oder verwirrend sein. Aus diesem Grund wird die Option [**Nichts ausblenden** für die routinemäßige Verwendung nicht empfohlen](versioning-system-monikers-ui-sql-server.md#anchor-allsql-hidenothing). 

## <a name="breadcrumbs"></a>Brotkrümelnavigation

Brotkrümel finden Sie unterhalb der Überschrift und über dem Inhaltsverzeichnis. Sie geben an, wo sich der aktuelle Artikel im Inhaltsverzeichnis befindet.  Dies hilft nicht nur, den Kontext darauf festzulegen, welche Art von Inhalt Sie lesen, sondern ermöglicht es Ihnen auch, in der Inhaltsverzeichnisstruktur zurück nach oben zu navigieren:

![Brotkrümelnavigation in der SQL-Dokumentation](media/sql-server-docs-navigation-guide/sql-docs-bread-crumbs.gif)

## <a name="article-section-navigation"></a>Navigation in Artikelabschnitten

Im rechten Navigationsbereich können Sie schnell zu Abschnitten in einem Artikel navigieren sowie Ihre Position im Artikel identifizieren.  

![Rechter Navigationsbereich](media/sql-server-docs-navigation-guide/sql-docs-right-hand-navigation.gif)


## <a name="submit-docs-feedback"></a>Übermitteln von Feedback zur Dokumentation

Wenn Sie einen Fehler in einem Artikel feststellen, können Sie Feedback an das SQL Content-Team für diesen Artikel übermitteln, indem Sie zum unteren Rand der Seite scrollen und dann **Inhaltsfeedback** auswählen.

![Inhaltsfeedback per „Git-Problem“](media/sql-server-get-help/git-issues.png)

Sie können auch allgemeines Feedback zur Dokumentation und Vorschläge unter [https://aka.ms/sqldocsfeedback](https://aka.ms/sqldocsfeedback)einreichen. 

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![SQL-Dokumentation bearbeiten](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="next-steps"></a>Nächste Schritte

- Erste Schritte mit der [technischen Dokumentation zu SQL Server](index.yml).
- Weitere Informationen zum Übermitteln von Feedback oder zum Erhalten von Hilfe zu SQL Server finden Sie auf der Seite [Hilfe erhalten](sql-server-get-help.md). 
- Über die Seite [Lernmaterialien für SQL](../sql-server/educational-sql-resources.yml) haben Sie schnellen Zugriff auf alle Schnellstarts und Tutorials.