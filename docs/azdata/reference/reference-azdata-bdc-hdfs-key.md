---
title: 'azdata bdc hdfs key: Referenz'
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu „azdata bdc hdfs key“-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c208d1c726bae41b349abdf475627afbc82ed2b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342501"
---
# <a name="azdata-bdc-hdfs-key"></a>azdata bdc hdfs key

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc hdfs key create](#azdata-bdc-hdfs-key-create) | Damit wird ein HDFS-Schlüssel erstellt.
[azdata bdc hdfs key list](#azdata-bdc-hdfs-key-list) | Damit werden alle Schlüssel der Hadoop-Verschlüsselungszone aufgelistet.
[azdata bdc hdfs key roll](#azdata-bdc-hdfs-key-roll) | Damit wird für einen HDFS-Schlüssel eine Rotation ausgeführt.
## <a name="azdata-bdc-hdfs-key-create"></a>azdata bdc hdfs key create
Damit wird ein HDFS-Schlüssel mit einem bestimmten Namen und einer bestimmten Größe erstellt.
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>Beispiele
Einen 256-Bit-Schlüssel mit dem Namen „key1“ erstellen Sie mit: azdata hdfs key create --name key1 --size 256
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--name -n`
Name des Schlüssels der Hadoop-Verschlüsselungszone 
### <a name="optional-parameters"></a>Optionale Parameter
#### `--size -size`
Bitlänge des Hadoop-Verschlüsselungsschlüssels, die Standardlänge beträgt 256.
`256`
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
## <a name="azdata-bdc-hdfs-key-list"></a>azdata bdc hdfs key list
Damit werden alle Schlüssel der Hadoop-Verschlüsselungszone aufgelistet.
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>Beispiele
Alle Schlüssel der Hadoop-Verschlüsselungszone listen Sie auf mit: azdata bdc hdfs key list
```bash
azdata bdc hdfs key list
```
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
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata bdc hdfs key roll
Damit wird für einen HDFS-Schlüssel mit einem bestimmten Namen eine Rotation ausgeführt.
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>Beispiele
Ein Schlüssel mit dem Namen „key1“ wird mit: azdata hdfs key roll --name key1 rotiert.
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--name -n`
Name des Schlüssels der Verschlüsselungszone, für die ein Rotation auf eine neue Version ausgeführt werden soll. 
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
