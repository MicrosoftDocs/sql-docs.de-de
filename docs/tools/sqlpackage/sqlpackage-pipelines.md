---
title: SqlPackage in Entwicklungspipelines
description: Erfahren Sie, wie Sie mit „SqlPackage.exe“ Fehler in Pipelines zur Datenbankentwicklung beheben können, indem Sie die Nummer des installierten Builds überprüfen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 5bc3b1591dd05722f1b0d89a0113df4723f26d67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060995"
---
# <a name="sqlpackage-in-development-pipelines"></a>SqlPackage in Entwicklungspipelines

**sqlPackage.exe** ist ein Befehlszeilenhilfsprogramm, das verschiedene Aufgaben bei der Datenbankentwicklung automatisiert und in CI-/CD-Pipelines integriert werden kann.

## <a name="managed-virtual-environments"></a>Verwaltete virtuelle Umgebungen

Die virtuellen Umgebungen, die für die von GitHub Actions gehosteten Runner und Azure Pipelines-VM-Images verwendet werden, werden im GitHub-Repository [virtual-environments](https://github.com/actions/virtual-environments) verwaltet.  SqlPackage ist in der Umgebung `windows-latest` enthalten. Updates der Images erfolgen binnen weniger Wochen nach jedem neuen SqlPackage-Release.

## <a name="checking-the-sqlpackage-version"></a>Überprüfen der Version von SqlPackage

Während der Problembehandlung ist es wichtig zu wissen, welche Version von SqlPackage im Einsatz ist.  Die Erfassung dieser Informationen kann durch Hinzufügen eines Schritts zur Pipeline erfolgen, durch den SqlPackage mit dem Parameter `/version` ausgeführt wird.  Im Folgenden finden Sie Beispiele, die auf den von Microsoft und GitHub verwalteten Umgebungen basieren. Selbst gehostete Umgebungen können unterschiedliche Installationspfade für das Arbeitsverzeichnis haben.

### <a name="azure-pipelines"></a>Azure Pipelines

Durch Nutzung des Schlüsselworts [script](/azure/devops/pipelines/yaml-schema#script) in einer Azure-Pipeline kann ihr ein Schritt hinzugefügt werden, der die Versionsnummer von SqlPackage liefert.

```yaml
- script: sqlpackage.exe /version
  workingDirectory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  displayName: 'get sqlpackage version'
```

### <a name="github-actions"></a>GitHub Actions

Durch Nutzung des Schlüsselworts [run](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) in eine GitHub Action-Workflow kann einer GitHub-Aktion ein Schritt hinzugefügt werden, der die Versionsnummer von SqlPackage liefert.

```yaml
- name: get sqlpackage version
  working-directory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  run: ./sqlpackage.exe /version
```

:::image type="content" source="media/sqlpackage-pipelines-github-action.png" alt-text="Ausgabe von GitHub-Aktion mit Buildnummer 15.0.4897.1":::

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)