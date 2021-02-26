---
title: Verwalten von Big Data-Clustern (BDC) mit Jupyter-Notebooks und Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Verwalten von Big Data-Clustern (BDC) mit Jupyter-Notebooks und Azure Data Studio in einem SQL Server 2019 Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3af391b2fe6a2c104a4854afdb57b4dc5eb95029
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489174"
---
# <a name="manage-big-data-clusters-bdc-the-cluster-with-notebooks"></a>Verwalten von Big Data-Clustern (BDC) mit Notebooks

Diese Seite ist ein Index der Notebooks für SQL Server Big Data-Cluster. Dieses ausführbaren Notebooks (IPYNB) sind für SQL Server 2019 entwickelt, um bei der Verwaltung von Big Data-Clustern zu helfen.

Jedes Notebook überprüft seine eigenen Abhängigkeiten. Die Ausführung von **run all cells** (Alle Zellen ausführen) wird entweder erfolgreich abgeschlossen oder löst eine Ausnahme aus, die einen Hinweis mit einem Link zu einem anderen Notebook enthält, um die fehlende Abhängigkeit aufzulösen. Folgen Sie dem Link im Hinweis zu dem folgenden Notebook, klicken Sie auf **run all cells**, und wenn die Aktion erfolgreich ist, kehren Sie zum ursprünglichen Notebook zurück, und führen Sie **run all cells** aus.

Wenn alle Abhängigkeiten installiert wurden, **run all cells** aber fehlschlägt, analysiert jedes Notebook die Ergebnisse und erzeugt, wo möglich, einen Hinweis mit Link zu einem weiteren Notebook, um bei der weitergehenden Behandlung des Problems zu helfen.


## <a name="installing-and-uninstalling-utilities-on-big-data-cluster-bdc"></a>Installieren und Deinstallieren von Hilfsprogrammen in Big Data-Cluster (BDC)

Dieser Abschnitt umfasst eine Gruppe von Notebooks zum Installieren und Deinstallieren von Befehlszeilentools und Paketen, die zum Verwalten von Big-Data-Clustern für SQL Server benötigt werden.

|Name |Beschreibung |
|---|---|
|SOP010: Upgrade eines Big-Data-Clusters|Verwenden Sie dieses Notebook für ein Upgrade eines Big Data-Clusters mithilfe von azdata. |
|SOP012: unixodbc für Mac installieren|Verwenden Sie dieses Notebook, wenn beim Verwenden von brew Fehler auftreten, um die odbc für SQL Server zu installieren.|
|SOP036: kubectl-CLI installieren|Verwenden Sie dieses Notebook, um die kubectl-Befehlszeilenschnittstelle unabhängig von Ihrem Betriebssystem zu installieren.|
|SOP037: kubectl-CLI deinstallieren|Verwenden Sie dieses Notebook, um die kubectl-Befehlszeilenschnittstelle unabhängig von Ihrem Betriebssystem zu deinstallieren.|
|SOP038: Azure CLI installieren|Verwenden Sie dieses Notebook, um die Azure CLI unabhängig von Ihrem Betriebssystem zu installieren.|
|SOP039: Azure CLI deinstallieren|Verwenden Sie dieses Notebook, um die Azure CLI unabhängig von Ihrem Betriebssystem zu deinstallieren.|
|SOP040 – pip-Upgrade in der ADS-Python-Sandbox ausführen|Verwenden Sie dieses Notebook für ein Upgrade von pip in der ADS-Python-Sandbox.|
|SOP054: azdata-CLI installieren|Verwenden Sie dieses Notebook, um die azdata-Befehlszeilenschnittstelle unabhängig von Ihrem Betriebssystem zu installieren.|
|SOP055: azdata-CLI deinstallieren|Verwenden Sie dieses Notebook, um die azdata-Befehlszeilenschnittstelle unabhängig von Ihrem Betriebssystem zu deinstallieren.|
|SOP059 – Kubernetes-Python-Modul installieren|Verwenden Sie dieses Notebook, um Kubernetes-Module mit Python zu installieren.|
|SOP060 – Kubernetes-Modul deinstallieren|Verwenden Sie dieses Notebook, um Kubernetes-Module mit Python zu deinstallieren.|
|SOP061: Löschen eines Big-Data-Clusters|Verwenden Sie dieses Notebook zum Löschen eines BDC-Clusters.|
|SOP062: Installieren der Module ipython-sql und pyodbc|Verwenden Sie dieses Notebook zum Installieren der Module ipython-sql und pyodbc.|
|SOP063: azdata-CLI installieren (mithilfe des Paket-Managers)|Verwenden Sie dieses Notebook zum Installieren der azdata-CLI (mithilfe des Paket-Managers).|
|SOP064: azdata-CLI deinstallieren (mithilfe des Paket-Managers)|Verwenden Sie dieses Notebook zum Deinstallieren der azdata-CLI (mithilfe des Paket-Managers).|
|SOP069: ODBC für SQL Server installieren|Verwenden Sie dieses Notebook, um den ODBC-Treiber zu installieren, da einige Unterbefehle in azdata den SQL Server ODBC-Treiber benötigen.|

## <a name="encryption-at-rest-utilities-on-big-data-cluster-bdc"></a>Hilfsprogramme zur Verschlüsselung ruhender Daten im Big Data-Cluster (BDC)

In diesem Abschnitt werden verschiedene Notebooks zum Verwalten von Features zur Verschlüsselung ruhender Daten im BDC beschrieben.

|Name |Beschreibung |
|---|---|
|SOP0124: Auflisten von Schlüsseln für die Verschlüsselung ruhender Daten|Verwenden Sie dieses Notebook, um alle HDFS-Schlüssel aufzulisten.|
|SOP0128: Aktivieren von HDFS-Verschlüsselungszonen in Big Data-Clustern|Verwenden Sie dieses Notebook, um HDFS-Verschlüsselungszonen beim Upgrade von CU6 oder früheren Versionen auf CU8 zu aktivieren. Nicht erforderlich bei neuen Bereitstellungen von CU8 und höher oder beim Upgrade auf CU9.|
|SOP0125: Löschen des Schlüssels für die Verschlüsselung ruhender Daten|Verwenden Sie dieses Notebook, um die Schlüssel der HDFS-Verschlüsselungszone zu löschen. __Vorsicht!__|
|SOP0126: Sichern Sie die Schlüssel für die Verschlüsselung ruhender Daten|Verwenden Sie dieses Notebook, um die Schlüssel der HDFS-Verschlüsselungszone zu sichern.|
|SOP0127: Wiederherstellen der Schlüssel für die Verschlüsselung ruhender Daten|Verwenden Sie dieses Notebook, um die Schlüssel der HDFS-Verschlüsselungszone wiederherzustellen.|

## <a name="managing-certificates-on-big-data-clusters-bdc"></a>Verwalten von Zertifikaten in Big Data-Clustern (BDC)

Eine Gruppe von Notebooks zum Ausführen eines Notebooks für die Verwaltung von Zertifikaten in Big Data-Clustern.

|Name |Beschreibung |
|---|---|
|CER001: Ein Zertifikat der Stammzertifizierungsstelle generieren|Generieren Sie ein Zertifikat der Stammzertifizierungsstelle. Erwägen Sie, ein einziges Zertifikat der Stammzertifizierungsstelle für alle Nicht-Produktionscluster in jeder Umgebung zu verwenden, da durch diese Methode die Anzahl von Stammzertifikaten der Zertifizierungsstelle verringert wird, die auf Clients hochgeladen werden müssen, die eine Verbindung mit diesen Clustern herstellen. |
|CER002: Vorhandenes Zertifikat der Stammzertifizierungsstelle herunterladen|Verwenden Sie dieses Notebook, um ein generiertes Zertifikat der Stammzertifizierungsstelle von einem Cluster herunterzuladen.|
|CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen|CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen.|
|CER004: Vorhandenes Zertifikat der Stammzertifizierungsstelle herunter- und hochladen|Ein vorhandenes Zertifikat der Stammzertifizierungsstelle herunter- und hochladen. |
|CER010: Generiertes Zertifikat der Stammzertifizierungsstelle lokal installieren|Dieses Notebook kopiert das generierte Zertifikat der Stammzertifizierungsstelle (aus einem Big Data-Cluster), das entweder mit **CER001: Ein Zertifikat der Stammzertifizierungsstelle generiert**  oder mit **CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen** installiert wurde, lokal, und installiert anschließend das Zertifikat der Stammzertifizierungsstelle im lokalen Zertifikatspeicher dieses Computers.|
|CER020: Verwaltungsproxyzertifikat erstellen|Mit diesem Notebook wird ein Zertifikat für den Verwaltungsproxy-Endpunkt erstellt.|
|CER021: Knox-Zertifikat erstellen|Mit diesem Notebook wird ein Zertifikat für den Knox-Gatewayendpunkt erstellt.|
|CER022: App- Proxyzertifikat erstellen|Mit diesem Notebook wird ein Zertifikat für den App-Bereitstellungsproxy-Endpunkt erstellt.|
|CER023: Masterzertifikat erstellen|Mit diesem Notebook wird ein Zertifikat für den Masterendpunkt erstellt.|
|CER030: Verwaltungsproxyzertifikat mit generierter Zertifizierungsstelle signieren|Mit diesem Notebook wird das Zertifikat, das mithilfe von **CER020: Verwaltungsproxyzertifikat erstellen** erstellt wurde, mit dem generierten Zertifikat der Stammzertifizierungsstelle generiert, signiert, das entweder mit **CER001: Ein Zertifikat der Stammzertifizierungsstelle generieren** oder mit **CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen** erstellt wurde.|
|CER031: Knox-Zertifikat mit generierter Zertifizierungsstelle signieren|Mit diesem Notebook wird das Zertifikat, das mithilfe von **CER021: Knox-Zertifikat erstellen** erstellt wurde, mit dem generierten Zertifikat der Stammzertifizierungsstelle generiert, signiert, das entweder mit **CER001: Ein Zertifikat der Stammzertifizierungsstelle generieren** oder mit **CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen** erstellt wurde.|
|CER032: App-Proxyzertifikat mit generierter Zertifizierungsstelle signieren|Mit diesem Notebook wird das Zertifikat, das mithilfe von **CER022: App-Proxyzertifikat erstellen** erstellt wurde, mit dem generierten Zertifikat der Stammzertifizierungsstelle generiert, signiert, das entweder mit **CER001: Ein Zertifikat der Stammzertifizierungsstelle generieren** oder mit **CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen** erstellt wurde.|
|CER033: Masterzertifikat mit generierter Zertifizierungsstelle signieren|Mit diesem Notebook wird das Zertifikat, das mithilfe von **CER023: Masterzertifikat erstellen** erstellt wurde, mit dem generierten Zertifikat der Stammzertifizierungsstelle generiert, signiert, das entweder mit **CER001: Ein Zertifikat der Stammzertifizierungsstelle generieren** oder mit **CER003: Vorhandenes Zertifikat der Stammzertifizierungsstelle hochladen** erstellt wurde.|
|CER040: Signiertes Verwaltungsproxyzertifikat installieren|Mit diesem Notebook wird das Zertifikat im Big Data-Cluster installiert, das mit **CER030: Verwaltungsproxyzertifikat mit generierter Zertifizierungsstelle signieren** signiert wurde.|
|CER041: Signiertes Knox-Zertifikat installieren|Mit diesem Notebook wird das Zertifikat im Big Data-Cluster installiert, das mit **CER031: Knox-Zertifikat mit generierter Zertifizierungsstelle signieren** signiert wurde.|
|CER042: Signiertes App-Proxyzertifikat installieren|Mit diesem Notebook wird das Zertifikat im Big Data-Cluster installiert, das mit **CER032: App-Proxyzertifikat mit generierter Zertifizierungsstelle signieren** signiert wurde.|
|CER043: Signiertes Controllerzertifikat installieren|Mit diesem Notebook wird das Zertifikat im Big Data-Cluster installiert, das mit **CER034: Controllerzertifikat mit Cluster-Stammzertifizierungsstelle signieren** signiert wurde. Beachten Sie ferner, dass am Ende dieses Notebooks der Controllerpod sowie alle Pods, die PolyBase (Masterpool- und Computepoolpods) verwenden, neu gestartet werden, um die neuen Zertifikate zu laden.|
|CER050: Auf Fehlerfreiheit von BDC warten|Dieses Notebook wartet, bis sich der Big Data-Cluster wieder in einem fehlerfreien Zustand befindet, nachdem der Controllerpod und die Pods, die PolyBase verwenden, neu gestartet wurden, um die neuen Zertifikate zu laden.|
|CER100: Cluster mit selbstsignierten Zertifikaten konfigurieren|Dieses Notebook generiert eine neue Stammzertifizierungsstelle im Big Data-Cluster, und erstellt neue Zertifikate für jeden Endpunkt (diese Endpunkte sind: Verwaltung, Gateway, App-Proxy und Controller). Signieren Sie jedes neue Zertifikat mit der neuen generierten Stammzertifizierungsstelle, mit Ausnahme des Controllerzertifikats (das mit der vorhandenen Cluster-Stammzertifizierungsstelle signiert ist), und installieren Sie dann jedes Zertifikate im Big Data-Cluster. Laden Sie die neue generierte Stammzertifizierungsstelle in den Zertifikatspeicher „Vertrauenswürdige Stammzertifizierungsstellen“ herunter. Alle generierten selbstsignierten Zertifikate werden im Controllerpod am Speicherort „test_cert_store_root“ gespeichert.|
|CER101: Cluster mit selbstsignierten Zertifikaten mithilfe der vorhandenen Stammzertifizierungsstelle konfigurieren|In diesem Notebook wird eine vorhandene generierte Stammzertifizierungsstelle im Big Data-Cluster verwendet (hochgeladen mit **CER003**), und es werden für jeden Endpunkt (Verwaltung, Gateway, App-Proxy und Controller) neue Zertifikate erstellt. Anschließend wird jedes neue Zertifikat mit der neuen generierten Stammzertifizierungsstelle signiert, mit Ausnahme des Controllerzertifikats (das mit der vorhandenen Cluster-Stammzertifizierungsstelle signiert ist), und jedes Zertifikat wird im Big Data-Cluster installiert. Alle generierten selbstsignierten Zertifikate werden im Controllerpod (am Speicherort „test_cert_store_root“) gespeichert. Nach Abschluss dieses Notebooks wird der gesamte „https://“-Zugriff auf den Big Data-Cluster von diesem Computer aus (und von jedem Computer, auf dem die neue Stammzertifizierungsstelle installiert wird) als sicher angezeigt. Im Kapitel „Notebook-Runner“ wird sichergestellt, dass CronJobs, die zur Ausführung von App-Deploy erstellt wurden (OPR003), die Cluster-Stammzertifizierungsstelle installieren, damit ein sicheres Abrufen von JWT-Token und der Datei „swagger.json“ ermöglicht wird.|

## <a name="backup-and-restore-from-big-data-cluster-bdc"></a>Sichern und Wiederherstellen von Big Data-Clustern (BDC)

Dieser Abschnitt enthält eine Gruppe von Notebooks, die für Sicherungs- und Wiederherstellungsvorgänge für SQL Server Big Data-Cluster nützlich sind.

| Name | Beschreibung |
|--|--|
| SOP008: HDFS-Dateien in Azure Data Lake Store Gen2 mit distcp sichern | Dieses Standardbetriebsverfahren (Standard Operating Procedure, SOP) sichert Daten aus dem HDFS-Quelldateisystem des SQL Server 2019 BDC-Clusters im Azure Data Lake Store Gen2-Konto, das Sie angeben. Stellen Sie sicher, dass das Azure Data Lake Store Gen2-Konto mit aktiviertem „hierarchischem Namespace“ konfiguriert ist. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
