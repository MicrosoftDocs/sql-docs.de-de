---
title: Erstellen einer Jupyter Book-Erweiterung
description: Hier erfahren Sie, wie Sie ein Jupyter Book mit dem Erweiterungsgenerator in einer Erweiterung packen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 37875ecc79d471d1e7226337cec65c3323e19875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052061"
---
# <a name="create-a-jupyter-book-extension"></a>Erstellen einer Jupyter Book-Erweiterung

Dieses Tutorial veranschaulicht, wie Sie eine neue Azure Data Studio Jupyter Book-Erweiterung erstellen. Die Erweiterung enthält ein Beispiel für ein Jupyter Book, das in Azure Data Studio geöffnet und ausgeführt werden kann.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> - Erstellen eines Erweiterungsprojekts
> - Installieren des Erweiterungsgenerators
> - Erstellen einer Jupyter Book-Erweiterung
> - Ausführen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

## <a name="apis-used"></a>Verwendete APIs

- `bookTreeView.openBook`

### <a name="extension-use-cases"></a>Anwendungsfälle für Erweiterungen

Es gibt verschiedene Gründe, warum Sie eine Jupyter Book-Erweiterung erstellen möchten:

- Freigeben von organisierter und in Abschnitte aufgeteilter interaktiver Dokumentation
- Freigeben eines vollständigen Buchs (ähnlich einem E-Book, aber über Azure Data Studio verteilt)
- Versionserstellung und Nachverfolgung von Jupyter Book-Aktualisierungen

Der Hauptunterschied zwischen einem Jupyter Book- und einer Notebook-Erweiterung besteht darin, dass ein Jupyter Book Ihnen Organisationsmöglichkeiten bietet. Dutzende von Notebooks können in einem Jupyter Book in verschiedene Kapitel aufgeteilt werden, aber die Notebooks-Erweiterung ist für die Bereitstellung einer kleinen Anzahl einzelner Notebooks vorgesehen.

## <a name="prerequisites"></a>Voraussetzungen

Azure Data Studio ist auf dem gleichen Framework wie Visual Studio Code aufgebaut, sodass Erweiterungen für Azure Data Studio mithilfe von Visual Studio Code erstellt werden. Sie benötigen die folgenden Komponenten:

- [Node.js](https://nodejs.org) – installiert und in Ihrem `$PATH` verfügbar. Node.js enthält [npm](https://www.npmjs.com/), den Node.js-Paket-Manager, der zum Installieren des Erweiterungsgenerators verwendet wird.
- [Visual Studio Code](https://code.visualstudio.com), um Änderungen an Ihrer Erweiterung vorzunehmen und die Erweiterung zu debuggen.
- Stellen Sie sicher, dass sich `azuredatastudio` in Ihrem Pfad befindet. Wenn Sie unter Windows arbeiten, wählen Sie in „setup.exe“ die Option **Zu PATH hinzufügen** aus. Führen Sie unter macOS oder Linux über die Befehlspalette in Azure Data Studio **Befehl 'azuredatastudio' in PATH installieren** aus.

## <a name="install-the-extension-generator"></a>Installieren des Erweiterungsgenerators

Um den Prozess der Erstellung von Erweiterungen zu vereinfachen, haben wir einen [Erweiterungsgenerator](https://www.npmjs.com/package/generator-azuredatastudio) erstellt, der Yeoman verwendet. Um diesen zu installieren, führen Sie an der Eingabeaufforderung den folgenden Befehl aus:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-extension"></a>Erstellen der Erweiterung

So erstellen Sie eine Erweiterung:

1. Starten Sie den Erweiterungsgenerator mit dem folgenden Befehl:

   `yo azuredatastudio`

1. Wählen Sie **Neues Jupyter Book** aus der Liste der Erweiterungstypen aus.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-extension-generator.png" alt-text="Screenshot mit dem Erweiterungsgenerator":::

1. Führen Sie die Schritte zum Ausfüllen des Erweiterungsnamens aus. Verwenden Sie für dieses Tutorial **Test Book**. Geben Sie dann einen Herausgebernamen ein. Verwenden Sie für dieses Tutorial **Microsoft**. Geben Sie schließlich eine Beschreibung ein.

Sie können ein vorhandenes Jupyter Book bereitstellen, ein bereitgestelltes Beispielbuch verwenden oder ein neues Jupyter Book erstellen. Alle drei Optionen werden nachstehend vorgestellt.

### <a name="provide-an-existing-book"></a>Angeben eines vorhandenen Buchs

Wenn Sie ein bereits erstelltes Buch bereitstellen möchten, geben Sie den absoluten Dateipfad zu dem Ordner an, in dem sich Ihre Buchinhalte befinden. Sie können dann damit fortfahren, sich weiter über die Erweiterung zu informieren und sie bereitzustellen.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-existing-book.png" alt-text="Screenshot mit einem vorhandenen Buch":::

### <a name="use-the-sample-book"></a>Verwenden des Beispielbuchs

Wenn Sie nicht über ein vorhandenes Buch oder Notebooks verfügen, können Sie das bereitgestellte Beispiel im Generator verwenden.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-sample-path.png" alt-text="Screenshot mit einem Jupyter-Beispielbuch":::

Das Beispielbuch veranschaulicht, wie ein einfaches Jupyter Book aussieht. Weitere Informationen zum Anpassen eines Jupyter Books finden Sie im folgenden Abschnitt zum Erstellen eines neuen Buchs mit vorhandenen Notebooks.

### <a name="create-a-new-book"></a>Erstellen eines neuen Buchs

Wenn Sie über Notebooks verfügen, die Sie in ein Jupyter Book packen möchten, ist dies möglich. Der Generator fragt Sie, ob Sie Kapitel in Ihrem Buch wünschen, und wenn ja, wie viele und mit welchen Titeln. Wie das Auswahlverfahren aussieht, können Sie hier sehen. Verwenden Sie die Leertaste, um auszuwählen, welche Notebooks in den einzelnen Kapiteln platziert werden sollen.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-create-book.png" alt-text="Screenshot, der das Erstellen eines Jupyter-Buchs zeigt":::

Durch Ausführen der oben genannten Schritte wird ein neuer Ordner mit Ihrem neuen Jupyter Book erstellt. Öffnen Sie den Ordner in Visual Studio Code, und schon sind Sie bereit, die Jupyter Book-Erweiterung bereitzustellen!

## <a name="understand-your-extension"></a>Verstehen der Erweiterung

Ihr Projekt sollte derzeit wie folgt aussehen:

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-file-structure-generator.png" alt-text="Screenshot, der die Dateistruktur der Erweiterung zeigt":::

Die Datei `vsc-extension-quickstart.md` enthält einen Verweis auf die wichtigen Dateien. In der `README.md`-Datei können Sie die Dokumentation für Ihre neue Erweiterung bereitstellen. Beachten Sie die Dateien `package.json`, `jupyter-book.ts`, `content` und `toc.yml`. Der Ordner `content` enthält alle Notebook- oder Markdowndateien. `toc.yml` strukturiert das Jupyter Book und wird automatisch generiert, wenn Sie sich für die Erstellung eines benutzerdefinierten Jupyter Book über den Erweiterungsgenerator entschieden haben.

Wenn Sie ein Buch mit dem Generator erstellt und sich für Kapitel in Ihrem Buch entschieden haben, sieht die Ordnerstruktur etwas anders aus. Anstelle Ihrer Markdown- und Jupyter Notebook-Dateien, die im Ordner `content` vorhanden sind, würde es Unterordner geben, die den Titeln entsprechen, die Sie für Ihre Kapitel ausgewählt haben.

Wenn Dateien oder Ordner vorhanden sind, die Sie nicht veröffentlichen möchten, können Sie deren Namen in die Datei `.vscodeignore` einschließen.

Werfen wir einen Blick auf `jupyter-book.ts`, um zu verstehen, was unsere neu erstellte Erweiterung bewirkt.

```javascript
// This function is called when you run the command `Launch Book: Test Book` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchBook.test-book', () => {
        processNotebooks();
    }));

    // Add other code here if you want to register another command.
}
```

Die `activate`-Funktion ist die Hauptaktion ihrer Erweiterung. Alle Befehle, die Sie registrieren möchten, sollten in der `activate`-Funktion enthalten sein, ähnlich wie bei unserem `launchBook.test-book`-Befehl. In der `processNotebooks`-Funktion finden wir den Erweiterungsordner, der das Jupyter Book enthält, und es wird `bookTreeView.openBook` mit dem Ordner der Erweiterung als Parameter aufgerufen.

Die Datei `package.json` spielt außerdem eine wichtige Rolle beim Registrieren des Befehls der Erweiterung.

```json
"activationEvents": [
        "onCommand:launchBook.test-book"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchBook.test-book",
                "title": "Launch Book: Test Book"
            }
        ]
    }
```

Das Aktivierungsereignis(`onCommand`) löst die Funktion aus, die wir registriert haben,wenn wir den Befehl aufrufen. Es gibt einige weitere Aktivierungsereignisse, die zur weiteren Anpassung möglich sind. Weitere Informationen finden Sie unter [Aktivierungsereignisse](https://code.visualstudio.com/api/references/activation-events).

## <a name="package-your-extension"></a>Packen der Erweiterung

Um Ihre Erweiterung für andere Benutzer freizugeben, müssen Sie sie in eine einzelne Datei packen. Ihre Erweiterung kann im Marketplace für Azure Data Studio-Erweiterungen veröffentlicht oder für Ihr Team oder Ihre Community freigegeben werden. Zu diesem Zweck müssen Sie über die Befehlszeile ein weiteres npm-Paket installieren.

```console
npm install -g vsce
```

Bearbeiten Sie die `README.md`-Datei nach Ihren Wünschen. Navigieren Sie anschließend zum Basisverzeichnis der Erweiterung, und führen Sie `vsce package` aus. Optional können Sie ein Repository mit ihrer Erweiterung verknüpfen oder den Vorgang ohne Repository fortsetzen. Um ein Repository hinzuzufügen, fügen Sie der Datei `package.json` eine ähnliche Zeile hinzu.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testbook.git"
}
```

Sobald diese Zeilen hinzugefügt werden, wird die Datei `my test-book-0.0.1.vsix` erstellt und ist nun für die Installation in Azure Data Studio bereit.

## <a name="run-your-extension"></a>Ausführen der Erweiterung

Um die Erweiterung auszuführen und zu testen, öffnen Sie Azure Data Studio, und öffnen Sie dann die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken. Suchen Sie nach dem Befehl **Extensions: Install from VSIX** (Erweiterungen: Aus VSIX installieren), und navigieren Sie zu dem Ordner, der die neue Erweiterung enthält. Sie sollte nun in Ihrem Erweiterungsbereich in Azure Data Studio angezeigt werden.

   :::image type="content" source="media/jupyter-book-extension/install-vsix.png" alt-text="Screenshot, der die Installation von VSIX anzeigt":::

Öffnen Sie die Befehlspalette noch mal, und suchen Sie nach dem von uns registrierten Befehl **Launch Book: Test Notebook** (Notebook starten: Notebook testen). Bei der Ausführung sollte das Jupyter Book geöffnet werden, das wir mit unserer Erweiterung gepackt haben.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-launch-ads.png" alt-text="Screenshot, der den Notebook-Befehl anzeigt":::

Herzlichen Glückwunsch! Sie haben Ihre erste Jupyter Book-Erweiterung erstellt und können sie jetzt bereitstellen. Weitere Informationen zu Jupyter Books finden Sie unter [Bücher mit Jupyter](https://jupyterbook.org/intro.html).

## <a name="publish-your-extension-to-the-marketplace"></a>Veröffentlichen der Erweiterung im Marketplace

Der Marketplace für Azure Data Studio-Erweiterungen befindet sich in Bearbeitung. Zum Veröffentlichen hosten Sie die Erweiterung VSIX an einem beliebigen Ort, z. B. auf einer GitHub-Releaseseite. Senden Sie einen Pull Request, der [diese JSON-Datei](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) mit Ihren Erweiterungsinformationen aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> - Erstellen eines Erweiterungsprojekts
> - Installieren des Erweiterungsgenerators
> - Erstellen einer Jupyter Book-Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

Wir hoffen, dass Sie nach Lesen dieses Artikels Ideen zu Jupyter Books haben, die Sie mit der Azure Data Studio-Community teilen möchten.

Wenn Sie eine Idee haben, aber nicht sicher sind, wie Sie anfangen sollen, eröffnen Sie ein Issue, oder senden Sie einen Tweet an das Team unter [azuredatastudio](https://twitter.com/azuredatastudio).

Weitere Informationen finden Sie im [Visual Studio Code-Erweiterungsleitfaden](https://code.visualstudio.com/docs/extensions/overview). Dieser enthält alle vorhandenen APIs und Muster.
