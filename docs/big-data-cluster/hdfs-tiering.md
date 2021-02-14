---
title: Konfigurieren von HDFS-Tiering
titleSuffix: SQL Server big data clusters
description: In diesem Artikel wird beschrieben, wie Sie HDFS-Tiering konfigurieren, um ein externes Azure Data Lake Storage-Dateisystem auf einem SQL Server 2019-Big Data-Cluster in HDFS einzubinden.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c95bb10dbd063c56029a01b5a828f80eb5ba4d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046688"
---
# <a name="configure-hdfs-tiering-on-big-data-clusters-2019"></a>Konfigurieren von HDFS-Tiering in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

HDFS-Tiering bietet die Möglichkeit, ein externes, HDFS-kompatibles Dateisystem in HDFS einzubinden. In diesem Artikel wird erläutert, wie Sie HDFS-Tiering für SQL Server-Big Data-Cluster konfigurieren. Zurzeit unterstützen wir das Herstellen einer Verbindung mit Azure Data Lake Storage Gen2 und Amazon S3. 

## <a name="hdfs-tiering-overview"></a>Übersicht zum HDFS-Tiering

Beim Tiering können Anwendungen nahtlos auf Daten in einer Vielzahl externer Speicher zugreifen, als ob sich die Daten im lokalen HDFS befinden. Die Einbindung ist ein Metadatenvorgang, bei dem die Metadaten, die den Namespace im externen Dateisystem beschreiben, in Ihr lokales HDFS kopiert werden. Diese Metadaten enthalten Informationen zu den externen Verzeichnissen und Dateien zusammen mit ihren Berechtigungen und ACLs. Die entsprechenden Daten werden nur bei Bedarf kopiert, wenn auf die Daten selbst z.B. über eine Abfrage zugegriffen wird. Der Zugriff auf die externen Dateisystemdaten ist jetzt über den SQL Server-Big Data-Cluster möglich. Sie können Spark-Aufträge und SQL-Abfragen für diese Daten auf die gleiche Weise ausführen, wie Sie sie für allen lokalen Daten ausführen, die in HDFS im Cluster gespeichert sind.

Dieses 7-minütige Video enthält eine Übersicht über HDFS-Tiering:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Unify-your-data-lakes-with-HDFS-tiering/player?WT.mc_id=dataexposed-c9-niner]


### <a name="caching"></a>Caching
Heutzutage wird standardmäßig 1% des gesamten HDFS-Speichers für das Zwischenspeichern der eingebundenen Daten reserviert. Das Zwischenspeichern ist eine Einbindungen übergreifende globale Einstellung.

> [!NOTE]
> HDFS-Tiering ist ein von Microsoft entwickeltes Feature, und eine frühere Version davon wurde als Teil der Apache Hadoop 3.1-Distribution veröffentlicht. Weitere Informationen finden Sie unter [https://issues.apache.org/jira/browse/HDFS-9806](https://issues.apache.org/jira/browse/HDFS-9806).

Die folgenden Abschnitte zeigen ein Beispiel für die Konfiguration von HDFS-Tiering mit einer Azure Data Lake Storage Gen2-Datenquelle.

## <a name="refresh"></a>Aktualisieren

Das HDFS-Tiering unterstützt die Aktualisierung. Aktualisieren Sie eine vorhandene Einbindung für die aktuelle Momentaufnahme der Remotedaten.

## <a name="prerequisites"></a>Voraussetzungen

- [Bereitgestellte Big Data-Cluster](deployment-guidance.md)
- [Big-Data-Tools](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="mounting-instructions"></a>Einbindungsanweisungen

Wir unterstützen das Herstellen einer Verbindung mit Azure Data Lake Storage Gen2 und Amazon S3. Anweisungen zum Einbinden dieser Speichertypen finden Sie in den folgenden Artikeln:

- [Vorgehensweise: Einbinden von ADLS Gen2 für HDFS-Tiering in einen Big Data-Cluster](hdfs-tiering-mount-adlsgen2.md)
- [Vorgehensweise: Einbinden von S3 für HDFS-Tiering in einen Big Data-Cluster](hdfs-tiering-mount-s3.md)

## <a name="known-issues-and-limitations"></a><a id="issues"></a> Einschränkungen und bekannte Probleme

Die folgende Liste enthält bekannte Probleme und aktuelle Einschränkungen bei der Verwendung von HDFS-Tiering in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]:

- Wenn die Einbindung für einen längeren Zeitraum in einem `CREATING`-Zustand bleibt, ist wahrscheinlich ein Fehler aufgetreten. Brechen Sie in diesem Fall den Befehl ab, und löschen Sie die Einbindung, falls erforderlich. Vergewissern Sie sich vor einem erneuten Versuch, dass die Parameter und Anmeldeinformationen richtig sind.

- Einbindungen können nicht in vorhandenen Verzeichnissen erstellt werden.

- Einbindungen können nicht in vorhandenen Einbindungen erstellt werden.

- Wenn keiner der Vorgänger des Bereitstellungspunkts vorhanden ist, wird er mit den standardmäßigen Berechtigungen r-xr-xr-x (555) erstellt.

- Die Erstellung der Einbindung kann je nach Anzahl und Größe der einzubindenden Dateien einige Zeit in Anspruch nehmen. Während dieses Vorgangs sind die Dateien unter der Einbindung für die Benutzer nicht sichtbar. Während die Einbindung erstellt wird, werden alle Dateien einem temporären Pfad hinzugefügt, der standardmäßig `/_temporary/_mounts/<mount-location>` lautet.

- Der Befehl zum Erstellen der Einbindung ist asynchron. Nachdem der Befehl ausgeführt wurde, kann der Einbindungsstatus geprüft werden, um den Status der Einbindung zu verstehen.

- Beim Erstellen der Einbindung ist das für **--mount-path** verwendete Argument im Grunde ein eindeutiger Bezeichner für die Einbindung. Die gleiche Zeichenfolge (einschließlich „/“ am Ende, falls vorhanden) muss in nachfolgenden Befehlen verwendet werden.

- Die Einbindungen sind schreibgeschützt. Es ist nicht möglich, Verzeichnisse oder Dateien unter einer Einbindung zu erstellen.

- Es wird nicht empfohlen, Verzeichnisse und Dateien einzubinden, die sich ändern können. Nach dem Erstellen der Einbindung werden Änderungen oder Aktualisierungen des Remotespeicherorts nicht mehr in der Einbindung im HDFS berücksichtigt. Wenn am Remotespeicherort Änderungen vorgenommen werden, können Sie wählen, die Einbindung zu löschen und neu zu erstellen, um den aktualisierten Zustand widerzuspiegeln.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
