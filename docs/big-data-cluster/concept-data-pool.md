---
title: Was sind Datenpools?
titleSuffix: SQL Server big data clusters
description: Erfahren Sie mehr über die Rolle von SQL Server-Datenpools in einem Big Data-Cluster in SQL Server sowie über die Architektur und die Funktionalität eines SQL-Datenpools.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69ef4f739b35c55a31cb19fd8cb317d7750f8639
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047468"
---
# <a name="what-are-data-pools-in-a-sql-server-big-data-cluster"></a>Was sind Datenpools in einem Big-Data-Cluster für SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel ist die Rolle beschrieben, die *SQL Server-Datenpools* in einem SQL Server-Big Data-Cluster spielen. In den folgenden Abschnitten werden die Architektur, die Funktionen und die Nutzungsszenarios eines Datenpools beschrieben.

In diesem fünfminütigen Video werden Datenpools vorgestellt, und es wird gezeigt, wie Sie Daten aus Datenpools abfragen:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Querying-Data-from-Big-Data-Cluster-Data-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="data-pool-architecture"></a>Datenpoolarchitektur

Ein Datenpool besteht aus mindestens einer SQL Server-Datenpoolinstanz, die einen persistenten SQL Server-Speicher für den Cluster bereitstellt. Datenpools ermöglichen das Abfragen von zwischengespeicherten Daten in Bezug auf externe Datenquellen und das Auslagern von Arbeit. Daten werden entweder mithilfe von T-SQL-Abfragen oder über Spark-Aufträge vom Datenpool erfasst. Zur Verbesserung der Leistung von großen Datasets werden die erfassten Daten in Shards unterteilt und auf alle SQL Server-Instanzen im Pool verteilt. Die Verteilungsmethoden Roundrobin sowie die Replikatverteilung werden unterstützt. Für die Optimierung des Lesezugriffs wird ein gruppierter Columnstore-Index für jede Tabelle in jeder Datenpoolinstanz erstellt. Ein Datenpool fungiert als Data Mart mit horizontaler Skalierung für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

![Data Mart mit horizontaler Skalierung](media/concept-data-pool/data-virtualization-improvements.png)

Der Zugriff auf die SQL Server-Instanzen im Datenpool wird über die SQL Server-Masterinstanz verwaltet. Neben externen PolyBase-Tabellen zum Speichen des Datencaches wird auch eine externe Datenquelle für den Datenpool erstellt. Im Hintergrund erstellt der Controller eine Datenbank im Datenpool mit Tabellen, die den externen Tabellen entsprechen. Der Workflow über die SQL Server-Masterinstanz ist transparent: Der Controller leitet die spezifischen Anforderungen, die an die externe Tabelle gesendet werden, an die SQL Server-Instanzen im Datenpool weiter, z. B. über den Computepool, führt Abfragen aus und gibt das Resultset zurück. Daten im Datenpool können nur erfasst oder abgefragt werden. Sie können nicht geändert werden. Zur Aktualisierung der Daten muss daher die Tabelle gelöscht und eine neue Tabelle erstellt werden, die anschließend wieder mit Daten aufgefüllt wird.

## <a name="data-pool-scenarios"></a>Datenpoolszenarios

 Datenpools werden häufig zu Berichterstellungszwecken verwendet. Beispielsweise kann eine komplexe Abfrage, die mehrere PolyBase-Datenquellen verbindet und für einen Wochenbericht verwendet wird, in den Datenpool ausgelagert werden. Die zwischengespeicherten Daten ermöglichen schnelles Computing auf lokaler Ebene und sorgen dafür, dass Sie nicht zu den ursprünglichen Datasets zurückgehen müssen. Ebenso können Dashboarddaten, die regelmäßig aktualisiert werden müssen, im Datenpool zwischengespeichert werden, um die Berichterstellung zu optimieren. Darüber hinaus bietet das Zwischenspeichern von Datasets im Datenpool auch bei der Untersuchung von Wiederholungen in Bezug auf Machine Learning einen Vorteil.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie in den folgenden Ressourcen:

- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]-Architektur](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
