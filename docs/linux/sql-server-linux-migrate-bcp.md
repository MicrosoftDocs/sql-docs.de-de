---
title: Massenkopieren von Daten in SQL Server für Linux
description: In diesem Artikel wird das bcp-Hilfsprogramm beschrieben. Sie können das bcp-Hilfsprogramm verwenden, um große Mengen von Zeilen in SQL Server-Tabellen zu importieren oder um Daten aus SQL Server-Tabellen in Datendateien zu exportieren.
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: c27e4095854abd409336c97ac5b8458bf6f62894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345613"
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>Massenkopieren von Daten mit BCP in SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie Sie das Befehlszeilen-Hilfsprogramm [BCP](../tools/bcp-utility.md) zum Massenkopieren von Daten zwischen einer Instanz von SQL Server für Linux und einer Datendatei in einem benutzerdefinierten Format verwenden.

Sie können mit `bcp` große Mengen neuer Zeilen in SQL Server-Tabellen importieren oder Daten aus SQL Server-Tabellen in Datendateien exportieren. Außer in Verbindung mit der Option „queryout“ sind für `bcp` keine Kenntnisse von Transact-SQL erforderlich. Das Befehlszeilen-Hilfsprogramm `bcp` kann in Microsoft SQL Server lokal ausgeführt werden oder in der Cloud, unter Linux, Windows oder Docker sowie SQL-Datenbank und Azure Synapse Analytics.

In diesem Artikel lernen Sie Folgendes:
- Importieren von Daten in eine Tabelle mithilfe des `bcp in`
- Exportieren von Daten aus einer Tabelle mithilfe des `bcp out`-Befehls

## <a name="install-the-sql-server-command-line-tools"></a>Installieren der SQL Server-Befehlszeilentools

`bcp` ist Teil der SQL Server-Befehlszeilentools, die nicht automatisch mit SQL Server für Linux installiert werden. Wenn Sie die SQL Server-Befehlszeilentools nicht bereits auf Ihrem Linux-Computer installiert haben, müssen Sie sie installieren. Um weitere Informationen zum Installieren der Tools zu erhalten, wählen Sie Ihre Linux-Distribution in der folgenden Liste aus:

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>Importieren von Daten mit BCP

In diesem Tutorial erstellen Sie eine Beispieldatenbank und eine Tabelle auf der lokalen SQL Server-Instanz (**localhost**) und laden dann mit `bcp` eine Textdatei auf dem Datenträger in die Beispieltabelle.

### <a name="create-a-sample-database-and-table"></a>Erstellen einer Beispieldatenbank und -tabelle

Zunächst erstellen wir eine Beispieldatenbank mit einer einfachen Tabelle, die im restlichen Tutorial verwendet wird.

1. Öffnen Sie in Ihrer Linux-Box ein Befehlsterminal.

2. Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminalfenster ein. Diese Befehle erstellen mit dem Befehlszeilen-Hilfsprogramm **sqlcmd** eine Beispieldatenbank (**BcpSampleDB**) und eine Tabelle (**TestEmployees**) auf der lokalen SQL Server-Instanz (**localhost**). Denken Sie daran, `username` und `<your_password>` vor dem Ausführen der Befehle nach Bedarf zu ersetzen.

Erstellen Sie die Datenbank **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
Erstellen Sie die Tabelle **TestEmployees** in der Datenbank **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>Erstellen der Quelldatendatei
Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminalfenster ein. Wir erstellen mit dem integrierten `cat`-Befehl eine Beispiel-Textdatendatei mit drei Datensätzen. Speichern Sie die Datei in Ihrem Basisverzeichnis als **~/test_data.txt**. Die Felder in den Datensätzen werden durch Kommas getrennt.

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

Sie können überprüfen, ob die Datendatei ordnungsgemäß erstellt wurde, indem Sie den folgenden Befehl im Terminalfenster ausführen:
```bash 
cat ~/test_data.txt
```

Im Terminalfenster sollte Folgendes angezeigt werden:
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>Importieren von Daten aus der Quelldatendatei
Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminalfenster ein. Dieser Befehl stellt mit `bcp` eine Verbindung mit der lokalen SQL Server-Instanz (**localhost**) her und importiert die Daten aus der Datendatei (**~/test_data.txt**) in die Tabelle (**TestEmployees**) in der Datenbank (**BcpSampleDB**). Denken Sie daran, den Benutzernamen und `<your_password>` vor dem Ausführen der Befehle nach Bedarf zu ersetzen.

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

Im Folgenden finden Sie eine kurze Übersicht über die Befehlszeilenparameter, die in diesem Beispiel mit `bcp` verwendet wurden:
- `-S`: Gibt die SQL Server-Instanz an, mit der eine Verbindung hergestellt werden soll.
- `-U`: Gibt die Anmelde-ID an, die zum Herstellen einer Verbindung mit SQL Server verwendet wird.
- `-P`: Gibt das Kennwort für die Anmelde-ID an.
- `-d`: Gibt die Datenbank an, mit der eine Verbindung hergestellt werden soll.
- `-c`: Führt die Vorgänge mithilfe eines Zeichendatentyps aus.
- `-t`: Gibt das Feldabschlusszeichen an. Wir verwenden `comma` als Feldabschlusszeichen für die Datensätze in der Datendatei.

> [!NOTE]
> In diesem Beispiel wird kein benutzerdefiniertes Zeilenabschlusszeichen angegeben. Die Zeilen in der Textdatendatei wurden mit `newline` ordnungsgemäß beendet, als zuvor der `cat`-Befehl zum Erstellen der Datendatei verwendet wurde.

Sie können überprüfen, ob die Daten ordnungsgemäß importiert wurden, indem Sie den folgenden Befehl im Terminalfenster ausführen. Denken Sie daran, `username` und `<your_password>` vor dem Ausführen des Befehls nach Bedarf zu ersetzen.
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

Daraufhin sollten folgende Ergebnisse angezeigt werden:
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>Exportieren von Daten mit BCP

In diesem Tutorial verwenden Sie `bcp`, um Daten aus der zuvor erstellten Beispieltabelle in eine neue Datendatei zu exportieren.

Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminalfenster ein. Diese Befehle exportieren mit dem Befehlszeilen-Hilfsprogramm `bcp` Daten aus der Tabelle **TestEmployees** in der Datenbank **BcpSampleDB** in eine neue Datendatei mit dem Namen **~/test_export.txt**.  Denken Sie daran, den Benutzernamen und `<your_password>` vor dem Ausführen des Befehls nach Bedarf zu ersetzen.

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

Sie können überprüfen, ob die Daten ordnungsgemäß exportiert wurden, indem Sie den folgenden Befehl im Terminalfenster ausführen:
```bash 
cat ~/test_export.txt
```

Im Terminalfenster sollte Folgendes angezeigt werden:
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>Weitere Informationen
- [bcp-Hilfsprogramm](../tools/bcp-utility.md)
- [Datenformate für die Kompatibilität bei Verwendung von BCP](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [Importieren von Massendaten mithilfe von BULK INSERT](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
