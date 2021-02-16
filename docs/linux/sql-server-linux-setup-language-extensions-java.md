---
title: Installieren der Java-Spracherweiterung unter Linux
titleSuffix: SQL Server Language Extensions
description: Hier erfahren Sie, wie Sie SQL Server-Spracherweiterung für Java unter Red Hat, Ubuntu und SUSE Linux installieren.
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/12/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a98eed7109a45958864e85822bbb69de4767a85d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073929"
---
# <a name="install-sql-server-java-language-extension-on-linux"></a>Installieren der Java-Spracherweiterung für SQL Server unter Linux

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Erfahren Sie, wie Sie die Komponente [Java-Spracherweiterung](../language-extensions/java-overview.md) für SQL Server unter Linux installieren. Die Java-Spracherweiterung ist Teil der [SQL Server-Spracherweiterungen](../language-extensions/language-extensions-overview.md) und ein Add-On für die Datenbank-Engine. 

Obwohl Sie die [Datenbank-Engine und Spracherweiterungen gleichzeitig installieren](#install-all) können, empfiehlt es sich, zuerst die SQL Server-Datenbank-Engine zu installieren und zu konfigurieren, damit Sie eventuelle Probleme beheben können, bevor Sie weitere Komponenten hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

+ Die Linux-Version muss [von SQL Server unterstützt](sql-server-linux-release-notes-2019.md#supported-platforms) werden, muss aber nicht die Docker-Engine enthalten. Folgende Versionen werden unterstützt:

   + [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
   + [SUSE Enterprise Linux Server](quickstart-install-connect-suse.md)
   + [Ubuntu](quickstart-install-connect-ubuntu.md)

+ Sie sollten über ein Tool zum Ausführen von T-SQL-Befehlen verfügen. Sie benötigen einen Abfrage-Editor für die Konfiguration und Validierung nach der Installation. Die Verwendung von [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md?view=sql-server-2017&preserve-view=true#get-azure-data-studio-for-linux) wird empfohlen. Dabei handelt es sich um einen kostenlosen Download, der unter Linux ausgeführt wird.

+ Erweiterungspakete für Java werden unter Linux in den Quellrepositorys für SQL Server gespeichert. Wenn Sie für die Installation der Datenbank-Engine bereits Quellrepositorys konfiguriert haben, können Sie die **mssql-server-extensibility-java**-Befehle für die Paketinstallation unter Verwendung derselben Repositorregistrierung ausführen.

+ Spracherweiterungen werden auch in Linux-Containern unterstützt. Wir stellen keine vorgefertigten Container mit Spracherweiterungen bereit. Sie können jedoch [mithilfe einer in GitHub verfügbaren Vorlage](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices) einen Container aus den SQL Server-Containern erstellen.

+ Spracherweiterungen und [Machine Learning Services](../machine-learning/index.yml) werden standardmäßig auf Big Data-Clustern für SQL Server installiert. Wenn Sie Big Data-Cluster verwenden, müssen Sie die Schritte in diesem Artikel nicht ausführen. Weitere Informationen finden Sie unter [Verwenden von Machine Learning Services (Python und R) in Big Data-Clustern](../big-data-cluster/machine-learning-services.md).

## <a name="package-list"></a>Paketliste

Auf einem mit dem Internet verbundenen Gerät werden Pakete unabhängig von der Datenbank-Engine mithilfe des Paketinstallationsprogramms für das betreffende Betriebssystem heruntergeladen und installiert. Die folgende Tabelle enthält alle verfügbaren Pakete.

| Paketname | Gilt für | BESCHREIBUNG |
|--------------|----------|-------------|
|mssql-server-extensibility  | Alle Sprachen | Verwendetes Erweiterbarkeitsframework für die Java-Spracherweiterung |
|mssql-server-extensibility-java | Java | Verwendetes Erweiterbarkeitsframework für die Java-Spracherweiterung, enthält außerdem eine unterstützte Java-Runtime |

<a name="RHEL"></a>

## <a name="install-java-language-extension"></a>Installieren der Java-Spracherweiterung

Sie können Spracherweiterungen und Java unter Linux über das Paket **mssql-server-extensibility-java** installieren. Wenn Sie das Paket **mssql-server-extensibility-java** installieren, wird auch automatisch JRE 11 installiert, sofern nicht bereits geschehen. Außerdem wird der JVM-Pfad einer Umgebungsvariablen mit dem Namen „JRE_HOME“ hinzugefügt.

> [!Note]
> Auf einem mit dem Internet verbundenen Server werden Paketabhängigkeiten im Rahmen der Hauptpaketinstallation heruntergeladen und installiert. Wenn Ihr Server nicht mit dem Internet verbunden ist, finden Sie weitere Informationen unter [Offlineeinrichtung](#offline-install).

### <a name="redhat-install-command"></a>Red Hat-Installationsbefehl

Mithilfe des folgenden Befehls können Sie Spracherweiterungen für Java unter Red Hat installieren.

> [!Tip]
> Führen Sie nach Möglichkeit `yum clean all` aus, um vor der Installation Pakete auf dem System zu aktualisieren.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility-java
```

<a name="ubuntu"></a>

### <a name="ubuntu-install-command"></a>Ubuntu-Installationsbefehl

Mithilfe des folgenden Befehls können Sie Spracherweiterungen für Java unter Ubuntu installieren.

> [!Tip]
> Führen Sie nach Möglichkeit `apt-get update` aus, um vor der Installation Pakete auf dem System zu aktualisieren. Außerdem verfügen einige Docker-Images von Ubuntu möglicherweise nicht über die HTTPS-Transportoption „apt“. Verwenden Sie `apt-get install apt-transport-https`, um diese zu installieren.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility-java
```

<a name="suse"></a>

### <a name="suse-install-command"></a>SUSE-Installationsbefehl

Mithilfe des folgenden Befehls können Sie Spracherweiterungen für Java unter SUSE installieren.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility-java
```

## <a name="post-install-config-required"></a>Konfigurieren nach der Installation (erforderlich)

1. Erteilen Sie Berechtigungen unter Linux.

    Wenn Sie externe Bibliotheken verwenden, müssen Sie diesen Schritt nicht ausführen. Es wird empfohlen, externe Bibliotheken für die Arbeit zu verwenden. Informationen zum Erstellen einer externen Bibliothek aus der JAR-Datei finden Sie unter [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md).

    Wenn Sie keine externen Bibliotheken verwenden, müssen Sie SQL Server Berechtigungen zum Ausführen der Java-Klassen in Ihrer JAR-Datei erteilen.

    Sie können einer JAR-Datei Lese-und Ausführungszugriff gewähren, indem Sie den folgenden **chmod**-Befehl auf diese ausführen. Wenn Sie mit SQL Server arbeiten, empfiehlt es sich, die Klassendateien immer in eine JAR-Datei zu speichern. Informationen zum Erstellen einer JAR-Datei finden Sie unter [Erstellen einer JAR-Datei](../language-extensions/how-to/create-a-java-jar-file-from-class-files.md).

    ```cmd
    chmod ug+rx <MyJarFile.jar>
    ```

    Außerdem müssen Sie „mssql_satellite“ Berechtigungen erteilen, um die JAR-Datei zu lesen oder auszuführen.

    ```cmd
    chown mssql_satellite:mssql_satellite <MyJarFile.jar>
    ```

    Die zusätzliche Konfiguration erfolgt in erster Linie über das Tool [mssql-conf](sql-server-linux-configure-mssql-conf.md).

2. Fügen Sie das MSSQL-Benutzerkonto hinzu, um den SQL Server-Dienst auszuführen. Dies ist erforderlich, wenn Sie das Setup vorher noch nicht ausgeführt haben.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

3. Aktivieren Sie den Zugriff auf ausgehenden Netzwerkdatenverkehr. Der Zugriff auf ausgehenden Netzwerkdatenverkehr ist standardmäßig deaktiviert. Sie können mithilfe des Tools mssql-conf die boolesche Eigenschaft „outboundnetworkaccess“ festlegen, um ausgehende Anforderungen zu aktivieren. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server für Linux mithilfe von mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access).

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. Starten Sie anschließend das SQL Server-Launchpad und die Datenbank-Engine-Instanz neu, um die aktualisierten Werte aus der INI-Datei zu lesen. Wenn eine Erweiterbarkeitseinstellung geändert wird, werden Sie in einer Neustartmeldung darauf hingewiesen.  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

5. Aktivieren Sie die externe Skriptausführung mithilfe von Azure Data Studio oder einem anderen Tool wie SQL Server Management Studio (nur unter Windows), das Transact-SQL ausführt.

   ```bash
   EXEC sp_configure 'external scripts enabled', 1
   RECONFIGURE WITH OVERRIDE
   ```

6. Starten Sie den Dienst `mssql-launchpadd` neu.

7. Für jede Datenbank, in der Sie Spracherweiterungen verwenden möchten, müssen Sie die externe Sprache über den Befehl [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) registrieren.

## <a name="register-external-language"></a>Registrieren einer externen Sprache

Für jede Datenbank, in der Sie Spracherweiterungen verwenden möchten, müssen Sie die externe Sprache über den Befehl [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) registrieren.

Im folgenden Beispiel wird die externe Sprache mit dem Namen Java zu einer Datenbank unter SQL Server für Linux hinzugefügt.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', 
    FILE_NAME = 'javaextension.so', 
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"/opt/mssql/lib/zulu-jre-11"}')
```
Für die Java-Erweiterung wird die Umgebungsvariable „JRE_HOME“ verwendet, um den Pfad festzulegen, über den JVM gefunden und initialisiert werden kann.

CREATE EXTERNAL LANGUAGE ddl stellt einen Parameter bereit (ENVIRONMENT_VARIABLES), mit dem Umgebungsvariablen speziell für den Prozess festgelegt werden können, der die Erweiterung hostet. Dies ist die empfohlene und effektivste Methode für das Festlegen von Umgebungsvariablen, die für externe Spracherweiterungen erforderlich sind.

Weitere Informationen finden Sie unter [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Überprüfen der Installation

Die Integration von Java-Funktionen umfasst keine Bibliotheken, aber Sie können `grep -r JRE_HOME /etc` ausführen, um die Erstellung der Umgebungsvariablen JAVA_HOME zu bestätigen.

Sie können die Installation überprüfen, indem Sie ein T-SQL-Skript ausführen, das eine gespeicherte Systemprozedur ausführt, die Java aufruft. Für diese Aufgabe benötigen Sie ein Abfragetool. Azure Data Studio ist eine gute Wahl. Andere häufig verwendete Tools wie SQL Server Management Studio oder PowerShell können nur unter Windows verwendet werden. Wenn Sie über einen Windows-Computer mit diesen Tools verfügen, verwenden Sie diesen, um eine Verbindung mit Ihrer Linux-Installation der Datenbank-Engine herzustellen.

<a name="install-all"></a>

## <a name="full-install-of-sql-server-and-java-language-extension"></a>Vollständige Installation von SQL Server und der Java-Spracherweiterung

Sie können die Datenbank-Engine und die Java-Spracherweiterung zusammen installieren und konfigurieren, indem Sie Java-Pakete und -Parameter an einen Befehl anfügen, der wiederum die Datenbank-Engine installiert.

1. Geben Sie eine Befehlszeile an, die die Datenbank-Engine sowie Spracherweiterungsfeatures enthält.

    Sie können Java-Erweiterbarkeit zur Installation einer Datenbank-Engine hinzufügen.

    ```bash
    sudo yum install -y mssql-server mssql-server-extensibility-java 
    ```

1. Akzeptieren Sie die Lizenzbedingungen, und schließen Sie die Konfiguration im Anschluss an die Installation ab. Verwenden Sie hierfür das Tool **mssql-conf**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf setup
    ```

    Sie werden dann aufgefordert, die Lizenzbedingungen für die Datenbank-Engine zu akzeptieren, eine Edition auszuwählen und das Administratorkennwort festzulegen. 

1. Starten Sie den Dienst neu, wenn Sie dazu aufgefordert werden.

    ```bash
    sudo systemctl restart mssql-server.service
    ```

## <a name="unattended-installation"></a>Unbeaufsichtigte Installation

Fügen Sie die Pakete für **mssql-server-extensibility-java** der Datenbank-Engine über eine [unbeaufsichtigte Installation](./sql-server-linux-setup.md#unattended) hinzu.

<a name="offline-install"></a>

## <a name="offline-installation"></a>Offlineinstallation

Befolgen Sie die Anweisungen zur [Offlineinstallation](sql-server-linux-setup.md#offline), um die Pakete zu installieren. Der nachfolgenden Liste können Sie entnehmen, auf welchen Websites Sie jeweils entsprechende Downloads finden. Laden Sie dann alle nötigen Pakete herunter.

> [!Tip]
> Einige der Paketverwaltungstools umfassen Befehle, mit denen Sie Paketabhängigkeiten ermitteln können. Verwenden Sie für yum `sudo yum deplist [package]`. Verwenden Sie für Ubuntu erst `sudo apt-get install --reinstall --download-only [package name]` und dann `dpkg -I [package name].deb`.

#### <a name="download-site"></a>Downloadsite

Sie können die Pakete von [https://packages.microsoft.com/](https://packages.microsoft.com/) herunterladen. Alle Pakete für Java werden zusammen mit dem Datenbank-Engine-Paket bereitgestellt.

#### <a name="redhat7-paths"></a>Red Hat/7-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/extensibility-java-Pakete | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |

#### <a name="ubuntu1604-paths"></a>Ubuntu/16.04-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/extensibility-java-Pakete | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |

#### <a name="suse12-paths"></a>SUSE/12-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/extensibility-java-Pakete | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |

#### <a name="package-list"></a>Paketliste
Je nachdem, welche Erweiterungen Sie verwenden möchten, müssen Sie die für die jeweilige Sprache erforderlichen Pakete herunterladen. Genaue Dateinamen enthalten Plattforminformationen im Suffix, aber die unten aufgeführten Dateinamen sollten genau genug sein, damit Sie herausfinden können, welche Dateien Sie benötigen.

```
# Core packages
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# Java
mssql-server-extensibility-java-15.0.1000
```

## <a name="limitations"></a>Einschränkungen

Die implizite Authentifizierung ist zurzeit nicht unter Linux verfügbar. Das bedeutet, dass Sie über Java-Code, der zu diesem Zeitpunkt ausgeführt wird, keine erneute Verbindung mit dem Server herstellen können, um auf Daten oder andere Ressourcen zuzugreifen.

### <a name="resource-governance"></a>Ressourcengovernance

In Bezug auf die [Ressourcengovernance](../t-sql/statements/create-external-resource-pool-transact-sql.md) besteht zwar zwischen Linux und Windows Parität für externe Ressourcenpools, aber die Statistiken für [sys.dm_resource_governor_external_resource_pools](../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) haben derzeit unterschiedliche Einheiten unter Linux.

| Spaltenname   | BESCHREIBUNG | Wert unter Linux |
|---------------|--------------|---------------|
|peak_memory_kb | Der maximale Speicher, der für den Ressourcenpool verwendet wird. | Unter Linux wird diese Statistik dem CGroups-Speichersubsystem entnommen. Dabei lautet der Wert „memory.max_usage_in_bytes“. |
|write_io_count | Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Resource Governor-Statistiken ausgegeben wurden. | Unter Linux wird diese Statistik dem CGroups-Speichersubsystem „blkio“ entnommen. Dabei lautet der Wert der Zeile für die Schreibvorgänge „blkio.throttle.io_serviced“. |
|read_io_count | Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Resource Governor-Statistiken ausgegeben wurden. | Unter Linux wird diese Statistik dem CGroups-Speichersubsystem „blkio“ entnommen. Dabei lautet der Wert der Zeile für die Lesevorgänge „blkio.throttle.io_serviced“. |
|total_cpu_kernel_ms | Die kumulierte CPU-Benutzerkernelzeit in Millisekunden, seitdem die Resource Governor-Statistiken zurückgesetzt wurden. | Unter Linux wird diese Statistik dem CGroups-Speichersubsystem „cpuacct“ entnommen. Dabei lautet der Wert der Zeile für den Benutzer „cpuacct.stat“. |  
|total_cpu_user_ms | Die kumulierte CPU-Benutzerzeit in Millisekunden, seitdem die Resource Governor-Statistiken zurückgesetzt wurden.| Unter Linux wird diese Statistik dem CGroups-Speichersubsystem „cpuacct“ entnommen. Dabei lautet der Systemzeilenwert „cpuacct.stat“. |
|active_processes_count | Die Anzahl externer Prozesse, die zum Zeitpunkt der Anforderung ausgeführt werden.| Unter Linux wird diese Statistik dem CGroups-Speichersubsystem „pids“ entnommen. Dabei lautet der Wert „pids.current“. |

## <a name="next-steps"></a>Nächste Schritte

Java-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von Java unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Tutorial: Reguläre Ausdrücke in Java](../language-extensions/tutorials/search-for-string-using-regular-expressions-in-java.md)