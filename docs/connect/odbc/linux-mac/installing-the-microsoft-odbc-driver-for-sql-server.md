---
title: Installation von Microsoft ODBC Driver for SQL Server (Linux)
description: Erfahren Sie, wie Microsoft ODBC Driver for SQL Server auf Linux-Clients installieren, um Datenbankkonnektivität zu ermöglichen.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4e2ee02544955cab007d542bca75b8f14cfeeee
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770458"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>Installation von Microsoft ODBC Driver for SQL Server (Linux)

In diesem Artikel wird die Installation von Microsoft ODBC Driver for SQL Server unter Linux erläutert. Er enthält außerdem Anweisungen für die optionalen Befehlszeilentools für SQL Server (`bcp` und `sqlcmd`) und die unixODBC-Entwicklungsheader.

In diesem Artikel finden Sie Befehle zum Installieren des ODBC-Treibers über die Bash-Shell. Informationen zum direkten Herunterladen der Pakete finden Sie unter [Herunterladen von ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

In den folgenden Abschnitten erfahren Sie, wie Sie Microsoft ODBC Driver 17 für verschiedene Linux-Distributionen über die Bash-Shell herunterladen.

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux und Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> Wenn Sie das `msodbcsql`-Paket der Version 17 installiert haben, das kurz verfügbar war, sollten Sie es entfernen, bevor Sie das `msodbcsql17`-Paket installieren. Dadurch werden Konflikte vermieden. Das `msodbcsql17`-Paket und das `msodbcsql`-Paket der Version 13 können nebeneinander installiert werden.

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.2.1-1_amd64.sig msodbcsql17_17.7.2.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.2.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> Die Treiberversion 17.5 oder höher ist für die Unterstützung von Alpine erforderlich.

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> Anstatt die Umgebungsvariable ACCEPT_EULA festzulegen, können Sie auch die debconf-Variable msodbcsql/ACCEPT_EULA festlegen: `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server und Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - Die Treiberversion 17.2 oder höher ist für die Unterstützung von Ubuntu 18.04 erforderlich.
> - Die Treiberversion 17.3 oder höher ist für die Unterstützung von Ubuntu 18.10 erforderlich.

> [!NOTE]
> Anstatt die Umgebungsvariable ACCEPT_EULA festzulegen, können Sie auch die debconf-Variable msodbcsql/ACCEPT_EULA festlegen: `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>Vorgängerversionen

In den folgenden Abschnitten finden Sie Anweisungen zum Installieren vorheriger Versionen von Microsoft ODBC Driver unter Linux. Die folgenden Treiberversionen werden abgedeckt:

- [Microsoft ODBC Driver 13.1 for SQL Server](#13.1)
- [Microsoft ODBC Driver 13 for SQL Server](#13)
- [Microsoft ODBC Driver 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

In den folgenden Abschnitten erfahren Sie, wie Sie Microsoft ODBC Driver 13.1 für verschiedene Linux-Distributionen über die Bash-Shell herunterladen.

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

In den folgenden Abschnitten erfahren Sie, wie Sie Microsoft ODBC Driver 13 für verschiedene Linux-Distributionen über die Bash-Shell herunterladen.

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Offlineinstallation

Wenn Sie es vorziehen/es erforderlich ist, dass der [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC-Treiber 13 auf einem Computer ohne Internetverbindung installiert wird, müssen Paketabhängigkeiten manuell aufgelöst werden. Der [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC-Treiber 13 hat die folgenden direkten Abhängigkeiten:
- Ubuntu: libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat: ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE: ```glibc, libuuid1, krb5, openssl, unixODBC```

Jedes dieser Pakete hat wiederum seine eigenen Abhängigkeiten, die auf dem System entweder vorhanden oder nicht vorhanden sind. Eine allgemeine Lösung für das Problem finden Sie in der Paket-Manager-Dokumentation Ihrer Distribution: [Red Hat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) und [SUSE](https://en.opensuse.org/Portal:Zypper).

Es ist außerdem gängig, alle abhängigen Pakete manuell herunterzuladen und sie zusammen auf dem Installationscomputer zu platzieren, dann wiederum jedes Paket manuell zu installieren und dabei mit dem [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC-Treiberpaket 13 zu enden.

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- Laden Sie die aktuellste `msodbcsql``.rpm`-Datei hier herunter: [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/).
- Installieren Sie die Abhängigkeiten und den Treiber.
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- Laden Sie die aktuellste `msodbcsql``.deb`-Datei hier herunter: [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/).
- Installieren Sie die Abhängigkeiten und den Treiber.

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- Laden Sie die aktuellste `msodbcsql``.rpm`-Datei hier herunter: [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/).
- Installieren Sie die Abhängigkeiten und den Treiber.

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

Nachdem die Paketinstallation abgeschlossen wurde, können Sie überprüfen, ob der [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 alle seine Abhängigkeiten finden kann, indem Sie „Idd“ ausführen und die entsprechende Ausgabe nach fehlenden Bibliotheken untersuchen:

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

In den folgenden Abschnitten wird die Installation von Microsoft ODBC Driver 11 unter Linux erläutert. Sie müssen den unixODBC-Treiber-Manager installieren, bevor Sie den Treiber verwenden können. Weitere Informationen finden Sie unter [Installieren des Treiber-Managers](../../../connect/odbc/linux-mac/installing-the-driver-manager.md).

### <a name="installation-steps"></a>Installationsschritte  

> [!IMPORTANT]  
> Diese Anleitung bezieht sich auf `msodbcsql-11.0.2270.0.tar.gz`, die Installationsdatei für Red Hat Linux. Wenn Sie die Vorschauversion von SUSE Linux installieren, ist der Dateiname `msodbcsql-11.0.2260.0.tar.gz`.  
  
Den Treiber installieren:

1. Stellen Sie sicher, dass Sie die Root-Berechtigung besitzen.  

2. Wechseln Sie zu dem Verzeichnis, in dem der Download die Datei `msodbcsql-11.0.2270.0.tar.gz` platziert hat. Stellen Sie sicher, dass Sie die zu Ihrer Linux-Version passende Datei „ \*.tar.g“ besitzen. Führen Sie den folgenden Befehl aus, um die Dateien zu extrahieren: `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3. Wechseln Sie zum Verzeichnis `msodbcsql-11.0.2270.0`, das eine Datei namens **install.sh** enthalten sollte.  
  
4. Um eine Liste aller verfügbaren Installationsoptionen zu erhalten, führen Sie den folgenden Befehl aus: **./install.sh**.  
  
5. Führen Sie eine Sicherung von **odbcinst.ini** mittels Backup durch. Die Treiberinstallation aktualisiert **odbcinst.ini**. „odbcinst.ini“ beinhaltet die Liste der Treiber, die beim unixODBC-Treiber-Manager registriert sind. Um den Speicherort von „odbcinst.ini“ auf Ihrem Computer zu finden, führen Sie den folgenden Befehl aus: ```odbc_config --odbcinstini```.  
  
6. Führen Sie den folgenden Befehl aus, bevor Sie den Treiber installieren: `./install.sh verify`. Die Ausgabe von `./install.sh verify` gibt an, ob Ihr Computer über die erforderliche Software verfügt, um den ODBC-Treiber unter Linux zu unterstützen.  
  
7. Wenn Sie bereit sind, den ODBC-Treiber unter Linux zu installieren, führen Sie diesen Befehl aus: `./install.sh install`. Falls Sie einen Installationsbefehl angeben müssen (`bin-dir` oder `lib-dir`), geben Sie den Befehl nach der Option **install** an.  
  
8. Lesen Sie die Lizenzvereinbarung und geben Sie **YES** ein, um mit der Installation fortzufahren.  
  
Die Installation platziert den Treiber in `/opt/microsoft/msodbcsql/11.0.2270.0`. Der Treiber und seine Unterstützungsdateien müssen sich in `/opt/microsoft/msodbcsql/11.0.2270.0` befinden.  
  
Um zu überprüfen, ob der ODBC-Treiber unter Linux erfolgreich registriert wurde, führen Sie den folgenden Befehl aus: ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
### <a name="uninstall"></a>Deinstallieren  
  
Sie können den ODBC-Treiber 11 unter Linux deinstallieren, indem Sie die folgenden Befehle ausführen:  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>Treiberdateien

Der ODBC-Treiber unter Linux besteht aus den folgenden Komponenten:

|Komponente|BESCHREIBUNG|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X oder libmsodbcsql-13.X.so.X.X|Das freigegebene Objekt (`so`) der dynamischen Bibliotheksdatei, das die gesamte Funktionalität des Treibers enthält. Diese Datei wird in `/opt/microsoft/msodbcsql17/lib64/` für den Treiber 17 und in `/opt/microsoft/msodbcsql/lib64/` für den Treiber 13 installiert.|  
|`msodbcsqlr17.rll` oder `msodbcsqlr13.rll`|Die begleitende Ressourcendatei für die Treiberbibliothek. Diese Datei wird in `[driver .so directory]../share/resources/en_US/` installiert.| 
|msodbcsql.h|Die Headerdatei, die alle erforderlichen neuen Definitionen für die Verwendung des Treibers enthält.<br /><br /> **Hinweis:**  Sie können im selben Programm nicht auf „msodbcsql.h“ und auf „odbcss.h“ verweisen.<br /><br /> „msodbcsql“ wird in `/opt/microsoft/msodbcsql17/include/` für den Treiber 17 und in `/opt/microsoft/msodbcsql/include/` für den Treiber 13 installiert. |
|LICENSE.txt|Die Textdatei, die die Bestimmungen des Endbenutzer-Lizenzvertrags enthält. Diese Datei wird in `/usr/share/doc/msodbcsql17/` für den Treiber 17 und in `/usr/share/doc/msodbcsql/` für den Treiber 13 platziert.|
|RELEASE_NOTES|Die Textdatei, die die Versionshinweise enthält. Diese Datei wird in `/usr/share/doc/msodbcsql17/` für den Treiber 17 und in `/usr/share/doc/msodbcsql/` für den Treiber 13 platziert.|

## <a name="resource-file-loading"></a>Laden der Ressourcendatei

Der Treiber muss die Ressourcendatei laden, um zu funktionieren. Diese Datei heißt `msodbcsqlr17.rll` oder `msodbcsqlr13.rll`, je nach Treiberversion. Wie oben in der Tabelle aufgeführt, ist der Speicherort der `.rll`-Datei relativ zum Speicherort des Treibers selbst (`so` oder `dylib`). Ab Version 17.1 versucht der Treiber auch, die `.rll`-Datei aus dem Standardverzeichnis zu laden, wenn das Laden aus dem relativen Pfad fehlschlägt. Der Standardressourcendatei-Pfad unter Linux lautet `/opt/microsoft/msodbcsql17/share/resources/en_US/`.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie mit dem ODBC-Treiber keine Verbindung mit SQL Server herstellen können, finden Sie im Artikel zu bekannten Problemen weitere Informationen zur [Problembehandlung bei Verbindungsproblemen](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Treiber installiert haben, können Sie die [C++-ODBC-Beispielanwendung](../../odbc/cpp-code-example-app-connect-access-sql-db.md) testen. Weitere Informationen zum Entwickeln von ODBC-Anwendungen finden Sie unter [Entwickeln von Anwendungen](../../../odbc/reference/develop-app/developing-applications.md).

Weitere Informationen zum ODBC-Treiber finden Sie in den [Versionshinweisen](release-notes-odbc-sql-server-linux-mac.md) und den [Systemanforderungen](system-requirements.md).
