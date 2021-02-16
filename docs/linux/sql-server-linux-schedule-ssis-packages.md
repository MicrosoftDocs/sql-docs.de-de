---
title: Planen von SSIS-Paketen unter Linux mit Cron
description: In diesem Artikel wird die Planung von SSIS-Paketen (SQL Server Integration Services) unter Linux mit dem Cron-Dienst beschrieben.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 75ac9be9d128f76157cee054674ecfe29de06063
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346429"
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>Zeitliches Planen der Ausführung von SSIS-Paketen unter Linux mit Cron

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Wenn Sie SQL Server Integration Services (SSIS) und SQL Server unter Windows ausführen, können Sie die Ausführung von SSIS-Paketen mit dem SQL Server-Agent automatisieren. Wenn Sie SQL Server und SSIS unter Linux ausführen, ist das SQL Server-Agent-Hilfsprogramm jedoch nicht zum Planen von Aufträgen unter Linux verfügbar. Stattdessen verwenden Sie den Cron-Dienst, der häufig auf Linux-Plattformen verwendet wird, um die Paketausführung zu automatisieren.

Dieser Artikel enthält Beispiele, die zeigen, wie Sie die Ausführung von SSIS-Paketen automatisieren. Die Beispiele sind so geschrieben, dass sie in Red Hat Enterprise ausgeführt werden. Der Code ist für andere Linux-Distributionen wie z.B. Ubuntu ähnlich.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Cron-Dienst zum Ausführen von Aufträgen verwenden, überprüfen Sie, ob er auf dem Computer ausgeführt wird.

Verwenden Sie den folgenden Befehl, um den Status des Cron-Diensts zu überprüfen: `systemctl status crond.service`.

Wenn der Dienst nicht aktiv ist (d.h. nicht ausgeführt wird), wenden Sie sich an Ihren Administrator, um den Cron-Dienst ordnungsgemäß einzurichten und zu konfigurieren.

## <a name="create-jobs"></a>Erstellen von Aufträgen

Ein Cron-Auftrag ist eine Aufgabe, die Sie so konfigurieren können, dass sie regelmäßig in einem bestimmten Intervall ausgeführt wird. Der Auftrag kann so einfach wie ein Befehl sein, den Sie normalerweise direkt in der Konsole eingeben oder als Shellskript ausführen.

Zur Vereinfachung der Verwaltung und Wartung empfiehlt es sich, die Paketausführungsbefehle in ein Skript einzufügen, das einen beschreibenden Namen enthält.

Im Folgenden finden Sie ein Beispiel für ein einfaches Shellskript zum Ausführen eines Pakets. Es enthält nur einen einzigen Befehl, aber Sie können nach Bedarf weitere Befehle hinzufügen.

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>Planen von Aufträgen mit dem Cron-Dienst

Nachdem Sie Ihre Aufträge definiert haben, können Sie planen, dass sie automatisch mit dem Cron-Dienst ausgeführt werden.

Um den Auftrag zum Ausführen durch Cron hinzuzufügen, fügen Sie den Auftrag der crontab-Datei hinzu. Verwenden Sie den folgenden Befehl, um die crontab-Datei in einem Editor zu öffnen, in dem Sie den Auftrag hinzufügen oder aktualisieren können: `crontab -e`.

Wenn Sie planen möchten, dass der zuvor beschriebene Auftrag täglich um 2:10 Uhr ausgeführt wird, fügen Sie der crontab-Datei die folgende Zeile hinzu:

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

Speichern Sie die crontab-Datei, und beenden Sie dann den Editor.

Um das Format des Beispielbefehls zu verstehen, lesen Sie die Informationen im folgenden Abschnitt.
 
## <a name="format-of-a-crontab-file"></a>Format einer crontab-Datei

Die folgende Abbildung zeigt die Formatbeschreibung der Auftragszeile, die der crontab-Datei hinzugefügt wird.

![Formatbeschreibung für den Eintrag in die crontab-Datei](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

Um eine ausführlichere Beschreibung des Formats der crontab-Datei zu erhalten, verwenden Sie den folgenden Befehl: `man 5 crontab`.

Im Folgenden sehen Sie ein Teilbeispiel der Ausgabe, um das Beispiel in diesem Artikel zu erläutern:

![Ausführliche teilweise Beschreibung des crontab-Formats](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>Verwandte Inhalte zu SSIS unter Linux
-   [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](sql-server-linux-migrate-ssis.md)
-   [Installieren von SSIS (SQL Server Integration Services) unter Linux](sql-server-linux-setup-ssis.md)
-   [Konfigurieren von SQL Server Integration Services unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md)
-   [Einschränkungen und bekannte Probleme bei SSIS unter Linux](sql-server-linux-ssis-known-issues.md)
