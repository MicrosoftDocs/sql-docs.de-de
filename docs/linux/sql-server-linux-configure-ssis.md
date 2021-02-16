---
title: Konfigurieren von SSIS unter Linux mit ssis-conf
description: In diesem Artikel wird die Konfiguration von SQL Server Integration Services (SSIS) unter Linux mit dem Hilfsprogramm ssis-conf beschrieben.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: da546bddb3b7b45753b224539a546f96663704c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348197"
---
# <a name="configure-sql-server-integration-services-on-linux-with-ssis-conf"></a>Konfigurieren von SQL Server Integration Services unter Linux mit ssis-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Sie führen das `ssis-conf`-Konfigurationsskript aus, wenn Sie SQL Server Integration Services (SSIS) für Red Hat Enterprise Linux und Ubuntu installieren. Weitere Informationen zum Installieren von SSIS finden Sie unter [Installieren von SSIS (SQL Server Integration Services) unter Linux](sql-server-linux-setup-ssis.md).

Sie können auch das `ssis-conf`-Hilfsprogramm verwenden, um die folgenden Eigenschaften zu konfigurieren:

| Get-Help | BESCHREIBUNG |
|-------------|---------------------------------------------------------------------|
| set-edition | Einstellen der SQL Server-Edition                                       |
| telemetry   | Aktivieren oder Deaktivieren des SQL Server Integration Services-Telemetriediensts |
| Setup       | Initialisieren und Einrichten von Microsoft SQL Server Integration Services      |
|||

## <a name="run-ssis-conf"></a>Ausführen von ssis-conf

In den Beispielen in diesem Artikel wird `ssis-conf` durch Angabe des vollständigen Pfads ausgeführt: `/opt/ssis/bin/ssis-conf`. Wenn Sie zu diesem Speicherort navigieren, bevor Sie `ssis-conf` ausführen, können Sie das Hilfsprogramm im Kontext des aktuellen Verzeichnisses ausführen: `./ssis-conf`.

Stellen Sie sicher, dass Sie die in diesem Artikel beschriebenen Befehle mit Stammberechtigungen ausführen. Führen Sie z.B. `sudo /opt/ssis/bin/ssis-conf setup` und nicht `/opt/ssis/bin/ssis-conf setup` aus.

Wenn Sie diese Befehle mit Eingabeaufforderungen in der von Ihnen bevorzugten Sprache ausführen möchten, können Sie ein Gebietsschema angeben. Führen Sie z.B. den folgenden Befehl aus, um Eingabeaufforderungen in Chinesisch zu erhalten: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="use-set-edition-to-set-the-edition-of-sql-server-integration-services"></a>Verwenden von „set-edition“, um die Edition von SQL Server Integration Services einzustellen

Die Edition von SSIS hängt von der Edition von SQL Server ab.

Geben Sie den folgenden Befehl ein: `$ sudo /opt/ssis/bin/ssis-conf set-edition`.

Nachdem Sie den Befehl eingegeben haben, erhalten Sie die folgende Eingabeaufforderung:

```
Choose an edition of SQL Server:

1) Evaluation (free, no production use rights, 180-day limit)

2) Developer (free, no production use rights)

3) Express (free)

4) Web (PAID)

5) Standard (PAID)

6) Enterprise (PAID)

7) Enterprise Core (PAID)

8) I bought a license through a retail sales channel and have a product key to enter.

Details about editions can be found at https://go.microsoft.com/fwlink/?LinkId=852748&clcid=0x409.

Use of PAID editions of this software requires separate licensing through a Microsoft Volume Licensing program.

By choosing a PAID edition, you are verifying that you have the appropriate number of licenses in place to install and run this software.

Enter your edition (1-8):
```

Wenn Sie einen Wert von 1 bis 7 eingeben, konfiguriert das System eine kostenlose oder BEZAHLT-Edition. Wenn Sie 8 eingeben, werden Sie vom Hilfsprogramm aufgefordert, den von Ihnen erworbenen Product Key einzugeben:

```
Enter the 25-character product key:
```

## <a name="use-telemetry-to-configure-customer-feedback"></a>Verwenden von „telemetry“ zum Konfigurieren von Kundenfeedback

Der `telemetry`-Befehl bestimmt, ob SSIS Feedback an Microsoft sendet.

Für kostenlose Editionen (d.h. Express-, Developer- und Evaluation-Editionen) ist der Telemetriedienst immer aktiviert. Wenn Sie über eine kostenlose Edition verfügen, können Sie den `telemetry`-Befehl nicht verwenden, um Telemetrie zu deaktivieren.

Geben Sie den folgenden Befehl ein: `$ sudo /opt/ssis/bin/ssis-conf telemetry`.

Bei BEZAHLT-Editionen erhalten Sie nach Eingabe des Befehls die folgende Eingabeaufforderung:

```
Send feature usage data to Microsoft. Feature usage data includes information about your hardware configuration and how you use SQL Server Integration Services.

[Yes/No]:
```

Wenn Sie **Ja** auswählen, wird der Telemetriedienst aktiviert und die Ausführung gestartet. Der Dienst wird nach jedem Start des Computers automatisch gestartet. Wenn Sie **Nein** auswählen, wird der Telemetriedienst angehalten und ist deaktiviert.

## <a name="use-setup-to-initialize-and-set-up-microsoft-sql-server-integration-services"></a>Verwenden von „setup“ zum Initialisieren und Einrichten von Microsoft SQL Server Integration Services

Verwenden Sie den `setup`-Befehl jedes Mal, wenn Sie SSIS installieren.

Geben Sie den folgenden Befehl ein: `sudo /opt/ssis/bin/ssis-conf setup`.

Das Hilfsprogramm fordert Sie auf, Werte für die folgenden Elemente zu bestätigen oder anzugeben:
-   Produktlizenz
-   Endbenutzer-Lizenzvertrag
-   Telemetriedienst
-   Die von Integration Services verwendete Sprache

Wenn Sie den `setup`-Befehl mit Eingabeaufforderungen in der von Ihnen bevorzugten Sprache ausführen möchten, können Sie ein Gebietsschema angeben. Führen Sie z.B. den folgenden Befehl aus, um Eingabeaufforderungen in Chinesisch zu erhalten: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="ssisconf-format"></a>ssis.conf-Format

Die folgende `/var/opt/ssis/ssis.conf`-Datei enthält für jede Einstellung ein Beispiel.

Für SQL Server können Sie Systemeinstellungen ändern, indem Sie die Werte in der `mssql.conf`-Datei ändern. Für SSIS können Sie Systemeinstellungen *nicht* ändern, indem Sie die Werte in der `ssis.conf`-Datei ändern. Die `ssis.conf`-Datei zeigt nur die Ergebnisse des Setups an. Wenn Sie die Einstellungen für SSIS ändern möchten, können Sie die `ssis.conf`-Datei löschen und den `setup`-Befehl erneut ausführen.

Hier ist das Beispiel einer `ssis.conf`-Datei. Jedes Feld entspricht dem Ergebnis eines Setupschritts.

```
[LICENSE]
                       
registered = Y        
                       
pid = enterprisecore  
                       
[EULA]
                       
accepteula = Y        
                       
[TELEMETRY]
                       
enabled = Y           
                       
[language]
                       
lcid = 2052
```

## <a name="related-content-about-ssis-on-linux"></a>Verwandte Inhalte zu SSIS unter Linux
-   [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](sql-server-linux-migrate-ssis.md)
-   [Installieren von SSIS (SQL Server Integration Services) unter Linux](sql-server-linux-setup-ssis.md)
-   [Einschränkungen und bekannte Probleme bei SSIS unter Linux](sql-server-linux-ssis-known-issues.md)
-   [Zeitliches Planen der Ausführung von SSIS-Paketen unter Linux mit Cron](sql-server-linux-schedule-ssis-packages.md)
