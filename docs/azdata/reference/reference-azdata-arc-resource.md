---
title: Referenz zu „azdata arc resource“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc resource-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5b134a3ca21b74405fb6ee2cb115015788b59f62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048960"
---
# <a name="azdata-arc-resource"></a>azdata arc resource

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc resource-kind list](#azdata-arc-resource-kind-list) | Hiermit werden die verfügbaren benutzerdefinierten Ressourcentypen für Arc aufgelistet, die definiert und erstellt werden können.
[azdata arc resource-kind get](#azdata-arc-resource-kind-get) | Hiermit wird die Vorlagendatei für den Arc-Ressourcentyp abgerufen.
## <a name="azdata-arc-resource-kind-list"></a>azdata arc resource-kind list
Hiermit werden die verfügbaren benutzerdefinierten Ressourcentypen für Arc aufgelistet, die definiert und erstellt werden können. Nach dem Auflisten können Sie die Vorlagendatei abrufen, die zum Definieren oder Erstellen dieser benutzerdefinierten Ressource erforderlich ist.
```bash
azdata arc resource-kind list 
```
### <a name="examples"></a>Beispiele
Dies ist ein Beispielbefehl zum Auflisten der verfügbaren benutzerdefinierten Ressourcentypen für Arc.
```bash
azdata arc resource-kind list
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
## <a name="azdata-arc-resource-kind-get"></a>azdata arc resource-kind get
Hiermit wird die Vorlagendatei für den Arc-Ressourcentyp abgerufen.
```bash
azdata arc resource-kind get --kind -k 
                             [--dest -d]
```
### <a name="examples"></a>Beispiele
Dies ist ein Beispielbefehl zum Abrufen der CRD-Vorlagendatei für einen Arc-Ressourcentyp.
```bash
azdata arc resource-kind get --kind sqldb
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--kind -k`
Dies ist die Art der Arc-Ressource, für die Sie die Vorlagendatei erstellen möchten.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--dest -d`
Dies ist das Verzeichnis, in dem Sie die Vorlagendateien platzieren möchten.
`template`
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

