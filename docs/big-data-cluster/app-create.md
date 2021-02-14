---
title: Bereitstellen von Anwendungen mit azdata
titleSuffix: SQL Server Big Data Clusters
description: Bereitstellen eines Python- oder R-Skripts als Anwendung auf einem Big Data-Cluster für SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9d475edb720d329d5fb52f7bd75c055fc3a8494a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047930"
---
# <a name="how-to-deploy-an-app-on-sql-server-big-data-clusters"></a>Bereitstellen einer App in einem Big Data-Cluster in SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In Big Data-Clustern (BDC) in SQL Server bereitgestellte Anwendungen profitieren von vielen Vorteilen wie der Rechenleistung des Clusters und können auch auf umfangreiche Daten zugreifen, die im Cluster verfügbar sind. Dies führt zu einer erheblichen Verbesserung der Leistung, weil sich Ihre App in dem Cluster befindet, in dem auch die Daten gespeichert sind.

In diesem Artikel wird beschrieben, wie Sie R- und Python-Skripts als Anwendung in einem Big Data-Cluster in SQL Server bereitstellen und verwalten.

## <a name="whats-new-and-improved"></a>Neuerungen und Verbesserungen

- Ein einzelnes Befehlszeilen-Hilfsprogramm zum Verwalten von Cluster und App.
- Vereinfachte App-Bereitstellung bei gleichzeitiger Bereitstellung einer differenzierten Steuerung durch Spezifikationsdateien.
- Unterstützung für das Hosting zusätzlicher Anwendungstypen: SQL Server Integration Services (SSIS) und MLeap.
- [Visual Studio Code-Erweiterung](app-deployment-extension.md) zum Verwalten der Anwendungsbereitstellung

Anwendungen werden mit [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] bereitgestellt und verwaltet. Dieser Artikel enthält Beispiele für die Bereitstellung von Apps über die Befehlszeile. Weitere Informationen zur Verwendung in Visual Studio Code finden Sie unter [Visual Studio Code-Erweiterung](app-deployment-extension.md).

Die folgenden Typen von Apps werden unterstützt:

- **Python**: Eine der beliebtesten allgemeinen Programmiersprachen für verschiedene Personas (wie z. B. Datentechniker, Datenanalysten oder DevOps-Techniker) unterstützt zahlreiche Szenarien wie Data Wrangling, Automatisierung, Prototypenerstellung. In gewissem Umfang wird sie auch zunehmend für die Programmierung von Anwendungen auf Unternehmensebene verwendet, die mit Webentwicklungsframeworks wie Flask und Django zusammenarbeiten, um unterschiedlichen Unternehmensanforderungen gerecht zu werden.  
- **R**: Eine weitere beliebte Programmiersprache für Datentechniker und Datenanalysten. Im Vergleich zu Python ist R eine Programmiersprache, deren Schwerpunkt auf statistischem Computing und Grafiken liegt.  
- **SQL Server Integration Services (SSIS)** : Hochleistungsfähige Datenintegrationslösungen für das Erstellen und Debuggen von ETL-Paketen. Das Feature verwendet das Data Transformation Services-Paketdateiformat (DTSX), ein XML-basiertes Dateiformat, in dem die Anweisungen für die Verarbeitung der Datenmigration zwischen Datenbanken und die Integration externer Datenquellen gespeichert werden.   
- **MLeap**: Ein gängiges Serialisierungsformat, das alles bietet, was zum Ausführen und Serialisieren von SparkML- und anderen Pipelines erforderlich ist, die dann zur Laufzeit geladen werden können, um ML-Bewertungsaufgaben nahezu in Echtzeit und in der Nähe der Daten zu verarbeiten.  

## <a name="prerequisites"></a>Voraussetzungen

- [SQL Server 2019: Big Data-Cluster](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Funktionen

In SQL Server 2019 können Sie Ihre Anwendung erstellen, löschen, beschreiben, initialisieren, auflisten und aktualisieren. In der folgenden Tabelle werden die Befehle für die Anwendungsbereitstellung beschrieben, die Sie mit **azdata** verwenden können.

|Get-Help |BESCHREIBUNG |
|:---|:---|
|`azdata login` | Anmelden bei einem Big Data-Cluster für SQL Server |
|`azdata app create` | Erstellen einer Anwendung. |
|`azdata app delete` | Löschen einer Anwendung. |
|`azdata app describe` | Beschreiben einer Anwendung. |
|`azdata app init` | Starten eines neuen Anwendungsgerüsts. |
|`azdata app list` | Auflisten von Anwendungen. |
|`azdata app run` | Ausführen einer Anwendung. |
|`azdata app update`| Aktualisieren einer Anwendung. |

Mit dem Parameter `--help` können Sie wie im folgenden Beispiel Hilfe erhalten:

```bash
azdata app create --help
```

In den folgenden Abschnitten werden diese Parameter eingehender beschrieben.

## <a name="sign-in"></a>Anmelden

Bevor Sie Anwendungen bereitstellen oder mit ihnen interagieren, melden Sie sich zunächst mit dem `azdata login`-Befehl bei Ihrem SQL Server-Big Data-Cluster an. Geben Sie die externe IP-Adresse des `controller-svc-external`-Diensts (z.B. `https://ip-address:30080`) zusammen mit dem Benutzernamen und dem Kennwort für den Cluster an.

```bash
azdata login --controller-endpoint https://<ip-address-of-controller-svc-external>:30080 --controller-username <user-name>
```

## <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Wenn Sie AKS verwenden, müssen Sie den folgenden Befehl in einem Bash- oder CMD-Fenster ausführen, um die IP-Adresse des `controller-svc-external`-Diensts abzurufen:


```bash
kubectl get svc controller-svc-external -n <name of your big data cluster>
```

## <a name="kubernetes-clusters-created-with-kubeadm"></a>Mit kubeadm erstellte Kubernetes-Cluster

Führen Sie folgenden Befehl aus, um die IP-Adresse für die Anmeldung beim Cluster abzurufen:

```bash
kubectl get node --selector='node-role.kubernetes.io/master'
```

## <a name="create-an-app-skeleton"></a>Erstellen eines App-Gerüsts

Der Befehl **azdata app init** bietet ein Gerüst mit den relevanten Artefakten, die für die Bereitstellung einer App erforderlich sind. Im folgenden Beispiel wird „add-app“ durch Ausführen des Befehls erstellt.

```bash
azdata app init --name add-app --version v1 --template python
```

„hello“ ist der Name des erstellten Ordners.  Sie können mit dem Befehl `cd` in das Verzeichnis wechseln und die generierten Dateien im Ordner überprüfen. „spec.yaml“ definiert die App, z. B. deren Name, Version und Quellcode. Sie können die Spezifikation bearbeiten, um den Namen, die Version, die Eingabe und die Ausgaben zu ändern.

Nachfolgend sehen Sie eine Beispielausgabe des Befehls „init“, die im Ordner enthalten ist.

```
add-app.py
run-spec.yaml
spec.yaml
```

## <a name="create-an-app"></a>Erstellen einer App

Verwenden Sie zum Erstellen einer Anwendung [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] mit dem `app create`-Befehl. Diese Dateien befinden sich lokal auf dem Computer, auf dem Sie die App erstellen.

Verwenden Sie die folgende Syntax, um eine neue App im Big Data-Cluster zu erstellen:

```bash
azdata app create --spec <directory containing spec file>
```

Der Befehl könnte z.B. wie folgt aussehen:

```bash
azdata app create --spec ./addpy
```

Dabei wird vorausgesetzt, dass Sie die Anwendung im `addpy`-Ordner gespeichert haben. Dieser Ordner sollte auch eine Spezifikationsdatei für die Anwendung mit dem Namen `spec.yaml` enthalten. Weitere Informationen zur Datei `spec.yaml` finden Sie auf der Seite [Anwendungsbereitstellung](concept-application-deployment.md).

Um diese Beispiel-App bereitzustellen, erstellen Sie die folgenden Dateien in einem Verzeichnis mit dem Namen `addpy`:

- `add.py`. Kopieren Sie den folgenden Python-Code in diese Datei:
   ```py
   #add.py
  def add(x, y):
    result = x+y
    return result
  result=add(x,y)
   ```
- `spec.yaml`. Kopieren Sie den folgenden Code in diese Datei:
   ```yaml
   #spec.yaml
   name: add-app #name of your python script
   version: v1  #version of the app
   runtime: Python #the language this app uses (R or Python)
   src: ./add.py #full path to the location of the app
   entrypoint: add #the function that will be called upon execution
   replicas: 1  #number of replicas needed
   poolsize: 1  #the pool size that you need your app to scale
   inputs:  #input parameters that the app expects and the type
     x: int
     y: int
   output: #output parameter the app expects and the type
     result: int
   ```

Führen Sie anschließend den folgenden Befehl aus:

```bash
azdata app create --spec ./addpy
```

Mithilfe des list-Befehls können Sie überprüfen, ob die App bereitgestellt wurde:

```bash
azdata app list
```

Wenn die Bereitstellung noch nicht abgeschlossen ist, wird für `state` die Angabe `WaitingforCreate` wie im folgenden Beispiel angezeigt:

```json
[
  {
    "name": "add-app",
    "state": "WaitingforCreate",
    "version": "v1"
  }
]
```

Nach erfolgreicher Bereitstellung sollte `state` in den `Ready`-Status wechseln:

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="list-an-app"></a>Auflisten einer App

Sie können mit dem `app list`-Befehl alle erfolgreich erstellten Apps auflisten.

Der folgende Befehl listet alle verfügbaren Anwendungen in Ihrem Big Data-Cluster auf:

```bash
azdata app list
```

Wenn Sie einen Namen und eine Version angeben, werden diese spezifische App und deren Status („Wird erstellt“ oder „Bereit“) aufgelistet:

```bash
azdata app list --name <app_name> --version <app_version>
```

Dieser Befehl wird im folgenden Beispiel veranschaulicht:

```bash
azdata app list --name add-app --version v1
```

Die Ausgabe sollte etwa folgendem Beispiel entsprechen:

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="delete-an-app"></a>Löschen einer App

Verwenden Sie die folgende Syntax, um eine App aus Ihrem Big Data-Cluster zu löschen:

```bash
azdata app delete --name add-app --version v1
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Ausführen von Anwendungen in Big Data-Clustern](app-run.md) darüber, wie Sie in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] bereitgestellte Apps in Ihre eigenen Anwendungen integrieren können, und lesen Sie [Verwenden von Anwendungen in Big Data-Clustern](app-consume.md), um weitere Informationen zu erhalten. Weitere Beispiele finden Sie außerdem unter den [Beispielen zur App-Bereitstellung](https://aka.ms/sql-app-deploy).

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).