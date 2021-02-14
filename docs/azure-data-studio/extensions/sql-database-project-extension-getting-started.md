---
title: Erste Schritte mit der Erweiterung „SQL Database Projects“
description: Erste Schritte mit der Erweiterung „SQL Database Projects“ für Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 07/30/2020
ms.openlocfilehash: 397bdebe2bdea292009e0216c6b3004ac9475b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040314"
---
# <a name="getting-started-with-the-sql-database-projects-extension-preview"></a>Erste Schritte mit der Erweiterung „SQL Database Projects“ (Vorschau)

In diesem Artikel werden drei Möglichkeiten für die ersten Schritte mit der Erweiterung „SQL Database Projects“ beschrieben:

1. Erstellen Sie ein neues Datenbankprojekt, indem Sie unter „Explorer“ zum Viewlet **Projects** (Projekte) navigieren oder in der Befehlspalette nach **New Database Project** (Neues Datenbankprojekt) suchen.
2. Vorhandene Datenbankprojekte können in der Befehlspalette über **Open Database Project** (Datenbankprojekt öffnen) geöffnet werden.
3. Starten Sie mit einer vorhandenen Datenbank, indem Sie in der Befehlspalette auf **Import New Database Project** (Neues Datenbankprojekt importieren) klicken.

    ![Neues Viewlet](media/sql-database-projects-extension/projects-viewlet.png)

## <a name="create-an-empty-database-project"></a>Erstellen eines leeren Datenbankprojekts

Klicken Sie unter **Explorer** im Viewlet **Projekte** auf die Schaltfläche **Neues Projekt**, und geben Sie in der angezeigten Texteingabe einen Projektnamen ein.  Wählen Sie im angezeigten Dialogfeld „Select a Folder“ (Ordner auswählen) ein Verzeichnis für den Ordner, die .sqlproj-Datei und andere Inhalte des Projekts aus.
Das leere Projekt wird geöffnet und im Viewlet **Projects** (Projekte) für die Bearbeitung angezeigt.

## <a name="open-an-existing-project"></a>Öffnen eines vorhandenen Projekts

Klicken Sie im Viewlet **Projekte** auf die Schaltfläche **Open Project** (Projekt öffnen), und öffnen Sie über die angezeigte Dateiauswahl eine vorhandene *SQLPROJ*-Datei. Vorhandene Projekte können aus Azure Data Studio oder [Visual Studio SQL Server Data Tools](../../ssdt/sql-server-data-tools.md) stammen.

Das vorhandene Projekt wird geöffnet und seine Inhalte werden im Viewlet **Projects** (Projekte) für die Bearbeitung angezeigt.

## <a name="create-a-database-project-from-an-existing-database"></a>Erstellen eines Datenbankprojekts aus einer vorhandenen Datenbank

Klicken Sie im Viewlet **Projekte** auf die Schaltfläche **Import Project from Database** (Projekt aus Datenbank importieren), und stellen Sie eine Verbindung mit einer SQL Server-Instanz her.  Wählen Sie, sobald die Verbindung hergestellt wurde, eine Datenbank aus der Liste der verfügbaren Datenbanken aus, und legen Sie den Namen des Projekts fest.

Wählen Sie abschließend eine Zielstruktur für die Extraktion aus.  Das neue Projekt wird geöffnet und enthält SQL-Skripts für die Inhalte der ausgewählten Datenbank.

## <a name="build-and-publish"></a>Erstellen und Veröffentlichen

In der Erweiterung „SQL Database Projects“ für Azure Data Studio stellen Sie das Datenbankprojekt bereit, indem Sie es als [Datenschichtanwendungs-Datei](../../relational-databases/data-tier-applications/data-tier-applications.md) (DACPAC) erstellen und auf einer unterstützten Plattform veröffentlichen. Weitere Informationen zu diesem Prozess finden Sie unter [Erstellen und Veröffentlichen eines Projekts](sql-database-project-extension-build.md).

## <a name="schema-compare"></a>Schemavergleich

Die Erweiterung „SQL Database Projects“ interagiert mit der [Erweiterung „Schemavergleich“](schema-compare-extension.md), sofern installiert, um die Inhalte eines Projekts mit einem DAC-Paket oder einer vorhandenen Datenbank zu vergleichen.  Der resultierende Schemavergleich kann verwendet werden, um die Unterschiede zwischen Quelle und Ziel anzuzeigen und zu übernehmen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Veröffentlichen Sie ein Projekt mit der Erweiterung „SQL Database Projects“ für Azure Data Studio.](sql-database-project-extension-build.md)