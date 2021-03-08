---
title: Auswerten von Clusterprotokollen mit einem Kibana-Dashboard
titleSuffix: SQL Server Big Data Clusters
description: Überwachen eines Clusters mit einem Kibana-Dashboard in einem SQL Server 2019-Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 02/25/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c26dc7e193d3dd5ad688af4d4dc79e2dd3eeea7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835967"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Auswerten von Clusterprotokollen mit einem Kibana-Dashboard

In diesem Artikel wird beschrieben, wie Sie eine Anwendung in einem [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] überwachen.

## <a name="prerequisites"></a>Voraussetzungen

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Befehlszeilen-Hilfsprogramm „azdata“](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Funktionen

In [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] können Sie Ihre Anwendung erstellen, löschen, beschreiben, initialisieren, auflisten und aktualisieren. In der folgenden Tabelle werden die Befehle für die Anwendungsbereitstellung beschrieben, die Sie mit **azdata** verwenden können.

|Get-Help |BESCHREIBUNG |
|:---|:---|
|`azdata bdc endpoint list` | Listet die Endpunkte für [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] auf. |


Sie können das folgende Beispiel verwenden, um den Endpunkt eines **Kibana-Dashboards** aufzulisten:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

In der Ausgabe wird der Endpunkt angegeben, den Sie für die Anmeldung mit dem Benutzernamen und dem Kennwort Ihres Clusters verwenden können. 

![Kibana-Dashboard](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Der Link zu einem Kibana-Dashboard:

![Kibana-Dashboard](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Der ältere Microsoft Edge-Browser ist nicht mit Kibana kompatibel. Sie müssen den Edge Chromium-basierten Browser verwenden, damit das Dashboard ordnungsgemäß angezeigt wird. Wenn Sie die Dashboards in einem nicht unterstützten Browser laden, wird eine leere Seite angezeigt. Weitere Informationen finden Sie unter [Unterstützte Browser für Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).


