---
title: Überwachen von Anwendungen mit azdata und einem Grafana-Dashboard
titleSuffix: SQL Server Big Data Clusters
description: Überwachen Sie Anwendungen mit azdata und Grafana in einem Big Data-Cluster in SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 380a6c8ab8812ee6a8aae132cbc12125777e2960
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047790"
---
# <a name="monitor-applications-with-azdata-and-grafana-dashboard"></a>Überwachen von Anwendungen mit azdata und einem Grafana-Dashboard

Grafana ist eines der besten cloudnativen Virtualisierungstools, mit denen Sie verschiedene Überwachungsmetriken für Ihre in Kubernetes ausgeführte Anwendung bereitstellen können.  

In diesem Artikel wird beschrieben, wie Sie eine Anwendung in einem Big Data-Cluster in SQL Server überwachen.

## <a name="prerequisites"></a>Voraussetzungen

- [SQL Server 2019: Big Data-Cluster](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Funktionen

In SQL Server 2019 können Sie Ihre Anwendung erstellen, löschen, beschreiben, initialisieren, auflisten und aktualisieren. In der folgenden Tabelle werden die Befehle für die Anwendungsbereitstellung beschrieben, die Sie mit **azdata** verwenden können.

|Get-Help |BESCHREIBUNG |
|:---|:---|
|`azdata bdc endpoint list` | Listet die Endpunkte für den Big Data-Cluster auf. |


Sie können das folgende Beispiel verwenden, um den Endpunkt des **Grafana-Dashboards** aufzulisten:

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

In der Ausgabe wird der Endpunkt angegeben, den Sie für die Anmeldung mit dem Benutzernamen und dem Kennwort Ihres Clusters verwenden können. 

![Grafana-Dashboard](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)


Wechseln Sie nach dem Öffnen des Dashboards zu  **Host Apps Metrics**. Dort können Sie weitere Erkenntnisse über Ihre Anwendung erhalten und den Überblick behalten.  

![„Host Apps Metrics“](media/big-data-cluster-monitor-apps/host-apps-metrics.png)


Um weitere Erkenntnisse über einen einzelnen Pod der Anwendung zu erhalten (in einigen Fällen verfügen Sie über mehrere Kopien Ihrer Anwendung), wechseln Sie zu  **Host Pods Metrics** , und wählen Sie den entsprechenden Pod aus. Daraufhin wird Ihnen eine Ansicht der Metriken wie folgt angezeigt:  

![„Host Pods Metrics“](media/big-data-cluster-monitor-apps/host-pods-metrics.png) 


## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter den [Beispielen zur App-Bereitstellung](https://aka.ms/sql-app-deploy).

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).