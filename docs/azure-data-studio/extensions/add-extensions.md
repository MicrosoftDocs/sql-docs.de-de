---
title: Hinzufügen von Erweiterungen
description: Erfahren Sie, wie Sie Funktionen zu Azure Data Studio hinzufügen, indem Sie Erweiterungen auswählen und installieren, die von Microsoft und Drittanbietern bereitgestellt werden.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 10/03/2019
ms.openlocfilehash: 3cc965ffd169d53e59706ca4ba68f2bcdddb5aa9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052120"
---
# <a name="extend-the-functionality-of-azure-data-studio"></a>Erweitern der Funktionalität von Azure Data Studio

Erweiterungen in Azure Data Studio bieten eine einfache Möglichkeit, der Basisinstallation von Azure Data Studio weitere Funktionen hinzuzufügen.

Erweiterungen werden vom Azure Data Studio-Team (Microsoft) sowie von der Drittanbietercommunity (Ihnen!) bereitgestellt. Weitere Informationen zum Erstellen von Erweiterungen finden Sie unter [Erweiterungserstellung](./extension-authoring.md).

## <a name="add-azure-data-studio-extensions"></a>Hinzufügen von Azure Data Studio-Erweiterungen

1. Greifen Sie auf die verfügbaren Erweiterungen zu, indem Sie auf das Symbol „Erweiterungen“ klicken oder **Erweiterungen** im Menü **Ansicht** auswählen. Sie können den Befehl **Sicht: Erweiterungen anzeigen** verwenden, der in der **Befehlspalette** (F1 oder `Ctrl+Shift+P`) verfügbar ist.

    ![Erweiterungs-Manager-Symbol](media/add-extensions/extension-manager-icon.png)

    Sie können auch schnell auf den Erweiterungs-Manager zugreifen, indem Sie `Ctrl+Shift+X` (Windows/Linux) oder `Command+Shift+X` (Mac) drücken.

2. Wählen Sie eine verfügbare Erweiterung aus, um deren Details anzuzeigen.

    ![Details zur Erweiterung](media/add-extensions/extension-details.png)

3. Wählen Sie die gewünschte Erweiterung aus, und **installieren** Sie diese.

4. Klicken Sie nach der Installation auf **Erneut laden**, um die Erweiterung in Azure Data Studio zu aktivieren (nur bei der ersten Installation einer Erweiterung erforderlich).

Wenn Sie Probleme beim Zugriff auf den Erweiterungs-Manager von Azure Data Studio haben, können Sie die benötigte Erweiterung aus unserem [GitHub-Wiki](https://github.com/microsoft/azuredatastudio/wiki/List-of-Extensions) herunterladen.

## <a name="manage-extensions"></a>Verwalten von Erweiterungen

### <a name="list-installed-extensions"></a>Auflisten installierter Erweiterungen

In der Standardansicht „Erweiterungen“ werden die Erweiterungen angezeigt, die derzeit aktiviert sind, alle für Sie empfohlenen Erweiterungen sowie ein reduzierter Container aller zurzeit deaktivierten Erweiterungen. Der Befehl **Erweiterungen: Installierte Erweiterungen anzeigen**, der in der **Befehlspalette** oder im Dropdownmenü **Weitere Aktionen**`(...)` verfügbar ist, zeigt eine Liste aller installierten Erweiterungen an, einschließlich deaktivierter Erweiterungen.

### <a name="uninstall-an-extension"></a>Deinstallieren einer Erweiterung

Um eine Erweiterung zu deinstallieren, klicken Sie auf das Zahnradsymbol rechts neben einem Erweiterungseintrag, und wählen Sie in dem Dropdownmenü **Deinstallieren** aus. Dadurch wird die ausgewählte Erweiterung deinstalliert, und Sie werden aufgefordert, Azure Data Studio neu zu laden.

 ![Dropdown „Erweiterung“](media/add-extensions/extension-gear-dropdown.png)

### <a name="disable-an-extension"></a>Deaktivieren einer Erweiterung

Sie können eine Erweiterung vorübergehend deaktivieren, anstatt sie dauerhaft zu entfernen. Sie können eine Erweiterung entweder in allen Azure Data Studio-Sitzungen deaktivieren (**Deaktivieren**) oder nur für Ihren aktuellen Arbeitsbereich (**Deaktivieren (Arbeitsbereich)** ). Sie können auch alle Ihrer aktuell installierten Erweiterungen über die **Befehlspalette** mit den Befehlen **Erweiterungen: Alle Erweiterungen deaktivieren** und **Erweiterungen: Ale Erweiterungen deaktivieren (Arbeitsbereich)** deaktivieren.

### <a name="enable-an-extension"></a>Aktivieren einer Erweiterung

Wenn eine Erweiterung deaktiviert wurde, befindet sie sich im Abschnitt **Deaktiviert** der Liste der Erweiterungen und ist als **_Deaktiviert_ *_ gekennzeichnet. Sie können sie mit den Befehlen _* Aktivieren** oder **Aktivieren (Arbeitsbereich)** im Dropdownmenü erneut aktivieren. Mit der **Befehlspalette** können Sie auch alle Erweiterungen mit den Befehlen **Erweiterungen: Alle Erweiterungen aktivieren** und **Erweiterungen: Ale Erweiterungen aktivieren (Arbeitsbereich)** aktivieren.

![Aktivieren von Erweiterungen](media/add-extensions/extensions-enable.png)

### <a name="updating-an-extension"></a>Aktualisieren einer Erweiterung

Azure Data Studio sucht automatisch nach Updates für alle Ihrer installierten Erweiterungen und installiert diese. Wenn Sie die automatische Aktualisierungsfunktion deaktivieren möchten, können Sie dies mit dem Befehl **Erweiterungen: Automatische Aktualisierung von Erweiterungen deaktivieren**.

Um eine Erweiterung manuell zu aktualisieren, können Sie mit dem Befehl **Erweiterungen: Veraltete Erweiterungen anzeigen** nach Aktualisierungen für Erweiterungen suchen. Dieser Befehl durchsucht Ihre Liste der Erweiterungen unter Verwendung des Filters `@outdated`. Dadurch werden alle verfügbaren Updates für alle zurzeit installierten Erweiterungen angezeigt. Klicken Sie bei einer veralteten Erweiterung auf die Schaltfläche **Update**, woraufhin das Update installiert wird. Sie werden dann aufgefordert, Azure Data Studio erneut zu laden. Sie können auch alle Ihre veralteten Erweiterungen gleichzeitig mit dem Befehl **Erweiterungen: Alle Erweiterungen aktualisieren** aktualisieren.

Der Befehl **Erweiterungen: Auf Updates für Erweiterungen überprüfen** ist eine weitere Möglichkeit, um zu überprüfen, für welche Ihrer Erweiterungen Updates verfügbar sind.

## <a name="install-from-a-vsix"></a>Installieren aus einer VSIX

Sie können eine Azure Data Studio-Erweiterung, die in einer `.vsix`-Datei verpackt ist, mithilfe des Befehls **Aus VSIX installieren** im Befehlsdropdown der Ansicht „Erweiterungen“ manuell installieren oder mit dem Befehl **Erweiterungen: Aus VSIX installieren** in der Befehlspalette und Zeigen auf die `.vsix`-Datei der Erweiterung.

## <a name="access-installed-azure-data-studio-extensions"></a>Zugreifen auf installierte Azure Data Studio-Erweiterungen

Jede Erweiterung verbessert die Benutzerfreundlichkeit in Azure Data Studio auf eine andere Weise. Infolgedessen kann der Einstiegspunkt für Erweiterungen unterschiedlich sein. Informationen dazu, wie auf die Features zugegriffen werden kann, finden Sie in der Dokumentation der jeweiligen installierten Erweiterung.

## <a name="extensions-view-filters"></a>Filter für die Ansicht „Erweiterungen“

Das Suchfeld der Ansicht „Erweiterungen“ unterstützt Filter, die Ihnen beim Suchen und Verwalten von Erweiterungen helfen. Die Befehle **Installierte Erweiterungen anzeigen** und **Empfohlene Erweiterungen anzeigen** verwenden Filter wie `@installed` und `@recommended` im Suchfeld.

Eine vollständige Liste aller Filter und Sortierbefehle können Sie anzeigen, indem Sie im Suchfeld der Ansicht „Erweiterungen“ „@“ eingeben und durch die Vorschläge navigieren:

![Erweiterungssortierung](media/add-extensions/extension-sort.png)

Im Folgenden finden Sie die Filter der Ansicht „Erweiterungen“:

- `@builtin`: Erweiterungen anzeigen, die in Azure Data Studio enthalten sind. Gruppiert nach Typ (Programmiersprachen, Designs usw.).
- `@disabled`: Deaktivierte installierte Erweiterungen anzeigen.
- `@enabled`: Aktivierte installierte Erweiterungen anzeigen. Erweiterungen können einzeln aktiviert/deaktiviert werden.
- `@installed`: Installierte Erweiterungen anzeigen.
- `@outdated`: Veraltete installierte Erweiterungen anzeigen. Im Marketplace ist eine neuere Version verfügbar.
- `@recommended`: Empfohlene Erweiterungen anzeigen. Gruppiert als arbeitsbereichsspezifisch oder für allgemeine Verwendung.
- `@category`: Erweiterungen anzeigen, die der angegebenen Kategorie angehören. Im Folgenden finden Sie ein paar der unterstützten Kategorien. Um eine vollständige Liste anzuzeigen, geben Sie @category ein, und befolgen Sie die Optionen in der Vorschlagsliste:
    - `@category:themes`
    - `@category:formatters`
    - `@category:snippets`: Diese Filter können ebenfalls kombiniert werden. Beispielsweise zeigt `@installed @category:themes` alle installierten Designs an.

Wenn kein Filter angegeben wird, werden in der Ansicht „Erweiterungen“ die aktuell installierten und empfohlenen Erweiterungen angezeigt.

### <a name="sorting"></a>Sortierung

Sie können Erweiterungen mit dem Filter `@sort` sortieren, der die folgenden Werte annehmen kann:

- `installs`: Sortieren nach Anzahl der Installationen im Erweiterungenkatalog in absteigender Reihenfolge.
- `rating`:Sortieren nach Bewertung im Erweiterungenkatalog (1–5 Sterne) in absteigender Reihenfolge.
- `name` Alphabetisch sortieren nach Erweiterungsnamen.

## <a name="common-questions"></a>Häufig gestellte Fragen

### <a name="where-are-extensions-installed"></a>Wo werden Erweiterungen installiert?

Erweiterungen werden in einem Ordner „Erweiterungen“ pro Benutzer installiert. Je nach Ihrer Plattform befindet sich der Speicherort im folgenden Ordner:

- Windows `%USERPROFILE%\.azuredatastudio\extensions`
- macOS `~/.azuredatastudio/extensions`
- Linux `~/.azuredatastudio/extensions`
