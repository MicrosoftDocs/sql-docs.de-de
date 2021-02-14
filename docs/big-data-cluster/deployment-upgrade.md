---
title: Upgrade auf ein neues Release
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie, wie Sie Big Data-Cluster für SQL Server auf ein neues Release aktualisieren.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/02/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 48edd5e76a87a866a2090329f144c5a7dc0934a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046880"
---
# <a name="how-to-upgrade-big-data-clusters-2019"></a>Upgraden von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Der Upgradepfad hängt von der aktuellen Version des Big Data-Clusters (BDC) für SQL Server ab. Für ein Upgrade von einem unterstützten Release, einschließlich allgemeine Vertriebsversion, kumulatives Update oder Quick Fix Engineering (QFE), kann ein direktes Upgrade durchgeführt werden. Ein direktes Upgrade von einer CTP- (Community Technology Preview) oder Release Candidate-Version des BDC wird nicht unterstützt. Sie müssen den Cluster entfernen und neu erstellen. In den folgenden Abschnitten werden die jeweils erforderlichen Schritte beschrieben:

- [Upgrade von einem unterstützten Release](#upgrade-from-supported-release)
- [Aktualisieren einer BDC-Bereitstellung von einer CTP- oder Release Candidate-Version](#update-a-bdc-deployment-from-ctp-or-release-candidate)

>[!NOTE]
>Das erste unterstützte Release von Big Data-Clustern ist SQL Server 2019 GDR1.

## <a name="upgrade-release-notes"></a>Upgrade-Versionshinweise

Bevor Sie fortfahren, sollten Sie die [Upgrade-Versionshinweise auf bekannte Probleme prüfen](release-notes-big-data-cluster.md#known-issues).

## <a name="upgrade-from-supported-release"></a>Upgrade von einem unterstützten Release

In diesem Abschnitt wird erläutert, wie ein BDC für SQL Server von einem unterstützten Release (ab SQL Server 2019 GDR1) auf ein neueres unterstütztes Release aktualisiert wird.

1. Prüfen Sie auf aktive Livy-Sitzungen.

   Vergewissern Sie sich, dass in Azure Data Studio keine aktiven Livy-Sitzungen oder Batchaufträge ausgeführt werden. Eine einfache Möglichkeit, dies zu bestätigen, ist entweder über den Befehl `curl` oder über einen Browser, um diese URLs anzufordern:

    - `<your-gateway-endpoint>/gateway/default/livy/v1/sessions`
    - `<your-gateway-endpoint>/gateway/default/livy/v1/batches`

1. Sichern Sie die SQL Server-Masterinstanz.

1. Sichern Sie das HDFS (Hadoop Distributed File System).

   ```
   azdata bdc hdfs cp --from-path <path> --to-path <path>
   ```
   
   Beispiel: 

   ```
   azdata bdc hdfs cp --from-path hdfs://user/hive/warehouse/%%D --to-path ./%%D
   ```

1. Aktualisieren Sie [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   Befolgen Sie die Anweisungen zur Installation von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 
   - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
   - [Linux mit apt](../azdata/install/deploy-install-azdata-linux-package.md)
   - [Linux mit yum](../azdata/install/deploy-install-azdata-yum.md)
   - [Linux mit zypper](../azdata/install/deploy-install-azdata-zypper.md)

   >[!NOTE]
   >Wenn [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] mit `pip` installiert wurde, müssen Sie es vor der Installation mit Windows Installer oder mit dem Linux-Paket-Manager manuell entfernen.

1. Aktualisieren Sie den Big Data-Cluster.

   ```
   azdata bdc upgrade -n <clusterName> -t <imageTag> -r <containerRegistry>/<containerRepository>
   ```

   Das folgende Skript verwendet beispielsweise das Imagetag `2019-CU6-ubuntu-16.04`:

   ```
   azdata bdc upgrade -n bdc -t 2019-CU6-ubuntu-16.04 -r mcr.microsoft.com/mssql/bdc
   ```

>[!NOTE]
>Die neuesten Imagetags finden Sie in den [Versionshinweisen zu Big Data-Clustern in SQL Server 2019](release-notes-big-data-cluster.md).

>[!IMPORTANT]
>Wenn Sie ein privates Repository verwenden, um die Images für die Bereitstellung oder das Upgrade eines BDC vorab abzurufen, stellen Sie sicher, dass sich die aktuellen Buildimages sowie die Zielbuildimages im privaten Repository befinden. Dadurch kann bei Bedarf ein Rollback durchgeführt werden. Wenn Sie die Anmeldeinformationen des privaten Repositorys seit der ursprünglichen Bereitstellung geändert haben, müssen Sie zudem die entsprechenden Umgebungsvariablen (DOCKER_PASSWORD und DOCKER_USERNAME) aktualisieren. Ein Upgrade unter Verwendung unterschiedlicher privater Repositorys für den aktuellen Build und den Zielbuild wird nicht unterstützt.

### <a name="increase-the-timeout-for-the-upgrade"></a>Erhöhen des Timeoutwerts für das Upgrade

Ein Timeout kann auftreten, wenn bestimmte Komponenten nicht in der zugewiesenen Zeit aktualisiert werden. Der folgende Code zeigt, wie der Fehler aussehen könnte:

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

Sie können die Timeouts für ein Upgrade erhöhen, indem Sie die Parameter **--controller-timeout** und **--component-timeout** nutzen, um höhere Werte festzulegen, wenn Sie das Upgrade initiieren. Diese Option ist nur ab SQL Server 2019 CU2 verfügbar. Beispiel:

   ```bash
   azdata bdc upgrade -t 2019-CU6-ubuntu-16.04 --controller-timeout=40 --component-timeout=40 --stability-threshold=3
   ```
**--controller-timeout** gibt die Wartezeit in Minuten an, bis der Controller oder die Controllerdatenbank das Upgrade abgeschlossen hat.
**--component-timeout** gibt die Zeitdauer an, in der jede nachfolgende Phase des Upgrades abgeschlossen sein muss.

Bearbeiten Sie in Versionen vor SQL Server 2019 CU2 die Konfigurationszuordnung für Upgrades, um die Timeoutwerte für Upgrades zu erhöhen. So bearbeiten Sie die Konfigurationszuordnung für Upgrades:

Führen Sie den folgenden Befehl aus:

   ```bash
   kubectl edit configmap controller-upgrade-configmap
   ```

Bearbeiten Sie die unten aufgeführten Felder:

   **controllerUpgradeTimeoutInMinutes** Gibt die Wartezeit in Minuten an, bis der Controller oder die Controllerdatenbank das Upgrade abgeschlossen hat. Der Standardwert ist 5. Aktualisieren Sie diesen Wert auf mindestens 20.
   **totalUpgradeTimeoutInMinutes**: Gibt die kombinierte Zeitdauer an, bis der Controller und die Controllerdatenbank das Upgrade abgeschlossen haben (Upgrade von Controller und Controllerdatenbank). Der Standardwert ist 10. Aktualisieren Sie diesen Wert auf mindestens 40.
   **componentUpgradeTimeoutInMinutes**: Gibt die Zeitdauer an, in der jede nachfolgende Phase des Upgrades abgeschlossen sein muss. Der Standardwert ist 30. Aktualisieren Sie diesen Wert auf 45.

Speichern Sie Ihre Änderungen, und schließen Sie die Anwendung.

## <a name="update-a-bdc-deployment-from-ctp-or-release-candidate"></a>Aktualisieren einer BDC-Bereitstellung von einer CTP- oder Release Candidate-Version

Ein direktes Upgrade von einem CTP- oder Release Candidate-Build von SQL Server-Big Data-Clustern wird nicht unterstützt. Im folgenden Abschnitt wird erläutert, wie der Cluster manuell entfernt und neu erstellt wird.

### <a name="backup-and-delete-the-old-cluster"></a>Sichern und Löschen des alten Clusters

Für Big Data-Cluster, die vor SQL Server 2019 GDR1 bereitgestellt wurden, ist kein direktes Upgrade möglich. Um ein Upgrade auf ein neues Release durchzuführen, muss der Cluster manuell entfernt und neu erstellt werden. Jedes Release verfügt über eine eindeutige Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], die mit der vorherigen Version nicht kompatibel ist. Auch wenn ein neueres Containerimage auf einem Cluster herunterladen wird, der mit einer älteren Version bereitgestellt wurde, ist das aktuelle Image möglicherweise nicht mit den älteren Images auf dem Cluster kompatibel. Das neuere Image wird abgerufen, wenn Sie das Imagetag `latest` in der Bereitstellungskonfigurationsdatei für die Containereinstellungen verwenden. Standardmäßig verfügt jedes Release über ein bestimmtes Imagetag, das der Releaseversion von SQL Server entspricht. Führen Sie die folgenden Schritte aus, um ein Upgrade auf das neueste Release durchzuführen:

1. Sichern Sie vor dem Löschen des alten Clusters die Daten auf der SQL Server-Masterinstanz und auf HDFS. Für die SQL Server-Masterinstanz können Sie [SQL Server-Sicherung und -Wiederherstellung](data-ingestion-restore-database.md) verwenden. Für HDFS [können Sie die Daten mit `curl` herauskopieren](data-ingestion-curl.md).

1. Löschen Sie den alten Cluster mit dem `azdata delete cluster`-Befehl.

   ```bash
    azdata bdc delete --name <old-cluster-name>
   ```

   > [!Important]
   > Verwenden Sie die Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], die Ihrem Cluster entspricht. Löschen Sie keinen älteren Cluster mit der neueren Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   > [!Note]
   > Wenn Sie einen `azdata bdc delete`-Befehl ausführen, werden alle innerhalb des Namespace erstellten Objekte mit dem Namen des Big Data-Clusters gelöscht, der Namespace jedoch nicht. Der Namespace kann für nachfolgende Bereitstellungen wiederverwendet werden, solange er leer ist und keine anderen Anwendungen in ihm erstellt wurden.

1. Deinstallieren Sie die alte Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   ```powershell
   pip3 uninstall -r https://azdatacli.blob.core.windows.net/python/azdata/2019-rc1/requirements.txt
   ```

1. Installieren Sie die neueste Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Mit den folgenden Befehlen wird [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] aus dem neuesten Release installiert:

   **Windows:**

   ```powershell
   pip3 install -r https://aka.ms/azdata
   ```

   **Linux:**

   ```bash
   pip3 install -r https://aka.ms/azdata --user
   ```

   > [!IMPORTANT]
   > Für jedes Release ändert sich der Pfad zur `n-1`-Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Auch wenn Sie zuvor [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] installiert haben, müssen Sie vor dem Erstellen des neuen Clusters vom aktuellen Pfad aus neu installieren.

### <a name="verify-the-azdata-version"></a><a id="azdataversion"></a> Überprüfen der azdata-Version

Vergewissern Sie sich vor dem Bereitstellen eines neuen Big Data-Clusters, dass Sie die neueste Version von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] mit dem `--version`-Parameter verwenden:

```bash
azdata --version
```

### <a name="install-the-new-release"></a>Installieren des neuen Releases

Nachdem Sie den vorherigen Big Data-Cluster entfernt und die neueste [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Version installiert haben, stellen Sie den neuen Big Data-Cluster mithilfe der aktuellen Bereitstellungsanweisungen bereit. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] auf Kubernetes](deployment-guidance.md). Stellen Sie anschließend alle erforderlichen Datenbanken oder Dateien wieder her.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).