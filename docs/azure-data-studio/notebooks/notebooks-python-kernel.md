---
title: Notebooks mit Python-Kernel in Azure Data Studio
description: In diesem Tutorial erfahren Sie, wie Sie ein Python-Notebook erstellen und ausführen.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: garyericson
ms.author: garye
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: e1859c5e1af823e067210c5b69247702e8c78f89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040320"
---
# <a name="create-and-run-a-python-notebook"></a>Erstellen und Ausführen eines Python-Notebooks

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

In diesem Tutorial wird veranschaulicht, wie Sie mithilfe des Python-Kernels ein Notebook in Azure Data Studio erstellen und ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- [Installierte Azure Data Studio-Instanz](../download-azure-data-studio.md)

## <a name="create-a-notebook"></a>Erstellen eines Notebooks

Führen Sie die folgenden Schritte aus, um eine Notebook-Datei in Azure Data Studio zu erstellen:

1. Öffnen Sie Azure Data Studio. Wenn Sie dazu aufgefordert werden, eine Verbindung mit einer SQL Server-Instanz herzustellen, können Sie diese Verbindung herstellen oder auf **Abbrechen** klicken.

1. Klicken Sie im Menü **Datei** auf **Neues Notebook**.

1. Wählen Sie **Python 3** als **Kernel** aus. **Anfügen an** ist auf „localhost“ festgelegt.

   :::image type="content" source="media/notebooks-python-kernel/set-kernel-and-attach-to-python.png" alt-text="Festlegen des Kernels":::

Sie können das Notebook mit dem Befehl **Speichern** oder **Speichern unter...** im Menü **Datei** speichern.

Zum Öffnen eines Notebooks können Sie den Befehl **Datei öffnen...** im Menü **Datei** verwenden. Wählen Sie auf der **Begrüßungs**-Seite den Befehl **Öffnen** aus, oder verwenden Sie den Befehl **Datei: Öffnen** aus der Befehlspalette.

## <a name="change-the-python-kernel"></a>Ändern des Python-Kernels

Wenn Sie zum ersten Mal eine Verbindung mit dem Python-Kernel in einem Notebook herstellen, wird die Seite **Python für Notebooks konfigurieren** angezeigt. Sie können eine der folgenden Optionen auswählen:

- **Neue Python-Installation**, um eine neue Kopie von Python für Azure Data Studio zu installieren, oder
- **Vorhandene Python-Installation verwenden**, um den Pfad zu einer vorhandenen Python-Installation anzugeben, die Azure Data Studio verwenden soll

Erstellen Sie eine Codezelle, und führen Sie die folgenden Python-Befehle aus, um den Speicherort und die Version des aktiven Python-Kernels anzuzeigen:

```python
import os
import sys
print(sys.version_info)
print(os.path.dirname(sys.executable))
```

So stellen Sie mit einer anderen Python-Installation eine Verbindung her

1. Klicken Sie im Menü **Datei** auf **Einstellungen** und anschließend auf **Einstellungen**.
1. Scrollen Sie unter **Erweiterungen** zu **Notebook-Konfiguration**.
1. Deaktivieren Sie unter **Verwenden vorhandener Python-Installation** die Option „Lokaler Pfad zu einer bereits vorhandenen Python-Installation, die von Notebooks verwendet wird“.
1. Starten Sie Azure Data Studio neu.

Wenn Azure Data Studio startet, und Sie eine Verbindung mit dem Python-Kernel herstellen,m wird die Seite **Python für Notebooks konfigurieren** angezeigt, und Sie können auswählen, ob Sie eine neue Python-Installation erstellen oder einen Pfad zu einer vorhandenen Installation angeben möchten.

## <a name="run-a-code-cell"></a>Ausführen einer Codezelle

Sie können Zellen erstellen, die SQL-Code enthalten, den Sie direkt ausführen können, indem Sie auf die Schaltfläche **Zelle ausführen** (der runde schwarze Pfeil) links neben der Zelle klicken. Die Ergebnisse werden im Notebook angezeigt, nachdem das Ausführen der Zelle abgeschlossen ist.

Beispiel:

1. Fügen Sie eine neue Python-Codezelle hinzu, indem Sie in der Symbolleiste auf den Befehl **+ Code** klicken.

   :::image type="content" source="media/notebooks-python-kernel/notebook-toolbar-python.png" alt-text="Notebook-Symbolleiste":::

1. Kopieren Sie das folgende Beispiel, fügen Sie es in die Zelle ein, und klicken Sie auf **Zelle ausführen**. In diesem Beispiel wird eine einfache Berechnung durchgeführt und das Ergebnis unten angezeigt.

   ```python
   a = 1
   b = 2
   c = a/b
   print(c)
   ```

   :::image type="content" source="media/notebooks-python-kernel/run-notebook-cell-python.png" alt-text="Ausführen der Notebook-Zelle":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Notebooks:

- [Erweitern von Python mit Kqlmagic](./notebooks-kqlmagic.md)
- [Verwenden von Notebooks in Azure Data Studio](./notebooks-guidance.md)
- [Erstellen und Ausführen eines SQL Server-Notebooks](./notebooks-sql-kernel.md)
