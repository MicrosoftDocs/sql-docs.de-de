---
title: azdata bdc hdfs encryption-zone reference
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata bdc hdfs encryption-zone-Befehlen.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c094d489cb925e280f5a44a8234d70fc817554b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342524"
---
# <a name="azdata-bdc-hdfs-encryption-zone"></a>azdata bdc hdfs encryption-zone

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
[azdata bdc hdfs encryption-zone create](#azdata-bdc-hdfs-encryption-zone-create) | Damit wird ein HDFS-Ordner in eine Verschlüsselungszone konvertiert.
[azdata bdc hdfs encryption-zone list](#azdata-bdc-hdfs-encryption-zone-list) | Damit werden alle Verschlüsselungszonen und mit den Verschlüsselungszonen verknüpfte Schlüssel angezeigt.
[azdata bdc hdfs encryption-zone get-file-encryption-info](#azdata-bdc-hdfs-encryption-zone-get-file-encryption-info) | Damit werden die Verschlüsselungsinformationen für einen bestimmten HDFS-Dateipfad abgerufen.
[azdata bdc hdfs encryption-zone status](#azdata-bdc-hdfs-encryption-zone-status) | Damit wird der Neuverschlüsselungsstatus für den HDFS-Cluster abgerufen.
[azdata bdc hdfs encryption-zone reencrypt](#azdata-bdc-hdfs-encryption-zone-reencrypt) | Damit wird die Neuverschlüsselung der durch das „--path“-Argument angegebenen Verschlüsselungszone gesteuert.
## <a name="azdata-bdc-hdfs-encryption-zone-create"></a>azdata bdc hdfs encryption-zone create
Damit wird ein HDFS-Ordner in eine Verschlüsselungszone konvertiert. Hierzu wird der Schlüssel verwendet, der für die Verschlüsselung der Dateien in der Verschlüsselungszone bereitgestellt wird.
```bash
azdata bdc hdfs encryption-zone create --path -p 
                                       --keyname -k
```
### <a name="examples"></a>Beispiele
So konvertieren Sie den vorhandenen Ordner „/user/securefolder“ mit einem Schlüssel mit dem Namen „securelake“ in eine Verschlüsselungszone
```bash
azdata bdc hdfs encryption-zone create --path /home/securefolder --keyname securelake
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad des HDFS-Ordners, der in eine Verschlüsselungszone konvertiert werden soll.
#### `--keyname -k`
Hadoop-KMS-Schlüssel, der zum Schutz der Verschlüsselungszone verwendet werden soll.
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
## <a name="azdata-bdc-hdfs-encryption-zone-list"></a>azdata bdc hdfs encryption-zone list
Damit werden alle Verschlüsselungszonen und mit den Verschlüsselungszonen verknüpfte Schlüssel angezeigt.
```bash
azdata bdc hdfs encryption-zone list 
```
### <a name="examples"></a>Beispiele
Listen Sie alle Verschlüsselungszonen auf.
```bash
azdata bdc hdfs encryption-zone list
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
## <a name="azdata-bdc-hdfs-encryption-zone-get-file-encryption-info"></a>azdata bdc hdfs encryption-zone get-file-encryption-info
Damit werden die Verschlüsselungsinformationen für einen bestimmten HDFS-Dateipfad abgerufen.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path -p 
                                                         
```
### <a name="examples"></a>Beispiele
Rufen Sie die Verschlüsselungsinformationen für eine Datei unter „/user/securefolder/data.csv“ ab.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path /user/securefolder/data.csv
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
HDFS-Dateipfad, für den Verschlüsselungsinformationen abgerufen werden sollen.
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
## <a name="azdata-bdc-hdfs-encryption-zone-status"></a>azdata bdc hdfs encryption-zone status
Damit wird der Neuverschlüsselungsstatus für den HDFS-Cluster abgerufen.
```bash
azdata bdc hdfs encryption-zone status 
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
## <a name="azdata-bdc-hdfs-encryption-zone-reencrypt"></a>azdata bdc hdfs encryption-zone reencrypt
Damit wird die Neuverschlüsselung der durch das „--path“-Argument angegebenen Verschlüsselungszone gesteuert.
```bash
azdata bdc hdfs encryption-zone reencrypt --path -p 
                                          --action -a
```
### <a name="examples"></a>Beispiele
Starten Sie die Neuverschlüsselung der Verschlüsselungszone „securelake“.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
```
Beenden Sie die Neuverschlüsselung der Verschlüsselungszone „securelake“.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action cancel
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad des Ordners der HDFS-Verschlüsselungszone, die erneut verschlüsselt werden soll
#### `--action -a`
Neuverschlüsselungsaktion, die ausgeführt werden soll Gültige Werte: „start“ und „cancel“
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
