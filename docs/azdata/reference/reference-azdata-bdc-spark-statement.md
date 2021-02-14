---
title: 'azdata bdc spark statement: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc spark statement-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9f911c8d9e2a7f39d02e84c03d772a0390a388c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048790"
---
# <a name="azdata-bdc-spark-statement"></a>azdata bdc spark statement

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
[azdata bdc spark statement list](#azdata-bdc-spark-statement-list) | Auflisten aller Anweisungen in der angegebenen Spark-Sitzung.
[azdata bdc spark statement create](#azdata-bdc-spark-statement-create) | Erstellen einer neuen Spark-Anweisung in der angegebenen Sitzung.
[azdata bdc spark statement info](#azdata-bdc-spark-statement-info) | Abrufen von Informationen über die angeforderte Anweisung in der angegebenen Spark-Sitzung.
[azdata bdc spark statement cancel](#azdata-bdc-spark-statement-cancel) | Abbrechen einer Anweisung in der angegebenen Spark-Sitzung.
## <a name="azdata-bdc-spark-statement-list"></a>azdata bdc spark statement list
Auflisten aller Anweisungen in der angegebenen Spark-Sitzung.
```bash
azdata bdc spark statement list --session-id -i 
                                
```
### <a name="examples"></a>Beispiele
Listen Sie alle Sitzungsanweisungen auf.
```bash
azdata bdc spark statement list --session-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--session-id -i`
Die ID-Nummer der Spark-Sitzung.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.
## <a name="azdata-bdc-spark-statement-create"></a>azdata bdc spark statement create
Hiermit wird eine neue Anweisung in der angegebenen Sitzung erstellt und wird diese Anweisung ausgeführt.  Erfolgt die Ausführung schnell, enthält das Ergebnis die Ausgabe der Ausführung.  Andernfalls kann das Ergebnis mit „spark session info“ abgerufen werden, nachdem die-Anweisung beendet wurde.
```bash
azdata bdc spark statement create --session-id -i 
                                  --code -c
```
### <a name="examples"></a>Beispiele
Führen Sie eine Anweisung aus.
```bash
azdata bdc spark statement create --session-id 0 --code "2+2"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--session-id -i`
Die ID-Nummer der Spark-Sitzung.
#### `--code -c`
Die Zeichenfolge, die den Code enthält, der als Teil der Anweisung ausgeführt wird.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.
## <a name="azdata-bdc-spark-statement-info"></a>azdata bdc spark statement info
Hiermit werden der Ausführungsstatus und die Ausführungsergebnisse abgerufen, wenn die Anweisung abgeschlossen wurde. Die Anweisungs-ID wird von „spark statement create“ zurückgegeben.
```bash
azdata bdc spark statement info --session-id -i 
                                --statement-id -s
```
### <a name="examples"></a>Beispiele
Rufen Sie die Anweisungsinformationen für die Sitzung mit der ID 0 und die Anweisung mit der ID 0 ab.
```bash
azdata bdc spark statement info --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--session-id -i`
Die ID-Nummer der Spark-Sitzung.
#### `--statement-id -s`
Die ID-Nummer der Spark-Anweisung in der angegebenen Sitzungs-ID.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.
## <a name="azdata-bdc-spark-statement-cancel"></a>azdata bdc spark statement cancel
Hiermit wird eine Anweisung in der angegebenen Spark-Sitzung abgebrochen. Die Anweisungs-ID wird von „spark statement create“ zurückgegeben.
```bash
azdata bdc spark statement cancel --session-id -i 
                                  --statement-id -s
```
### <a name="examples"></a>Beispiele
Brechen Sie eine Anweisung ab.
```bash
azdata bdc spark statement cancel --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--session-id -i`
Die ID-Nummer der Spark-Sitzung.
#### `--statement-id -s`
Die ID-Nummer der Spark-Anweisung in der angegebenen Sitzungs-ID.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: json, jsonc, table, tsv.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md). 

Weitere Informationen zur Installation des Tools **azdata** finden Sie unter [Installieren von azdata](..\install\deploy-install-azdata.md).

