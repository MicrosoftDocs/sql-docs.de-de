---
title: Entwickeln einer Tastenzuordnungserweiterung
description: Dieses Tutorial veranschaulicht, wie Sie eine Tastenzuordnungserweiterung entwickeln, um benutzerdefinierte Funktionen zu Azure Data Studio hinzuzufügen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 1cf1b48b3293f3315c11328b5b5c4beb7ab751ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048560"
---
# <a name="create-an-azure-data-studio-keymap-extension"></a>Entwickeln einer Azure Data Studio-Erweiterung für Tastenzuordnungen

Dieses Tutorial veranschaulicht, wie Sie eine neue Azure Data Studio-Erweiterung erstellen. Die Erweiterung erstellt bekannte SSMS-Tastenzuordnungen in Azure Data Studio.

In diesem Artikel wird Folgendes behandelt:
> [!div class="checklist"]
> - Erstellen eines Erweiterungsprojekts
> - Installieren des Erweiterungsgenerators
> - Erstellen der Erweiterung
> - Hinzufügen benutzerdefinierter Tastenzuordnungen zu einer Erweiterung
> - Testen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

## <a name="prerequisites"></a>Voraussetzungen

Azure Data Studio ist auf dem gleichen Framework wie Visual Studio Code aufgebaut, sodass Erweiterungen für Azure Data Studio mithilfe von Visual Studio Code erstellt werden. Sie benötigen die folgenden Komponenten:

- [Node.js](https://nodejs.org) – installiert und in Ihrem `$PATH` verfügbar. Node.js enthält [npm](https://www.npmjs.com/), den Node.js-Paket-Manager, der zum Installieren des Erweiterungsgenerators verwendet wird.
- [Visual Studio Code](https://code.visualstudio.com) zum Debuggen der Erweiterung.
- Die [Debugerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) von Azure Data Studio (optional). Damit können Sie Ihre Erweiterung testen, ohne sie zu packen und in Azure Data Studio zu installieren.
- Stellen Sie sicher, dass sich `azuredatastudio` in Ihrem Pfad befindet. Wenn Sie unter Windows arbeiten, wählen Sie in „setup.exe“ die Option `Add to Path` aus. Führen Sie unter macOS oder Linux über die Befehlspalette in Azure Data Studio **Befehl 'azuredatastudio' in PATH installieren** aus.

## <a name="install-the-extension-generator"></a>Installieren des Erweiterungsgenerators

Um den Prozess der Erstellung von Erweiterungen zu vereinfachen, haben wir einen [Erweiterungsgenerator](https://code.visualstudio.com/docs/extensions/yocode) erstellt, der Yeoman verwendet. Führen Sie den Code in der folgenden Eingabeaufforderung aus, um den Generator zu installieren:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-keymap-extension"></a>Entwickeln einer Tastenzuordnungserweiterung

So erstellen Sie eine Erweiterung:

1. Starten Sie den Erweiterungsgenerator mit dem folgenden Befehl:

   `yo azuredatastudio`

2. Wählen Sie **Neue Tastenzuordnung** aus der Liste der Erweiterungstypen aus:

   :::image type="content" source="media/keymap-extension/extension-generator.png" alt-text="Erweiterungsgenerator":::

3. Geben Sie den Erweiterungsnamen an (verwenden Sie für dieses Tutorial **ssmskeymap2**), und fügen Sie eine Beschreibung hinzu.

Durch Ausführen der oben genannten Schritte wird ein neuer Ordner erstellt. Öffnen Sie den Ordner in Visual Studio Code, und schon sind Sie bereit, selbst eine Erweiterung für Tastenzuordnungen zu erstellen!

### <a name="add-a-keyboard-shortcut"></a>Hinzufügen einer Tastenkombination

**Schritt 1: Suchen der zu ersetzenden Tastenkombinationen**

Nachdem unsere Erweiterung jetzt betriebsbereit ist, fügen Sie einige SSMS-Tastenkombinationen (oder Tastenzuordnungen) zu Azure Data Studio hinzu. Ich habe mich vom [Andy Mallons Cheatsheet](https://am2.co/2018/02/updated-cheat-sheet/) und von der Liste mit Tastenkombinationen von RedGate inspirieren lassen.

Einige fehlende Aspekte sind mir aufgefallen:

- Ausführen einer Abfrage mit aktiviertem tatsächlichem Ausführungsplan. Dies ist **STRG+M** in SSMS und verfügt über keine Zuordnung in Azure Data Studio.
- **STRG+UMSCHALT+E** als zweite Methode zum Ausführen einer Abfrage. Dies fehlte laut Benutzerfeedback.
- **ALT+F1** zum Ausführen von `sp_help`. Wir haben dies in Azure Data Studio hinzugefügt, aber da diese Tastenkombination bereits verwendet wurde, haben wir stattdessen **ALT+F2** zugeordnet.
- Umschalten des Vollbildmodus (**UMSCHALT+ALT+EINGABE**).
- **F8** zum Anzeigen der Ansicht **Objekt-Explorer** / **Server**.

Diese Tastenzuordnungen lassen sich einfach finden und ersetzen. Führen Sie *Tastenkombinationen öffnen* aus, um die Registerkarte **Tastenkombinationen** in Azure Data Studio anzuzeigen. Suchen Sie nach *Abfrage*, und wählen Sie **Tastenzuordnung ändern** aus. Wenn Sie mit dem Ändern der Tastenzuordnungen fertig sind, können Sie die aktualisierte Zuordnung in der Datei „keybindings.json“ anzeigen (führen Sie dazu *Tastenkombinationen öffnen* aus).

:::image type="content" source="media/keymap-extension/keyboard-shortcuts.png" alt-text="Tastenkombinationen":::

:::image type="content" source="media/keymap-extension/key-bindings-json.png" alt-text="Keybindings.json-Erweiterung":::

**Schritt 2: Hinzufügen von Tastenkombinationen zur Erweiterung**

Zum Hinzufügen von Tastenkombinationen zur Erweiterung öffnen Sie die Datei *package.json* (in der Erweiterung), und ersetzen Sie den Abschnitt `contributes` durch den folgenden Code:

```json
"contributes": {
  "keybindings": [
    {
      "key": "shift+cmd+e",
      "command": "runQueryKeyboardAction"
    },
    {
      "key": "ctrl+cmd+e",
      "command": "workbench.view.explorer"
    },
    {
      "key": "alt+f1",
      "command": "workbench.action.query.shortcut1"
    },
    {
      "key": "shift+alt+enter",
      "command": "workbench.action.toggleFullScreen"
    },
    {
      "key": "f8",
      "command": "workbench.view.connections"
    },
    {
      "key": "ctrl+m",
      "command": "runCurrentQueryWithActualPlanKeyboardAction"
    }
  ]
}
```

## <a name="test-your-extension"></a>Testen der Erweiterung

Stellen Sie sicher, dass sich `azuredatastudio` in Ihrem PATH befindet, indem Sie in Azure Data Studio im PATH-Befehl den Befehl „Install azuredatastudio“ ausführen.

Stellen Sie sicher, dass die Azure Data Studio-Debugerweiterung in Visual Studio Code installiert ist.

Drücken Sie **F5**, um Azure Data Studio mit ausgeführter Erweiterung im Debugmodus zu starten:

:::image type="content" source="media/keymap-extension/install-extension.png" alt-text="Installieren der Erweiterung":::

:::image type="content" source="media/keymap-extension/test-extension.png" alt-text="Testen der Erweiterung":::

Tastenzuordnungen gehören zu den Erweiterungen, die sich sehr schnell erstellen lassen, Ihre neue Erweiterung sollte jetzt also funktionsfähig und bereit für die Freigabe sein.

## <a name="package-your-extension"></a>Packen der Erweiterung

Um Ihre Erweiterung für andere Benutzer freizugeben, müssen Sie sie in eine einzelne Datei packen. Diese kann im Marketplace für Azure Data Studio-Erweiterungen veröffentlicht oder für Ihr Team oder Ihre Community freigegeben werden. Zu diesem Zweck müssen Sie über die Befehlszeile ein weiteres npm-Paket installieren:

```console
npm install -g vsce
```

Navigieren Sie zum Basisverzeichnis der Erweiterung, und führen Sie `vsce package` aus. Ich musste einige zusätzliche Zeilen einfügen, um Fehler im *vsce*-Tool zu vermeiden:

```json
"repository": {
    "type": "git",
    "url": "https://github.com/kevcunnane/ssmskeymap.git"
},
"bugs": {
    "url": "https://github.com/kevcunnane/ssmskeymap/issues"
},
```

Danach war meine ssmskeymap-0.1.0.vsix-Datei erstellt, und ich konnte die Erweiterung installieren und mit der ganzen Welt teilen!

:::image type="content" source="media/keymap-extension/extensions.png" alt-text="Installieren":::

## <a name="publish-your-extension-to-the-marketplace"></a>Veröffentlichen der Erweiterung im Marketplace

Da sich der Marketplace für Azure Data Studio-Erweiterungen noch in Bearbeitung befindet, wird die VSIX-Erweiterung an einem beliebigen Ort (z. B. auf einer GitHub-Releaseseite) gehostet. Anschließend führen einen Pull Request aus, der [diese JSON-Datei](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) mit Ihren Erweiterungsinformationen aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> - Erstellen eines Erweiterungsprojekts
> - Installieren des Erweiterungsgenerators
> - Erstellen der Erweiterung
> - Hinzufügen benutzerdefinierter Tastenzuordnungen zu einer Erweiterung
> - Testen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

Wir hoffen, dass dieser Artikel Sie auch dazu inspiriert, selbst eine Erweiterung für Azure Data Studio zu erstellen. Wir bieten Unterstützung für Dashboardeinblicke (ansprechende Diagramme, die in Ihrer SQL Server-Instanz ausgeführt werden können), eine Reihe von SQL-spezifischen APIs sowie eine Vielzahl von vorhandenen Erweiterungspunkten, die aus Visual Studio Code geerbt wurden.

Wenn Sie eine Idee haben, aber nicht sicher sind, wie Sie anfangen sollen, eröffnen Sie ein Issue, oder senden Sie einen Tweet an das Team: [azuredatastudio](https://twitter.com/azuredatastudio).

Sie können jederzeit im [Visual Studio Code-Erweiterungsleitfaden](https://code.visualstudio.com/docs/extensions/overview) nachschlagen, dieser enthält alle vorhandenen APIs und Muster.

Um zu erfahren, wie Sie mit T-SQL in Azure Data Studio arbeiten, führen Sie das Tutorial zum T-SQL-Editor aus:

> [!div class="nextstepaction"]
> [Verwenden des Transact-SQL-Editors zum Erstellen von Datenbankobjekten](../tutorial-sql-editor.md)
