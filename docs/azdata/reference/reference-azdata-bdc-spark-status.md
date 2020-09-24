---
title: 'azdata bdc spark status: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc spark status-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: da13babdf51bc1b4f7e1afa297e7712994932c64
ms.sourcegitcommit: d56f1eca807c55cf606a6316f3872585f014fec1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90914559"
---
# <a name="azdata-bdc-spark-status"></a>azdata bdc spark status

Gilt für `azdata`e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
[azdata bdc spark status show](#azdata-bdc-spark-status-show) | Status des Spark-Diensts
## <a name="azdata-bdc-spark-status-show"></a>azdata bdc spark status show
Status des Spark-Diensts
```bash
azdata bdc spark status show [--resource -r] 
                             [--all -a]
```
### <a name="examples"></a>Beispiele
Abrufen des Status des Spark-Diensts
```bash
azdata bdc spark status show
```
Abrufen des Status des Spark-Diensts mit allen Instanzen
```bash
azdata bdc spark status show --all
```
Abrufen des Status der Speicherressource im Spark-Dienst
```bash
azdata bdc spark status show --resource storage-0
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--resource -r`
Abrufen dieser Ressource in diesem Dienst
#### `--all -a`
Anzeigen aller Instanzen jeder Ressource im Dienst
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

