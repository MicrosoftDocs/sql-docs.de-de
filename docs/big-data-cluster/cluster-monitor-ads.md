---
title: Überwachen eines Clusters mit Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Überwachen eines Clusters mit Azure Data Studio in einem SQL Server 2019 Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d03dc9c26f33fd512e035ab3cc9e167e1d9ef7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047740"
---
# <a name="monitor-cluster-status-with-azure-data-studio"></a>Überwachen des Clusterstatus mit Azure Data Studio

In diesem Artikel wird erläutert, wie Sie den Status eines Big-Data-Clusters mit Azure Data Studio anzeigen.

## <a name="use-azure-data-studio"></a><a id="datastudio"></a> Verwenden von Azure Data Studio

Nachdem Sie den neuesten **Insider-Build** von [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) heruntergeladen haben, können Sie mithilfe des Dashboards des Big-Data-Clusters für SQL Server Dienstendpunkte sowie den Status eines Big-Data-Clusters anzeigen. Einige der unten aufgeführten Features sind erstmals im Insider-Build von Azure Data Studio verfügbar.

1. Stellen Sie zunächst eine Verbindung mit Ihrem Big-Data-Cluster in Azure Data Studio her. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mithilfe von Azure Data Studio](connect-to-big-data-cluster.md).

1. Klicken Sie mit der rechten Maustaste auf den Endpunkt des Big-Data-Clusters, und klicken Sie auf **Verwalten**.

   ![Rechtsklick auf „Verwalten“](media/view-cluster-status/right-click-manage.png)

1. Klicken Sie auf die Registerkarte **SQL Server Big Data Cluster** (Big-Data-Cluster für SQL Server), um auf das Dashboard des Big-Data-Clusters zuzugreifen.

   ![Dashboard des Big-Data-Clusters](media/view-cluster-status/bdc-dashboard.png)

### <a name="service-endpoints"></a>Dienstendpunkte

Es ist wichtig, einfach auf die verschiedenen Dienste innerhalb eines Big-Data-Clusters zugreifen zu können. Das Dashboard des Big-Data-Clusters stellt eine Dienstendpunkttabelle bereit, in der die Dienstendpunkte angezeigt werden und aus der diese kopiert werden können.

![Dienstendpunkte](media/view-cluster-status/service-endpoints.png)

Diese Dienste listen die Endpunkte auf, die Sie kopieren und einfügen können, wenn Sie den Endpunkt für die Verbindung mit diesen Diensten benötigen. Sie können beispielsweise auf das Kopiersymbol rechts neben dem Endpunkt klicken und diesen dann in ein Textfenster einfügen, in dem er angefordert wird. Der Endpunkt des Clusterverwaltungsdiensts wird zum Ausführen des [Notebooks für den Clusterstatus](#notebook) benötigt.

### <a name="dashboards"></a>Dashboards

In der Dienstendpunkttabelle werden auch mehrere Dashboards für die Überwachung offengelegt:

- Metriken (Grafana)
- Protokolle (Kibana)
- Spark-Auftragsüberwachung
- Spark-Ressourcenverwaltung

Sie können direkt auf diese Links klicken. Sie müssen sich beim Zugriff auf diese Dashboards authentifizieren. Geben Sie für die Dashboards „Metriken“ und „Protokolle“ die Controller-Administratoranmeldeinformationen an, die Sie zum Zeitpunkt der Bereitstellung mithilfe der Umgebungsvariablen **AZDATA_USERNAME** und **AZDATA_PASSWORD** festlegen. Spark-Dashboards verwenden Gateway-Anmeldeinformationen (Knox): entweder eine AD-Identität in einem Cluster, der mit AD integriert ist oder **AZDATA_USERNAME**, und **AZDATA_PASSWORD**, wenn die Standardauthentifizierung in Ihrem Cluster verwendet wird.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

### <a name="cluster-status-notebook"></a><a id="notebook"></a> Notebook für den Clusterstatus

1. Sie können auch den Clusterstatus des Big-Data-Clusters anzeigen, indem Sie das Notebook für den Clusterstatus starten. Klicken Sie auf die Aufgabe **Clusterstatus**, um das Notebook zu starten.

    ![Starten](media/view-cluster-status/cluster-status-launch.png)

2. Bevor Sie beginnen, benötigen Sie folgende Elemente:

    - Name des Big-Data-Clusters
    - Benutzername des Controllers
    - Kennwort des Controllers
    - Endpunkte des Controllers

    Der Standardname des Big-Data-Clusters ist **mssql-cluster**, sofern Sie diesen nicht im Laufe der Bereitstellung angepasst haben. Sie finden den Endpunkt des Controllers in der Dienstendpunkttabelle im Dashboard des Big-Data-Clusters. Der Endpunkt wird als **Clusterverwaltungsdienst** aufgeführt. Wenn Sie die Anmeldeinformationen nicht kennen, fragen Sie den Administrator, der Ihren Cluster bereitgestellt hat.

3. Klicken Sie in der oberen Symbolleiste auf **Run Cells** (Zellen ausführen).

4. Befolgen Sie die Aufforderung zur Eingabe Ihrer Anmeldeinformationen. Nachdem Sie die Anmeldeinformationen für den Namen des Big-Data-Clusters, den Benutzernamen sowie das Kennwort des Controllers eingegeben haben, drücken Sie die EINGABETASTE.

    > [!Note]
    > Wenn Sie keine Konfigurationsdatei für Ihre Big Data eingerichtet haben, werden Sie nach dem Endpunkt des Controllers gefragt. Geben oder fügen Sie ihn ein, und drücken Sie dann die EINGABETASTE, um fortzufahren.

5. Wenn Sie erfolgreich eine Verbindung hergestellt haben, zeigt der Rest des Notebooks die Ausgabe der einzelnen Komponenten des Big-Data-Clusters an. Wenn Sie eine bestimmte Codezelle erneut ausführen möchten, zeigen Sie mit der Maus auf die Codezelle, und klicken Sie auf das Symbol zum **Ausführen**.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).