---
title: PolyBase-Erweiterungsgruppen | Microsoft-Dokumentation
description: Verwenden Sie das PolyBase-Gruppenfeature, um ein Cluster aus SQL Server-Instanzen zu erstellen. Dies optimiert die Abfrageleistung für große Datasets aus externen Quellen.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- sql13.swb.polybasescaleoutcluster.page.f1
helpviewer_keywords:
- PolyBase
- PolyBase, scale-out groups
- scale-out PolyBase
ms.assetid: c7810135-4d63-4161-93ab-0e75e9d10ab5
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: c7c2594dfd40ff952c28cf203e05f37c789b3d06
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076661"
---
# <a name="polybase-scale-out-groups"></a>PolyBase-Erweiterungsgruppen

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Eine eigenständige SQL Server-Instanz mit PolyBase kann bei der Verarbeitung von sehr großen Datasets in Hadoop oder Azure Blob Storage zu einem Leistungsengpass werden. Die PolyBase-Gruppenfunktion ermöglicht Ihnen die Erstellung eines Clusters aus SQL Server-Instanzen, um große Datasets in einer Skalierungsart zu verarbeiten, die zu besseren Abfrageleistungen führt. Diese Datasets stammen aus externen Datenquellen, wie beispielsweise Hadoop oder Azure Blob Storage. Nun können Sie Ihre SQL Server-Computeressourcen skalieren, um den Leistungsanforderungen Ihrer Workload gerecht zu werden. Mit PolyBase-Erweiterungsgruppen, einer Gruppe von SQL Server-Instanzen, können Sie große externe Datasets in einer Architektur für die Parallelverarbeitung verarbeiten. Durch das Hinzufügen von weiteren SQL Server-Instanzen zur Gruppe kann die Datenlade- und -abfrageleistung linear erhöht werden. 
  
Siehe [Get started with PolyBase](./polybase-guide.md) (Erste Schritte mit PolyBase) und [PolyBase Guide](../../relational-databases/polybase/polybase-guide.md)(PolyBase-Handbuch).
  
![Diagramm: PolyBase-Erweiterungsgruppen](../../relational-databases/polybase/media/polybase-scale-out-groups.png "PolyBase-Erweiterungsgruppen")  
  
## <a name="head-node"></a>Hauptknoten  

Der Hauptknoten enthält die SQL Server-Instanz, an die die PolyBase-Abfragen geschickt werden. Jede PolyBase-Gruppe kann nur einen Hauptknoten haben. Ein Hauptknoten ist eine logische Gruppe aus SQL Server-Datenbank-Engine, PolyBase-Engine und PolyBase-Datenverschiebungsdienst auf der SQL Server-Instanz. Für SQL Server 2017 und SQL Server 2016 ist ein Enterprise Edition-Hauptknoten erforderlich. Ab SQL Server 2019 ist ein Enterprise oder Standard Edition-PolyBase-Hauptknoten möglich.
  
## <a name="compute-node"></a>Computeknoten

Ein Computeknoten enthält die SQL Server-Instanz, die bei der Verarbeitung von Hochskalierungsabfragen für externe Daten hilft. Ein Computeknoten ist eine logische Gruppe aus SQL Server und dem PolyBase-Datenverschiebungsdienst auf der SQL Server-Instanz. Eine PolyBase-Gruppe kann mehrere Computeknoten umfassen. Auf den Hauptknoten und den Computeknoten muss die gleiche Version von SQL Server ausgeführt werden. Im ersten Release von SQL Server 2016 waren Enterprise oder Standard Edition-Computeknoten möglich. Seit SQL Server 2016 SP1 sind Computeknoten aller Editionen von SQL Server möglich.

## <a name="scale-out-reads"></a>Horizontale Leseskalierung

Beim Abfragen von externen SQL Server-, Oracle- oder Teradata-Instanzen profitieren partitionierte Tabellen von der horizontalen Leseskalierung. Jeder Knoten in einer PolyBase-Erweiterungsgruppe kann zum Lesen externer Daten auf bis zu 8 Leser aufgestockt werden. Jeder Leser wird dabei einer Partition zum Lesen in der externen Tabelle zugewiesen. 

Beispiel: Angenommen, Sie verfügen über eine externe SQL Server-Tabelle mit 12 Partitionen für jeden Monat und einer PolyBase-Erweiterungsgruppe mit 3 Knoten. Jeder Knoten verwendet 4 PolyBase-Leser, um jede der 12 Partitionen zu verarbeiten. Dies wird in der folgenden Abbildung veranschaulicht. 

> [!NOTE]
>  Dieser Vorgang darf nicht mit der horizontalen Leseskalierung über Hadoop verwechselt werden. 

![Lesevorgänge für PolyBase-Erweiterungsgruppen](../../relational-databases/polybase/media/polybase-scale-out-groups2.png "PolyBase-Erweiterungsgruppen")
  
## <a name="distributed-query-processing"></a>Verarbeiten verteilter Abfragen  

PolyBase-Abfragen werden an den SQL Server auf dem Hauptknoten gesendet. Der Teil der Abfrage, der sich auf externe Tabellen bezieht, wird an die PolyBase-Engine übergeben.
  
Die PolyBase-Engine ist die zentrale Komponente von PolyBase-Abfragen. Es analysiert die Abfrage an externen Daten, erstellt den Abfrageplan und gibt die Arbeit für die Ausführung an den Datenverschiebungsdienst auf den Computeknoten weiter. Nach Abschluss der Arbeit erhält es die Ergebnisse von den Computeknoten und übergibt sie an SQL Server für die Verarbeitung und die Rückgabe an den Client.
  
Der PolyBase-Datenverschiebungsdienst erhält Anweisungen von der PolyBase-Engine und überträgt die Daten zwischen HDFS und SQL Server sowie zwischen SQL Server-Instanzen auf den Haupt- und Serverknoten.
  
## <a name="next-steps"></a>Nächste Schritte

Um eine PolyBase-Erweiterungsgruppe konfigurieren zu können, lesen Sie den folgenden Leitfaden:

[Verbessern von PolyBase-Erweiterungsgruppen unter Windows](configure-scale-out-groups-windows.md)

## <a name="see-also"></a>Weitere Informationen

 [sys-dm-exec-compute-nodes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)   
 [sys-dm-exec-compute-node-status](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)   
 [sys.dm_exec_compute_node_errors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
