---
title: Quellcodeverwaltung
description: Azure Data Studio unterstützt Git für die Quellcodeverwaltung (Source Control Management, SCM). Erfahren Sie, wie Sie ein vorhandenes Git-Repository öffnen und ein neues initialisieren.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2019
ms.openlocfilehash: 014376dd56045b9e5ef440054ae2c1d1df836cad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040020"
---
# <a name="source-control-in-azure-data-studio"></a>Quellcodeverwaltung in Azure Data Studio

Azure Data Studio unterstützt Git für die Versionskontrolle/Quellcodeverwaltung.

## <a name="git-support-in-azure-data-studio"></a>Git-Unterstützung in Azure Data Studio

Azure Data Studio wird mit einem Git-Quellcodeverwaltungs-Manager (Source Control Manager, SCM) bereitgestellt. Sie müssen dennoch weiterhin [Git (Version 2.0.0 oder höher) installieren](https://git-scm.com/download), damit diese Features verfügbar sind.

## <a name="open-an-existing-git-repository"></a>Öffnen eines vorhandenen Git-Repositorys

1. Wählen Sie im Menü **Datei** den Befehl **Ordner öffnen...** aus.
2. Navigieren Sie zu dem Ordner, der die Dateien enthält, die von Git nachverfolgt werden, und wählen Sie **Ordner auswählen** aus. Unterordner in Ihrem lokalen Repository können hier ausgewählt werden.

## <a name="initialize-a-new-git-repository"></a>Initialisieren eines neuen Git-Repositorys

1. Wählen Sie **Quellcodeverwaltung** und dann das Git-Symbol aus.

   ![Git-Symbol für Quellcodeverwaltung](media/source-control/source-control.png)

1. Geben Sie den Pfad zu dem Ordner ein, den Sie als Git-Repository initialisieren möchten, und drücken Sie die **EINGABETASTE**.

   ![Git-Repository initialisieren](media/source-control/initialize-git-repository.png)

## <a name="working-with-git-repositories"></a>Arbeiten mit Git-Repositorys

Azure Data Studio erbt die Git-Implementierung von VS Code, unterstützt derzeit jedoch keine weiteren SCM-Anbieter. Ausführliche Informationen über das Arbeiten mit Git, nachdem Sie ein Repository geöffnet oder initialisiert haben, finden Sie unter [Git support in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Git-Dokumentation](https://git-scm.com/documentation)
