---
description: Balanced Data Distributor (BDD)-Transformation
title: Balanced Data Distributor-Transformation | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.balanceddatadistributor.f1
ms.assetid: ae0b33dd-f44b-42df-b6f6-69861770ce10
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0af6a36d456b52791f2cafccc9c017f644893975
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194644"
---
# <a name="balanced-data-distributor-transformation"></a>Balanced Data Distributor (BDD)-Transformation

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die BDD (Balanced Data Distributor)-Transformation profitiert von der Fähigkeit moderner CPUs, eine parallele Verarbeitung durchzuführen. Sie verteilt Puffer mit eingehenden Zeilen gleichmäßig auf Ausgaben für separate Threads. Indem für jeden Ausgabepfad separate Threads verwendet werden, verbessert die BDD-Komponente die Leistung eines SSIS-Pakets auf Mehrkern- oder Mehrprozessorcomputern.  
  
 Das folgende Diagramm zeigt ein einfaches Verwendungsbeispiel für die BDD-Transformation. In diesem Beispiel wählt die BDD-Transformation jeweils einen Pipelinepuffer aus den Eingabedaten einer Flatfilequelle aus und sendet ihn nach dem Roundrobin-Prinzip an einen der drei Ausgabepfade. In den SQL Server Data Tools können Sie die Werte von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.DefaultBufferSize%2A>(Standardgröße des Pipelinepuffers) und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.DefaultBufferMaxRows%2A>(maximale Zeilenanzahl in einem Pipelinepuffer) im Fenster **Eigenschaften** überprüfen, das die Eigenschaften eines Datenflusstasks enthält.  
  
 ![Balanced Data Distributor](../../../integration-services/data-flow/transformations/media/balanceddatadistributor.JPG "Balanced Data Distributor")  
  
 Die BDD (Balanced Data Distributor)-Transformation verbessert die Paketausführungsleistung in einem Szenario, das die folgenden Bedingungen erfüllt:  
  
1.  Eine große Datenmenge wird an die BDD-Transformation übermittelt. Bei einer geringen Datenmenge und nur einem Puffer, der die Daten aufnehmen kann, ergibt die Verwendung der BDD-Transformation keinen Sinn. Bei einer großen Datenmenge und mehreren Puffern zum Speichern der Daten kann BDD die Pufferdaten jedoch effizient parallel in separaten Threads verarbeiten.  
  
2.  Die Daten werden schneller gelesen, als sie im weiteren Datenfluss verarbeitet werden. In diesem Szenario werden die für die Daten ausgeführten Transformationen im Vergleich zur Eingangsgeschwindigkeit der Daten langsam ausgeführt. Wenn sich der Engpass am Ziel befindet, muss das Ziel eine ausreichende Parallelität aufweisen.  
  
3.  Die Daten müssen keine bestimmte Reihenfolge einhalten. Wenn Sie jedoch eine bestimmte Reihenfolge beibehalten müssen, sollten Sie die Daten nicht mithilfe der BDD-Transformation unterteilen.  
  
 Wenn der Engpass bei einem SSIS-Paket auf die Geschwindigkeit zurückzuführen ist, mit der Daten aus der Quelle gelesen werden können, kann die Leistung mittels der BDD-Komponente nicht verbessert werden. Liegt der Engpass des SSIS-Pakets daran, dass das Ziel keine Parallelität unterstützt, ist BDD ebenfalls nicht die richtige Lösung. In einem passenden Szenario können Sie mit BDD jedoch alle Transformationen parallel ausführen und die Ausgabedaten, die aus den verschiedenen Ausgabepfaden der BDD-Transformation stammen, mithilfe der UNION ALL-Transformation kombinieren, bevor Sie die Daten an das Ziel senden.  
  
> [!IMPORTANT]  
>   Ein Verwendungsbeispiel dieser Transformation finden Sie im [BDD (Balanced Data Distributor)-Video](/previous-versions/dn912438(v=msdn.10)) in der TechNet Library.  
  
