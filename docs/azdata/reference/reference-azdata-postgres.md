---
title: Referenz zu azdata postgres
titleSuffix: SQL Server big data clusters
description: Hier finden Sie einen Referenzartikel zu azdata postgres-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6403db5f9b716684c3a791ade183a8f9c5edad47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052271"
---
# <a name="azdata-postgres"></a>azdata postgres

Gilt für [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]e

Der folgende Artikel enthält Referenzinformationen zu den **sql**-Befehlen im **azdata**-Tool. Weitere Informationen zu anderen **azdata**-Befehlen finden Sie unter [azdata](reference-azdata.md).

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
[azdata postgres shell](#azdata-postgres-shell) | Eine Befehlszeilenshellschnittstelle für Postgres. Siehe https://www.pgcli.com/.
[azdata postgres query](#azdata-postgres-query) | Der Abfragebefehl ermöglicht die Ausführung von PostgreSQL-Befehlen in einer Datenbanksitzung.
## <a name="azdata-postgres-shell"></a>azdata postgres shell
Eine Befehlszeilenshellschnittstelle für Postgres. Siehe https://www.pgcli.com/.
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>Beispiele
Beispielbefehlszeile, um die interaktive Umgebung zu starten.
```bash
azdata postgres shell
```
Beispielbefehlszeile mit bereitgestellter Datenbank und bereitgestelltem Benutzer
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
Beispielbefehlszeile für den Beginn mit einer vollständigen Verbindungszeichenfolge.
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--dbname -d`
Name der Datenbank, zu der eine Verbindung hergestellt werden soll.
#### `--host`
Hostadresse der Postgres-Datenbank.
#### `--port -p`
Portnummer, an der die Postgres-Instanz lauscht.
#### `--password -w`
Hiermit wird die Eingabeaufforderung für Kennwörter erzwungen.
#### `--no-password`
Hiermit wird die Aufforderung zur Eingabe eines Kennworts niemals erzwungen.
#### `--single-connection`
Hiermit werden keine eigenen Verbindungen für Abschlüsse verwendet.
#### `--username -u`
Benutzername zur Herstellung einer Verbindung zur Postgres-Datenbank.
#### `--pgclirc`
Speicherort der pgclirc-Datei.
#### `--dsn`
Hiermit wird der im [alias_dsn]-Abschnitt der pgclirc-Datei konfigurierte DSN verwendet.
#### `--list-dsn`
Liste des im [alias_dsn]-Abschnitt der pgclirc-Datei konfigurierten DSN.
#### `--row-limit`
Hiermit wird ein Schwellenwert für die Aufforderung für ein Zeilenlimit festgelegt. Verwenden Sie 0, um die Aufforderung zu deaktivieren.
#### `--less-chatty`
Hiermit wird das Intro beim Start und Beenden übersprungen.
#### `--prompt`
Eingabeaufforderungsformat (Standardwert: „\u@\h:\d> “).
#### `--prompt-dsn`
Eingabeaufforderungsformat für Verbindungen, für die DSN-Aliasse verwendet werden (Standardwert: „\u@\h:\d> “).
#### `--list -l`
Hiermit werden die verfügbaren Datenbanken aufgelistet. Danach wird der Befehl beendet.
#### `--auto-vertical-output`
Hiermit wird automatisch zum vertikalen Ausgabemodus gewechselt, wenn das Ergebnis breiter als die Breite des Terminals ist.
#### `--warn`
Hiermit wird eine Warnung ausgegeben, bevor eine destruktive Abfrage ausgeführt wird.
#### `--no-warn`
Hiermit wird eine Warnung ausgegeben, bevor eine destruktive Abfrage ausgeführt wird.
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
## <a name="azdata-postgres-query"></a>azdata postgres query
Der Abfragebefehl ermöglicht die Ausführung von PostgreSQL-Befehlen in einer Datenbanksitzung.
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>Beispiele
Hiermit werden alle Tabellen in information_schema aufgelistet.
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--q -q`
Die PostgreSQL-Abfrage, die ausgeführt werden soll.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--host`
Hostadresse der Postgres-Datenbank.
`localhost`
#### `--dbname -d`
Die Datenbank, in der die Abfrage ausgeführt werden soll.
#### `--port -p`
Portnummer, an der die Postgres-Instanz lauscht.
`5432`
#### `--username -u`
Benutzername zur Herstellung einer Verbindung zur Postgres-Datenbank.
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

