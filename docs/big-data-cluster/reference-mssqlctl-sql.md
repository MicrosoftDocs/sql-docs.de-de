---
title: Mssqlctl Sql-Referenz
titleSuffix: SQL Server big data clusters
description: Der Referenzartikel für die Sql-Befehlen Mssqlctl.
author: rothja
ms.author: jroth
manager: jroth
ms.date: 06/26/2019
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 536d4712a6ccb288ca60c038a37e99c2be8a5eba
ms.sourcegitcommit: ce5770d8b91c18ba5ad031e1a96a657bde4cae55
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2019
ms.locfileid: "67394202"
---
# <a name="mssqlctl-sql"></a>Mssqlctl sql

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

Der folgende Artikel bietet Referenz für die **Sql** Befehle in der **Mssqlctl** Tool. Weitere Informationen zu anderen **Mssqlctl** Befehle finden Sie unter [Mssqlctl Verweis](reference-mssqlctl.md).

## <a name="commands"></a>Befehle
|     |     |
| --- | --- |
[Mssqlctl Sql-shell](#mssqlctl-sql-shell) | Die SQL-DB-CLI ermöglicht den Benutzer für die Interaktion mit SQL Server über T-SQL.
[mssqlctl sql query](#mssqlctl-sql-query) | Der Abfragebefehl ermöglicht das Ausführen einer T-SQL-Abfrage.
## <a name="mssqlctl-sql-shell"></a>Mssqlctl Sql-shell
Die SQL-DB-CLI ermöglicht den Benutzer für die Interaktion mit SQL Server über T-SQL.
```bash
mssqlctl sql shell 
```
### <a name="examples"></a>Beispiele
Beispielbefehlszeile, um die interaktive Umgebung zu starten.
```bash
mssqlctl sql shell
```
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Erhöht die protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt.
#### `--help -h`
Zeigen Sie diese hilfemeldung an und beendet.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: Json, Jsonc, Table, Tsv.  Standardwert: Json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Finden Sie unter [ http://jmespath.org/ ](http://jmespath.org/]) für Weitere Informationen und Beispiele.
#### `--verbose`
Erhöht die protokollierungsausführlichkeit. Verwenden Sie--Debug, wenn Sie vollständige Debugprotokolle wünschen.
## <a name="mssqlctl-sql-query"></a>Mssqlctl Sql-Abfrage
Der Abfragebefehl ermöglicht das Ausführen einer T-SQL-Abfrage.
```bash
mssqlctl sql query --database -d 
                   -q
```
### <a name="examples"></a>Beispiele
Wählen Sie die Liste der Tabellennamen.  Der Standardwert ist Master-Datenbank.
```bash
mssqlctl sql query 'SELECT name FROM SYS.TABLES'
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--database -d`
Die Datenbank zum Ausführen der Abfrage.  Der Standardwert ist master.
#### `-q`
Auszuführende T-SQL-Abfrage.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Erhöht die protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt.
#### `--help -h`
Zeigen Sie diese hilfemeldung an und beendet.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: Json, Jsonc, Table, Tsv.  Standardwert: Json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Finden Sie unter [ http://jmespath.org/ ](http://jmespath.org/]) für Weitere Informationen und Beispiele.
#### `--verbose`
Erhöht die protokollierungsausführlichkeit. Verwenden Sie--Debug, wenn Sie vollständige Debugprotokolle wünschen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Installieren der **Mssqlctl** finden Sie unter [Mssqlctl zum Verwalten von SQL Server-2019 big Data-Cluster installieren](deploy-install-mssqlctl.md).