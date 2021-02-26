---
title: 'azdata bdc settings: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc settings-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 37688a962fc17679a1a642af1b83609d2b8f480a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342491"
---
# <a name="azdata-bdc-settings"></a>azdata bdc settings

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc settings set](#azdata-bdc-settings-set) | Damit werden Einstellungen auf Clusterebene festgelegt.
[azdata bdc settings apply](#azdata-bdc-settings-apply) | Damit werden die Änderungen an ausstehenden Einstellungen auf den BDC angewendet.
[azdata bdc settings cancel-apply](#azdata-bdc-settings-cancel-apply) | Damit wird die Anwendung der BDC-Einstellungen beendet.
[azdata bdc settings show](#azdata-bdc-settings-show) | Damit werden die Einstellungen auf Clusterebene oder mithilfe von „--recursive“ alle Clustereinstellungen angezeigt.
[azdata bdc settings revert](#azdata-bdc-settings-revert) | Damit werden Änderungen an ausstehenden Einstellungen im BDC auf allen Ebenen wiederhergestellt.
## <a name="azdata-bdc-settings-set"></a>azdata bdc settings set
Damit wird eine Einstellung auf Clusterebene festgelegt. Geben Sie hierzu den vollständigen Namen der Einstellung und den Wert an. Führen Sie „apply“ aus, um die Einstellung anzuwenden und die BDC-Einstellungen zu aktualisieren.
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>Beispiele
Legen Sie den Clusterstandard für „bdc.telemetry.customerFeedback“ fest.
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--settings -s`
Damit wird der konfigurierte Wert für die angegebenen Einstellungen festgelegt. Mithilfe einer durch Trennzeichen getrennten Liste können mehrere Einstellungen festgelegt werden.
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
## <a name="azdata-bdc-settings-apply"></a>azdata bdc settings apply
Damit werden die Änderungen an ausstehenden Einstellungen auf den BDC angewendet.
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>Beispiele
Damit werden die Änderungen an ausstehenden Einstellungen auf den BDC angewendet.
```bash
azdata bdc settings apply
```
Erzwingen Sie „apply“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--force -f`
Erzwingen Sie „apply“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
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
## <a name="azdata-bdc-settings-cancel-apply"></a>azdata bdc settings cancel-apply
Wenn bei der Anwendung von Einstellungen ein Fehler auftritt, setzen Sie den Big Data-Cluster in den zuletzt ausgeführten Status zurück. Dieser Befehl wird nicht ausgeführt, wenn er auf einen ausgeführten Cluster angewendet wird. Änderungen an zuvor ausstehenden Einstellungen sind nach dem Abbruch weiterhin ausstehend.
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>Beispiele
Damit wird die Anwendung der BDC-Einstellungen beendet.
```bash
azdata bdc settings cancel-apply
```
Erzwingen Sie „cancel-apply“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--force -f`
Erzwingen Sie „cancel-apply“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
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
## <a name="azdata-bdc-settings-show"></a>azdata bdc settings show
Damit werden die Einstellungen des BDCs auf Clusterebene angezeigt. Mit diesem Befehl werden standardmäßig vom Benutzer konfigurierte Einstellungen auf Clusterebene angezeigt. Dabei sind weitere Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und benutzerkonfigurierbare und übernommene Einstellungen), alle konfigurierbaren Einstellungen oder ausstehende Einstellungen angezeigt werden können. Wenn Sie eine bestimmte Einstellung auf der Clusterebene anzeigen möchten, geben Sie deren Namen an. Wenn Sie die Einstellungen auf allen Ebenen (Cluster, Dienst und Ressource) anzeigen möchten, können Sie „recursive“ angeben.
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>Beispiele
Zeigen Sie an, ob die BDC-Telemetriesammlung aktiviert ist.
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
Zeigen Sie vom Benutzer konfigurierte Einstellungen im BDC auf allen Ebenen an.
```bash
azdata bdc settings show --recursive
```
Zeigen Sie alle ausstehenden Einstellungen im BDC auf allen Ebenen an.
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--settings -s`
Damit werden Informationen zu den angegebenen Einstellungsnamen angezeigt.
#### `--filter-option -f`
Damit wird gefiltert, welche Einstellungen auf Dienstebene zusätzlich zu den vom Benutzer konfigurierten Einstellungen angezeigt werden. Dabei sind Filter verfügbar, mit denen alle Einstellungen (vom System verwaltete und benutzerkonfigurierbare Einstellungen), alle konfigurierbaren Einstellungen oder ausstehende Einstellungen angezeigt werden können.
`userConfigured`
#### `--recursive -rec`
Damit werden Einstellungsinformationen zur Clusterebene und allen Komponenten auf darunterliegenden Ebenen (Dienste, Ressourcen) angezeigt.
#### `--include-details -i`
Damit werden zusätzliche Details zu den ausgewählten Einstellungen, die angezeigt werden sollen, einbezogen.
#### `--description -d`
Damit wird eine Beschreibung der Einstellung einbezogen.
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
## <a name="azdata-bdc-settings-revert"></a>azdata bdc settings revert
Damit werden Änderungen an ausstehenden Einstellungen im BDC auf allen Ebenen wiederhergestellt.
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>Beispiele
Stellen Sie die Änderungen an ausstehenden Einstellungen im BDC wieder her.
```bash
azdata bdc settings revert
```
Erzwingen Sie „revert“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--force -f`
Erzwingen Sie „revert“, sodass der Benutzer zu keiner Bestätigung aufgefordert wird und alle Issues über „stderr“ ausgegeben werden.
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

Weitere Informationen zur Installation des Tools **azdata** finden Sie unter [Installieren von azdata](..\install\deploy-install-azdata.md).
