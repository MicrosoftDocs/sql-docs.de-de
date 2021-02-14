---
title: Referenz zu „azdata arc sql mi config“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc sql mi config-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1913f0754001f69436b02f465711ac45245c504c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052571"
---
# <a name="azdata-arc-sql-mi-config"></a>azdata arc sql mi config

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc sql mi config init](#azdata-arc-sql-mi-config-init) | Hiermit werden die CRD-Datei und die Spezifikationsdatei für eine verwaltete SQL-Instanz initialisiert.
[azdata arc sql mi config add](#azdata-arc-sql-mi-config-add) | Fügt einen Wert in einem JSON-Pfad in einer Konfigurationsdatei hinzu.
[azdata arc sql mi config remove](#azdata-arc-sql-mi-config-remove) | Entfernt einen Wert in einem JSON-Pfad in einer Konfigurationsdatei.
[azdata arc sql mi config replace](#azdata-arc-sql-mi-config-replace) | Ersetzt einen Wert in einem JSON-Pfad in einer Konfigurationsdatei.
[azdata arc sql mi config patch](#azdata-arc-sql-mi-config-patch) | Patcht eine Konfigurationsdatei auf der Grundlage einer JSON-Patchdatei.
## <a name="azdata-arc-sql-mi-config-init"></a>azdata arc sql mi config init
Hiermit werden die CRD-Datei und die Spezifikationsdatei für eine verwaltete SQL-Instanz initialisiert.
```bash
azdata arc sql mi config init --path -p 
                              
```
### <a name="examples"></a>Beispiele
Hiermit werden die CRD-Datei und die Spezifikationsdatei für eine verwaltete SQL-Instanz initialisiert.
```bash
azdata arc sql mi config init --path ./template
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Dies ist ein Pfad, unter dem die CRD und die Spezifikation für die verwaltete SQL-Instanz geschrieben werden sollen.
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
## <a name="azdata-arc-sql-mi-config-add"></a>azdata arc sql mi config add
Fügt den Wert im JSON-Pfad in der Konfigurationsdatei hinzu.  Alle nachfolgenden Beispiele sind in Bash angegeben.  Beachten Sie bei Verwenden einer anderen Befehlszeile, dass Sie möglicherweise entsprechende Escapezeichen hinzufügen müssen.  Alternativ können Sie auch die Patchdateifunktionen verwenden.
```bash
azdata arc sql mi config add --path -p 
                             --json-values -j
```
### <a name="examples"></a>Beispiele
Bsp. 1: Hinzufügen von Speicher
```bash
azdata arc sql mi config add --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad zur benutzerdefinierten Ressourcenspezifikation, d. h. custom/spec.json
#### `--json-values -j`
Eine Schlüssel-Wert-Paar-Liste von JSON-Pfaden zu Werten: key1.subkey1=value1,key2.subkey2=value2 Sie können JSON-Inlinewerte angeben, z. B.: key='{"kind":"cluster","name":"test-cluster"}' oder einen Dateipfad, z. B. key=./values.json. Der Vorgang „Hinzufügen“ unterstützt KEINE Bedingungen.  Wenn der Inlinewert, den Sie bereitstellen, selbst ein Schlüssel-Wert-Paar mit „=“ und „,“ ist, verwenden Sie an den entsprechenden Stellen Escapezeichen.  Zum Beispiel key1=„key2\=val2\,key3\=val3“. Beispiele dazu, wie Ihr Pfad aussehen sollte, finden Sie unter http://jsonpatch.com/.  Wenn Sie auf ein Array zugreifen möchten, müssen Sie dazu den Index angeben, z. B. key.0=value.
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
## <a name="azdata-arc-sql-mi-config-remove"></a>azdata arc sql mi config remove
Entfernt den Wert im JSON-Pfad in der Konfigurationsdatei.  Alle nachfolgenden Beispiele sind in Bash angegeben.  Beachten Sie bei Verwenden einer anderen Befehlszeile, dass Sie möglicherweise entsprechende Escapezeichen hinzufügen müssen.  Alternativ können Sie auch die Patchdateifunktionen verwenden.
```bash
azdata arc sql mi config remove --path -p 
                                --json-path -j
```
### <a name="examples"></a>Beispiele
Bsp. 1: Entfernen von Speicher
```bash
azdata arc sql mi config remove --path custom/spec.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad zur benutzerdefinierten Ressourcenspezifikation, d. h. custom/spec.json
#### `--json-path -j`
Eine Liste von JSON-Pfaden, die auf der JSONPATCH-Bibliothek basiert und angibt, welche Werte entfernt werden sollen, z. B.: key1.subkey1,key2.subkey2. Der Vorgang „Entfernen“ unterstützt KEINE Bedingungen. Beispiele dazu, wie Ihr Pfad aussehen sollte, finden Sie unter http://jsonpatch.com/.  Wenn Sie auf ein Array zugreifen möchten, müssen Sie dazu den Index angeben, z. B. key.0=value.
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
## <a name="azdata-arc-sql-mi-config-replace"></a>azdata arc sql mi config replace
Ersetzt den Wert im JSON-Pfad in der Konfigurationsdatei.  Alle nachfolgenden Beispiele sind in Bash angegeben.  Beachten Sie bei Verwenden einer anderen Befehlszeile, dass Sie möglicherweise entsprechende Escapezeichen hinzufügen müssen.  Alternativ können Sie auch die Patchdateifunktionen verwenden.
```bash
azdata arc sql mi config replace --path -p 
                                 --json-values -j
```
### <a name="examples"></a>Beispiele
Bsp. 1: Ersetzen des Ports eines einzelnen Endpunkts
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Bsp. 2: Ersetzen des Speichers
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad zur benutzerdefinierten Ressourcenspezifikation, d. h. custom/spec.json
#### `--json-values -j`
Eine Schlüssel-Wert-Paar-Liste von JSON-Pfaden zu Werten: key1.subkey1=value1,key2.subkey2=value2 Sie können JSON-Inlinewerte angeben, z. B.: key='{"kind":"cluster","name":"test-cluster"}' oder einen Dateipfad, z. B. key=./values.json. Der Vorgang „Ersetzen“ unterstützt Bedingungen über die JSONPATH-Bibliothek.  Um diesen Parameter zu verwenden, starten Sie den Pfad mit „$“. Dies ermöglicht die Verwendung einer Bedingung, z. B. -j $.key1.key2[?(@.key3=="someValue"].key4=value. Wenn der Inlinewert, den Sie bereitstellen, selbst ein Schlüssel-Wert-Paar mit „=“ und „,“ ist, verwenden Sie an den entsprechenden Stellen Escapezeichen.  Zum Beispiel key1=„key2\=val2\,key3\=val3“. Nachfolgend sehen Sie hierzu Beispiele. Zusätzliche Hilfe finden Sie unter https://jsonpath.com/.
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
## <a name="azdata-arc-sql-mi-config-patch"></a>azdata arc sql mi config patch
Patcht die Konfigurationsdatei entsprechend der angegebenen Patchdatei. Informationen zum besseren Verständnis der Zusammensetzung der Pfade finden Sie unter http://jsonpatch.com/. Der Vorgang „Ersetzen“ kann Bedingungen im Pfad aufgrund der JSONPATH-Bibliothek verwenden (https://jsonpath.com/ ). Alle JSON-Patchdateien müssen mit einem „Patch“-Schlüssel beginnen, der über ein Array von Patches mit dem entsprechenden Vorgang (Hinzufügen, Ersetzen, Entfernen), Pfad und Wert verfügt. Der Vorgang „Entfernen“ erfordert keinen Wert, sondern nur einen Pfad. Siehe hierzu folgende Beispiele.
```bash
azdata arc sql mi config patch --path -p 
                               --patch-file
```
### <a name="examples"></a>Beispiele
Bsp. 1: Ersetzen des Ports eines einzelnen Endpunkts mit Patchdatei
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Bsp. 2: Ersetzen des Speichers mit Patchdatei
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Pfad zur benutzerdefinierten Ressourcenspezifikation, d. h. custom/spec.json
#### `--patch-file`
Pfad zu einer JSON-Patchdatei, die auf der JSONPATCH-Bibliothek basiert: http://jsonpatch.com/ Sie müssen die JSON-Patchdatei mit einem Schlüssel mit der Bezeichnung „Patch“ starten, bei dessen Wert es sich um ein Array von Patchvorgängen handelt, die Sie ausführen möchten. Für den Pfad eines Patchvorgangs können Sie für die meisten Vorgänge die Punktnotation verwenden, z. B. „key1.key2“. Wenn Sie einen „Ersetzen“-Vorgang durchführen möchten und einen Wert in einem Array ersetzen, das eine Bedingung erfordert, verwenden Sie die JSONPATH-Notation, indem Sie den Pfad mit einem Dollarzeichen („$“) beginnen. Dies ermöglicht die Verwendung einer Bedingung wie z. B. $.key1.key2[?(@.key3=="someValue"].key4. Siehe hierzu folgende Beispiele. Zusätzliche Hilfe zu Bedingungen finden Sie unter https://jsonpath.com/.
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

