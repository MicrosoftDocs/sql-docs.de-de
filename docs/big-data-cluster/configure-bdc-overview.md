---
title: Übersicht über die Konfiguration von SQL Server Big Data-Clustern
titleSuffix: SQL Server big data clusters
description: Übersicht über die Konfiguration von Big Data-Clustern
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343984"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>Konfigurieren eines Big Data-Clusters in SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
Mithilfe der Konfigurationsverwaltung können Administratoren sicherstellen, dass der Big Data-Cluster immer den Anforderungen der jeweiligen Workloads entspricht. Clusteradministratoren können mit dieser Funktion verschiedene Teile des Big Data-Clusters zum Zeitpunkt der Bereitstellung oder nach der Bereitstellung ändern oder optimieren. Zudem erhalten sie tiefere Einblicke in die Konfigurationen, die in den BDCs ausgeführt werden. 

Mit der Konfigurationsverwaltung kann ein Administrator SQL-Agent aktivieren, Baselineressourcen für Spark-Aufträge der Organisation definieren oder erkennen, welche Einstellungen auf den einzelnen Ebenen konfigurierbar sind. Zum Zeitpunkt der Bereitstellung können Konfigurationen mithilfe der Bereitstellungsdatei `bdc.json` und nach der Bereitstellung über die azdata-CLI konfiguriert werden.

## <a name="configuration-scopes"></a>Konfigurationsebenen
Big Data-Cluster können auf drei Ebenen konfiguriert werden: `cluster`, `service` und `resource`. Auch die Hierarchie der Einstellungen entspricht dieser Reihenfolge, von oben nach unten. BDC-Komponenten übernehmen den Wert der Einstellung auf, der auf der niedrigsten Ebene definiert ist. Wenn die Einstellung nicht auf einer bestimmten Ebene definiert ist, wird der Wert der übergeordneten Ebene übernommen.

Angenommen, Sie möchten die Standardanzahl der Kerne definieren, die der Spark-Treiber im Speicherpool und in `Sparkhead`-Ressourcen verwenden soll. Wenn Sie die Standardanzahl der Kerne definieren möchten, können Sie eine der folgenden Aktionen ausführen:

- Standardwert für Kerne auf der Dienstebene `Spark` angeben

- Standardwert für Kerne auf der Ressourcenebene `storage-0` und `sparkhead` angeben

Im ersten Szenario *übernehmen* alle Ressourcen auf niedrigeren Ebenen des Spark-Diensts (Speicherpool und `Sparkhead`) die Standardanzahl der Kerne vom Standardwert des Spark-Diensts.

Im zweiten Szenario nutzt jede Ressource den auf ihrer jeweiligen Ebene definierten Wert.

Wenn die Standardanzahl von Kernen sowohl auf Dienst- als auch auf Ressourcenebene konfiguriert ist, überschreibt der Wert für die Ressourcenebene den Wert für die Dienstebene, da dies die niedrigste **vom Benutzer konfigurierte** Ebene für die gegebene Einstellung ist.

## <a name="next-steps"></a>Nächste Schritte

Spezifische Informationen zur Konfiguration finden Sie in den folgenden entsprechenden Artikeln:

- [Konfigurieren von Bereitstellungseinstellungen für Clusterressourcen und -dienste](deployment-custom-configuration.md)
- [Konfigurieren von Big Data-Clustern nach der Bereitstellung](configure-bdc-postdeployment.md)
- [Konfigurieren von Big Data-Clustern: bis CU8](configure-bdc-pre-configuration.md)

Referenz: 
- [Konfigurationseigenschaften für SQL Server Big Data-Cluster](reference-config-bdc-overview.md)
- [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Konfigurationseigenschaften der SQL Server-Masterinstanz (vor CU9)](reference-config-master-instance.md)
- [azdata CLI](../azdata/reference/reference-azdata.md)