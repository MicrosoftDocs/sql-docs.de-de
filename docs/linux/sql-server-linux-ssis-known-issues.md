---
title: Einschränkungen und bekannte Probleme bei SSIS unter Linux
description: In diesem Artikel werden Einschränkungen und bekannte Probleme bei den SQL Server Integration Services (SSIS) auf Linux-Computern beschrieben.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 06/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ce07fb02a81b4e44375d80f643713b9a0b62d80d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838996"
---
# <a name="limitations-and-known-issues-for-ssis-on-linux"></a>Einschränkungen und bekannte Probleme bei SSIS unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden Einschränkungen und bekannte Probleme bei den SQL Server Integration Services (SSIS) unter Linux beschrieben.

## <a name="general-limitations-and-known-issues"></a>Allgemeine Einschränkungen und bekannte Probleme

Die folgenden Features werden in diesem Release von SSIS unter Linux nicht unterstützt:
  - SSIS-Katalog-Datenbank
  - Geplante Paketausführung des SQL Agent
  - Windows-Authentifizierung
  - Drittanbieterkomponenten
  - Change Data Capture (CDC)
  - SSIS Scale Out
  - Azure Feature Pack für SSIS
  - Hadoop- und HDFS-Unterstützung
  - Microsoft Connector for SAP BW

Weitere Einschränkungen und bekannte Probleme mit SSIS unter Linux finden Sie in den [Versionshinweisen](sql-server-linux-release-notes.md#ssis).

## <a name="supported-and-unsupported-components"></a><a name="components"></a> Unterstützte und nicht unterstützte Komponenten

Die folgenden integrierten Integration Services-Komponenten werden unter Linux unterstützt. Für einige von ihnen gelten Einschränkungen auf der Linux-Plattform. Integrierte Komponenten, die hier nicht aufgeführt sind, werden unter Linux nicht unterstützt.

## <a name="supported-control-flow-tasks"></a>Unterstützte Ablaufsteuerungsaufgaben
- Masseneinfügungstask
- Datenflusstask
- Datenprofilerstellungs-Task
- SQL ausführen (Task)
- T-SQL-Anweisung ausführen (Task)
- Task 'Ausdruck'
- FTP-Task
- Webdienst (Task)
- XML-Task

## <a name="control-flow-tasks-supported-with-limitations"></a>Unterstützte Ablaufsteuerungsaufgaben mit Einschränkungen

| Aufgabe | Einschränkungen |
|------------|---|
| Task Prozess ausführen | Nur im In-Process-Modus unterstützt. |
| Dateisystem (Task) | Die Aktionen *Verzeichnis verschieben* und *Dateiattribute festlegen* werden nicht unterstützt. |
| Skripttask | Unterstützt nur standardmäßige .NET Framework-APIs. |
| Mail senden (Task) | Unterstützt nur den anonymen Benutzermodus. |
| Datenbank übertragen (Task) | UNC-Pfade werden nicht unterstützt. |
| | |

## <a name="supported-and-unsupported-maintenance-plan-tasks"></a>Unterstützte und nicht unterstützte Wartungsplantasks

In einem SQL Server-Wartungsplan können Sie in der Regel eine Vielzahl von SSIS-Tasks verwenden.

Die folgenden Wartungsplantasks werden unter Linux nicht unterstützt:
- Operator benachrichtigen
- Auftrag des SQL Server-Agent ausführen

Die folgenden Wartungsplantasks werden unter Linux unterstützt:
- Datenbankintegrität überprüfen
- Verkleinern der Datenbank
- Index neu organisieren
- Index neu erstellen
- Statistikaktualisierung
- Verlauf bereinigen
- Datenbank sichern
- T-SQL-Anweisung

## <a name="supported-control-flow-containers"></a>Unterstützte Ablaufsteuerungscontainer
- Sequenzcontainer
- For-Schleifencontainer
- Foreach-Schleifencontainer

## <a name="supported-data-flow-sources-and-destinations"></a>Unterstützte Datenflussquellen und -ziele
- Rohdatendatei-Quelle und -Ziel
- XML-Quelle

## <a name="data-flow-sources-and-destinations-supported-with-limitations"></a>Datenflussquellen und -ziele, die mit Einschränkungen unterstützt werden

| Komponente | Einschränkungen |
|------------|---|
| ADO.NET-Quelle und -Ziel | Unterstützt nur den SQLClient-Datenanbieter. |
| Flatfilequelle und -ziel | 1. Unterstützt nur Dateipfade im Windows-Stil, auf die die Standardpfad-Zuordnungsregel angewendet wird. `D:\home\ssis\travel.csv` wird z.B. zu `/home/ssis/travel.csv`. 2. Die unter Red Hat 7 bereitgestellte Azure-Dateifreigabe wird nicht unterstützt. |
| OData-Quelle | Unterstützt nur die Standardauthentifizierung. |
| ODBC-Quelle und -Ziel | Unterstützt 64-Bit-Unicode-ODBC-Treiber unter Linux. Hängt vom UnixODBC-Treiber-Manager unter Linux ab. |
| OLE DB-Quelle und -Ziel | Unterstützt nur SQL Server Native Client 11.0 und Microsoft OLE DB-Anbieter für SQL Server. |
| | |

## <a name="supported-data-flow-transformations"></a>Unterstützte Datenflusstransformationen
- Aggregat
- Audit
- Balanced Data Distributor
- Zeichenzuordnung
- Bedingtes Teilen
- Kopieren von Spalten
- Datenkonvertierung
- Abgeleitete Spalte
- Exportieren von Spalten
- Fuzzygruppierung
- Fuzzysuche
- Importieren von Spalten
- Nachschlagen
- Merge
- Merge Join
- Multicast
- Pivotieren
- Zeilenanzahl
- Langsam veränderliche Dimensionen
- Sortieren
- Ausdruckssuche
- Union All
- Entpivotieren

## <a name="data-flow-transformations-supported-with-limitations"></a>Mit Einschränkungen unterstützte Datenflusstransformationen

| Komponente | Einschränkungen |
|------------|---|
| Transformation für OLE DB-Befehl | Die gleichen Einschränkungen wie OLE DB-Quelle und -Ziel. |
| Skriptkomponente | Unterstützt nur standardmäßige .NET Framework-APIs. |
| | |

## <a name="supported-and-unsupported-log-providers"></a>Unterstützte und nicht unterstützte Protokollanbieter
Alle integrierten SSIS-Protokollanbieter außer dem Windows-Ereignisprotokollanbieter werden unter Linux unterstützt.

Der SQL Server-Protokollanbieter unterstützt nur die SQL-Authentifizierung, nicht die Windows-Authentifizierung.

Die SSIS-Protokollanbieter für Textdateien, XML-Dateien und SQL Server Profiler schreiben ihre Ausgabe in eine von Ihnen angegebene Datei. Die folgenden Überlegungen betreffen den Dateipfad:
-   Wenn Sie keinen Pfad angeben, schreibt der Protokollanbieter in das aktuelle Verzeichnis des Hosts. Wenn der aktuelle Benutzer nicht über die Berechtigung zum Schreiben in das aktuelle Verzeichnis des Hosts verfügt, löst der Protokollanbieter einen Fehler aus.
-   Es ist nicht möglich, eine Umgebungsvariable in einem Dateipfad zu verwenden. Wenn Sie eine Umgebungsvariable angeben, wird der von Ihnen angegebene literale Text im Dateipfad angezeigt. Wenn Sie z.B. `%TMP%/log.txt` angeben, fügt der Protokollanbieter den literalen Text `/%TMP%/log.txt` an das aktuelle Hostverzeichnis an.

## <a name="related-content-about-ssis-on-linux"></a>Verwandte Inhalte zu SSIS unter Linux
-   [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](sql-server-linux-migrate-ssis.md)
-   [Installieren von SSIS (SQL Server Integration Services) unter Linux](sql-server-linux-setup-ssis.md)
-   [Konfigurieren von SQL Server Integration Services unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md)
-   [Zeitliches Planen der Ausführung von SSIS-Paketen unter Linux mit Cron](sql-server-linux-schedule-ssis-packages.md)
