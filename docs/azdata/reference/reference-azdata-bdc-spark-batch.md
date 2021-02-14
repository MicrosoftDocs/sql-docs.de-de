---
title: „azdata bdc spark batch“-Referenz
titleSuffix: SQL Server big data clusters
description: Referenzartikel für „azdata bdc spark batch“-Befehle.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8e6dea113ff65125f24819b1f579f686e9452ee1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048800"
---
# <a name="azdata-bdc-spark-batch"></a>azdata bdc spark batch

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
[azdata bdc spark batch create](#azdata-bdc-spark-batch-create) | Erstellen eines neuen Spark-Batchs.
[azdata bdc spark batch list](#azdata-bdc-spark-batch-list) | Auflisten aller Batches in Spark.
[azdata bdc spark batch info](#azdata-bdc-spark-batch-info) | Abrufen von Informationen über einen aktiven Spark-Batch.
[azdata bdc spark batch log](#azdata-bdc-spark-batch-log) | Abrufen von Ausführungsprotokollen für einen Spark-Batch.
[azdata bdc spark batch state](#azdata-bdc-spark-batch-state) | Abrufen des Ausführungsstatus für einen Spark-Batch.
[azdata bdc spark batch delete](#azdata-bdc-spark-batch-delete) | Löschen eines Spark-Batchs.
## <a name="azdata-bdc-spark-batch-create"></a>azdata bdc spark batch create
Hiermit wird ein neuer Spark-Batchauftrag erstellt, der den bereitgestellten Code ausführt.
```bash
azdata bdc spark batch create --file -f 
                              [--class-name -c]  
                              
[--arguments -a]  
                              
[--jar-files -j]  
                              
[--py-files -p]  
                              
[--files]  
                              
[--driver-memory]  
                              
[--driver-cores]  
                              
[--executor-memory]  
                              
[--executor-cores]  
                              
[--executor-count]  
                              
[--archives]  
                              
[--queue -q]  
                              
[--name -n]  
                              
[--config]
```
### <a name="examples"></a>Beispiele
Erstellen eines neuen Spark-Batchs.
```bash
azdata bdc spark batch create --code "2+2"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--file -f`
Pfad zur auszuführenden Datei.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--class-name -c`
Der Name der Klasse, die beim Übergeben einer oder mehrerer JAR-Dateien ausgeführt werden soll.
#### `--arguments -a`
Liste von Argumenten.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--jar-files -j`
Liste der JAR-Dateipfade.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--py-files -p`
Liste der Python-Dateipfade.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--files`
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
#### `--archives`
Liste der Pfade für Archive.  Um eine Liste zu übergeben, codieren Sie die Werte im JSON-Format.  Beispiel: '["Eintrag1", "Eintrag2"]'.
#### `--queue -q`
Name der Spark-Warteschlange, in der die Sitzung ausgeführt werden soll.
#### `--name -n`
Name der Spark-Sitzung.
#### `--config`
Liste mit den Name-Wert-Paaren, die Spark-Konfigurationswerte enthalten.  Codiert als JSON-Wörterbuch.  Beispiel: '{"Name":"Wert", "Name2":"Wert2"}'.
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
## <a name="azdata-bdc-spark-batch-list"></a>azdata bdc spark batch list
Auflisten aller Batches in Spark.
```bash
azdata bdc spark batch list 
```
### <a name="examples"></a>Beispiele
Auflisten aller aktiven Batches.
```bash
azdata bdc spark batch list
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
## <a name="azdata-bdc-spark-batch-info"></a>azdata bdc spark batch info
Hiermit werden die Informationen für einen Spark-Batch mit der angegebenen ID abgerufen.  Die Batch-ID wird von „spark batch create“ zurückgegeben.
```bash
azdata bdc spark batch info --batch-id -i 
                            
```
### <a name="examples"></a>Beispiele
Abrufen von Batch-Informationen für den Batch mit der ID „0“.
```bash
azdata bdc spark batch info --batch-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--batch-id -i`
Spark-Batch-ID-Nummer.
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
## <a name="azdata-bdc-spark-batch-log"></a>azdata bdc spark batch log
Hiermit werden die Batchprotokolleinträge für einen Spark-Batch mit der angegebenen ID abgerufen.  Die Batch-ID wird von „spark batch create“ zurückgegeben.
```bash
azdata bdc spark batch log --batch-id -i 
                           
```
### <a name="examples"></a>Beispiele
Abrufen des Batchprotokolls für den Batch mit der ID „0“.
```bash
azdata bdc spark batch log --batch-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--batch-id -i`
Spark-Batch-ID-Nummer.
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
## <a name="azdata-bdc-spark-batch-state"></a>azdata bdc spark batch state
Hiermit wird der Batchstatus für einen Spark-Batch mit der angegebenen ID abgerufen.  Die Batch-ID wird von „spark batch create“ zurückgegeben.
```bash
azdata bdc spark batch state --batch-id -i 
                             
```
### <a name="examples"></a>Beispiele
Abrufen des Batchstatus für den Batch mit der ID „0“.
```bash
azdata bdc spark batch state --batch-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--batch-id -i`
Spark-Batch-ID-Nummer.
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
## <a name="azdata-bdc-spark-batch-delete"></a>azdata bdc spark batch delete
Hiermit wird ein Spark-Batch gelöscht. Die Batch-ID wird von „spark batch create“ zurückgegeben.
```bash
azdata bdc spark batch delete --batch-id -i 
                              
```
### <a name="examples"></a>Beispiele
Löschen eines Batchs.
```bash
azdata bdc spark batch delete --batch-id 0
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--batch-id -i`
Spark-Batch-ID-Nummer.
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

