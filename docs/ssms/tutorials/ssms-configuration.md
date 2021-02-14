---
title: Komponenten und Konfiguration von SSMS
description: Ein Tutorial, in dem die Komponenten und grundlegenden Konfigurationsoptionen für Ihre SQL Server Management Studio-Umgebung erläutert werden.
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/16/2018
ms.openlocfilehash: 3c2145db468fb0c6423cbd2ec1bc45d7902b2cdf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081811"
---
# <a name="sql-server-management-studio-components-and-configuration"></a>SQL Server Management Studio-Komponenten und -Konfiguration

In diesem Tutorial werden die verschiedenen Fensterkomponenten in SQL Server Management Studio (SSMS) und einige grundlegende Konfigurationsoptionen für Ihren Arbeitsbereich erläutert. In diesem Artikel werden folgende Vorgehensweisen behandelt: 

> [!div class="checklist"]
> * Wie Sie die Komponenten der SSMS-Umgebung ermitteln
> * Wie Sie das Layout der Umgebung ändern und auf den Standard zurücksetzen
> * Wie Sie den Abfrage-Editor maximieren
> * Ändern der Schriftart
> * Wie Sie die Startoptionen konfigurieren
> * Wie Sie die Konfiguration auf den Standard zurücksetzen

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio.  

* Installieren Sie [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

## <a name="sql-server-management-studio-components"></a>Komponenten von SQL Server Management Studio

In diesem Abschnitt werden die verschiedenen Fensterkomponenten beschrieben, die im Arbeitsbereich verfügbar sind, und wie Sie diese verwenden können.

* Klicken Sie in der Ecke rechts oben auf der Titelleiste auf das **X**, um ein Fenster zu schließen.
* Sie können ein Fenster erneut öffnen, indem Sie es im Menü **Ansicht** auswählen.

    ![Das Menü „Ansicht“](media/ssms-configuration/viewmenu.png)

* **Objekt-Explorer** (F8): Objekt-Explorer enthält eine Strukturansicht aller Datenbankobjekte eines Servers. Diese Ansicht enthält die Datenbanken von SQL Server-Datenbank-Engine, SQL Server Analysis Services, SQL Server Reporting Services und SQL Server Integration Services. Der Objekt-Explorer umfasst Informationen zu allen Servern, mit denen eine Verbindung besteht. 

    ![Objekt-Explorer](media/ssms-configuration/objectexplorer.png)
* **Abfragefenster** (STRG+N): Geben Sie Ihre Transact-SQL-Abfragen (T-SQL) in dem Fenster ein, das geöffnet wird, wenn Sie auf die Schaltfläche **Neue Abfrage** klicken. Die Ergebnisse Ihrer Abfrage werden hier ebenfalls angezeigt.

    ![Fenster „Neue Abfrage“](media/ssms-configuration/newquery.png)

* **Eigenschaften** (F4): Die Ansicht „Eigenschaften“ wird angezeigt, wenn das Abfragefenster geöffnet wurde. Die Ansicht zeigt die grundlegenden Eigenschaften der Abfrage an. Es werden beispielsweise der Startzeitpunkt einer Abfrage, die Anzahl zurückgegebener Zeilen und Verbindungsdetails angezeigt.  

    ![Konfigurationseigenschaften](media/ssms-configuration/properties.png)

* **Vorlagenbrowser** (STRG+ALT+T): Der Vorlagenbrowser enthält verschiedene vorgefertigte T-SQL-Vorlagen. Mithilfe dieser Vorlagen können Sie verschiedene Funktionen ausführen, wie z.B. das Erstellen oder Sichern einer Datenbank. 

    ![Vorlagenbrowser](media/ssms-configuration/templates.png)

* **Details zum Objekt-Explorer** (F7): Diese Ansicht enthält mehr Details als die Ansicht im Objekt-Explorer. In den Details zum Objekt-Explorer können Sie mehrere Objekte gleichzeitig bearbeiten. In diesem Fenster können Sie z.B. mehrere Datenbanken auswählen und diese gleichzeitig löschen oder ausarbeiten. 

    ![Details zum Objekt-Explorer](media/ssms-configuration/objectexplorerdetails.PNG) 

## <a name="change-the-environment-layout"></a>Ändern des Umgebungslayouts 

In diesem Abschnitt wird beschrieben, wie Sie das Umgebungslayout ändern, z.B. durch Verschieben verschiedener Fenster. 

* Sie können Fenster verschieben, indem Sie auf den Titel klicken und ziehen. 
* Sie können auf das Stecknadel-Symbol in der Titelleiste eines Fensters klicken, um es anzuheften oder zu lösen:

    ![Anheften eines Objekts](media/ssms-configuration/pushpin.png)

* Jede Fensterkomponente verfügt über ein Dropdownmenü, das Ihnen verschiedene Möglichkeiten zum Bearbeiten des Fensters bietet: 

    ![Fensteroptionen](media/ssms-configuration/windowoptions.png)

* Wenn mindestens zwei Abfragefenster geöffnet sind, können die Fenster vertikal oder horizontal im Registerkartenformat angedockt werden, sodass beide Abfragefenster sichtbar sind. Klicken Sie zum Anzeigen von Fenstern im Registerkartenformat mit der rechten Maustaste auf den Titel der Abfrage, und wählen Sie die gewünschte Option für das Registerkartenformat aus:

    ![Optionen für das Registerkartenformat der Abfrage](media/ssms-configuration/querytabbedoptions.png)

    * So sieht eine horizontale Registerkartengruppe aus:

      ![Beispiel für eine horizontale Registerkartengruppe](media/ssms-configuration/horizontaltab.png)

    * So sieht eine vertikale Registerkartengruppe aus:

      ![Beispiel für eine vertikale Registerkartengruppe](media/ssms-configuration/verticaltabgroup.png)

    * Klicken Sie mit der rechten Maustaste auf den Abfragetitel, und klicken Sie auf **Move to Previous Tab Group** (Zu vorheriger Registerkartengruppe verschieben) oder **Move to Next Tab Group** (Zu nächster Registerkartengruppe verschieben), um Registerkarten zusammenzuführen:

      ![Registerkarten der Abfrage zusammenführen](media/ssms-configuration/mergetabgroups.png)

* Klicken Sie zum Wiederherstellen des Standardumgebungslayouts im Menü **Fenster** auf **Fensterlayout zurücksetzen**:

    ![Fensterlayout wiederherstellen](media/ssms-configuration/resetwindowlayout.png)

## <a name="maximize-query-editor"></a>Maximieren des Abfrage-Editors

Sie können den Abfrage-Editor in den Vollbildmodus maximieren:

1. Klicken Sie auf eine beliebige Stelle im Abfrage-Editorfenster.

2. Drücken Sie UMSCHALT+ALT+EINGABETASTE, um zwischen dem Vollbildmodus und dem normalen Modus zu wechseln. 

Diese Tastenkombination gilt für jedes Dokumentfenster. 

## <a name="change-basic-settings"></a>Ändern der Standardeinstellungen

In diesem Abschnitt wird beschrieben, wie Sie einige Standardeinstellungen in SSMS im Menü **Tools** bearbeiten.

  ![Extras (Menü)](media/ssms-configuration/tools.png)

* Klicken Sie zum Ändern der hervorgehobenen Symbolleiste auf **Tools** > **Anpassen**:

    ![Anpassen einer Symbolleiste](media/ssms-configuration/toolbar.png)

### <a name="change-the-font"></a>Ändern der Schriftart

* Klicken Sie zum Ändern der Schriftart auf **Tools** > **Optionen** > **Schriftarten und Farben**:

     ![Ändern von Schriftarten und Farben](media/ssms-configuration/fontsandcolors.png)

### <a name="change-startup-options"></a>Ändern der Startoptionen

* Die Startoptionen bestimmen, wie Ihr Arbeitsbereich beim ersten Öffnen von SSMS aussieht. Klicken Sie zum Ändern der Startoptionen auf **Tools** > **Optionen** > **Start**:

    ![Ändern der Startoptionen](media/ssms-configuration/startup.png)

### <a name="reset-settings-to-the-default"></a>Zurücksetzen auf die Standardeinstellungen

* Sie können all diese Einstellungen aus dem Menü exportieren und importieren. Klicken Sie zum Importieren oder Exportieren von Einstellungen oder zum Wiederherstellen der Standardeinstellungen auf **Tools** > **Einstellungen importieren und exportieren**. 

    ![Importieren und Exportieren von Einstellungen](media/ssms-configuration/settings.png)

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Dieses *Tutorial* und Artikel zu *Vorgehensweisen* unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind.  In diesen Artikeln erfahren Sie, wie Sie die Komponenten von SSMS verwalten und wie Sie die Funktionen finden, die Sie regelmäßig verwenden.

* [Herstellen einer Verbindung mit einer Instanz und Abfragen einer Instanz](../quickstarts/ssms-connect-query-sql-server.md)
* [Skripterstellung](scripting-ssms.md)
* [Verwenden von Vorlagen in SSMS](../template/templates-ssms.md)
* [Zusätzliche Tipps und Tricks für die Verwendung von SSMS](ssms-tricks.md)