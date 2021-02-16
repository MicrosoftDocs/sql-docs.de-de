---
title: Installation in Docker
titleSuffix: SQL Server Machine Learning Services
description: Hier erfahren Sie, wie Sie SQL Server Machine Learning Services (Python und R) in Docker installieren.
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 05/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 1e6e658023419a9a676b861efb8769a5bbf6cadf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030793"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-docker"></a>Installieren von SQL Server Machine Learning Services (Python und R) in Docker

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

In diesem Artikel wird die Installation von SQL Server Machine Learning Services in Docker erläutert. Sie können Machine Learning Services verwenden, um Python- und R-Skripts in einer Datenbank auszuführen. Mit Machine Learning Services werden keine vorgefertigten Container bereitgestellt. Sie können einen Container mithilfe einer [Beispielvorlage auf GitHub](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices) aus den SQL Server-Containern erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Git-Befehlszeilenschnittstelle.

- Docker Engine 1.8 oder höher für eine beliebige unterstützte Linux-Distribution oder Docker für Mac/Windows. Weitere Informationen finden Sie unter [Get Docker (Docker-Download)](https://docs.docker.com/get-docker/).

- Außerdem gelten die [Systemanforderungen für SQL Server für Linux](sql-server-linux-setup.md#system).

## <a name="clone-the-mssql-docker-repository"></a>Klonen des mssql-docker-Repositorys

Mit dem folgenden Befehl wird das Git-Repository `mssql-docker` in ein lokales Verzeichnis geklont.

1. Öffnen Sie ein Bash-Terminal unter Linux oder Mac.

2. Erstellen Sie ein Verzeichnis, in dem eine lokale Kopie des mssql-docker-Repositorys gespeichert wird.

3. Führen Sie den Befehl „git clone“ aus, um das mssql-docker-Repository zu klonen:

    ```bash
    git clone https://github.com/microsoft/mssql-docker mssql-docker
    ```

## <a name="build-a-sql-server-linux-container-image"></a>Erstellen eines SQL Server-Linux-Containerimage

Führen Sie die folgenden Schritte durch, um das Docker-Image zu erstellen:

1. Wechseln Sie zum Verzeichnis „mssql-mlservices“:
    
    ```bash
    /mssql-docker/linux/preview/examples/mssql-mlservices
    ```

2. Führen Sie im gleichen Verzeichnis den folgenden Befehl aus:

    ```bash
    docker build -t mssql-server-mlservices .
    ```

3. Führen Sie den folgenden Befehl aus:

    ```bash
    docker run -d -e MSSQL_PID=Developer -e ACCEPT_EULA=Y -e ACCEPT_EULA_ML=Y -e MSSQL_SA_PASSWORD=<password> -v <directory on the host OS>:/var/opt/mssql -p 1433:1433 mssql-server-mlservices
    ```
  
    > [!NOTE]
    > Die folgenden Werte können für MSSQL_PID verwendet werden: Developer (kostenlos), Express (kostenlos), Enteprise (kostenpflichtig), Standard (kostenpflichtig). Wenn Sie eine kostenpflichtige Edition verwenden, stellen Sie sicher, dass Sie eine Lizenz erworben haben. Ersetzen Sie (Kennwort) durch Ihr tatsächliches Kennwort. Die Volumebereitstellung mit -v ist optional. Ersetzen Sie (Verzeichnis auf dem Hostbetriebssystem) durch ein tatsächliches Verzeichnis, in das Sie die Datenbankdaten und Protokolldateien einbinden möchten.
    

4. Bestätigen Sie die Änderung, indem Sie den folgenden Befehl ausführen:

    ```bash
    docker ps -a
    ```

   > [!NOTE]
   > Zum Erstellen des Docker-Images müssen Sie Pakete installieren, die mehrere GB groß sind. Abhängig von der Netzwerkbandbreite kann es einige Zeit dauern, bis die Skriptausführung abgeschlossen ist.

## <a name="run-the-sql-server-linux-container-image"></a>Ausführen des SQL Server-Linux-Containerimages

1. Legen Sie vor dem Ausführen des Containers Ihre Umgebungsvariablen fest. Legen Sie PATH_TO_MSSQL-Umgebungsvariable auf ein Hostverzeichnis fest:

   ```bash
   export MSSQL_PID='Developer'
   export ACCEPT_EULA='Y'
   export ACCEPT_EULA_ML='Y'
   export PATH_TO_MSSQL='/home/mssql/'
   ```
  
   > [!NOTE]
   > Die Vorgehensweise zum Ausführen von SQL Server-Produktionseditionen in Containern weicht hiervon minimal ab. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server-Containerimages in Docker](./sql-server-linux-docker-container-deployment.md). Bei Verwendung derselben Containernamen und Ports können Sie die restlichen Schritte dieser exemplarischen Vorgehensweise auch mit Containern für Produktionsumgebungen ausführen.

2. Führen Sie den Befehl `docker ps` aus, um Ihre Docker-Container anzuzeigen:

   ```bash
   sudo docker ps -a
   ```

3. Wenn die Spalte **STATUS** den Wert **Up** (Aktiv) enthält, wird SQL Server im Container ausgeführt und überwacht den in der Spalte **PORTS** angegebenen Port. Wenn in der Spalte **STATUS** Ihres SQL Server-Containers **Exited** (Beendet) steht, lesen Sie bitte im [Abschnitt „Problembehebung“ im Konfigurationshandbuch](./sql-server-linux-docker-container-troubleshooting.md) nach.

 
    Ausgabe:

    ```
    CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
    941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour     0.0.0.0:1401->1433/tcp   sql1
    ```

## <a name="enable-machine-learning-services"></a>Aktivieren von Machine Learning Services

Stellen Sie zum Aktivieren von Machine Learning Services eine Verbindung mit Ihrer SQL Server-Instanz her, und führen Sie die folgende T-SQL-Anweisung aus:

```sql
EXEC sp_configure  'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE
```

## <a name="next-steps"></a>Nächste Schritte

Python-Entwickler können in den folgenden Tutorials erfahren, wie Python mit SQL Server verwendet werden kann:

+ [Python-Tutorial: Bereitstellen eines linearen Regressionsmodells in SQL Server Machine Learning Services](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python-Tutorial: Kategorisieren von Kunden mithilfe von K-Means-Clustering mit SQL Server Machine Learning Services](../machine-learning/tutorials/python-clustering-model.md)

R-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von R unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Schnellstart: Ausführen von R in T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Tutorial: Datenbankinterne Analysen für R-Entwickler](../machine-learning/tutorials/r-taxi-classification-introduction.md)