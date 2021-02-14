---
title: Ausführen von Anwendungen mit azdata
titleSuffix: SQL Server Big Data Clusters
description: Hier finden Sie Informationen zum Ausführen von Anwendungen mit azdata in Big Data-Clustern in SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b83c79398dbcc6c7d55eb0ddfe6cb1d8f0d398ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039590"
---
# <a name="run-apps-with-azdata---sql-server-big-data-clusters"></a>Ausführen von Apps mit azdata – Big Data-Cluster in SQL Server

In diesem Artikel wird beschrieben, wie Sie eine Anwendung in einem Big Data-Cluster in SQL Server ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- [SQL Server 2019: Big Data-Cluster](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Funktionen

In SQL Server 2019 können Sie Ihre Anwendung erstellen, löschen, beschreiben, initialisieren, auflisten und aktualisieren. In der folgenden Tabelle werden die Befehle für die Anwendungsbereitstellung beschrieben, die Sie mit **azdata** verwenden können.

|Get-Help |BESCHREIBUNG |
|:---|:---|
|`azdata app describe` | Beschreiben einer Anwendung. |
|`azdata app run` | Ausführen einer Anwendung. |


In den folgenden Abschnitten werden diese Parameter eingehender beschrieben.


## <a name="run-an-app"></a>Ausführen einer App

Wenn sich die App in einem `Ready`-Status befindet, können Sie sie verwenden, indem Sie sie mit den angegebenen Eingabeparametern ausführen. Verwenden Sie die folgende Syntax zum Ausführen einer App:

```bash
azdata app run --name <app_name> --version <app_version> --inputs <inputs_params>
```

Der folgende Beispielbefehl veranschaulicht den run-Befehl:

```bash
azdata app run --name add-app --version v1 --inputs x=1,y=2
```

Wenn die Ausführung erfolgreich war, sollte die Ausgabe angezeigt werden, wie Sie beim Erstellen der App angegeben haben. Die folgende Ausgabe dient als Beispiel.

```json
{
  "changedFiles": [],
  "consoleOutput": "",
  "errorMessage": "",
  "outputFiles": {},
  "outputParameters": {
    "result": 3
  },
  "success": true
}
```


## <a name="describe-an-app"></a>Beschreiben einer Anwendung

Der describe-Befehl stellt ausführliche Informationen zur App bereit, darunter auch den Endpunkt in Ihrem Cluster. Dieser wird in der Regel von einem App-Entwickler dazu verwendet, eine App mithilfe des Swagger-Clients zu erstellen und mithilfe des Webdiensts RESTful mit der App zu interagieren. Weitere Informationen finden Sie unter [Nutzen von Anwendungen in Big Data-Clustern](app-consume.md).

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/api/app/add-app/v1",
    "swagger": "https://10.1.1.3:30080/api/app/add-app/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

Im Artikel [Nutzen von Anwendungen auf Big Data-Clustern](app-consume.md) erfahren Sie, wie Sie Apps, die in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] bereitgestellt wurden, in Ihre eigenen Anwendungen integrieren können. Weitere Beispiele finden Sie außerdem unter den [Beispielen zur App-Bereitstellung](https://aka.ms/sql-app-deploy).

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).