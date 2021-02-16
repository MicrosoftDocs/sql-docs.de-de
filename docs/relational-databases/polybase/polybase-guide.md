---
title: Was ist PolyBase? | Microsoft-Dokumentation
description: Mit PolyBase kann Ihre SQL Server-Instanz Transact-SQL-Abfragen verarbeiten, die Daten aus externen Datenquellen wie Hadoop und Azure Blob Storage lesen.
ms.date: 12/14/2019
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
ms.custom: contperf-fy21q2
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: e4f0bfbac2fe4e49261f9940a97fd73e05676661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062225"
---
# <a name="what-is-polybase"></a>Was ist PolyBase?

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

PolyBase ermöglicht Ihrer SQL Server-Instanz das Verarbeiten von Transact-SQL-Abfragen, die Daten aus externen Datenquellen lesen. Die gleiche Abfrage kann auch auf relationale Tabellen in Ihrer Instanz von SQL Server zugreifen. Mit PolyBase kann dieselbe Abfrage auch die Daten aus externen Quellen und SQL Server verknüpfen.

Gehen Sie in einer Instanz von SQL Server wie folgt vor, um PolyBase zu verwenden:

1. [Installieren von PolyBase unter Windows](polybase-installation.md)
1. Erstellen einer [externen Datenquelle](../../t-sql/statements/create-external-data-source-transact-sql.md)
1. Erstellen einer [externen Tabelle](../../t-sql/statements/create-external-table-transact-sql.md)

Diese stellen gemeinsam die Verbindung zur externen Datenquelle her.

SQL Server 2016 führt PolyBase mit Unterstützung für Verbindungen mit Hadoop und Azure Blob Storage ein.

Mit SQL Server 2019 werden zusätzliche Connectors eingeführt, einschließlich SQL Server, Oracle, Teradata und MongoDB.

![PolyBase-Logik](../../relational-databases/polybase/media/polybase-logical.png "PolyBase-Logik")

PolyBase überträgt einige Berechnungen per Push an die externe Quelle, um die Abfrage insgesamt zu optimieren. Der externe PolyBase-Zugriff ist nicht auf Hadoop beschränkt. Andere unstrukturierte nicht relationale Tabellen werden ebenfalls unterstützt, z.B. durch Trennzeichen getrennte Textdateien.

Beispiele für externe Connectors:

- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)

### <a name="supported-sql-products-and-services"></a>Unterstützte SQL-Produkte und -Dienste

PolyBase bietet dieselben Funktionen für die folgenden SQL-Produkte von Microsoft:

- SQL Server 2016 und höhere Versionen (nur unter Windows)
- Analytics Platform System (ehemals Parallel Data Warehouse)
- Azure Synapse Analytics

### <a name="azure-integration"></a>Azure-Integration

Durch die zugrunde liegende Unterstützung von PolyBase können T-SQL-Abfragen außerdem Daten aus Azure Blob Storage importieren und exportieren. PolyBase ermöglicht Azure Synapse Analytics zudem das Importieren und Exportieren von Daten aus Azure Data Lake Store und aus Azure Blob Storage.

## <a name="why-use-polybase"></a>Gründe für die Verwendung von PolyBase

Mit PolyBase können Sie Daten aus einer SQL Server-Instanz mit externen Daten verbinden. Vor PolyBase konnten Sie zum Verknüpfen von Daten mit externen Datenquellen wie folgt vorgehen:

- Übertragen der Hälfte Ihrer Daten, damit sich alle Daten an einem Speicherort befinden.
- Abfragen beider Datenquellen und Schreiben benutzerdefinierter Abfragelogik zum Verknüpfen und Integrieren der Daten auf Clientebene.

Mit PolyBase können Sie einfach Transact-SQL verwenden, um die Daten zu verknüpfen.

Mit PolyBase müssen Sie keine zusätzliche Software in Ihrer Hadoop-Umgebung installieren. Sie fragen externe Daten anhand der gleichen T-SQL-Syntax ab, die auch zum Abfragen einer Datenbanktabelle verwendet wird. Alle von PolyBase implementierten Unterstützungsaktionen werden transparent durchgeführt. Der Abfrageautor benötigt keine Kenntnisse über die externe Quelle.

### <a name="polybase-uses"></a>Verwendungszwecke von PolyBase

PolyBase ermöglicht die folgenden Szenarios in SQL Server:

- **Abfragen von in Hadoop gespeicherten Daten aus einer SQL Server-Instanz oder einem PDW.** Benutzer speichern Daten in kostengünstigen verteilten und skalierbaren Systemen, wie z.B. Hadoop. PolyBase vereinfacht die Abfrage der Daten mithilfe von T-SQL.

- **Abfragen von in Azure Blob Storage gespeicherten Daten.** Ein Azure-Blobspeicher ist eine bequeme Möglichkeit, Daten zu speichern, die von Azure-Diensten verwendet werden.  PolyBase vereinfacht den Zugriff auf die Daten mithilfe von T-SQL.

- **Importieren von Daten aus Hadoop, Azure Blob Storage oder Azure Data Lake Store.** Profitieren Sie von der Geschwindigkeit der Columnstore-Technologie und den Analysefunktionen von Microsoft SQL, indem Sie Daten aus Hadoop, Azure Blob Storage oder Azure Data Lake Store in relationale Tabellen importieren. Sie benötigen keine separaten ETL-Funktionen und kein Importtool.

- **Exportieren von Daten in Hadoop, Azure-Blobspeicher oder Azure Data Lake Store.** Archivieren Sie Daten in Hadoop, Azure-Blobspeichern oder Azure Data Lake Store, um kostengünstigen Speicherplatz zu nutzen und Daten für einfachen Zugriff online zu halten.

- **Integrieren in BI-Tools.** Verwenden Sie PolyBase zusammen mit den Business Intelligence- und Analysfunktionen von Microsoft, oder verwenden Sie beliebige Drittanbietertools, die mit SQL Server kompatibel sind.

## <a name="performance"></a>Leistung

- **Übertragen von Berechnungen an Hadoop.** Der Abfrageoptimierer trifft eine kostenbasierte Entscheidung darüber, ob die Berechnung an Hadoop übertragen wird, wenn die Abfrageleistung dadurch verbessert wird.  Für diese kostenbasierte Entscheidung verwendet der Abfrageoptimierer Statistiken in externen Tabellen. Bei der Übertragung der Berechnung werden MapReduce-Aufträge erstellt und die verteilten Berechnungsressourcen von Hadoop genutzt.

- **Skalieren von Berechnungsressourcen.** Um die Abfrageleistung zu verbessern, können Sie [PolyBase-Erweiterungsgruppen](../../relational-databases/polybase/polybase-scale-out-groups.md)von SQL Server verwenden. Die ermöglicht eine parallele Datenübertragung zwischen SQL Server-Instanzen und Hadoop-Knoten und fügt Berechnungsressourcen für die Verarbeitung der externen Daten hinzu.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie PolyBase verwenden können, müssen Sie [das PolyBase-Feature installieren](polybase-installation.md). Befolgen Sie dann je nach Datenquelle die Anweisungen in einem der folgenden Konfigurationshandbücher:

- [Hadoop](polybase-configure-hadoop.md)
- [Azure Blob Storage](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Generische ODBC-Typen](polybase-configure-odbc-generic.md)