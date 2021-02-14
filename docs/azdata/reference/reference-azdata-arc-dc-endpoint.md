---
title: Referenz zu „azdata arc dc endpoint reference“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc dc endpoint-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0e0fadcace976f55ea2e22fd1bd2c1c957f5c238
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049025"
---
# <a name="azdata-arc-dc-endpoint"></a>azdata arc dc endpoint

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc dc endpoint list](#azdata-arc-dc-endpoint-list) | Hiermit wird der Datencontrollerendpunkt angezeigt.
## <a name="azdata-arc-dc-endpoint-list"></a>azdata arc dc endpoint list
Hiermit wird der Datencontrollerendpunkt angezeigt.
```bash
azdata arc dc endpoint list [--endpoint-name -e] 
                            
```
### <a name="examples"></a>Beispiele
Hiermit wird der Datencontrollerendpunkt in einem bestimmten Namespace aufgelistet.
```bash
azdata arc dc endpoint list --namespace <ns>
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--endpoint-name -e`
Dies ist der Name des Arc-Datencontrollerendpunkts.
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

