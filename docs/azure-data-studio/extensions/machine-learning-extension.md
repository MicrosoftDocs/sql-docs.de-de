---
title: Machine-Learning-Erweiterung
description: Mit der Machine-Learning-Erweiterung für Azure Data Studio können Sie Pakete verwalten, Machine Learning-Modelle importieren, Vorhersagen treffen und Notebooks erstellen, um Experimente für Ihre SQL-Datenbanken durchzuführen.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: 8eb7dd5f5a11c508af0d85e7559349c14e558adb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051951"
---
# <a name="machine-learning-extension-for-azure-data-studio-preview"></a>Machine Learning-Erweiterung für Azure Data Studio (Vorschau)

Mit der Machine-Learning-Erweiterung für [Azure Data Studio](../what-is-azure-data-studio.md) können Sie Pakete verwalten, Machine Learning-Modelle importieren, Vorhersagen treffen und Notebooks erstellen, um Experimente für Ihre SQL-Datenbanken durchzuführen. Diese Erweiterung befindet sich zurzeit in der Vorschau.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden erforderlichen Komponenten müssen auf dem Computer installiert sein, auf dem Azure Data Studio ausgeführt wird.

- [Python 3](https://www.python.org/downloads/). Nachdem Sie Python installiert haben, müssen Sie unter [Erweiterungseinstellungen](#settings) den lokalen Pfad zu einer Python-Installation angeben. Wenn Sie in Azure Data Studio ein [Python-Kernel-Notebook](../notebooks/notebooks-python-kernel.md) verwenden, übernimmt die Erweiterung automatisch den Pfad des Notebooks.

- [Microsoft ODBC Driver 17 for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md) für Windows, macOS oder Linux

- [R 3.5](https://www.r-project.org/) (optional). Andere Versionen als 3.5 werden zurzeit nicht unterstützt. Nachdem Sie R 3.5 installiert haben, müssen Sie R aktivieren und unter [Erweiterungseinstellungen](#settings) den lokalen Pfad zu einer R-Installation angeben. Dies ist nur erforderlich, wenn Sie in Ihrer Datenbank R-Pakete verwalten möchten.

### <a name="trouble-installing-python-3-from-within-ads"></a>Haben Sie Probleme mit der Installation von Python 3 in ADS?

Wenn Sie versuchen, Python 3 zu installieren, aber eine Fehlermeldung bezüglich TLS/SSL erhalten, fügen Sie die folgenden zwei optionalen Komponenten hinzu:

_Beispielfehler:_
```
$: ~/0.0.1/bin/python3 -m pip install --user "jupyter>=1.0.0" --extra-index-url https://prose-python-packages.azurewebsites.net
WARNING: pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Looking in indexes: https://pypi.org/simple, https://prose-python-packages.azurewebsites.net
Requirement already satisfied: jupyter
```

_Installieren Sie folgende Komponenten:_

- [Homebrew](https://brew.sh) (optional): Installieren Sie Homebrew, und führen Sie dann `brew update` über die Befehlszeile aus.

- *openssl* (optional): Führen Sie anschließend `brew install openssl` aus.

## <a name="install-the-extension"></a>Installieren der Erweiterung

Führen Sie die folgenden Schritte aus, um die Machine-Learning-Erweiterung in Azure Data Studio zu installieren.

1. Öffnen Sie den Erweiterungs-Manager in Azure Data Studio. Dazu können Sie entweder auf das Erweiterungssymbol oder im Menü „Ansicht“ auf „Erweiterungen“ klicken.

1. Wählen Sie die Erweiterung **Machine Learning** aus, und zeigen Sie die Details an.

1. Klicken Sie auf **Installieren**.

1. Wählen Sie **Erneut laden** aus, um die Erweiterung zu aktivieren. Dies ist nur bei der ersten Installation einer Erweiterung erforderlich.

<a name="settings"></a>

## <a name="extension-settings"></a>Erweiterungseinstellungen

Führen Sie die folgenden Schritte aus, um die Einstellungen für die Machine-Learning-Erweiterung zu ändern.

1. Öffnen Sie den Erweiterungs-Manager in Azure Data Studio. Dazu können Sie entweder auf das Erweiterungssymbol oder im Menü „Ansicht“ auf „Erweiterungen“ klicken.

1. Suchen Sie in den **aktivierten** Erweiterungen nach der Erweiterung **Machine Learning**.

1. Wählen Sie das Symbol **Verwalten** aus.

1. Wählen Sie das Symbol **Erweiterungseinstellungen** aus.

Die Erweiterungseinstellungen sehen wie folgt aus:

:::image type="content" source="media/machine-learning-extension/ml-extension-settings.png" alt-text="Einstellungen der Machine-Learning-Erweiterung":::

### <a name="enable-python"></a>Aktivieren von Python

Führen Sie die folgenden Schritte aus, um die Machine Learning-Erweiterung zusammen mit der Python-Paketverwaltung in Ihrer Datenbank zu verwenden.

> [!IMPORTANT]
> Python muss aktiviert und für den größtmöglichen Funktionsumfang konfiguriert sein, damit die Machine-Learning-Erweiterung funktioniert. Dies ist selbst dann erforderlich, wenn Sie die Python-Paketverwaltung gar nicht in Ihrer Datenbank verwenden möchten.

1. Stellen Sie sicher, dass **Machine Learning: Enable Python** (Machine Learning: Python aktivieren) aktiviert ist. Diese Einstellung ist standardmäßig aktiviert.

1. Geben Sie den Pfad zu Ihrer bereits vorhandenen Python-Installation unter **Machine Learning: Python Path** (Machine Learning: Python-Pfad) an. Dies kann entweder der vollständige Pfad zur ausführbaren Python-Datei oder der Ordner sein, in dem sich die ausführbare Datei befindet. Wenn Sie in Azure Data Studio ein [Python-Kernel-Notebook](../notebooks/notebooks-python-kernel.md) verwenden, übernimmt die Erweiterung automatisch den Pfad des Notebooks.

### <a name="enable-r"></a>Aktivieren von R

Führen Sie die folgenden Schritte aus, um mithilfe der Machine Learning-Erweiterung R-Pakete in Ihrer Datenbank verwalten zu können.

1. Stellen Sie sicher, dass **Machine Learning: Enable R** (Machine Learning: R aktivieren) aktiviert ist. Diese Einstellung ist standardmäßig deaktiviert.

1. Geben Sie den Pfad zu Ihrer bereits vorhandenen R-Installation unter **Machine Learning: R Path** (Machine Learning: R-Pfad) an. Dabei muss es sich um den vollständigen Pfad zur ausführbaren R-Datei handeln. 

## <a name="use-the-machine-learning-extension"></a>Verwenden der Machine-Learning-Erweiterung

Führen Sie die folgenden Schritte aus, um die Machine-Learning-Erweiterung in Azure Data Studio zu verwenden.

1. Öffnen Sie das Viewlet **Verbindungen** in Azure Data Studio.

1. Klicken Sie mit der rechten Maustaste auf Ihren Server, und wählen Sie **Verwalten** aus.

1. Wählen Sie im Menü auf der linken Seite unter **Allgemein** die Option **Machine Learning** aus.

Öffnen Sie die Links unter **Nächste Schritte**, und informieren Sie sich darüber, wie Sie mit der Machine-Learning-Erweiterung Pakete verwalten, Vorhersagen treffen und Modellen in Ihre Datenbank importieren.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Paketen in der Datenbank](machine-learning-extension-manage-packages.md)
- [Treffen von Vorhersagen](machine-learning-extension-predictions.md)
- [Importieren und Anzeigen von Modellen](machine-learning-extension-import-view-models.md)
- [Notebooks in Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Machine-Learning-Dokumentation für SQL](../../machine-learning/index.yml)
- [Machine Learning und KI mit ONNX in SQL Edge (Vorschau)](/azure/azure-sql-edge/onnx-overview)