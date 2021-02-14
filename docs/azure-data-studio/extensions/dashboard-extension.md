---
title: Erstellen einer Dashboarderweiterung
description: Dieses Tutorial veranschaulicht, wie Sie eine Dashboarderweiterung erstellen, um Azure Data Studio benutzerdefinierte Funktionen hinzuzufügen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 400dae97922503d37b1ca1c2b726264032fa12cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048600"
---
# <a name="create-an-azure-data-studio-dashboard-extension"></a>Erstellen einer Azure Data Studio-Dashboarderweiterung

In diesem Tutorial wird veranschaulicht, wie Sie eine neue Azure Data Studio-Dashboarderweiterung erstellen. Die Erweiterung trägt zum Azure Data Studio-Verbindungsdashboard bei, sodass Sie die Funktionalität von Azure Data Studio so erweitern können, dass Sie für Benutzer leicht zu erkennen sind.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> - Installieren des Erweiterungs-Generators
> - Erstellen der Erweiterung
> - Beitragen zum Dashboard in der Erweiterung
> - Testen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

## <a name="prerequisites"></a>Voraussetzungen

Azure Data Studio ist auf dem gleichen Framework wie Visual Studio Code aufgebaut, sodass Erweiterungen für Azure Data Studio mithilfe von Visual Studio Code erstellt werden. Sie benötigen die folgenden Komponenten:

- [Node.js](https://nodejs.org) – installiert und in Ihrem `$PATH` verfügbar. Node.js enthält [npm](https://www.npmjs.com/), den Node.js-Paket-Manager, der zum Installieren des Erweiterungsgenerators verwendet wird.
- [Visual Studio Code](https://code.visualstudio.com) zum Debuggen der Erweiterung.
- Die [Debugerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) von Azure Data Studio (optional). Mit der Debugerweiterung können Sie Ihre Erweiterung testen, ohne sie zu packen und in Azure Data Studio zu installieren.
- Stellen Sie sicher, dass sich `azuredatastudio` in Ihrem Pfad befindet. Wenn Sie unter Windows arbeiten, wählen Sie in „setup.exe“ die Option **Zu PATH hinzufügen** aus. Führen Sie unter macOS oder Linux über die Befehlspalette in Azure Data Studio **Befehl 'azuredatastudio' in PATH installieren** aus.

## <a name="install-the-extension-generator"></a>Installieren des Erweiterungsgenerators

Um den Prozess der Erstellung von Erweiterungen zu vereinfachen, haben wir einen [Erweiterungsgenerator](https://code.visualstudio.com/docs/extensions/yocode) erstellt, der Yeoman verwendet. Um diesen zu installieren, führen Sie an der Eingabeaufforderung den folgenden Befehl aus:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-dashboard-extension"></a>Erstellen Ihrer Dashboarderweiterung

### <a name="introduction-to-the-dashboard"></a>Einführung in das Dashboard

Das Verbindungsdashboard von Azure Data Studio ist ein leistungsstarkes Tool, das die Verbindungen eines Benutzers zusammenfasst und Einblick in diese gibt.

Es sind zwei Varianten des Dashboards verfügbar. Im *Serverdashboard* wird der gesamte Server zusammengefasst, während auf dem *Datenbankdashboard* eine einzelne Datenbank zusammengefasst wird. Der Zugriff auf beide Dashboards erfolgt, indem Sie mit der rechten Maustaste im Viewlet **Verbindungen** von Azure Data Studio auf einen Server oder eine Datenbank und dann auf **Verwalten** klicken:

:::image type="content" source="media/dashboard-extension/dashboard-summary.gif" alt-text="Screenshot: Einführung in Dashboards":::

Es gibt drei wichtige Bereiche, in denen Erweiterungen dem Dashboard Funktionalität hinzufügen können:

1. **Registerkarte mit dem vollständigen Dashboard:** Eine separate Registerkarte im Dashboard für Ihre Erweiterung. Kann einem Server- oder einem Datenbankdashboard hinzugefügt werden. Anpassbar mit Widgets, einer Symbolleiste und einem Navigationsabschnitt.
2. **Homepageaktionen**: Aktionsschaltflächen am oberen Rand der Verbindungssymbolleiste.
3. **Widgets**: Diagramme, die für Ihre SQL Server-Instanz ausgeführt werden.

   :::image type="content" source="media/dashboard-extension/dashboard-contrib-points.png" alt-text="Screenshot: Bereiche, deren Funktionalität durch Erweiterungen erweitert werden kann":::

### <a name="run-the-extension-generator"></a>Ausführen des Erweiterungsgenerators

So erstellen Sie eine Erweiterung:

1. Starten Sie den Erweiterungs-Generator mit dem folgenden Befehl:

   `yo azuredatastudio`

1. Wählen Sie **Neues Dashboard** aus der Liste der Erweiterungstypen aus.

1. Füllen Sie die Eingabeaufforderungen wie gezeigt aus, um eine Erweiterung zu erstellen, die dem Serverdashboard eine Registerkarte hinzufügt.

   :::image type="content" source="media/dashboard-extension/dashboard-generator.png" alt-text="Screenshot: Erweiterungs-Generator":::

   Nachstehend werden die angezeigten Eingabeaufforderungen etwas detaillierter erläutert:

   :::image type="content" source="media/dashboard-extension/dashboard-flowchart.png" alt-text="Screenshot: Flussdiagramm für das Dashboard":::

Durch Ausführen der oben genannten Schritte wird ein neuer Ordner erstellt. Öffnen Sie den Ordner in Visual Studio Code, und schon sind Sie bereit, selbst eine Dashboarderweiterung zu erstellen.

### <a name="run-the-extension"></a>Ausführen der Erweiterung

Sehen wir uns nun an, was die Dashboardvorlage bietet, indem wir die Erweiterung ausführen. Stellen Sie vor der Ausführung sicher, dass die **Azure Data Studio-Debugerweiterung** in Visual Studio Code installiert ist.

Drücken Sie **F5** in Visual Studio Code, um Azure Data Studio mit ausgeführter Erweiterung im Debugmodus zu starten. Anschließend können Sie anzeigen, wie diese Standardvorlage zum Dashboard beiträgt.

Als nächstes sehen wir uns an, wie dieses Standarddashboard geändert werden kann.

### <a name="develop-the-dashboard"></a>Entwickeln des Dashboards

Die wichtigste Datei für den Einstieg in die Erweiterungsentwicklung ist `package.json`. Dies ist die Manifestdatei, in der die Dashboardbeiträge registriert werden. Beachten Sie die Abschnitte `dashboard.tabs`, `dashboard.insights` und `dashboard.containers`.

Im Folgenden sind einige Änderungen aufgeführt, die Sie ausprobieren sollten:

- Experimentieren Sie mit den Einblicktypen, zu denen auch „bar“, „horizontalBar“ und „timeSeries“ gehören.
- Schreiben Sie eigene Abfragen, um sie für Ihre SQL Server-Verbindung auszuführen.
- Tutorials zu spezifischen Einblicken finden Sie in diesem [Einblickbeispiel](../tutorial-qds-sql-server.md) oder [unter diesem Link](../tutorial-table-space-sql-server.md).

## <a name="package-your-extension"></a>Packen der Erweiterung

Um Ihre Erweiterung für andere Benutzer freizugeben, müssen Sie sie in eine einzelne Datei packen. Ihre Erweiterung kann im Marketplace für Azure Data Studio-Erweiterungen veröffentlicht oder für Ihr Team oder Ihre Community freigegeben werden. Zu diesem Zweck müssen Sie über die Befehlszeile ein weiteres npm-Paket installieren.

```console
npm install -g vsce
```

Bearbeiten Sie die `README.md`-Datei nach Ihren Wünschen. Navigieren Sie anschließend zum Basisverzeichnis der Erweiterung, und führen Sie `vsce package` aus. Optional können Sie ein Repository mit ihrer Erweiterung verknüpfen oder den Vorgang ohne Repository fortsetzen. Um ein Repository hinzuzufügen, fügen Sie der Datei `package.json` eine ähnliche Zeile hinzu.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Nachdem diese Zeilen hinzugefügt wurden, wird die Datei `my-test-extension-0.0.1.vsix` erstellt und ist nun für die Installation in Azure Data Studio bereit.

:::image type="content" source="media/dashboard-extension/install-vsix.png" alt-text="Screenshot: Installation von VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>Veröffentlichen der Erweiterung im Marketplace

Da sich der Marketplace für Azure Data Studio-Erweiterungen noch in Bearbeitung befindet, wird die VSIX-Erweiterung an einem beliebigen Ort, z. B. auf einer GitHub-Releaseseite gehostet. Anschließend Sie senden einen Pull Request, der [diese JSON-Datei](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) mit Ihren Erweiterungsinformationen aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> - Installieren des Erweiterungs-Generators
> - Erstellen der Erweiterung
> - Beitragen zum Dashboard in der Erweiterung
> - Testen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

Hoffentlich hat Sie dieser Artikel dazu inspiriert, selbst eine Erweiterung für Azure Data Studio zu erstellen. Azure Data Studio bietet Unterstützung für Dashboardeinblicke (ansprechende Diagramme, die gegen Ihre SQL Server-Instanz ausgeführt werden), verschiedene SQL-spezifische APIs sowie eine Vielzahl von vorhandenen Erweiterungspunkten, die aus Visual Studio Code übernommen wurden.

Wenn Sie eine Idee haben, aber nicht sicher sind, wie Sie anfangen sollen, eröffnen Sie ein Issue, oder senden Sie einen Tweet an das Team unter [azuredatastudio](https://twitter.com/azuredatastudio).

Weitere Informationen finden Sie im [Visual Studio Code-Erweiterungsleitfaden](https://code.visualstudio.com/docs/extensions/overview). Dieser deckt alle vorhandenen APIs und Muster ab.

Um zu erfahren, wie Sie mit T-SQL in Azure Data Studio arbeiten, führen Sie das Tutorial zum T-SQL-Editor aus:

> [!div class="nextstepaction"]
> [Verwenden des Transact-SQL-Editors zum Erstellen von Datenbankobjekten](../tutorial-sql-editor.md)
