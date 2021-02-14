---
title: Referenz zu azdata bdc gateway status
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc gateway status-Befehlen
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ef05cb490757b1b21b9f3faa7cdb0b881a0089ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052471"
---
# <a name="azdata-bdc-gateway-status"></a>azdata bdc gateway status

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
[azdata bdc gateway status show](#azdata-bdc-gateway-status-show) | Status des Gatewaydiensts
## <a name="azdata-bdc-gateway-status-show"></a>azdata bdc gateway status show
Status des Gatewaydiensts
```bash
azdata bdc gateway status show [--resource -r] 
                               [--all -a]
```
### <a name="examples"></a>Beispiele
Ruft den Status des Gatewaydiensts ab
```bash
azdata bdc gateway status show
```
Ruft den Status des Gatewaydiensts mit allen Instanzen ab
```bash
azdata bdc gateway status show --all
```
Ruft den Status der Gatewayressource im Gatewaydienst ab
```bash
azdata bdc gateway status show --resource gateway
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

