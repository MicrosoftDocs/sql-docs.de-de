---
title: Parametrisierung von Notebooks in Azure Data Studio
description: In diesem Tutorial erfahren Sie, wie Sie in ADS ein parametrisiertes Notebook erstellen können.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vabhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: 25e8ea8c4f10ccdb7ee2901dced68f4a66d57000
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836077"
---
# <a name="create-a-parameterized-notebook"></a>Erstellen eines parametrisierten Notebooks

**Parametrisierung** ist die Fähigkeit, ein Notebook mit unterschiedlichen Parametern auszuführen.

In diesem Artikel wird beschrieben, wie in Azure Data Studio mithilfe des Python-Kernels ein parametrisiertes Notebook erstellt und ausgeführt werden kann.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>Installieren und Einrichten von Papermill in Azure Data Studio

Alle Schritte dieses Abschnitts werden in einem Azure Data Studio-Notebook ausgeführt.

1. Erstellen Sie ein neues Notebook, und ändern Sie den **Kernel** in *Python 3*.

   ![Neues Notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Möglicherweise werden Sie aufgefordert, Ihre Python-Pakete zu aktualisieren, wenn die Pakete aktualisiert werden müssen.

   ![Ja](media/notebooks-kqlmagic/install-python-yes.png)

3. Installieren von Papermill:

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   Überprüfen Sie die Installation:

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Liste":::

5. Sie können überprüfen, ob Papermill ordnungsgemäß geladen wurde, indem Sie die Version von Papermill überprüfen.

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Überprüfung":::

## <a name="set-up-a-parameterized-notebook"></a>Einrichten eines parametrisierten Notebooks

1. Stellen Sie sicher, dass der **Kernel** auf *Python 3* festgelegt ist.

   ![Änderung des Kernels](media/notebooks-kqlmagic/change-kernel.png)

2. Erstellen Sie eine neue Codezelle, und kennzeichnen Sie sie als **Parameterzelle**.

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Parameterzellen-Notebook":::

3. Fügen Sie weitere Zellen hinzu, um andere Parameter zu testen.

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   Zellen im beispielhaften Eingabe-Notebook: :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="Zusätzliche Zellen des Eingabe-Notebooks":::

4. Speichern Sie das Notebook als **Input.ipynb**.
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Notebook speichern":::

## <a name="how-to-execute-papermill-notebook"></a>Ausführen des Papermill-Notebooks

Es gibt zwei Möglichkeiten, Papermill auszuführen:

- Befehlszeilenschnittstelle (CLI)
- Python-API

### <a name="parameterized-cli-execution"></a>Ausführung über eine parametrisierte Befehlszeilenschnittstelle

Wenn Sie ein Notebook über die Befehlszeilenschnittstelle ausführen möchten, geben Sie im Terminal den Papermill-Befehl mit Eingabe-Notebook, Speicherort für das Ausgabe-Notebook und anderen Optionen ein.

> [!Note]
   > Die Dokumentation zur Befehlszeilenschnittstelle von Papermill finden Sie [hier](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Führen Sie das Eingabe-Notebook mit neuen Parametern aus.

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   Dadurch wird das Eingabe-Notebook mit neuen Werten für die Parameter **x** und **y** ausgeführt.

2. Zeigen Sie nach der Ausführung die neue Ausgabe des parametrisierten Notebooks an.
   Wie Sie sehen, ist nun eine neue Zelle mit der Bezeichnung **# Injected-Parameters** vorhanden. Sie enthält die über die Befehlszeilenschnittstelle übergebenen neuen Parameterwerte.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Ausgabe-Notebook":::

### <a name="parameterized-python-api-execution"></a>Ausführung über eine parametrisierte Python-API

> [!Note]
   > Die Dokumentation zur Python-API für Papermill finden Sie [hier](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Erstellen Sie ein neues Notebook, und ändern Sie den **Kernel** in *Python 3*.
   ![Neues Notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Fügen Sie eine neue Codezelle hinzu, und verwenden Sie Papermill zur Verwendung der Ausführungsmethode.

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Ausführung über die Python-API für Papermill](media/notebooks-parameterization/python-api-execute.png)

3. Zeigen Sie nach der Ausführung die neue Ausgabe des parametrisierten Notebooks an.

   Wie Sie sehen, ist nun eine neue Zelle mit der Bezeichnung **# Injected-Parameters** vorhanden. Sie enthält die über die Befehlszeilenschnittstelle übergebenen neuen Parameterwerte.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Ausgabe-Notebook":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Notebooks und zur Parametrisierung:

- [Verwenden von Notebooks in Azure Data Studio](./notebooks-guidance.md)
- [Dokumentation zur Parametrisierung mit Papermill](https://papermill.readthedocs.io/en/latest/index.html)