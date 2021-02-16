---
title: Problembehandlung bei SQL Server unter Linux
description: Behandeln Sie Probleme bei Microsoft SQL Server für Linux oder in einem Docker-Container. Erfahren Sie, wo Sie Informationen zu unterstützten Features und bekannten Einschränkungen finden.
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 56961a0dd9f57085ebe2d937668fe32c7e28dc73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346219"
---
# <a name="troubleshoot-sql-server-on-linux"></a>Problembehandlung bei SQL Server unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Dokument wird beschrieben, wie Sie Probleme mit Microsoft SQL Server behandeln, die unter Linux oder in Docker-Containern ausgeführt werden. Berücksichtigen Sie bei der Problembehebung bei SQL Server unter Linux die Informationen zu den unterstützten Features und bekannten Einschränkungen in Abschnitt [SQL Server on Linux Release Notes (Versionsanmerkungen zu SQL Server unter Linux)](sql-server-linux-release-notes.md).

> [!TIP]
> Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu SQL Server für Linux](sql-server-linux-faq.md).

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> Beheben von Verbindungsfehlern
Wenn Sie Probleme beim Herstellen einer Verbindung mit Ihrem SQL Server unter Linux haben, müssen Sie einige Dinge überprüfen.

- Wenn es nicht möglich ist, mithilfe von **Localhost** eine Verbindung lokal herzustellen, probieren es mit der Verwendung der IP-Adresse 127.0.0.1. Es ist möglich, dass **Localhost** dieser Adresse nicht ordnungsgemäß zugeordnet ist.

- Überprüfen Sie, ob der Servername oder die IP-Adresse über den Clientcomputer erreichbar ist.

   > [!TIP]
   > Um die IP-Adresse Ihres Ubuntu-Computers zu ermitteln, können Sie den Befehl „ifconfig“ wie im folgenden Beispiel ausführen:
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Für Red Hat können Sie die IP-Adresse wie im folgenden Beispiel verwenden:
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Eine Ausnahme von dieser Methode stellen Azure-VMs dar. Die [öffentliche IP-Adresse für Azure-VMs finden Sie im Azure-Portal](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Überprüfen Sie ggf., ob Sie den SQL Server-Port (standardmäßig 1433) in der Firewall geöffnet haben.

- Überprüfen Sie bei Verwendung von Azure-VMs, [ob für den SQL Server-Standardport eine Netzwerksicherheitsgruppen-Regel festgelegt ist](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Vergewissern Sie sich, dass der Benutzername und das Kennwort keine Tippfehler oder zusätzliche Leerzeichen enthalten oder falsch geschrieben wurden.

- Versuchen Sie, das Protokoll und die Portnummer explizit mit dem Servernamen wie im folgenden Beispiel festzulegen: **tcp:servername,1433**.

- Probleme bei der Netzwerkverbindung können auch Verbindungsfehler und Timeouts verursachen. Nachdem Sie die Verbindungsinformationen und die Netzwerkkonnektivität überprüft haben, versuchen Sie erneut, die Verbindung herzustellen.

## <a name="manage-the-sql-server-service"></a>Verwalten des SQL Server-Diensts

In den folgenden Abschnitten wird beschrieben, wie der SQL Server-Dienst gestartet, beendet, neu gestartet und dessen Status überprüft wird.

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Verwalten des MSSQL-Server-Diensts unter Red Hat Enterprise Linux (RHEL) und Ubuntu 

Überprüfen Sie den Status des SQL Server-Diensts mit dem folgenden Befehl:

   ```bash
   sudo systemctl status mssql-server
   ```

Sie können den SQL Server-Dienst nach Bedarf mit den folgenden Befehlen beenden, starten oder neu starten:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Verwalten der Ausführung des MSSQL-Docker-Containers

Sie können den Status und die Container-ID des zuletzt erstellten SQL Server Docker-Containers durch Ausführen des folgenden Befehls abrufen (die ID befindet sich in der Spalte **CONTAINER-ID**):

   ```bash
   sudo docker ps -l
   ```
   
Sie können den SQL Server-Dienst nach Bedarf mit den folgenden Befehlen beenden oder neu starten:
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Weitere Tipps zur Problembehandlung für Docker finden Sie unter [Troubleshooting SQL Server Docker containers (Problembehandlung bei SQL Server in Docker-Containern)](./sql-server-linux-docker-container-troubleshooting.md).

## <a name="access-the-log-files"></a>Zugreifen auf die Protokolldateien
   
Die SQL Server-Engine protokolliert sowohl in der Linux- als auch in der Docker-Installation in der Datei „/var/opt/mssql/log/errorlog“. Sie müssen sich im Modus „Superuser“ befinden, um dieses Verzeichnis durchsuchen zu können.

Das Installationsprogramm protokolliert hier: „/var/opt/mssql/setup-“< Zeitstempel, der die Uhrzeit der Installation darstellt > Sie können die Fehlerprotokolldateien mit einem beliebigen UTF-16-kompatiblen Tool wie „vim“ oder „cat“ wie folgt durchsuchen: 

   ```bash
   sudo cat errorlog
   ```

Wenn Sie möchten, können Sie die Dateien auch in UTF-8 konvertieren, um sie mit „more“ oder „less“ mit dem folgenden Befehl zu lesen:
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Erweiterte Ereignisse

Erweiterte Ereignisse können über einen SQL-Befehl abgefragt werden.  Weitere Informationen zu erweiterten Ereignissen finden Sie [hier](../relational-databases/extended-events/extended-events.md):

## <a name="crash-dumps"></a>Absturzabbilder 

Suchen Sie im Protokollverzeichnis unter Linux nach Abbildern. Überprüfen Sie das Verzeichnis „/var/opt/mssql/log“ auf Linux-Kernabbilder (.tar.gz2-Erweiterung) oder SQL-Miniabbilder (.mdmp-Erweiterung).

Für Kernabbilder 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Für SQL-Abbilder 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Starten von SQL Server im Minimalkonfigurationsmodus oder Einzelbenutzermodus

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Starten von SQL Server im Minimalkonfiguration
Dies ist hilfreich, wenn der Server aufgrund der Einstellung eines Konfigurationswerts (z. B. aufgrund von Arbeitsspeichermangel) nicht gestartet werden kann.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Starten von SQL Server im Einzelbenutzermodus
Unter bestimmten Umständen müssen Sie möglicherweise eine Instanz von SQL Server mithilfe der Startoption „-m“ im Einzelbenutzermodus starten. Dies ist z. B. der Fall, wenn Sie Serverkonfigurationsoptionen ändern oder eine beschädigte master-Datenbank oder andere Systemdatenbanken wiederherstellen möchten. Ein Beispiel ist, wenn Sie die Serverkonfigurationsoptionen ändern oder eine beschädigte Masterdatenbank oder andere Systemdatenbanken wiederherstellen müssen.   

Starten von SQL Server im Einzelbenutzermodus
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Starten von SQL Server im Einzelbenutzermodus mit „SQLCMD“
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Starten Sie SQL Server unter Linux mit dem Benutzer „MSSQL“, um zukünftige Startprobleme zu vermeiden. Beispiel „sudo-u mssql /opt/mssql/bin/sqlservr [STARTOPTIONEN]“ 

Wenn Sie SQL Server versehentlich mit einem anderen Benutzer gestartet haben, müssen Sie den Besitzer der SQL Server-Datenbankdateien wieder in den Benutzer „MSSQL“ ändern, bevor Sie SQL Server mit „systemd“ starten. Wenn Sie z. B. den Besitzer aller Datenbankdateien unter „/var/opt/mssql“ in den Benutzer „MSSQL“ ändern möchten, führen Sie den folgenden Befehl aus:

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Neuerstellen von Systemdatenbanken
Als letztes Mittel können Sie die Master-und Modeldatenbanken wieder auf die Standardversionen zurücksetzen.

> [!WARNING]
> Mit diesen Schritten **werden alle SQL Server-Systemdaten GELÖSCHT**, die Sie konfiguriert haben. Dies schließt die Informationen über Ihre Benutzerdatenbanken ein (jedoch nicht die Benutzerdatenbanken selbst). Außerdem werden andere in den Systemdatenbanken gespeicherte Informationen gelöscht, einschließlich der folgenden: Informationen zum Hauptschlüssel, alle in „master“ geladenen Zertifikate, SA-Anmeldekennwörter, auftragsbezogene Informationen aus der MSDB-Datenbank, Datenbank-E-Mail-Informationen aus der MSDB-Datenbank und sp_configure-Optionen. Verwenden Sie diese Methode nur, wenn Sie sich über die Konsequenzen im Klaren sind!

1. Beenden Sie SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Führen Sie **sqlservr** mit dem Parameter **force-setup** aus. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > Siehe die vorherige Warnung! Außerdem muss die Ausführung als **MSSQL**-Benutzer erfolgen, wie hier gezeigt.

1. Wenn die Meldung „Recovery is complete“ (Die Wiederherstellung ist abgeschlossen) angezeigt wird, drücken Sie STRG+C. SQL Server wird daraufhin heruntergefahren.

1. Konfigurieren Sie das Systemadministratorkennwort neu.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Starten Sie SQL Server, und konfigurieren Sie den Server neu. Dies schließt die Wiederherstellung oder das erneute Anfügen von Benutzerdatenbanken ein.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>Verbessern der Leistung

Es gibt zahlreiche Faktoren, wie z. B. Datenbankentwurf, Hardware und Workloadanforderungen, die sich auf die Leistung auswirken. Wenn Sie die Leistung verbessern möchten, lesen Sie zunächst die bewährten Methoden im Artikel [Performance best practices and configuration guidelines for SQL Server on Linux (Bewährte Methoden für die Leistung und Konfigurationsrichtlinien für SQL Server für Linux)](sql-server-linux-performance-best-practices.md). Erkunden Sie dann einige der verfügbaren Tools zur Behandlung von Leistungsproblemen.

- [Abfragespeicher](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Dynamische Systemverwaltungssichten (DMVs)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Leistungsdashboard in SQL Server Management Studio](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)

## <a name="common-issues"></a>Häufige Probleme

1. Sie können keine Verbindung mit der SQL Server-Remoteinstanz herstellen.

   Weitere Informationen finden Sie im Abschnitt zur Fehlerbehebung im Artikel [Connect to SQL Server on Linux (Herstellen einer Verbindung mit SQL Server)](#connection).

2. FEHLER: Der Hostname darf höchstens 15 Zeichen enthalten.

   Dies ist ein bekanntes Problem, das auftritt, wenn der Name des Computers, auf dem das Debian-Paket von SQL Server installiert werden soll, länger als 15 Zeichen ist. Diese Problem kann zurzeit nur durch Ändern des Computernamens umgangen werden. Bearbeiten Sie hierzu die Hostnamendatei und starten Sie den Computer neu. Im folgenden [Onlineartikel](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/) wird die Vorgehensweise ausführlich erläutert.

3. Zurücksetzen des Systemadministratorkennworts

   Wenn Sie das Kennwort des Systemadministrators vergessen haben oder es aus einem anderen Grund zurücksetzen müssen, führen Sie die folgenden Schritte aus.

   > [!NOTE]
   > Während der Ausführung dieser Schritten wird der SQL Server-Dienst vorübergehend beendet.

   Melden Sie sich beim Hostterminal an, führen Sie die folgenden Befehle aus, und befolgen Sie die Anweisungen zum Zurücksetzen des Systemadministratorkennworts:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Verwenden von Sonderzeichen in Kennwörtern

   Bei Verwenden einiger Zeichen im Anmeldekennwort für SQL Server müssen Sie diese möglicherweise mit einem Escapezeichen bzw. umgekehrten Schrägstrich versehen, wenn Sie es in einem Linux-Befehl im Terminal verwenden. Beispielsweise müssen Sie das Dollarzeichen ($) jedes Mal mit Escapezeichen versehen, wenn Sie es in einem Terminalbefehl/Shellskript verwenden:

   Funktioniert nicht:

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Funktioniert:

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Ressourcen: [Sonderzeichen](https://tldp.org/LDP/abs/html/special-chars.html)
   [Escapezeichen](https://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]