---
title: Die DACPAC-Erweiterung für SQL Server
description: Erfahren Sie, wie Sie den Datenschichtanwendungs-Assistenten installieren und starten, der das Bereitstellen und Extrahieren von DACPAC-Dateien und das Importieren und Exportieren von BACPAC-Dateien vereinfacht.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: e0cfe057767488eb0a7ddb741934a10410898ffa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040260"
---
# <a name="sql-server-dacpac-extension"></a>Die DACPAC-Erweiterung für SQL Server

**Der Datenschichtanwendungs-Assistent** macht die Bereitstellung und Extrahierung von DACPAC-Dateien und das Importieren und Exportieren von BACPAC-Dateien einfach.

## <a name="features"></a>Features

* Bereitstellen eines DACPACs in einer SQL Server-Instanz
* Extrahieren einer SQL Server-Instanz in ein DACPAC
* Erstellen einer Datenbank aus einem BACPAC
* Exportieren von Schemas und Daten in ein BACPAC

![GIF einer DACPAC-Erweiterungsdemo](media/sql-server-dacpac-extension/dacpac-extension-demo.gif)

## <a name="why-would-i-use-the-data-tier-application-wizard"></a>Warum sollte ich den Datenschichtanwendungs-Assistenten verwenden?

Der Assistent erleichtert die Verwaltung von DACPAC- und BACPAC-Dateien, die die Entwicklung und Bereitstellung von Datenschichtelementen zur Unterstützung Ihrer Anwendung vereinfachen. Weitere Informationen zur Verwendung von Datenschichtanwendungen finden Sie in der zugehörigen [Dokumentation](../../relational-databases/data-tier-applications/data-tier-applications.md).

## <a name="install-the-extension"></a>Installieren der Erweiterung

1. Klicken Sie auf das Symbol „Erweiterungen“, um die verfügbaren Erweiterungen anzuzeigen.

    ![Erweiterungs-Manager-Symbol](media/add-extensions/extension-manager-icon.png)

2. Suchen Sie nach der Erweiterung **SQL Server-DACPAC**, und wählen Sie sie aus, um die Details anzuzeigen. Wählen Sie **Installieren** aus, um die Erweiterung hinzuzufügen.

3. Klicken Sie nach der Installation auf **Erneut laden**, um die Erweiterung in Azure Data Studio zu aktivieren (nur bei der ersten Installation einer Erweiterung erforderlich).

## <a name="launch-the-data-tier-application-wizard"></a>Starten des Datenschichtanwendungs-Assistenten

Klicken Sie zum Starten des Assistenten mit der rechten Maustaste auf den Ordner „Datenbanken“, oder klicken Sie im Objekt-Explorer mit der rechten Maustaste auf eine bestimmte Datenbank. Wählen Sie dann **Assistent zum Extrahieren von Datenebenenanwendungen** aus.

![Startmenü der DACPAC-Erweiterung](media/sql-server-dacpac-extension/dacpac-extension-launch.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DACPACs finden Sie in der zugehörigen [Dokumentation](../../relational-databases/data-tier-applications/data-tier-applications.md).
Probleme und Featureanforderungen können Sie [hier](https://github.com/microsoft/azuredatastudio/issues) mitteilen.