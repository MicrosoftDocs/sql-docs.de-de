---
title: Referenz zu „azdata arc dc debug“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc dc debug-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3979dbd8561eeedf3e5c0ab03a51b04b2bd73674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052681"
---
# <a name="azdata-arc-dc-debug"></a>azdata arc dc debug

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc dc debug copy-logs](#azdata-arc-dc-debug-copy-logs) | Kopieren von Protokollen.
[azdata arc dc debug dump](#azdata-arc-dc-debug-dump) | Auslösen eines Speicherabbilds
## <a name="azdata-arc-dc-debug-copy-logs"></a>azdata arc dc debug copy-logs
Hiermit werden die Debugprotokolle aus dem Datencontroller kopiert – auf Ihrem System ist eine Kubernetes-Konfiguration erforderlich.
```bash
azdata arc dc debug copy-logs --namespace -ns 
                              [--container -c]  
                              
[--target-folder -d]  
                              
[--pod]  
                              
[--resource-kind -rk]  
                              
[--resource-name -rn]  
                              
[--timeout -t]  
                              
[--skip-compress -sc]  
                              
[--exclude-dumps -ed]
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--namespace -ns`
Kubernetes-Namespace des Datencontrollers
### <a name="optional-parameters"></a>Optionale Parameter
#### `--container -c`
Kopieren Sie die Protokolle für die Container mit ähnlichem Namen. Standardmäßig werden die Protokolle für alle Container kopiert. Dieser Parameter kann nicht mehrfach angegeben werden. Ist er mehrfach angegeben, wird der letzte Parameter verwendet.
#### `--target-folder -d`
Der Pfad des Zielordners, in den die Protokolle kopiert werden sollen. Optional. Standardmäßig wird das Ergebnis im lokalen Ordner erstellt.  Dieser Parameter kann nicht mehrfach angegeben werden. Ist er mehrfach angegeben, wird der letzte Parameter verwendet.
#### `--pod`
Kopieren Sie die Protokolle für die Pods mit ähnlichem Namen. Optional. Standardmäßig werden die Protokolle für alle Pods kopiert. Dieser Parameter kann nicht mehrfach angegeben werden. Ist er mehrfach angegeben, wird der letzte Parameter verwendet.
#### `--resource-kind -rk`
Kopieren Sie die Protokolle für eine bestimmte Art von Ressource. Dieser Parameter kann nicht mehrfach angegeben werden. Ist er mehrfach angegeben, wird der letzte Parameter verwendet. Wenn dieser Parameter angegeben ist, sollte zum Identifizieren der Ressource auch --resource-name angegeben werden.
#### `--resource-name -rn`
Kopieren Sie die Protokolle für die Ressource mit dem angegebenen Namen. Dieser Parameter kann nicht mehrfach angegeben werden. Ist er mehrfach angegeben, wird der letzte Parameter verwendet. Wenn dieser Parameter angegeben ist, sollte zum Identifizieren der Ressource auch --resource-kind angegeben werden.
#### `--timeout -t`
Die Zeit in Sekunden, während der auf das Abschließen des Befehls gewartet wird. Der Standardwert ist „0“, d. h., das Warten ist unbegrenzt.
#### `--skip-compress -sc`
Gibt an, ob die Komprimierung des resultierenden Ordners übersprungen werden soll Der Standardwert ist „False“. Dadurch wird der resultierende Ordner komprimiert.
#### `--exclude-dumps -ed`
Gibt an, ob Sicherungsdateien aus dem resultierenden Ordner ausgeschlossen werden sollen Der Standardwert ist „False“. Dadurch werden Sicherungsdateien eingeschlossen.
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
## <a name="azdata-arc-dc-debug-dump"></a>azdata arc dc debug dump
Lösen Sie ein Speicherabbild aus, und kopieren Sie es aus dem Container. Hierfür ist eine Kubernetes-Konfiguration auf Ihrem System erforderlich.
```bash
azdata arc dc debug dump --namespace -ns 
                         [--container -c]  
                         
[--target-folder -d]
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--namespace -ns`
Kubernetes-Namespace des Datencontrollers
### <a name="optional-parameters"></a>Optionale Parameter
#### `--container -c`
Hierbei handelt es sich um den Zielcontainer, der zum Sichern der aktiven Prozesse ausgelöst werden soll.
`controller`
#### `--target-folder -d`
Hierbei handelt es sich um den Zielordner, aus dem das Abbild kopiert wird. `./output/dump`
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

