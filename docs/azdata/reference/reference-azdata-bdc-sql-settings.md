---
title: 'azdata bdc sql settings: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc sql settings-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 208f136f246cbcd6af612fbd4867a2a50c0f0cae
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567296"
---
# <a name="azdata-bdc-sql-settings"></a>azdata bdc sql settings

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql settings**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc sql settings set](#azdata-bdc-sql-settings-set) | Damit werden Einstellungen auf der Dienstebene von SQL festgelegt.
[azdata bdc sql settings show](#azdata-bdc-sql-settings-show) | Damit werden Einstellungen auf der Dienstebene von SQL und optional SQL-Einstellungen für angegebene Ressourcen angezeigt.

## <a name="azdata-bdc-sql-settings-set"></a>azdata bdc sql settings set
Bietet die Möglichkeit, eine Einstellung auf Dienst- oder Ressourcenebene festzulegen. Geben Sie hierzu den vollständigen Namen der Einstellung und den Wert an. Auf den aktuell ausgeführten BDC wird die Einstellung nicht angewendet. Wenn Sie dies möchten, führen Sie ein Upgrade aus.
```bash
azdata bdc sql settings set --settings -s 
                        
```
### <a name="examples"></a>Beispiele
Aktivieren Sie den SQL-Agent in der SQL Server-Masterinstanz.
```bash 
azdata bdc sql settings set --settings mssql.sqlagent.enabled=true --resources master 
``` 
Legen Sie die Anzahl der CPUs für SQL Server im Datenpool fest.
```bash 
azdata bdc sql settings set --settings mssql.numberOfCpus=10 –resources data-0 
``` 

### <a name="required-parameters"></a>Erforderliche Parameter
#### `--settings -s`
Damit wird der konfigurierte Wert für die angegebenen Einstellungen festgelegt. Mithilfe einer durch Trennzeichen getrennten Liste können mehrere Einstellungen festgelegt werden.
### <a name="optional-parameters"></a>Optionale Parameter 
#### `--resources` 
Damit werden die angegebenen Einstellungen für die bereitgestellten Ressourcen festgelegt. Ressourcen können in Form einer durch Trennzeichen getrennten Liste aufgeführt werden. 

### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.

## <a name="azdata-bdc-sql-settings-show"></a>azdata bdc sql settings show
Damit werden die BDC-Einstellungen auf Dienstebene (optional Ressourcenebene) für `sql` angezeigt. Mit diesem Befehl werden standardmäßig vom Benutzer konfigurierte Einstellungen auf Dienstebene angezeigt. Dabei sind Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und konfigurierbare Einstellungen), konfigurierbare Einstellungen oder ausstehende Einstellungen angezeigt werden können. Wenn Sie eine bestimmte Einstellung auf Dienst- oder Ressourcenebene anzeigen möchten, können Sie den Namen der Einstellung angeben. Mit „recursive“ können Sie Einstellungen für alle Ressourcen als Teil des Diensts anzeigen. 
```bash

azdata bdc sql settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Beispiele
Zeigen Sie vom Benutzer konfigurierte Einstellungen auf der Dienstebene von SQL an. 
```bash
azdata bdc sql settings show
```
Zeigen Sie die SQL-Konfiguration für den maximalen Serverarbeitsspeicher im Datenpool an.
```bash
azdata bdc sql settings show --settings mssql.maxServerMemory --resources data-0 
```
Zeigen Sie die Änderungen an ausstehenden Einstellungen auf der Dienst- und Ressourcenebene von SQL an.
```bash
azdata bdc sql settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>Optionale Parameter 
#### `--filter-options | -f` 
Optionen zum Filtern, welche Einstellungen auf Dienst- oder Ressourcenebene zusätzlich zu den vom Benutzer konfigurierten Einstellungen angezeigt werden. Dabei sind Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und benutzerkonfigurierbare Einstellungen), alle konfigurierbaren Einstellungen oder ausstehende Einstellungen angezeigt werden können. Optionen: `userConfigured`, `all`, `pending`, `configurable`.
#### `--settings | -s` 
Damit werden Informationen zu den angegebenen Einstellungsnamen angezeigt. 
#### `--include-details | -i` 
Damit werden zusätzliche Details zu den ausgewählten Einstellungen, die angezeigt werden sollen, einbezogen. 
#### `--description | -d` 
Damit wird die Beschreibung der Einstellung einbezogen. Muss mit „--include-details“ verwendet werden. 
#### `--resources | -r` 
Damit werden die Einstellungsinformationen für die angegebenen Ressourcen angezeigt. Ressourcen können in Form einer durch Trennzeichen getrennten Liste aufgeführt werden. 
#### `--recursive | -rec` 
Damit werden die Einstellungsinformationen für die angegebene Ebene (Dienst oder Dienst-Ressource) sowie alle Komponenten auf darunterliegenden Ebenen (Ressource) angezeigt. 

### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md). 

Weitere Informationen zum Installieren des Tools **azdata** finden Sie unter [Install azdata to manage SQL Server 2019 big data clusters (Installieren von azdata zum Verwalten von Big-Data-Clustern von SQL Server 2019)](../install/deploy-install-azdata.md).
