---
title: Verwalten von Paketen mit der Machine Learning-Erweiterung
description: Hier erfahren Sie, wie Sie Python-oder R-Pakete in Ihrer Datenbank mit der Machine Learning-Erweiterung für Azure Data Studio verwalten.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: c874317acf39e954a0e640c1954e3a0dac6e9f43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051961"
---
# <a name="manage-packages-in-database-with-machine-learning-extension-for-azure-data-studio-preview"></a>Verwalten von Paketen in Datenbanken mit der Machine Learning-Erweiterung für Azure Data Studio (Vorschauversion)

Hier erfahren Sie, wie Sie Python-oder R-Pakete in Ihrer Datenbank mit der [Machine Learning-Erweiterung für Azure Data Studio](machine-learning-extension.md) verwalten.

> [!IMPORTANT]
> Das Verwalten von Paketen in der Datenbank mit der Machine Learning-Erweiterung unterstützt derzeit nur [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) unter SQL Server 2019.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren und konfigurieren Sie die [Machine Learning-Erweiterung für Azure Data Studio](machine-learning-extension.md). Sie müssen die [Python- oder R-Installationspfade in den Erweiterungseinstellungen](machine-learning-extension.md#settings) angeben, damit die Paketverwaltung funktioniert.

- Sie benötigen das **sqlmlutils**-Paket. Wenn das Paket nicht bereits installiert ist, werden Sie von der Machine Learning-Erweiterung dazu aufgefordert, dieses zu installieren.

- Für SQL Server für Linux wird die Windows-Authentifizierung nicht unterstützt. Daher müssen Sie die SQL-Authentifizierung verwenden, um eine Verbindung mit Ihrer Instanz von SQL Server für Linux herzustellen.

## <a name="manage-python-packages"></a>Verwalten von Python-Paketen

Sie können Python-Pakete mit der Machine Learning-Erweiterung installieren und deinstallieren.

### <a name="install-new-python-package"></a>Installieren neuer Python-Pakete

Führen Sie die folgenden Schritte durch, um Python-Pakete in Ihrer Datenbank zu installieren.

1. Wählen Sie **Manage packages in database** (Pakete in Datenbank verwalten) aus.

1. Wenn Sie aufgefordert werden, **sqlmlutils** zu installieren, wählen Sie **Ja** aus.

1. Wählen Sie auf der Registerkarte **Installed** (Installiert) unter **Pakettyp** die Option **Python** und unter **Speicherort** Ihre Datenbank aus.

1. Wählen Sie die Registerkarte **Neu hinzufügen** aus.

1. Geben Sie unter **Search Python packages** (Python-Pakete suchen) das Python-Paket ein, das Sie installieren möchten, und wählen Sie **Suchen** aus.

1. Stellen Sie sicher, dass das Paket unter **Paketname** und die gewünschte Version unter **Paketversion** aufgeführt ist.

1. Klicken Sie auf **Installieren**.

1. Wählen Sie **Schließen** aus, und vergewissern Sie sich unter **Tasks**, dass das Paket erfolgreich installiert wurde.

### <a name="uninstall-a-python-package"></a>Deinstallieren eines Python-Pakets

Führen Sie die folgenden Schritte durch, um Python-Pakete in Ihrer Datenbank zu deinstallieren.

> [!NOTE]
> Sie können nur Pakete deinstallieren, die in Ihrer Datenbank installiert wurden. Ein Paket, das in Machine Learning Services für SQL Server bereits vorinstalliert war, kann nicht deinstalliert werden.

1. Wählen Sie **Manage packages in database** (Pakete in Datenbank verwalten) aus.

1. Wenn Sie aufgefordert werden, **sqlmlutils** zu installieren, wählen Sie **Ja** aus.

1. Wählen Sie auf der Registerkarte **Installed** (Installiert) unter **Pakettyp** die Option **Python** und unter **Speicherort** Ihre Datenbank aus.

1. Wählen Sie das Paket bzw. die Pakete aus, das oder die Sie deinstallieren möchten.

1. Wählen Sie **Ausgewählte Pakete deinstallieren** aus.

1. Wählen Sie **Schließen** aus, und vergewissern Sie sich unter **Tasks**, dass das Paket erfolgreich installiert wurde.

## <a name="manage-r-packages"></a>Verwalten von R-Paketen

Sie können R-Pakete mit der Machine Learning-Erweiterung installieren und deinstallieren.

### <a name="install-new-r-package"></a>Installieren eines neuen R-Pakets

Führen Sie die folgenden Schritte durch, um Python-Pakete in Ihrer Datenbank zu installieren.

1. Wählen Sie **Manage packages in database** (Pakete in Datenbank verwalten) aus.

1. Wenn Sie gefragt werden, ob **sqlmlutils** installiert werden soll, wählen Sie **Ja** aus.

1. Wählen Sie auf der Registerkarte **Installed** (Installiert) unter **Pakettyp** die Option **R** und unter **Speicherort** Ihre Datenbank aus.

1. Wählen Sie die Registerkarte **Neu hinzufügen** aus.

1. Geben Sie unter **Search R packages** (R-Pakete suchen) das R-Paket ein, das Sie installieren möchten, und wählen Sie **Suchen** aus.

1. Stellen Sie sicher, dass das Paket unter **Paketname** und die gewünschte Version unter **Paketversion** aufgeführt ist.

1. Klicken Sie auf **Installieren**.

1. Wählen Sie **Schließen** aus, und vergewissern Sie sich unter **Tasks**, dass das Paket erfolgreich installiert wurde.

### <a name="uninstall-an-r-package"></a>Deinstallieren eines R-Pakets

Führen Sie die folgenden Schritte durch, um R-Pakete in Ihrer Datenbank zu deinstallieren.

> [!NOTE]
> Sie können nur Pakete deinstallieren, die in Ihrer Datenbank installiert wurden. Ein Paket, das in Machine Learning Services für SQL Server bereits vorinstalliert war, kann nicht deinstalliert werden.

1. Wählen Sie **Manage packages in database** (Pakete in Datenbank verwalten) aus.

1. Wenn Sie gefragt werden, ob **sqlmlutils** installiert werden soll, wählen Sie **Ja** aus.

1. Wählen Sie auf der Registerkarte **Installed** (Installiert) unter **Pakettyp** die Option **R** und unter **Speicherort** Ihre Datenbank aus.

1. Wählen Sie das Paket bzw. die Pakete aus, das oder die Sie deinstallieren möchten.

1. Wählen Sie **Ausgewählte Pakete deinstallieren** aus.

1. Wählen Sie **Schließen** aus, und vergewissern Sie sich unter **Tasks**, dass das Paket erfolgreich installiert wurde.

## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning-Erweiterung in Azure Data Studio](machine-learning-extension.md)
- [Treffen von Vorhersagen](machine-learning-extension-predictions.md)
- [Importieren und Anzeigen von Modellen](machine-learning-extension-import-view-models.md)
- [Notebooks in Azure Data Studio](../notebooks/notebooks-guidance.md)
- [SQL Machine Learning-Dokumentation](../../machine-learning/index.yml)