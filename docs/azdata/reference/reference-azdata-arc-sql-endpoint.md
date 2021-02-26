---
title: 'azdata arc sql endpoint: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata arc sql endpoint-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 190bc4360d8f8cd35f0ceda04887a1fc912243e3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342537"
---
# <a name="azdata-arc-sql-endpoint"></a>azdata arc sql endpoint

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata arc sql endpoint list](#azdata-arc-sql-endpoint-list) | Damit werden die SQL-Endpunkte aufgelistet.
## <a name="azdata-arc-sql-endpoint-list"></a>azdata arc sql endpoint list
Damit werden die SQL-Endpunkte aufgelistet.
```bash
azdata arc sql endpoint list [--name -n] 
                             
```
### <a name="examples"></a>Beispiele
Listen Sie die Endpunkte für eine verwaltete SQL-Instanz auf.
```bash
azdata arc sql endpoint list -n sqlmi1
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--name -n`
Der Name der SQL-Instanz, die angezeigt werden soll. Wenn dieser Parameter nicht angegeben wird, werden alle Endpunkte für alle Instanzen angezeigt.
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
