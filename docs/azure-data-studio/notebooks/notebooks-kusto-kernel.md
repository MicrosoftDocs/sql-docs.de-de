---
title: Notebooks mit Kusto-Kernel in Azure Data Studio
description: In diesem Tutorial erfahren Sie, wie Sie ein Kusto-Notebook erstellen und ausführen.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 1362e6a5ccd6152a1cb2597076d25c2da4149fd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048230"
---
# <a name="create-and-run-a-kusto-kql-notebook-preview"></a>Erstellen und Ausführen eines Kusto-Notebooks (KQL) (Vorschau)

In diesem Artikel erfahren Sie, wie Sie mit der [Kusto-Erweiterung (KQL)](../extensions/kusto-extension.md) und einer Verbindung mit einem Azure Data Explorer-Cluster ein [Azure Data Studio-Notebook](./notebooks-guidance.md) erstellen und ausführen.

Mit der Kusto-Erweiterung (KQL) können Sie die Kerneloption in **Kusto** ändern.

Diese Funktion steht derzeit als Vorschau zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

- [Ein Azure Data Explorer-Cluster mit einer Datenbank, mit der Sie eine Verbindung herstellen können](/azure/data-explorer/create-cluster-database-portal)
- [Azure Data Studio](../download-azure-data-studio.md)
- [Kusto-Erweiterung (KQL) für Azure Data Studio](../extensions/kusto-extension.md)

## <a name="create-a-kusto-kql-notebook"></a>Erstellen eines Kusto-Notebooks (KQL)

Führen Sie die folgenden Schritte aus, um eine Notebook-Datei in Azure Data Studio zu erstellen:

1. Stellen Sie in Azure Data Studio eine Verbindung mit Ihrem Azure Data Explorer-Cluster her.

2. Navigieren Sie zum Bereich **Verbindungen**, klicken Sie im Fenster **Server** mit der rechten Maustaste auf die Kusto-Datenbank, und klicken Sie anschließend auf *New Notebook* (Neues Notebook). Sie können auch zu **Datei** > **Neues Notebook** wechseln.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-new-notebook.png" alt-text="Notebook öffnen":::

3. Wählen Sie für den **Kernel** *Kusto* aus. Vergewissern Sie sich, dass im Menü **Attach to** (Anfügen an) der Clustername und die Datenbank festgelegt sind. In diesem Artikel werden der Cluster „help.kusto.windows.net“ und Daten aus der Datenbank „Samples“ verwendet.

   :::image type="content" source="media/notebooks-kusto-kernel/set-kusto-kernel.png" alt-text="„Kernel“ und „Anfügen an“ festlegen":::

Sie können das Notebook mit dem Befehl **Speichern** oder **Speichern unter...** im Menü **Datei** speichern.

Zum Öffnen eines Notebooks können Sie den Befehl **Datei öffnen...** im Menü **Datei** verwenden. Wählen Sie auf der **Begrüßungs**-Seite den Befehl **Öffnen** aus, oder verwenden Sie den Befehl **Datei: Öffnen** aus der Befehlspalette.

## <a name="change-the-connection"></a>Ändern der Verbindung

So ändern Sie die Kusto-Verbindung für ein Notebook

1. Klicken Sie in der Symbolleiste des Notebooks auf das Menü **Anfügen an**, und wählen Sie anschließend die Option **Verbindung ändern** aus.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-select-attach-to-change-connections.png" alt-text="Ändern von Verbindungen":::

   > [!Note]
   > Stellen Sie sicher, dass ein Wert für die Datenbank angegeben ist. Bei Kusto-Notebooks ist die Angabe einer Datenbank erforderlich.

2. Hier können Sie entweder einen zuletzt verwendeten Verbindungsserver auswählen oder neue Verbindungsdetails für die Verbindung eingeben.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-change-connection-cluster.png" alt-text="Auswählen eines anderen Clusters":::

   > [!Note]
   > Geben Sie den Clusternamen ohne `https://` an.

## <a name="run-a-code-cell"></a>Ausführen einer Codezelle

Sie können Zellen erstellen, die KQL-Abfragen enthalten, die Sie direkt ausführen können, indem Sie links neben der Zelle auf die Schaltfläche **Run cell** (Zelle ausführen) klicken. Nach dem Ausführen der Zelle werden die Ergebnisse im Notebook angezeigt.

Beispiel:

1. Klicken Sie auf der Symbolleiste auf den Befehl **+ Code**, um eine neue Codezelle hinzuzufügen.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-kernel-code.png" alt-text="Kusto-Kernelcodeblock":::

2. Kopieren Sie das folgende Beispiel, fügen Sie es in die Zelle ein, und klicken Sie auf **Run cell** (Zelle ausführen). In diesem Beispiel werden die StormEvent-Daten für einen bestimmten Ereignistyp abgefragt.

   ```kusto
    StormEvents
    | where EventType == "Waterspout"
   ```

   :::image type="content" source="media/notebooks-kusto-kernel/run-kusto-notebook-cell.png" alt-text="Run cell (Zelle ausführen)":::

## <a name="save-the-result-or-show-chart"></a>Speichern des Ergebnisses oder Anzeigen eines Diagramms

Wenn Sie ein Skript ausführen, das ein Ergebnis zurückgibt, können Sie dieses Ergebnis in verschiedenen Formaten mithilfe der über dem Ergebnis angezeigten Symbolleiste speichern.

- Als CSV speichern
- Als Excel speichern
- Als JSON speichern
- Als XML speichern
- Diagramm anzeigen

```kusto
    StormEvents
    | limit 10
```

:::image type="content" source="media/notebooks-kusto-kernel/run-notebook-save-results.png" alt-text="Speichern des Ergebnisses":::

## <a name="known-issues"></a>Bekannte Probleme

| Details | Problemumgehung |
|---------|------------|
| [Das Abfrageergebnis zeigt nur Spaltenheader an](https://github.com/microsoft/azuredatastudio/issues/12565). | N/V |

Sie können eine [Featureanforderung](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=) senden, um Feedback für das Produktteam bereitzustellen.  
Sie können einen [Fehlerbericht](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=) senden, um Feedback für das Produktteam bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Notebooks:

- [Kusto-Erweiterung (KQL) für Azure Data Studio](../extensions/kusto-extension.md)
- [Verwenden von Notebooks in Azure Data Studio](./notebooks-guidance.md)
- [Erstellen und Ausführen eines Python-Notebooks](./notebooks-python-kernel.md)
- [Erstellen und Ausführen eines SQL Server-Notebooks](./notebooks-sql-kernel.md)