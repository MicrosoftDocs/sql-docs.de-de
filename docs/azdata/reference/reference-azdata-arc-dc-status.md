---
title: Referenz zu „azdata arc dc status“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc dc status-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 82408e8a7f1edb37c33a6f1748119f5ff24f563f
ms.sourcegitcommit: d56f1eca807c55cf606a6316f3872585f014fec1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942613"
---
# <a name="azdata-arc-dc-status"></a>azdata arc dc status

Gilt für `azdata`e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc dc status show](#azdata-arc-dc-status-show) | Hiermit zeigen Sie den Status des Datencontrollers an.
## <a name="azdata-arc-dc-status-show"></a>azdata arc dc status show
Hiermit zeigen Sie den Status des Datencontrollers an.
```bash
azdata arc dc status show [--namespace -ns] 
                          
```
### <a name="examples"></a>Beispiele
Hiermit zeigen Sie den Status des Datencontrollers in einem bestimmten Namespace an.
```bash
azdata arc dc status show --namespace <ns>
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--namespace -ns`
Der Kubernetes-Namespace, in dem der Datencontroller vorhanden ist
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

