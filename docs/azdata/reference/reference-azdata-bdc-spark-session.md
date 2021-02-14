---
title: 'azdata bdc spark session: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc spark session-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6d48576bf62b71c66a8bbdc33766084e2aaed37f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052411"
---
# <a name="azdata-bdc-spark-session"></a>azdata bdc spark session

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
[azdata bdc spark session create](#azdata-bdc-spark-session-create) | Erstellen einer neuen Spark-Sitzung.
[azdata bdc spark session list](#azdata-bdc-spark-session-list) | Auflisten aller aktiven Sitzungen in Spark.
[azdata bdc spark session info](#azdata-bdc-spark-session-info) | Abrufen von Informationen über eine aktive Spark-Sitzung.
[azdata bdc spark session log](#azdata-bdc-spark-session-log) | Abrufen von Ausführungsprotokollen für eine aktive Spark-Sitzung.
[azdata bdc spark session state](#azdata-bdc-spark-session-state) | Abrufen des Ausführungsstatus für eine aktive Spark-Sitzung.
[azdata bdc spark session delete](#azdata-bdc-spark-session-delete) | Löschen einer Spark-Sitzung.
## <a name="azdata-bdc-spark-session-create"></a>azdata bdc spark session create
Hiermit wird eine neue interaktive Spark-Sitzung erstellt. Die aufrufende Funktion muss den Typ der Spark-Sitzung angeben. Diese Sitzung ist auch nach der Lebensdauer einer azdata-Ausführung noch aktiv und muss mit „spark session delete“ gelöscht werden.
```bash
azdata bdc spark session create [--session-kind -k] 
                                [--jar-files -j]  
                                
[--py-files -p]  
                                
[--files -f]  
                                
[--driver-memory]  
                                
[--driver-cores]  
                                
[--executor-memory]  
                                
[--executor-cores]  
                                
[--executor-count]  
                                
[--archives -a]  
                                
[--queue -q]  
                                
[--name -n]  
                                
[--config -c]  
                                
[--timeout-seconds -t]
```
### <a name="examples"></a>Beispiele
Erstellen Sie eine Sitzung.
```bash
azdata bdc spark session create --session-kind pyspark
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--session-kind -k`
Der Name des Typs der zu erstellenden Sitzung.  Einer der folgenden Namen: spark, pyspark, sparkr, sql.
#### `--jar-files -j`
Liste der JAR-Dateipfade.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--py-files -p`
Liste der Python-Dateipfade.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--files -f`
Liste der Dateipfade.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--driver-memory`
Die Menge an Arbeitsspeicher, die dem Treiber zugeordnet werden soll.  Geben Sie Einheiten als Teil des Werts an.  Beispiel: 512M oder 2G.
#### `--driver-cores`
Die Anzahl der CPU-Kerne, die dem Treiber zugeordnet werden sollen.
#### `--executor-memory`
Die Menge an Arbeitsspeicher, die dem Executor zugeordnet werden soll.  Geben Sie Einheiten als Teil des Werts an.  Beispiel: 512M oder 2G.
#### `--executor-cores`
Die Anzahl der CPU-Kerne, die dem Executor zugeordnet werden sollen.
#### `--executor-count`
Die Anzahl der Executorinstanzen, die ausgeführt werden sollen.
#### `--archives -a`
Liste der Pfade für Archive.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--queue -q`
Name der Spark-Warteschlange, in der die Sitzung ausgeführt werden soll.
#### `--name -n`
Name der Spark-Sitzung.
#### `--config -c`
Liste mit den Name-Wert-Paaren, die Spark-Konfigurationswerte enthalten.  Codiert als JSON-Wörterbuch.  Beispiel: '{"Name":"Wert", "Name2":"Wert2"}'.
#### `--timeout-seconds -t`
Timeout für Sitzung im Leerlauf in Sekunden
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
## <a name="azdata-bdc-spark-session-list"></a>azdata bdc spark session list
Auflisten aller aktiven Sitzungen in Spark.
```bash
azdata bdc spark session list 
```
### <a name="examples"></a>Beispiele
Auflisten aller aktiven Sitzungen.
```bash
azdata bdc spark session list
```
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
## <a name="azdata-bdc-spark-session-info"></a>azdata bdc spark session info
Hiermit werden die Sitzungsinformationen für eine aktive Spark-Sitzung abgerufen, die die angegebene ID hat.  Die Sitzungs-ID wird von „spark session create“ zurückgegeben.
```bash
azdata bdc spark session info --session-id -i 
                              
```
### <a name="examples"></a>Beispiele
Rufen Sie die Sitzungsinformationen für die Sitzung mit der ID 0 ab.
```bash
azdata bdc spark session info --session-id 0
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
## <a name="azdata-bdc-spark-session-log"></a>azdata bdc spark session log
Hiermit werden die Sitzungsprotokolleinträge für eine aktive Spark-Sitzung abgerufen, die die angegebene ID hat.  Die Sitzungs-ID wird von „spark session create“ zurückgegeben.
```bash
azdata bdc spark session log --session-id -i 
                             
```
### <a name="examples"></a>Beispiele
Rufen Sie das Sitzungsprotokoll für die Sitzung mit der ID 0 ab.
```bash
azdata bdc spark session log --session-id 0
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
## <a name="azdata-bdc-spark-session-state"></a>azdata bdc spark session state
Hiermit wird der Sitzungsstatus für eine aktive Spark-Sitzung abgerufen, die die angegebene ID hat.  Die Sitzungs-ID wird von „spark session create“ zurückgegeben.
```bash
azdata bdc spark session state --session-id -i 
                               
```
### <a name="examples"></a>Beispiele
Rufen Sie den Sitzungsstatus für die Sitzung mit der ID 0 ab.
```bash
azdata bdc spark session state --session-id 0
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
## <a name="azdata-bdc-spark-session-delete"></a>azdata bdc spark session delete
Hiermit wird eine interaktive Spark-Sitzung gelöscht. Die Sitzungs-ID wird von „spark session create“ zurückgegeben.
```bash
azdata bdc spark session delete --session-id -i 
                                
```
### <a name="examples"></a>Beispiele
Löschen Sie eine Sitzung.
```bash
azdata bdc spark session delete --session-id 0
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

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md). 

Weitere Informationen zur Installation des Tools **azdata** finden Sie unter [Installieren von azdata](..\install\deploy-install-azdata.md).

