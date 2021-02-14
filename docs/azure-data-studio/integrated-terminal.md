---
title: Integriertes Terminal
description: Erfahren Sie, wie Sie ein mit Azure Data Studio integriertes Terminal öffnen. Ein integriertes Terminal kann sich als praktischer als ein separates Terminal erweisen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 054bfd71962a5775374e8faa567dc24242d846c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048340"
---
# <a name="integrated-terminal"></a>Integriertes Terminal

In Azure Data Studio können Sie ein integriertes Terminal öffnen und am Stamm Ihres Arbeitsbereichs beginnen. Das ist möglicherweise ganz praktisch, da Sie weder zwischen Fenstern wechseln noch den Status eines vorhandenen Terminals ändern müssen, um eine einfache Befehlszeilenaufgabe auszuführen.

So öffnen Sie das Terminal:

* Verwenden Sie die Tastenkombination **STRG+`** mit dem Backtick.
* Verwenden Sie den Menübefehl **Ansicht** | **Integriertes Terminal**.
* Verwenden Sie in der **Befehlspalette** (**STRG+UMSCHALT+P**) den Befehl **Ansicht:Integriertes Terminal umschalten**.

![Terminal](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> Sie können weiterhin eine externe Shell mit dem Explorer-Befehl **In Eingabeaufforderung öffnen** (**In Terminal öffnen** unter Mac oder Linux) öffnen, wenn Sie lieber außerhalb von Azure Data Studio arbeiten möchten.

## <a name="managing-multiple-terminals"></a>Verwalten mehrerer Terminals

Sie können mehrere Terminals erstellen, die an verschiedenen Orten geöffnet sind, und ganz bequem zwischen diesen Terminals navigieren. Terminalinstanzen können Sie hinzufügen, indem Sie auf das Pluszeichen oben rechts im Panel **TERMINAL** klicken oder den Befehl **STRG+UMSCHALT+`** eingeben. Damit wird ein weiterer Eintrag in der Dropdownliste erstellt, über den Sie zwischen den Terminalinstanzen wechseln können.

![Mehrere Terminals](media/integrated-terminal/terminal-multiple-instances.png)

Entfernen Sie Terminalinstanzen, indem Sie auf die Schaltfläche mit dem Papierkorb klicken.

> [!TIP]
> Wenn Sie häufig mehrere Terminals verwenden, können Sie für die Befehle `focusNext`, `focusPrevious` und `kill` die im Abschnitt [Tastenzuordnungen](#key-bindings) beschriebenen Tastenzuordnungen hinzufügen, um eine Navigation über die Tastatur zu ermöglichen.

## <a name="configuration"></a>Konfiguration

Die verwendete Shell verwendet standardmäßig `$SHELL` unter Linux und macOS, PowerShell unter Windows 10 und cmd.exe unter früheren Versionen von Windows. Diese Einstellungen können Sie manuell überschreiben, indem Sie `terminal.integrated.shell.*` in [settings](settings.md) festlegen. Argumente können unter Linux und macOS mithilfe der Einstellung `terminal.integrated.shellArgs.*` an die Terminalshell übergeben werden.

### <a name="windows"></a>Windows

Bei der ordnungsgemäßen Konfiguration der Shell unter Windows kommt es darauf an, die richtige ausführbare Datei zu finden und die Einstellung zu aktualisieren. Im Folgenden finden Sie eine Liste mit gängigen ausführbaren Shelldateien und deren Standardspeicherorten:

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> Damit die Verwendung als integriertes Terminal möglich ist, muss es sich bei der ausführbaren Shelldatei um eine Konsolenanwendung handeln, sodass `stdin/stdout/stderr` umgeleitet werden kann.

> [!TIP]
> Die integrierte Terminalshell wird mit den Berechtigungen von Azure Data Studio ausgeführt. Wenn Sie einen Shellbefehl mit erhöhten Rechten (Administratorrechten) oder anderen Berechtigungen ausführen müssen, können Sie Plattformdienstprogramme wie `runas.exe` in einem Terminal verwenden.

### <a name="shell-arguments"></a>Shellargumente

Sie können Shellargumente an die Shell übergeben, wenn diese gestartet wird.

Um beispielsweise die Ausführung von Bash als Anmelde-Shell (wodurch `.bash_profile` ausgeführt wird) zu ermöglichen, übergeben Sie das Argument `-l` (mit doppelten Anführungszeichen):

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>Einstellungen für die Terminalanzeige

Die Schriftart und die Zeilenhöhe für das integrierte Terminal können Sie mit den folgenden Einstellungen anpassen:

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a name="terminal-key-bindings"></a><a id="key-bindings"></a>Tastenzuordnungen für das Terminal

Der Befehl **Ansicht: Integriertes Terminal umschalten** ist der Tastenkombination **STRG+`** zugeordnet, sodass Sie das Panel des integrierten Terminals schnell ein- und ausblenden können.

Im Folgenden finden Sie die Tastenkombinationen zum schnellen Navigieren im integrierten Terminal:

|Schlüssel|Get-Help|  
|---|---|  
|**STRG+\`**|Integriertes Terminal anzeigen|  
|**STRG+UMSCHALT+\`**|Neues Terminal erstellen|  
|**STRG+NACH-OBEN**|Bildlauf nach oben|  
|**STRG+NACH-UNTEN**|Bildlauf nach unten|  
|**STRG+BildAuf**|Bildlauf nach oben für Seite|  
|**STRG+BildAb**|Bildlauf nach unten für Seite|  
|**STRG+POS1**|Bildlauf nach oben|  
|**STRG+ENDE**|Bildlauf nach unten|  
|**STRG+K**|Terminal löschen|  

Es sind weitere Terminalbefehle verfügbar, denen Sie die gewünschten Tastenkombinationen zuordnen können.

Sie lauten wie folgt:

* `workbench.action.terminal.focus`: Blendet das Terminal ein. Dieser Befehl entspricht dem Umschaltbefehl, blendet jedoch das Terminal ein, statt es auszublenden, sofern es angezeigt wird.
* `workbench.action.terminal.focusNext`: Blendet die nächste Terminalinstanz ein.
* `workbench.action.terminal.focusPrevious`: Blendet die vorherige Terminalinstanz ein.
* `workbench.action.terminal.kill`: Entfernt die aktuelle Terminalinstanz.
* `workbench.action.terminal.runSelectedText`: Führt den ausgewählten Text in der Terminalinstanz aus.
* `workbench.action.terminal.runActiveFile`: Führt die aktive Datei in der Terminalinstanz aus.

### <a name="run-selected-text"></a>Ausführen von ausgewähltem Text

Wenn Sie den Befehl `runSelectedText` verwenden möchten, wählen Sie in einem Editor Text aus und führen Sie den Befehl **Terminal: Ausgewählten Text im aktiven Terminal ausführen** über die **Befehlspalette** (**STRG+UMSCHALT+P**) aus. Das Terminal versucht, den ausgewählten Text auszuführen:

![Ausführen von ausgewähltem Text](media/integrated-terminal/terminal_run_selected.png)

Wenn im aktiven Editor kein Text ausgewählt wird, wird im Terminal die Zeile, in der sich der Cursor befindet, ausgeführt.

### <a name="copy--paste"></a>Kopieren und Einfügen

Die Tastenzuordnungen zum Kopieren und Einfügen entsprechen den Standards der jeweiligen Plattform:

* Linux: **STRG+UMSCHALT+C** und **STRG+UMSCHALT+V**
* Mac: **CMD+C** und **CMD+V**
* Windows: **STRG+C** und **STRG+V**

### <a name="find"></a>Suchen

Das integrierte Terminal verfügt über eine einfache Suchfunktion, die mit der Tastenkombination **STRG+F** aufgerufen werden kann.

Wenn Sie mit **STRG+F** unter Linux und Windows nicht das Suchwidget aufrufen, sondern stattdessen zur Shell wechseln möchten, müssen Sie die Tastenzuordnung wie folgt entfernen on Linux and Windows:

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>Umbenennen von Terminalsitzungen

Sitzungen mit dem integrierten Terminal können nun mit dem Befehl **Terminal: (`workbench.action.terminal.rename`) umbenennen** umbenannt werden. Der neue Name wird in der Dropdownliste für die Terminalauswahl angezeigt.

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>Erzwingen, dass Tastenzuordnungen an das Terminal weitergeleitet werden

Während der Fokus auf dem integrierten Terminal liegt, funktionieren viele Tastenzuordnungen nicht, da die Tastatureingaben an das Terminal selbst weitergeleitet und von diesem verwendet werden. Mit der Einstellung `terminal.integrated.commandsToSkipShell` kann dies umgangen werden. Sie enthält eine Reihe von Befehlsnamen, deren Tastenzuordnungen die Verarbeitung durch die Shell überspringen und stattdessen vom Tastenzuordnungssystem Azure Data Studio verarbeitet werden. Standardmäßig betrifft dies alle Tastenzuordnungen des Terminals sowie bestimmte weniger häufig verwendete Tastenzuordnungen.

