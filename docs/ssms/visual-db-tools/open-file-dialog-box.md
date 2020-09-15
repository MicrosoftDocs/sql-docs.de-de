---
description: Datei öffnen (Dialogfeld)
title: Datei öffnen (Dialogfeld)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.openfile
ms.assetid: 3e01b9f5-2b0a-4fb3-9da8-984d27d17b8a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: f6d9c0a9d83f0004c534f3548cb114f06de2861f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423044"
---
# <a name="open-file-dialog-box"></a>Datei öffnen (Dialogfeld)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Verwenden Sie das Dialogfeld **Datei öffnen**, um eine vorhandene Datei von einem Datenträger zu öffnen. Außerdem können Sie dieses Dialogfeld dazu verwenden, eine bereits geöffnete Datei mit unterschiedlichen Sprachcodierungsoptionen zu öffnen.  
  
Um auf dieses Dialogfeld zuzugreifen, wählen Sie im Menü **Datei** die Option **Öffnen** und dann **Datei**aus. Dieses Dialogfeld wird auch angezeigt, wenn Sie Dateien von anderen Elementen aus öffnen, z. B. dem Dialogfeld **Externe Tools** . Um das ähnliche Dialogfeld **Projekt öffnen** zu öffnen, wählen Sie im Menü **Datei**den Befehl **Öffnen** und dann **Projekt/Projektmappe** aus.  
  
> [!NOTE]  
> Überprüfen Sie vor dem Öffnen eines Projekts oder einer Komponente in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]die Vertrauenswürdigkeit des zugehörigen Codes. Durch das Öffnen des Projekts oder der Komponente in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] wird möglicherweise der Code des Projekts bzw. der Komponente in einem vertrauenswürdigen Prozess auf dem lokalen Computer ausgeführt.  
  
## <a name="option"></a>Option  
**Look in**  
Suchen Sie den vorhandenen Projektordner aus dieser Dropdownliste aus. Nach der Auswahl eines Ordners aus dieser Liste wird der Inhalt im primären Bereich angezeigt.  
  
## <a name="my-places-bar"></a>Meine Umgebung (Leiste)  
**Desktop**  
Zeigt die Dateien und Ordner auf dem Desktop an.  
  
**Meine Projekte**  
Zeigt die Dateien und Ordner im Ordner **Projekte** des Benutzers an.  
  
**Arbeitsplatz**  
Zeigt den Inhalt des Datenträgers, der Festplatte und des CD-ROM-Laufwerks an.  
  
## <a name="folder-list"></a>Ordnerliste  
**Dateiname**  
Mithilfe dieser Option können Sie die anzuzeigenden Dateien oder Ordner filtern. Geben Sie einen vollständigen oder teilweisen Namen ein, für den der Filtervorgang durchgeführt werden soll. Sie können das Sternchen (*) als Platzhalter verwenden.  
  
**Dateityp**  
Mithilfe dieser Option können Sie die Inhalte des in Suchen im ausgewählten Ordner oder Verzeichnis nach einem bestimmten Dateityp filtern.  
  
**Open With and Encoding Options**  
Wenn Sie mithilfe des Dialogfelds **Öffnen mit** einen Editor für die Zieldatei angeben möchten, wählen Sie das kleine Rechteck rechts neben der Schaltfläche **Öffnen** aus, und wählen Sie dann **Öffnen mit**aus. Bei Bedarf können Sie auch ein Sprachcodierungsschema angeben, das beim Öffnen der ausgewählten Datei angewendet werden soll. Dazu wählen Sie in der Liste ein Programm aus, das "**mit Codierung**" enthält, und wählen Sie anschließend **Öffnen** aus, um das Dialogfeld **Codierung**anzuzeigen. Diese Schaltfläche ist nicht immer verfügbar.  
  
## <a name="toolbar"></a>Symbolleiste  
**Rückwärts navigieren**  
Gibt den zuletzt angezeigten Ordner oder Internetstandort bzw. das zuletzt angezeigte Laufwerk zurück.  
  
**Eine Ebene nach oben**  
Navigiert zum nächst höheren Ordner in der Strukturansicht.  
  
**Im Web suchen**  
Diese Schaltfläche ist nicht verfügbar.  
  
**Löschen**  
Löscht die ausgewählten Dateien oder Ordner aus dem Speicher.  
  
**Neuer Ordner**  
Zeigt das Dialogfeld **Neuer Ordner** an. Verwenden Sie diese Option, um unterhalb des Ordners, der im Dropdown-Listenfeld **Suchen in** ausgewählt wurde, einen untergeordneten Ordner zu erstellen.  
  
## <a name="views"></a>Ansichten  
Stellt Optionen bereit, mit denen der Inhalt des im Dropdown-Listenfeld **Sichten** ausgewählten Elements angeordnet und angezeigt werden kann.  
  
**Miniaturansicht**  
Zeigt Miniaturansichten für Elemente im Anzeigebereich an.  
  
**Kacheln**  
Zeigt Dateien und Ordner als große Symbole an.  
  
**Symbole**  
Zeigt Dateien und Ordner als kleine Symbole an.  
  
**Liste**  
Zeigt Dateien und Ordner in einem Listenformat an.  
  
**Details**  
Zeigt den Namen, die Größe, den Typ und das letzte Änderungsdatum von Dateien und Ordnern in einem Listenformat an. Zum Sortieren nach einem bestimmten Detail klicken Sie auf den Spaltenheader des Details.  
  
**WebView**  
Dieser Befehl ist nicht verfügbar.  
  
## <a name="tools"></a>Tools  
Wählen Sie ein Tool aus, das auf das ausgewählte Element im Inhaltsbereich angewendet werden soll.  
  
**Löschen**  
Löscht die ausgewählte Datei bzw. den ausgewählten Ordner aus dem Speicher.  
  
**Netzlaufwerk verbinden**  
Öffnet das Dialogfeld **Netzlaufwerk verbinden** .  
