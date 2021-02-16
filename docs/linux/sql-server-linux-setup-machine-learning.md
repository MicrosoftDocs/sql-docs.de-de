---
title: Installation unter Linux
titleSuffix: SQL Server Machine Learning Services
description: 'Erfahren Sie, wie SQL Server Machine Learning Services (Python und R) unter Linux installiert wird: Red Hat, Ubuntu und SUSE.'
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/24/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 462857bd0a9aac7b63659bad809dcdc9a577f3b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031492"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-linux"></a>Installieren von SQL Server Machine Learning Services (Python und R) unter Linux

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

In diesem Artikel werden Sie durch die Installation von [SQL Server Machine Learning Services](../machine-learning//sql-server-machine-learning-services.md) unter Linux geführt. Python- und R-Skripts können mit Machine Learning Services in einer Datenbank ausgeführt werden.

Sie können Machine Learning Services unter Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) und Ubuntu installieren. Weitere Informationen finden Sie [im Abschnitt „Unterstützte Plattformen“ im Leitfaden für die Installation von SQL Server unter Linux](sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> Machine Learning Services wird standardmäßig auf SQL Server-Big Data-Clustern installiert. Weitere Informationen finden Sie unter [Verwenden von Machine Learning Services (Python und R) in Big Data-Clustern](../big-data-cluster/machine-learning-services.md).

<a name="mro"></a>

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

* [Installieren Sie SQL Server für Linux](sql-server-linux-setup.md), und überprüfen Sie die Installation.

* Suchen Sie in den SQL Server für Linux-Repositorys nach den Python- und R-Erweiterungen. 
  Wenn Sie für die Datenbank-Engine-Installation bereits Quellrepositorys konfiguriert haben, können Sie die **mssql-mlservices**-Befehle für die Paketinstallation mithilfe derselben Repositoryregistrierung ausführen.

* (Nur R) Microsoft R Open (MRO) enthält die grundlegende R-Verteilung für das R-Feature in SQL Server und ist eine Voraussetzung für RevoScaleR, MicrosoftML und andere R-Pakete, die mit Machine Learning Services installiert werden.
    * Die erforderliche Version ist MRO 3.5.2.
    * Wählen Sie für die Installation von MRO einen der beiden folgenden Ansätze aus:
        * Laden Sie den MRO-Tarball von MRAN herunter, entpacken Sie diesen, und führen Sie das Skript „install.sh“ aus. Wenn Sie diesen Ansatz wählen, finden Sie auf [MRAN](https://mran.microsoft.com/releases/3.5.2) Anweisungen zur Installation.
        * Registrieren Sie das Repository **packages.microsoft.com** wie unten beschrieben, um die folgenden zwei MRO-Verteilungen zu installieren: microsoft-r-open-mro und microsoft-r-open-mkl. 
    * Weitere Informationen zur Installation von MRO finden Sie in den Abschnitten weiter unten.

* Sie sollten über ein Tool zum Ausführen von T-SQL-Befehlen verfügen. 

  * Sie können z. B. [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) verwenden, ein kostenloses Datenbanktool, das unter Linux, Windows und macOS ausgeführt werden kann.

## <a name="package-list"></a>Paketliste

Auf einem mit dem Internet verbundenen Gerät werden Pakete unabhängig von der Datenbank-Engine mithilfe des Paketinstallationsprogramms für das betreffende Betriebssystem heruntergeladen und installiert. In der folgenden Tabelle werden alle verfügbaren Pakete beschrieben. Für R und Python geben Sie jedoch Pakete an, die entweder die vollständigen Installation oder die minimale Installation durchführen.

Verfügbare Installationspakete:

| Paketname | Gilt für | BESCHREIBUNG |
|--------------|----------|-------------|
|mssql-server-extensibility  | All | Erweiterbarkeitsframework zum Ausführen von Python und R |
| microsoft-openmpi  | Python, R | Die Schnittstelle zur Nachrichtenübergabe, die von den Revo*-Bibliotheken zur Parallelisierung unter Linux verwendet wird |
| mssql-mlservices-python | Python | Open-Source-Verteilung von Anaconda und Python. |
|mssql-mlservices-mlm-py  | Python | *Vollständige Installation:* Stellt vortrainierte revoscalepy- bzw. microsoftml-Modelle für Imagefeatures und Stimmungsanalyse für Texte bereit.| 
|mssql-mlservices-packages-py  | Python | *Minimale Installation:* Stellt revoscalepy und microsoftml bereit. <br/>Schließt vortrainierte Modelle aus. | 
| [microsoft-r-open*](#mro) | R | Open-Source-Verteilung von R, die aus drei Paketen besteht. |
|mssql-mlservices-mlm-r  | R | *Vollständige Installation:* Umfasst: RevoScaleR, MicrosoftML, sqlRUtils, olapR, vorab trainierte Modelle für Bildfeatures und Stimmungsanalyse.| 
|mssql-mlservices-packages-r  | R | *Minimale Installation:* Stellt RevoScaleR, sqlRUtils, MicrosoftML und olapR bereit. <br/>Schließt vortrainierte Modelle aus. |

<a name="RHEL"></a>

## <a name="install-on-rhel"></a>Installieren unter RHEL

Die folgenden Schritte beschreiben, wie Sie SQL Server Machine Learning Services unter Red Hat Enterprise Linux (RHEL) installieren.

### <a name="install-mro-on-rhel"></a>Installieren von MRO unter RHEL

Mit den folgenden Befehlen wird das Repository registriert, das MRO bereitstellt. Nach der Registrierung enthalten die Befehle zum Installieren anderer R-Pakete (z. B. mssql-mlservices-mml-r) automatisch MRO als Paketabhängigkeit.
```bash
# Import the Microsoft repository key

sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Set the location of the package repo at the "prod" directory
# The following command is for version 7.x
# For 6.x, replace 7 with 6 to get that version
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Update packages on your system (optional)
yum update
```

Die folgenden Installationsoptionen stehen für Python und R zur Verfügung:

*  Installieren Sie die Sprachunterstützung basierend auf Ihren Anforderungen (einzelne oder mehrere Sprachen).
*  Die *vollständige Installation* stellt alle verfügbaren Features sowie vorab trainierte Modelle bereit.
*  Die *minimale Installation* enthält die Modelle zwar nicht, jedoch sind alle Funktionen enthalten.

> [!Tip]
> Führen Sie nach Möglichkeit `yum clean all` aus, um vor der Installation Pakete auf dem System zu aktualisieren.

### <a name="full-installation"></a>Die vollständige Installation

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Erweiterungen (Python, R)
*  Machine Learning-Bibliotheken
*  Vorab trainierte Modelle für Python und R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# Be sure to include -9.4.7* in mlsservices package names
sudo yum install mssql-mlservices-mlm-py-9.4.7*
sudo yum install mssql-mlservices-mlm-r-9.4.7*
```

### <a name="minimum-installation"></a>Die minimale Installation

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Revo*-Kernbibliotheken
*  Machine Learning-Bibliotheken

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-packages-py-9.4.7*
sudo yum install mssql-mlservices-packages-r-9.4.7*
```
<a name="ubuntu"></a>

## <a name="install-on-ubuntu"></a>Installation unter Ubuntu

Die folgenden Schritte beschreiben, wie Sie SQL Server Machine Learning Services unter Ubuntu installieren.

### <a name="install-mro-on-ubuntu"></a>Installieren von MRO unter Ubuntu

Mit den folgenden Befehlen wird das Repository registriert, das MRO bereitstellt. Nach der Registrierung enthalten die Befehle zum Installieren anderer R-Pakete (z. B. mssql-mlservices-mml-r) automatisch MRO als Paketabhängigkeit.

```bash
# Install as root
sudo su

# Optionally, if your system does not have the https apt transport option
apt-get install apt-transport-https

# Set the location of the package repo the "prod" directory containing the distribution.
# This example specifies 16.04. Replace with 14.04 if you want that version
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

# Register the repo
dpkg -i packages-microsoft-prod.deb

# Update packages on your system (required), including MRO installation
sudo apt-get update
```

Die folgenden Installationsoptionen stehen für Python und R zur Verfügung:

*  Installieren Sie die Sprachunterstützung basierend auf Ihren Anforderungen (einzelne oder mehrere Sprachen).
*  Die *vollständige Installation* stellt alle verfügbaren Features sowie vorab trainierte Modelle bereit.
*  Die *minimale Installation* enthält die Modelle zwar nicht, jedoch sind alle Funktionen enthalten.

> [!Tip]
> Führen Sie nach Möglichkeit `apt-get update` aus, um vor der Installation Pakete auf dem System zu aktualisieren. 

### <a name="full-installation"></a>Die vollständige Installation 

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Python-Erweiterungen
*  R-Erweiterungen
*  Machine Learning-Bibliotheken
*  Vorab trainierte Modelle für Python und R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# There is no asterisk in this full install
sudo apt-get install mssql-mlservices-mlm-py 
sudo apt-get install mssql-mlservices-mlm-r 
```

### <a name="minimum-installation"></a>Die minimale Installation 

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Revo*-Kernbibliotheken
*  Machine Learning-Bibliotheken

```bash
# Install as root or sudo
# Minimum install of R, Python
# No asterisk
sudo apt-get install mssql-mlservices-packages-py
sudo apt-get install mssql-mlservices-packages-r
```

<a name="SLES"></a>

## <a name="install-on-sles"></a>Installieren unter SLES

Die folgenden Schritte beschreiben, wie Sie SQL Server Machine Learning Services unter SUSE Linux Enterprise Server (SLES) installieren.

### <a name="install-mro-on-sles"></a>Installieren von MRO unter SLES

Mit den folgenden Befehlen wird das Repository registriert, das MRO bereitstellt. Nach der Registrierung enthalten die Befehle zum Installieren anderer R-Pakete (z. B. mssql-mlservices-mml-r) automatisch MRO als Paketabhängigkeit.

```bash
# Install as root
sudo su

# Set the location of the package repo at the "prod" directory containing the distribution
# This example is for SLES12, the only supported version of SLES in Machine Learning Server
zypper ar -f https://packages.microsoft.com/sles/12/prod packages-microsoft-com

# Update packages on your system (optional)
zypper update
```

Die folgenden Installationsoptionen stehen für Python und R zur Verfügung:

*  Installieren Sie die Sprachunterstützung basierend auf Ihren Anforderungen (einzelne oder mehrere Sprachen).
*  Die *vollständige Installation* stellt alle verfügbaren Features sowie vorab trainierte Modelle bereit.
*  Die *minimale Installation* enthält die Modelle zwar nicht, jedoch sind alle Funktionen enthalten.

### <a name="full-installation"></a>Die vollständige Installation 

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Erweiterungen für Python und R
*  Machine Learning-Bibliotheken
*  Vorab trainierte Modelle für Python und R

```bash
# Install as root or sudo
# Add everything (all R, Python)
sudo zypper install mssql-mlservices-mlm-py
sudo zypper install mssql-mlservices-mlm-r
```

### <a name="minimum-installation"></a>Die minimale Installation 

Enthält:
*  Open-Source-Python
*  Open-Source-R
*  Erweiterbarkeitsframework
*  Microsoft-openmpi
*  Revo*-Kernbibliotheken
*  Machine Learning-Bibliotheken 

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
sudo zypper install mssql-mlservices-packages-py
sudo zypper install mssql-mlservices-packages-r
```

## <a name="post-install-config-required"></a>Konfigurieren nach der Installation (erforderlich)

Die zusätzliche Konfiguration erfolgt in erster Linie über das Tool [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. Nachdem die Paketinstallation abgeschlossen ist, führen Sie „mssql-conf setup“ aus, und befolgen Sie die Anweisungen, um das Systemadministratorkennwort festzulegen und Ihre Edition auszuwählen. Führen Sie diesen Schritt nur aus, wenn Sie SQL Server für Linux noch nicht konfiguriert haben. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

2. Akzeptieren Sie die Lizenzvereinbarungen für die Erweiterungen für Open-Source-R und -Python. Verwenden Sie den folgenden Befehl:

   ```bash
   # Run as SUDO or root
   # Use set + EULA 
   sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y
   ```
   Wenn `mssql-conf setup` ausgeführt wird, erkennt das Setup die mssql-mlservices-Pakete und fordert Sie zum Akzeptieren der EULA auf (sofern diese noch nicht akzeptiert wurde). Weitere Informationen zu den EULA-Parametern finden Sie unter [Konfigurieren von SQL Server mit dem Tool mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

3. Aktivieren Sie den Zugriff auf ausgehenden Netzwerkdatenverkehr. Der Zugriff auf ausgehenden Netzwerkdatenverkehr ist standardmäßig deaktiviert. Sie können mithilfe des Tools mssql-conf die boolesche Eigenschaft „outboundnetworkaccess“ festlegen, um ausgehende Anforderungen zu aktivieren. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server für Linux mithilfe von mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access).

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. Wenn Sie nur das R-Feature integrieren möchten, legen Sie die Umgebungsvariable **MKL_CBWR** über die Intel Math Kernel Library-Berechnungen auf [ensure consistent output](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) (Konsistente Ausgabe sicherstellen) fest.

   + Bearbeiten oder erstellen Sie eine Datei namens `.bash_profile` in im Benutzerstammverzeichnis, und fügen Sie die Zeile `export MKL_CBWR="AUTO"` zur Datei hinzu.

   + Führen Sie diese Datei aus, indem Sie `source .bash_profile` in der Bash-Eingabeaufforderung eingeben.

5. Starten Sie anschließend das SQL Server-Launchpad und die Datenbank-Engine-Instanz neu, um die aktualisierten Werte aus der INI-Datei zu lesen. Eine Benachrichtigungsmeldung wird angezeigt, wenn eine Einstellung bezüglich der Erweiterbarkeit geändert wird.  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

6. Aktivieren Sie die externe Skriptausführung mithilfe von Azure Data Studio oder einem anderen Tool wie SQL Server Management Studio (nur unter Windows), das Transact-SQL ausführt.

   ```sql
   EXEC sp_configure 'external scripts enabled', 1 
   RECONFIGURE WITH OVERRIDE 
   ```

7. Starten Sie den Launchpad-Dienst neu.

## <a name="verify-installation"></a>Überprüfen der Installation

R-Bibliotheken (z. B. MicrosoftML, RevoScaleR) finden Sie unter `/opt/mssql/mlservices/libraries/RServer`.

Python-Bibliotheken (microsoftml und revoscalepy) finden Sie unter `/opt/mssql/mlservices/libraries/PythonServer`.

So überprüfen Sie die Installation:

* Führen Sie ein T-SQL-Skript aus, das eine gespeicherte Systemprozedur ausführt, die Python oder R mit einem Abfragetool aufruft. 

* Führen Sie den folgenden SQL-Befehl aus, um die Ausführung von R in SQL Server testen. Treten Fehler auf? Versuchen Sie einen Dienstneustart (`sudo systemctl restart mssql-server.service`).
  ```sql
  EXEC sp_execute_external_script   
  @language =N'R', 
  @script=N' 
  OutputDataSet <- InputDataSet', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```
 
* Führen Sie den folgenden SQL-Befehl aus, um die Ausführung von Python in SQL Server testen. 
 
  ```sql
  EXEC sp_execute_external_script  
  @language =N'Python', 
  @script=N' 
  OutputDataSet = InputDataSet; 
  ', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```

<a name="install-all"></a>

## <a name="unattended-installation"></a>Unbeaufsichtigte Installation

Fügen Sie die Pakete für „mssql-mlservices“ und die Lizenzbedingungen über eine [unbeaufsichtigte Installation](sql-server-linux-setup.md#unattended) der Datenbank-Engine hinzu.

 Verwenden Sie einen der mlservices-spezifischen EULA-Parameter für die Open-Source-Verteilungen von R und Python:

```bash
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml
```

Die vollständige EULA finden Sie unter [Konfigurieren von SQL Server für Linux mithilfe des mssql-conf-Tools](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

## <a name="offline-installation"></a>Offlineinstallation

Befolgen Sie die Anweisungen zur [Offlineinstallation](sql-server-linux-setup.md#offline), um die Pakete zu installieren. Der nachfolgenden Liste können Sie entnehmen, auf welchen Websites Sie jeweils entsprechende Downloads finden. Laden Sie dann alle nötigen Pakete herunter.

> [!Tip]
> Einige der Paketverwaltungstools umfassen Befehle, mit denen Sie Paketabhängigkeiten ermitteln können. Verwenden Sie für yum `sudo yum deplist [package]`. Verwenden Sie für Ubuntu erst `sudo apt-get install --reinstall --download-only [package name]` und dann `dpkg -I [package name].deb`.

 
### <a name="download-site"></a>Downloadsite

Laden Sie Pakete unter [https://packages.microsoft.com/](https://packages.microsoft.com/) herunter. Alle mlservices-Pakete für Python und R werden zusammen mit dem Datenbank-Engine-Paket bereitgestellt. Die Basisversion für die mlservices-Pakete ist 9.4.6. Beachten Sie, dass die microsoft-r-open-Pakete sich in einem [anderen Repository](#mro) befinden.

### <a name="rhel7-paths"></a>RHEL/7-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/mlservices-Pakete | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |
| microsoft-r-open-Pakete | [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) | 

### <a name="ubuntu1604-paths"></a>Ubuntu/16.04-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/mlservices-Pakete | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |
| microsoft-r-open-Pakete | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/) | 

### <a name="sles12-paths"></a>SLES/12-Pfade

|Paket|Downloadspeicherort|
|--|----|
| mssql/mlservices-Pakete | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |
| microsoft-r-open-Pakete | [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) | 

Wählen Sie die Erweiterungen aus, die Sie verwenden möchten, und laden Sie die erforderlichen Pakete für eine spezifische Sprache herunter. Die Dateinamen sollten Plattforminformationen im Suffix enthalten.

### <a name="package-list"></a>Paketliste

Je nachdem, welche Erweiterungen Sie verwenden möchten, müssen Sie die für die jeweilige Sprache erforderlichen Pakete herunterladen. Genaue Dateinamen enthalten Plattforminformationen im Suffix, aber die unten aufgeführten Dateinamen sollten genau genug sein, damit Sie herausfinden können, welche Dateien Sie benötigen.

```
# Core packages 
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# R
microsoft-openmpi-3.0.0
microsoft-r-open-mkl-3.5.2
microsoft-r-open-mro-3.5.2
mssql-mlservices-packages-r-9.4.7.64
mssql-mlservices-mlm-r-9.4.7.64


# Python
microsoft-openmpi-3.0.0
mssql-mlservices-python-9.4.7.64
mssql-mlservices-packages-py-9.4.7.64
mssql-mlservices-mlm-py-9.4.7.64
```

## <a name="next-steps"></a>Nächste Schritte

Python-Entwickler können in den folgenden Tutorials erfahren, wie Python mit SQL Server verwendet werden kann:

+ [Python-Tutorial: Bereitstellen eines linearen Regressionsmodells in SQL Server Machine Learning Services](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python-Tutorial: Kategorisieren von Kunden mithilfe von K-Means-Clustering mit SQL Server Machine Learning Services](../machine-learning/tutorials/python-clustering-model.md)

R-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von R unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Schnellstart: Ausführen von R in T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Tutorial: Datenbankinterne Analysen für R-Entwickler](../machine-learning/tutorials/r-taxi-classification-introduction.md)
