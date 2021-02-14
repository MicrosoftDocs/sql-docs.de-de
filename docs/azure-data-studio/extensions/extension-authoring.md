---
title: Erstellen von Erweiterungen
description: Sie können Funktionen mithilfe einer Erweiterung zu Azure Data Studio hinzufügen. Hier erfahren Sie, wie Sie eine Erweiterung erstellen und im Erweiterungskatalog veröffentlichen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/26/2020
ms.openlocfilehash: a7cb4ede1c8ef11d55a40b5b72d825ba02c5a550
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048570"
---
# <a name="extend-functionality-by-creating-azure-data-studio-extensions"></a>Erweitern der Funktionalität durch Erstellen von Erweiterungen für Azure Data Studio

Erweiterungen in Azure Data Studio bieten eine einfache Möglichkeit, der Basisinstallation von Azure Data Studio weitere Funktionen hinzuzufügen.

Erweiterungen werden vom Azure Data Studio-Team (Microsoft) sowie von der Drittanbietercommunity (Ihnen!) bereitgestellt.

## <a name="author-an-extension"></a>Erstellen einer Erweiterung

Wenn Sie Azure Data Studio erweitern möchten, können Sie eine eigene Erweiterung erstellen und im Erweiterungskatalog veröffentlichen.

### <a name="write-an-extension"></a>Erstellen einer Erweiterung

#### <a name="prerequisites"></a>Voraussetzungen

Zum Entwickeln einer Erweiterung muss [Node.js](https://nodejs.org/) in Ihrem `$PATH` installiert und verfügbar sein. Node.js enthält npm, den Node.js-Paket-Manager, der zum Installieren des Erweiterungsgenerators verwendet wird.

Zum Erstellen der neuen Erweiterung können Sie den Erweiterungs-Generator für Azure Data Studio verwenden. Der [Erweiterungs-Generator](https://www.npmjs.com/package/generator-azuredatastudio) von Yeoman ist ein guter Ausgangspunkt für Erweiterungsprojekte. Geben Sie den folgenden Befehl in eine Eingabeaufforderung ein, um den Generator zu starten:

```console
npm install -g yo generator-azuredatastudio
yo azuredatastudio
```

Ausführliche Anleitungen zu den ersten Schritten mit Ihrer Erweiterungsvorlage finden Sie unter [keymap-Erweiterung](keymap-extension.md). Dort erhalten Sie schrittweise Anleitungen zum Erstellen einer Erweiterung.

### <a name="extensibility-references"></a>Referenzen zur Erweiterbarkeit

Weitere Informationen zur Erweiterbarkeit von Azure Data Studio finden Sie in der [Erweiterbarkeitsübersicht](../extensibility.md). [Hier](https://github.com/Microsoft/azuredatastudio/tree/main/samples) finden Sie auch Beispiele für die Verwendung der API.

## <a name="debug-an-extension"></a>Debuggen einer Erweiterung

Sie können die neue Erweiterung mithilfe der Visual Studio Code-Erweiterung [Azure Data Studio Debug](https://github.com/kevcunnane/sqlops-debug) debuggen.

So debuggen Sie die Erweiterung:

1. Öffnen Sie Ihre Erweiterung mit [Visual Studio Code](https://code.visualstudio.com/).
2. Installieren Sie die Azure Data Studio-Debug-Erweiterung.
3. Drücken Sie **F5**, oder wählen Sie das **Debugsymbol** aus, und klicken Sie dann auf **Starten**.
4. Eine neue Instanz von Azure Data Studio wird in einem speziellen Modus (Extension Development Host, Erweiterungsentwicklungshost) gestartet. Diese neue Instanz erkennt nun Ihre Erweiterung.

## <a name="create-an-extension-package"></a>Erstellen eines Erweiterungspakets

Nachdem Sie Ihre Erweiterung geschrieben haben, müssen Sie ein VSIX-Paket erstellen, das in Azure Data Studio installiert wird. Sie können [vscode-vsce](https://github.com/Microsoft/vscode-vsce) (Visual Studio Code-Erweiterungen) zum Erstellen des VSIX-Pakets verwenden.

```console
npm install -g vsce
cd myExtensionName
vsce package
# The myExtensionName.vsix file has now been generated
```

Mit einem VSIX-Paket können Sie Ihre Erweiterung lokal und privat freigeben, indem Sie die VSIX-Datei freigeben und den Befehl **Extensions: Install From VSIX File** (Erweiterungen: Aus VSIX-Datei installieren) aus der Befehlspalette ausführen, um die Erweiterung in Azure Data Studio zu installieren.

## <a name="publish-an-extension"></a>Veröffentlichen einer Erweiterung

So veröffentlichen Sie die neue Erweiterung in Azure Data Studio:

1. Fügen Sie dem [Erweiterungskatalog](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) Ihre Erweiterung hinzu.
2. Derzeit werden keine Drittanbietererweiterungen unterstützt. Anstatt die Erweiterung herunterzuladen, bietet Azure Data Studio die Option, eine Downloadseite aufzurufen. Zum Festlegen einer Downloadseite für Ihre Erweiterung müssen Sie den Wert der Ressource **Microsoft.AzureDataStudio.DownloadPage** festlegen.
3. Erstellen Sie einen Pull Request für den release/extensions-Branch.
4. Senden Sie eine Überprüfungsanforderung an das Team.

Ihre Erweiterung wird überprüft und dem Erweiterungskatalog hinzugefügt.

### <a name="publish-extension-updates"></a>Veröffentlichen von Updates für Erweiterungen

Der Prozess zum Veröffentlichen von Updates entspricht der Veröffentlichung der Erweiterung. Stellen Sie sicher, dass die Version in „package.json“ aktualisiert wird.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Tutorials zum Entwickeln von Erweiterungen finden Sie ausführliche Informationen zum Einstieg:

- [Tutorial für eine Tastenzuordnungserweiterung](keymap-extension.md)
- [Tutorial für eine Dashboarderweiterung](dashboard-extension.md)
- [Tutorial für eine Notebook-Erweiterung](notebook-extension.md)
- [Tutorial für eine Jupyter Book-Erweiterung](jupyter-book-extension.md)
- [Tutorial für eine Assistenten-Erweiterung](wizard-extension.md)
