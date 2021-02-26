---
title: 'azdata bdc gateway settings: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc gateway settings-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8da7bc79600d1fdd052109c55039b33642079d0c
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567300"
---
# <a name="azdata-bdc-gateway-settings"></a>azdata bdc gateway settings

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **gateway settings**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc gateway settings set](#azdata-bdc-gateway-settings-set) | Damit werden Einstellungen auf der Dienstebene des Gateways festgelegt.
[azdata bdc gateway settings show](#azdata-bdc-gateway-settings-show) | Damit werden Einstellungen auf der Dienstebene des Gateways und optional Gatewayeinstellungen für angegebene Ressourcen angezeigt.

## <a name="azdata-bdc-gateway-settings-set"></a>azdata bdc gateway settings set
Bietet die Möglichkeit, eine Einstellung auf Dienst- oder Ressourcenebene festzulegen. Geben Sie hierzu den vollständigen Namen der Einstellung und den Wert an. Auf den aktuell ausgeführten BDC wird die Einstellung nicht angewendet. Wenn Sie dies möchten, führen Sie ein Upgrade aus.
```bash
azdata bdc gateway settings set --settings -s 
                        
```
### <a name="examples"></a>Beispiele
Legen Sie einen Grenzwert für das Gatewaytimeout fest.
```bash 
azdata bdc gateway settings set --settings gateway-site.gateway.httpclient.socketTimeout=100s –resources gateway 
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

## <a name="azdata-bdc-gateway-settings-show"></a>azdata bdc gateway settings show
Damit werden die BDC-Einstellungen auf Dienstebene (optional Ressourcenebene) für `gateway` angezeigt. Mit diesem Befehl werden standardmäßig vom Benutzer konfigurierte Einstellungen auf Dienstebene angezeigt. Dabei sind Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und konfigurierbare Einstellungen), konfigurierbare Einstellungen oder ausstehende Einstellungen angezeigt werden können. Wenn Sie eine bestimmte Einstellung auf Dienst- oder Ressourcenebene anzeigen möchten, können Sie den Namen der Einstellung angeben. Mit „recursive“ können Sie Einstellungen für alle Ressourcen als Teil des Diensts anzeigen. 
```bash

azdata bdc gateway settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Beispiele
Zeigen Sie vom Benutzer konfigurierte Einstellungen auf der Ressourcenebene des Gateways an. 
```bash
azdata bdc gateway settings show --resource gateway 
```
Zeigen Sie einen Grenzwert für das Gatewaytimeout an.
```bash
azdata bdc gateway settings show --settings gateway-site.gateway.httpclient.socketTimeout --resources gateway 
```
Zeigen Sie die Änderungen an ausstehenden Einstellungen für die Gatewayressource an.
```bash
azdata bdc gateway settings show --filter-options=pending –-resource gateway --include-details
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
