---
title: Referenz zu „azdata arc postgres server backup“
titleSuffix: SQL Server big data clusters
description: Dies ist der Referenzartikel zu azdata arc postgres server backup-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 00519fa62e0813bc25cee6599cd2faca9ed1489d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052651"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe erstellt.
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe gelöscht.
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe wiederhergestellt.
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | Hiermit wird der Status des letzten Wiederherstellungsvorgangs abgerufen, sofern vorhanden.
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | Hiermit zeigen Sie Details für eine Sicherung für eine PostgreSQL-Servergruppe an.
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe erstellt.
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>Beispiele
Hiermit wird eine Sicherung für den Dienst „pg“ erstellt.
```bash
azdata arc postgres server backup create -sn pg
```
Hiermit wird eine benannte Sicherung für den Dienst „pg“ erstellt.
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe gelöscht.
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>Beispiele
Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe gelöscht.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
Hiermit wird eine Sicherung für eine PostgreSQL-Servergruppe wiederhergestellt.
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>Beispiele
Hiermit wird die letzte Sicherung wiederhergestellt.
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
Hiermit wird der Status des letzten Wiederherstellungsvorgangs abgerufen, sofern vorhanden.
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>Beispiele
Hiermit wird der aktuelle Wiederherstellungsstatus für den Dienst „pg“ nach ID abgerufen.
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
Hiermit zeigen Sie Details für eine Sicherung für eine PostgreSQL-Servergruppe an.
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>Beispiele
Hiermit wird eine Sicherung für den Dienst „pg“ nach ID abgerufen.
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md). 

Weitere Informationen zur Installation des Tools **azdata** finden Sie unter [Installieren von azdata](..\install\deploy-install-azdata.md).

