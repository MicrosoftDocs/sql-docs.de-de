---
title: Referenz zu azdata arc dc
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu azdata arc dc-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b223b3b69ba3eda1be5f882b4cabd868c345b20c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049010"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | Hiermit wird ein Datencontroller erstellt.
[azdata arc dc delete](#azdata-arc-dc-delete) | Hiermit wird ein Datencontroller gelöscht.
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | Endpunktbefehle
[azdata arc dc status](reference-azdata-arc-dc-status.md) | Statusbefehle
[azdata arc dc config](reference-azdata-arc-dc-config.md) | Konfigurationsbefehle
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | Befehle für Debuggen
[azdata arc dc export](#azdata-arc-dc-export) | Hiermit werden Metriken, Protokolle oder der Verbrauch exportiert.
[azdata arc dc upload](#azdata-arc-dc-upload) | Hiermit werden exportierte Datendateien hochgeladen.
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
Hiermit wird ein Datencontroller erstellt. Eine Kube-Konfiguration sowie die Umgebungsvariablen ['AZDATA_USERNAME', 'AZDATA_PASSWORD'] müssen in Ihrem System vorhanden sein.
```bash
azdata arc dc create --namespace -ns 
                     --name -n  
                     
--connectivity-mode  
                     
--resource-group -g  
                     
--location -l  
                     
--subscription -s  
                     
[--profile-name]  
                     
[--path -p]  
                     
[--storage-class -sc]
```
### <a name="examples"></a>Beispiele
Datencontrollerbereitstellung.
```bash
azdata arc dc create
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--namespace -ns`
Der Kubernetes-Namespace, in dem der Datencontroller bereitgestellt werden soll. Wenn er bereits vorhanden ist, wird er verwendet. Ist keiner vorhanden, wird zunächst versucht, ihn zu erstellen.
#### `--name -n`
Der Name des Datencontrollers.
#### `--connectivity-mode`
Die Konnektivität zu Azure – indirekt oder direkt, die für den Datencontroller gelten soll.
#### `--resource-group -g`
Die Azure-Ressourcengruppe, in der die Datencontrollerressource hinzugefügt werden soll.
#### `--location -l`
Der Speicherort in Azure, an dem die Metadaten des Datencontrollers gespeichert werden (z. B. eastus).
#### `--subscription -s`
Die Azure-Abonnement-ID, in der die Datencontrollerressource hinzugefügt werden soll.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--profile-name`
Der Name eines vorhandenen Konfigurationsprofils. Führen Sie `azdata arc dc config list` aus, damit verfügbare Optionen angezeigt werden. Einer der folgenden Werte: ['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-kubeadm', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci'].
#### `--path -p`
Der Pfad zu einem Verzeichnis, das ein benutzerdefiniertes Konfigurationsprofil enthält, das benutzt werden kann. Führen Sie `azdata arc dc config init` aus, um ein benutzerdefiniertes Konfigurationsprofil zu erstellen.
#### `--storage-class -sc`
Die Speicherklasse, die für alle persistenten Volumes für Daten und Protokolle für alle Datencontrollerpods verwendet werden soll, die sie erfordern.
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
Hiermit wird der Datencontroller gelöscht. Eine Kube-Konfiguration ist in Ihrem System erforderlich.
```bash
azdata arc dc delete --name -n 
                     --namespace -ns  
                     
[--force -f]
```
### <a name="examples"></a>Beispiele
Datencontrollerbereitstellung.
```bash
azdata arc dc delete
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--name -n`
Name des Datencontrollers.
#### `--namespace -ns`
Der Kubernetes-Namespace, in dem der Datencontroller vorhanden ist.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--force -f`
Hiermit wird das Löschen des Datencontrollers erzwungen.
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
Hiermit werden Metriken, Protokolle oder der Verbrauch in eine Datei exportiert.
```bash
azdata arc dc export --type -t 
                     --path -p  
                     
[--force -f]
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--type -t`
Der Typ der Daten, die exportiert werden sollen. Optionen: Protokolle, Metriken und Verbrauch.
#### `--path -p`
Der vollständige oder relative Pfad einschließlich des Dateinamens der Datei, die exportiert werden soll.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--force -f`
Hiermit wird die Erstellung der Ausgabedatei erzwungen. Hiermit werden vorhandene Dateien unter demselben Pfad überschrieben.
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
Hiermit werden von einem Datencontroller exportierte Datendateien zu Azure hochgeladen.
```bash
azdata arc dc upload --path -p 
                     
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--path -p`
Der vollständige oder relative Pfad einschließlich des Dateinamens der Datei, die hochgeladen werden soll.
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

