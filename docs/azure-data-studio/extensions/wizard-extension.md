---
title: Entwickeln einer Assistenten-Erweiterung
description: Dieses Tutorial veranschaulicht, wie Sie eine Assistenten-Erweiterung entwickeln, um Azure Data Studio benutzerdefinierte Funktionen hinzuzufügen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: ef9938d87de3963ab7757e0d4ec3af0e01d6f33a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048360"
---
# <a name="create-an-azure-data-studio-wizard-extension"></a>Entwickeln einer Azure Data Studio-Erweiterung für einen Assistenten

Dieses Tutorial veranschaulicht, wie Sie eine neue **Azure Data Studio-Erweiterung für einen Assistenten** entwickeln. Die Erweiterung stellt einen Assistenten für die Interaktion mit Azure Data Studio-Benutzern bereit.

In diesem Artikel wird Folgendes behandelt:
> [!div class="checklist"]
> - Installieren des Erweiterungsgenerators
> - Erstellen der Erweiterung
> - Hinzufügen eines benutzerdefinierten Assistenten zur Erweiterung
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

Um den Prozess der Erstellung von Erweiterungen zu vereinfachen, haben wir einen [Erweiterungsgenerator](https://code.visualstudio.com/docs/extensions/yocode) erstellt, der Yeoman verwendet. Um diesen zu installieren, führen Sie an der Eingabeaufforderung folgenden Befehl aus:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-wizard-extension"></a>Entwickeln einer Assistenten-Erweiterung

### <a name="introduction-to-wizards"></a>Einführung in Assistenten

Assistenten sind ein Benutzeroberfläche, die Benutzern nacheinander Seiten anzeigt, die diese ausfüllen müssen, um eine Aufgabe zu erledigen. Zu den gängigen Beispielen zählen Softwaresetup-Assistenten und Assistenten zur Problembehandlung. Beispiel:

:::image type="content" source="media/wizard-extension/dacpac-wizard.gif" alt-text="Dacpac-Assistent":::

Da Assistenten für die Arbeit mit Daten und die Erweiterung des Funktionsumfangs von Azure Data Studio häufig nützlich sind, enthält Azure Data Studio APIs, mit denen Sie benutzerdefinierte Assistenten erstellen können. Hier wird ausführlich erläutert, wie Sie eine Assistenten-Vorlage generieren und bearbeiten, um einen benutzerdefinierten Assistenten zu erstellen.

### <a name="run-the-extension-generator"></a>Ausführen des Erweiterungsgenerators

So erstellen Sie eine Erweiterung:

1. Starten Sie den Erweiterungsgenerator mit dem folgenden Befehl:

   `yo azuredatastudio`

2. Wählen Sie **New Wizard or Dialog** (Neuer Assistent/Neues Dialogfeld) aus der Liste der Erweiterungstypen aus. Wählen Sie dann **Assistent** und die Vorlage **Erste Schritte** aus.

3. Geben Sie den Erweiterungsnamen an (verwenden Sie für dieses Tutorial **My Test Extension**), und fügen Sie eine Beschreibung hinzu.

    :::image type="content" source="media/wizard-extension/extension-generator.png" alt-text="Erweiterungsgenerator":::

Durch Ausführen der oben genannten Schritte wird ein neuer Ordner erstellt. Öffnen Sie den Ordner in Visual Studio Code. Nun sind Sie bereit, selbst eine Assistenten-Erweiterung zu entwickeln.

### <a name="run-the-extension"></a>Ausführen der Erweiterung

Führen Sie die Erweiterung aus, um den Umfang der Assistenten-Vorlage kennenzulernen. Stellen Sie vor der Ausführung sicher, dass die **Azure Data Studio-Debugerweiterung** in Visual Studio Code installiert ist.

Drücken Sie **F5** in Visual Studio Code, um Azure Data Studio mit ausgeführter Erweiterung im Debugmodus zu starten. Führen Sie dann in Azure Data Studio den Befehl **Assistent starten** über die Befehlspalette (STRG+UMSCHALT+P) im neuen Fenster aus. Dadurch wird der Standard-Assistent dieser Erweiterung gestartet:

:::image type="content" source="media/wizard-extension/wizard-template.gif" alt-text="Assistenten-Vorlage":::

Als Nächstes erfahren Sie, wie dieser Standard-Assistent geändert werden kann.

### <a name="develop-the-wizard"></a>Entwickeln des Assistenten

Die wichtigsten Dateien für den Einstieg in die Erweiterungsentwicklung sind `package.json`, `src/main.ts` und `vsc-extension-quickstart.md`.

- `package.json`: Dies ist die Manifestressource, in der der Befehl **Assistent starten** registriert ist. Dort wird auch `main.ts` als Einstiegspunkt für das Hauptprogramm deklariert.
- `main.ts`: Enthält den Code zum Hinzufügen von Benutzeroberflächenelementen wie Seiten, Text und Schaltflächen zum Assistenten
- `vsc-extension-quickstart.md`: Enthält technische Dokumentation, die bei der Entwicklung als Referenz dienen kann

Nehmen Sie eine Änderung am Assistenten vor: Fügen Sie eine vierte, leere Seite hinzu. Ändern Sie `src/main.ts` wie unten dargestellt. Beim Starten des Assistenten sollte dann eine zusätzliche Seite angezeigt werden.

:::image type="content" source="media/wizard-extension/wizard-main.png" alt-text="Hauptbereich des Assistenten":::
Wenn Sie mit der Vorlage vertraut sind, finden Sie hier einige zusätzliche Ideen:

- Fügen Sie der neuen Seite eine Schaltfläche mit einer Breite von 300 hinzu.
- Fügen Sie eine flexible Komponente hinzu, in die die Schaltfläche integriert wird.
- Fügen Sie der Schaltfläche eine Aktion hinzu. Beispielsweise könnte ein Dialogfeld zum Öffnen einer Datei oder des Abfrage-Editors geöffnet werden, wenn auf die Schaltfläche geklickt wird.

## <a name="package-your-extension"></a>Packen der Erweiterung

Um Ihre Erweiterung für andere Benutzer freizugeben, müssen Sie sie in eine einzige Datei packen. Diese kann im Marketplace für Azure Data Studio-Erweiterungen veröffentlicht oder für Ihr Team oder Ihre Community freigegeben werden. Zu diesem Zweck müssen Sie über die Befehlszeile ein weiteres npm-Paket installieren:

```console
npm install -g vsce
```

Bearbeiten Sie `README.md` wie gewünscht, und navigieren Sie dann zum Basisverzeichnis der Erweiterung, und führen Sie `vsce package` aus. Optional können Sie ein Repository mit ihrer Erweiterung verknüpfen oder den Vorgang ohne Repository fortsetzen. Um ein Repository hinzuzufügen, fügen Sie der Datei `package.json` eine ähnliche Zeile hinzu.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Nachdem diese Zeilen hinzugefügt wurden, wurde die Datei „my-test-extension-0.0.1.vsix“ erstellt und ist nun für die Installation in Azure Data Studio bereit.

:::image type="content" source="media/wizard-extension/install-vsix.png" alt-text="Installieren über VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>Veröffentlichen der Erweiterung im Marketplace

Der Marketplace für Azure Data Studio-Erweiterungen ist noch nicht vollständig implementiert. Zurzeit sieht der Prozess folgendermaßen aus: Sie hosten die VSIX-Datei der Erweiterung an einem beliebigen Ort (z.B. auf einer GitHub-Releaseseite) und führen einen Pull Request aus, mit dem [diese JSON-Datei](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) mit Ihren Erweiterungsinformationen aktualisiert wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> - Installieren des Erweiterungsgenerators
> - Erstellen der Erweiterung
> - Hinzufügen eines benutzerdefinierten Assistenten zur Erweiterung
> - Testen der Erweiterung
> - Packen der Erweiterung
> - Veröffentlichen der Erweiterung im Marketplace

Wir hoffen, dass dieser Artikel Sie auch dazu inspiriert, selbst eine Erweiterung für Azure Data Studio zu erstellen. Wir bieten Unterstützung für Dashboardeinblicke (ansprechende Diagramme, die in Ihrer SQL Server-Instanz ausgeführt werden können), eine Reihe von SQL-spezifischen APIs sowie eine Vielzahl von vorhandenen Erweiterungspunkten, die aus Visual Studio Code geerbt wurden.

Wenn Sie eine Idee haben, aber nicht sicher sind, wo Sie anfangen sollen, eröffnen Sie ein Issue, oder senden Sie einen Tweet an das Team: [azuredatastudio](https://twitter.com/azuredatastudio).

Sie können jederzeit im [Visual Studio Code-Erweiterungsleitfaden](https://code.visualstudio.com/docs/extensions/overview) nachschlagen, dieser enthält alle vorhandenen APIs und Muster.

Um zu erfahren, wie Sie mit T-SQL in Azure Data Studio arbeiten, führen Sie das Tutorial zum T-SQL-Editor aus:

> [!div class="nextstepaction"]
> [Verwenden des Transact-SQL-Editors zum Erstellen von Datenbankobjekten](../tutorial-sql-editor.md)
