---
title: Verwaltungsressourcen für Big Data-Cluster (BDC)
titleSuffix: SQL Server
description: In diesem Artikel wird erläutert, wie Sie den Status eines Big-Data-Clusters mithilfe von Azure Data Studio, Notebooks und der azdata-Befehlen (Azure Data CLI) anzeigen.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 10/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194f74fa175ad3f1c08ceab542b6db7812987157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045639"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Verwaltungsressourcen für Big Data-Cluster (BDC) 

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel ist beschrieben, wie ein SQL Server-Big Data-Cluster von außen verwaltet wird.

## <a name="know-your-architecture"></a>Wissen, wie die Architektur aussieht

Ab SQL Server 2019 (15.x) können Sie SQL Server-Big Data-Cluster verwenden, um skalierbare Cluster von SQL Server-, Spark- und HDFS-Containern bereitzustellen, die auf Kubernetes ausgeführt werden. In den folgenden Artikeln erhalten Sie eine Übersicht über Big Data-Cluster:
- [Was sind Big Data-Cluster von SQL Server?](big-data-cluster-overview.md)

SQL Server-Big Data-Cluster stellen eine kohärente und konsistente Autorisierung und Authentifizierung bereit. In den folgenden Artikeln erhalten Sie eine Übersicht über die Big Data-Cluster-Sicherheit:
- [Sicherheitskonzepte für SQL Server-Big Data-Cluster](concept-security.md)

## <a name="manage-and-operate-with-tools"></a>Verwalten und Steuern mit Tools

In den folgenden Artikeln ist beschrieben, wie Big Data-Cluster auf folgende Arten verwaltet und gesteuert werden: 

- [Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mit Azure Data Studio](connect-to-big-data-cluster.md)
- [Verwalten von Big Data-Clustern mit dem Controllerdashboard von SQL Server](manage-with-controller-dashboard.md)
- [Verwalten von SQL Server-Big Data-Clustern mit Azure Data Studio-Notebooks](notebooks-manage-bdc.md)
- [Verwalten von Big Data-Clustern (BDC) mit Notebooks](cluster-manage-notebooks.md)
- [Ausführen eines Beispielnotebooks mithilfe von Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Überwachen mit Tools

In den folgenden Artikeln ist beschrieben, wie Sie Big Data-Cluster auf folgende Weisen überwachen: 

- [Überwachen des Clusterstatus mit Azure Data Studio](cluster-monitor-ads.md)
- [Überwachen eines Clusters mit azdata und kubectl](cluster-monitor-cmdlet.md)
- [Überwachen eines Clusters mit azdata und einem Grafana-Dashboard](cluster-monitor-grafana.md)
- [Überwachen eines Clusters mit Notebooks](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Überwachen und Untersuchen von Protokollen mit Notebooks

In den folgenden Artikeln sind viele der Jupyter-Notebooks aufgelistet, die in Azure Data Studio verfügbar sind.

- [Überwachen eines Clusters mit Notebooks](cluster-monitor-notebooks.md)
- [Sammeln und Analysieren von Protokollen im Cluster mit Notebooks](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Problembehandlungsressourcen für Big Data-Cluster

In den folgenden Artikeln ist beschrieben, wie Sie Probleme mit Big Data-Clustern beheben:

- [Problembehandlung für Big Data-Cluster für SQL Server auf Kubernetes](cluster-troubleshooting-commands.md) 
- [Problembehandlung für ein PySpark-Notebook](troubleshoot-pyspark-notebook.md)
- [Problembehandlung für Big Data-Cluster (BDC) mit Notebooks](cluster-troubleshooter-notebooks.md)
- [Wiederherstellen von HDFS-Berechtigungen](troubleshoot-hdfs-restore-admin.md)

In den folgenden Artikeln ist beschrieben, wie Sie Probleme mit Big Data-Clustern beheben, die im Active Directory-Modus bereitgestellt sind:
- [Behandeln von Problemen mit der Integration von Active Directory in SQL Server-Big Data-Clustern](troubleshoot-active-directory.md) 
- [Symptom: Fehler beim Anmelden im AD-Modus: nicht vertrauenswürdige Domäne (Big Data-Cluster)](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Bereitstellung im AD-Modus beendet: Fehlender Eintrag für Reverse-Lookupzone für DC](troubleshoot-ad-reverse-lookup-zone.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).