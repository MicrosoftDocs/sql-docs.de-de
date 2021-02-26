---
title: 'azdata bdc spark settings: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc spark settings-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 66c80bc7d77ebc1f1f5c18e5fe972e4c6d61419d
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567299"
---
# <a name="azdata-bdc-spark-settings"></a>azdata bdc spark settings

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **spark settings**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc spark settings set](#azdata-bdc-spark-settings-set) | Damit werden Einstellungen auf der Dienstebene von Spark festgelegt.
[azdata bdc spark settings show](#azdata-bdc-spark-settings-show) | Damit werden Einstellungen auf der Dienstebene von Spark und optional Spark-Einstellungen für angegebene Ressourcen angezeigt.

## <a name="azdata-bdc-spark-settings-set"></a>azdata bdc spark settings set
Bietet die Möglichkeit, eine Einstellung auf Dienst- oder Ressourcenebene festzulegen. Geben Sie hierzu den vollständigen Namen der Einstellung und den Wert an. Auf den aktuell ausgeführten BDC wird die Einstellung nicht angewendet. Wenn Sie dies möchten, führen Sie ein Upgrade aus.
```bash
azdata bdc spark settings set --settings -s 
                        
```
### <a name="examples"></a>Beispiele
Legen Sie für den Spark-Dienst die Standardanzahl der Treiberkerne auf 1 und den Treiberspeicher auf 1664 MB fest. 
```bash 
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m 
``` 
Legen Sie für den Speicherpool die Standardanzahl der Executorkerne auf 1 fest. 
```bash 
azdata bdc spark  settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0 
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

## <a name="azdata-bdc-spark-settings-show"></a>azdata bdc spark settings show
Damit werden die BDC-Einstellungen auf Dienstebene (optional Ressourcenebene) für `spark` angezeigt. Mit diesem Befehl werden standardmäßig vom Benutzer konfigurierte Einstellungen auf Dienstebene angezeigt. Dabei sind Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und konfigurierbare Einstellungen), konfigurierbare Einstellungen oder ausstehende Einstellungen angezeigt werden können. Wenn Sie eine bestimmte Einstellung auf Dienst- oder Ressourcenebene anzeigen möchten, können Sie den Namen der Einstellung angeben. Mit „recursive“ können Sie Einstellungen für alle Ressourcen als Teil des Diensts anzeigen. 
```bash

azdata bdc spark settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Beispiele
Zeigen Sie vom Benutzer konfigurierte Einstellungen auf der Dienstebene von Spark an.
```bash
azdata bdc spark settings show
```
Zeigen Sie den aktuellen und konfigurierten Wert für Spark-Treiberkerne im Speicherpool an. 
```bash
azdata bdc spark settings show --settings spark-defaults-conf.spark.driver.cores --resources storage-0 --include-details
```
Zeigen Sie alle konfigurierbaren speicherbezogenen Einstellungen für den Spark-Dienst an.
```bash
azdata bdc spark settings show --settings *memory* --resources storage-0 
```
Zeigen Sie die Änderungen an ausstehenden Einstellungen auf der Dienst- und Ressourcenebene von Spark an.
```bash
azdata bdc spark settings show --filter-options=pending --recursive --include-details
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
