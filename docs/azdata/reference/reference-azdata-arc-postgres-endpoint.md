---
title: Referenz zu „azdata arc postgres endpoint“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc postgres endpoint-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cb48072eaee6cdee6498655f05b88ae05c2062a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049000"
---
# <a name="azdata-arc-postgres-endpoint"></a>azdata arc postgres endpoint

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc postgres endpoint list](#azdata-arc-postgres-endpoint-list) | Hiermit listen Sie die Endpunkte von PostgreSQL-Servergruppen auf.
## <a name="azdata-arc-postgres-endpoint-list"></a>azdata arc postgres endpoint list
Hiermit listen Sie die Endpunkte von PostgreSQL-Servergruppen auf.
```bash
azdata arc postgres endpoint list --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>Beispiele
Hiermit listen Sie die Endpunkte von PostgreSQL-Servergruppen auf.
```bash
azdata arc postgres endpoint list -n postgres01
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--name -n`
Dies ist der Name der PostgreSQL-Servergruppe.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--engine-version -ev`
--engine-version kann zusammen mit --name verwendet werden, um eine PostgreSQL Hyperscale-Servergruppe zu identifizieren, wenn zwei Servergruppen mit unterschiedlicher Engine-Version denselben Namen haben. Der Parameter --engine-version ist optional und muss den Wert 11 oder 12 aufweisen, wenn er zum Identifizieren einer Servergruppe verwendet wird.
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

