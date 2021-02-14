---
title: Was ist die Masterinstanz?
titleSuffix: SQL Server big data clusters
description: In diesem Artikel wird die SQL Server-Masterinstanz in einem Big Data-Cluster für SQL Server 2019 beschrieben.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd97d5b0da5d88974dcd56aa0f32cbc3243532f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052102"
---
# <a name="what-is-the-master-instance-in-a-sql-server-big-data-cluster"></a>Was ist die Masterinstanz in einem SQL Server-Big Data Cluster?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird die Rolle der *SQL Server-Masterinstanz* in einem SQL Server-Big Data-Cluster beschrieben. Die Masterinstanz ist eine SQL Server-Instanz, die in einem SQL Server-Big Data-Cluster ausgeführt wird, um Konnektivität, Scale-Out-Abfragen, Metadaten- und Benutzerdatenbanken sowie Machine Learning-Dienste zu verwalten.

Die SQL Server-Masterinstanz bietet folgende Funktionen:

## <a name="connectivity"></a>Konnektivität

Die SQL Server-Masterinstanz stellt einen extern erreichbaren TDS-Endpunkt für den Cluster bereit. Sie können Anwendungen oder SQL Server-Tools wie Azure Data Studio oder SQL Server Management Studio mit diesem Endpunkt verbinden, genauso wie mit jeder anderen SQL Server-Instanz.

## <a name="scale-out-query-management"></a>Horizontale Skalierung der Abfrageverwaltung

Die SQL Server-Masterinstanz enthält die Abfrage-Engine für die horizontale Skalierung, mit der Abfragen auf SQL Server-Instanzen auf Knoten im [Computepool](concept-compute-pool.md) verteilt werden. Die Abfrage-Engine für die horizontale Skalierung bietet über Transact-SQL auch Zugriff auf alle Hive-Tabellen im Cluster, ohne dass zusätzliche Konfiguration erforderlich ist.

## <a name="metadata-and-user-databases"></a>Metadaten und Benutzerdatenbanken

Zusätzlich zu den standardmäßigen SQL Server-Systemdatenbanken enthält die SQL Server-Masterinstanz Folgendes:

- Eine Metadaten-Datenbank für HDFS-Tabellenmetadaten
- Eine Zuordnung der Datenebenenshards
- Details aus externen Tabellen, die Zugriff auf die Clusterdatenebene bieten.
- In Benutzerdatenbanken definierte externe PolyBase-Datenquellen und externe Tabellen.

Sie können der SQL Server-Masterdatenbank auch Ihre eigenen Benutzerdatenbanken hinzufügen.

## <a name="machine-learning-services"></a>Dienste für maschinelles Lernen

Die SQL Server-Dienste für maschinelles Lernen stehen als Add-On für die Datenbank-Engine zur Verfügung und werden für die Ausführung von Java-, R- und Python-Code in SQL Server verwendet. Dieses Feature basiert auf dem SQL Server-Erweiterbarkeitsframework, das externe Prozesse von den Engine-Kernprozessen isoliert, aber in Form von gespeicherten Prozeduren, T-SQL-Skript mit R- oder Python-Anweisungen oder Java-, R- oder Python-Code mit T-SQL vollständig in die relationalen Daten integriert ist.

Als Teil eines SQL Server-Big Data-Clusters sind die Dienste für maschinelles Lernen standardmäßig in der SQL Server-Masterinstanz verfügbar. Das bedeutet, dass Sie mit „sp_execute_external_script“ Java-, R- und Python-Skripts ausführen können, sobald die externe Skriptausführung in der SQL Server-Masterinstanz aktiviert ist.

### <a name="advantages-of-machine-learning-services-in-a-big-data-cluster"></a>Vorteile der Dienste für maschinelles Lernen in einem Big Data-Cluster

Über [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] lassen sich Big Data ganz einfach mit den Dimensionsdaten verknüpfen, die typischerweise in einer Unternehmensdatenbank gespeichert sind. Der Wert der Big Data vervielfacht sich, wenn diese nicht nur einem Teil der Organisation zur Verfügung stehen, sondern auch in Berichte, Dashboards und Anwendungen eingebunden werden können. Gleichzeitig können Data Scientists weiter die Tools des Spark/HDFS-Ökosystems nutzen und von einfachem Echtzeitzugriff auf die Daten in der SQL Server-Masterinstanz und in den externe Datenquellen profitieren, auf die _über_ die Masterinstanz zugegriffen werden kann.

Mit [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] können Sie mit den Data Lakes Ihres Unternehmens mehr erreichen. SQL Server-Entwickler und Analysten stehen folgende Möglichkeiten offen:

* Sie können Anwendungen erstellen, die Daten aus den Data Lakes des Unternehmens verwenden.
* Sie können mithilfe von Transact-SQL-Abfragen Schlussfolgerungen aus allen Daten ziehen.
* Sie können das vorhandene Ökosystem mit SQL Server-Tools und -Anwendungen verwenden, um auf Unternehmensdaten zuzugreifen und diese zu analysieren.
* Sie können mithilfe von Datenvirtualisierung und Data Marts die Anzahl von Datenverschiebungen reduzieren.
* Sie können weiterhin Spark für Big Data-Szenarien verwenden.
* Sie können mit Spark oder SQL Server intelligente Unternehmensanwendungen erstellen, um Modelle mit Data Lakes zu trainieren.
* Sie können Modelle in Produktionsdatenbanken operationalisieren, um eine optimale Leistung zu erzielen.
* Sie können Daten direkt in Unternehmens-Data Marts streamen, um Echtzeitanalysen durchzuführen.
* Sie können Daten mithilfe von interaktiven Analyse- und BI-Tools visuell erkunden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie in den folgenden Ressourcen:

- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]-Architektur](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
