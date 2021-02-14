---
title: 'Bereitstellen: Azure Data Studio-Notebook'
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie, wie Sie den Code und die Dokumentation in einem Notebook aus Azure Data Studio verwenden, um einen Big Data Cluster für SQL Server bereitzustellen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 027da3c742508ff962fcf10b084b638a7eeee708
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044240"
---
# <a name="deploy-sql-server-big-data-cluster-with-azure-data-studio-notebook"></a>Bereitstellen eines Big Data-Clusters für SQL Server mit einem Azure Data Studio-Notebook

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] stellt eine Erweiterung für Azure Data Studio bereit, die Bereitstellungsnotebooks umfasst. Ein Bereitstellungsnotebook enthält Dokumentation und Code, die Sie in Azure Data Studio verwenden können, um Big Data-Cluster für SQL Server zu erstellen.

[Notebooks](../azure-data-studio/notebooks/notebooks-guidance.md), die ursprünglich als Open-Source-Projekt implementiert wurden, wurden in [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) implementiert. Sie können Markdown für Text in den Textzellen und einen der verfügbaren Kernel verwenden, um Code in den Codezellen zu schreiben.

Sie können Notebooks verwenden, um Big Data-Cluster für [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] bereitzustellen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, damit auch das Notebook gestartet werden kann:

* Installation der neuesten Version von [Azure Data Studio Insiders-Build](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-master)

Zusätzlich hierzu sind folgende Komponenten erforderlich, um einen Big Data-Cluster für SQL Server 2019 bereitstellen zu können:

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Azure CLI (bei Bereitstellung in Azure)](/cli/azure/install-azure-cli)

## <a name="launch-the-notebook"></a>Starten des Notebooks

1. Starten Sie Azure Data Studio.

2. Wählen Sie auf der Registerkarte **Verbindungen** die Auslassungspunkte ( **...** ) aus, und wählen Sie dann **Deploy SQL Server...** (SQL Server bereitstellen...) aus.

   ![Dialogfeld „Deploy SQL Server“ (SQL Server bereitstellen)](media/notebooks-deploy/deploy-notebooks.png)

3. Wählen Sie unter den Bereitstellungsoptionen **SQL Server Big Data-Cluster** aus.

4. Wählen Sie in **Bereitstellungsziel** unter **Optionen** entweder **Neuer Azure Kubernetes-Cluster** oder **Vorhandener Azure Kubernetes Service-Cluster** aus.

5. Akzeptieren Sie die Datenschutz-und Lizenzbedingungen

6. In diesem Dialogfeld wird außerdem überprüft, ob die erforderlichen Tools für den ausgewählten SQL-Bereitstellungstyp auf dem Host vorhanden sind. Die Schaltfläche **Auswählen** wird erst aktiviert, wenn die Überprüfung der Tools erfolgreich war.

7. Wählen Sie die Schaltfläche **Auswählen** aus. Mit dieser Aktion wird die Bereitstellung gestartet.

## <a name="set-deployment-configuration-template"></a>Festlegen einer Vorlage für die Bereitstellungskonfiguration

Sie können die Einstellungen des Bereitstellungsprofils anpassen, indem Sie die folgenden Anweisungen befolgen.

### <a name="target-configuration-template"></a>Vorlage für die Zielkonfiguration

Wählen Sie die Vorlage für die Zielkonfiguration aus den verfügbaren Vorlagen aus. Die verfügbaren Profile werden abhängig vom Typ des Bereitstellungsziels gefiltert, das im vorherigen Dialogfeld ausgewählt wurde.

   ![Schritt 1: Vorlage für die Bereitstellungskonfiguration](media/notebooks-deploy/deployment-configuration-template.png)

### <a name="azure-settings"></a>Azure-Einstellungen

Wenn das Bereitstellungsziel ein neuer AKS-Cluster ist, werden zusätzliche Informationen wie die Azure-Abonnement-ID, die Ressourcengruppe, der AKS-Clustername, die Anzahl und Größe virtueller Computer sowie weitere zusätzliche Informationen benötigt, um den AKS-Cluster zu erstellen.

   ![Azure-Einstellungen](media/notebooks-deploy/azure-settings.png)

Wenn das Bereitstellungsziel ein vorhandener Kubernetes-Cluster ist, fordert der Assistent den Pfad zur *Kube*-Konfigurationsdatei zum Importieren der Kubernetes-Clustereinstellungen auf. Stellen Sie sicher, dass der entsprechende Clusterkontext ausgewählt ist, in dem der Big Data-Cluster für SQL Server 2019 bereitgestellt werden kann.

   ![Zielclusterkontext](media/notebooks-deploy/target-cluster-context.png)

### <a name="cluster-docker-and-ad-settings"></a>Einstellungen für Cluster, Docker und AD

1. Geben Sie den Clusternamen für den Big Data-Cluster für SQL Server 2019, den Administratorbenutzernamen und das Kennwort ein.
Hinweis: Für den Controller und SQL Server wird das gleiche Konto verwendet.

   ![Clustereinstellungen](media/notebooks-deploy/cluster-settings.png)

2. Geben Sie die Docker-Einstellungen entsprechend ein.

   ![Docker-Einstellungen](media/notebooks-deploy/docker-settings.png)

3. Geben Sie die AD-Einstellungen ein, wenn die AD-Authentifizierung verfügbar ist.

   ![Active Directory-Einstellungen](media/notebooks-deploy/active-directory-settings.png)

### <a name="service-settings"></a>Diensteinstellungen

Auf diesem Bildschirm können Sie verschiedene Einstellungen wie **Skalierung**, **Endpunkte**, **Speicher** und andere **erweiterte Speichereinstellungen** festlegen. Geben Sie die entsprechenden Werte ein, und wählen Sie **Weiter** aus.

#### <a name="scale-settings"></a>Skalierungseinstellungen

Geben Sie die Anzahl der Instanzen der einzelnen Komponenten im Big Data-Cluster ein.

Eine Spark-Instanz kann zusammen mit HDFS eingeschlossen werden. Sie ist im Speicherpool oder eigenständig im Spark-Pool enthalten.

   ![Diensteinstellungen](media/notebooks-deploy/service-settings.png)

Weitere Informationen zu den einzelnen Komponenten finden Sie unter [Masterinstanz](concept-master-instance.md), [Datenpool](concept-data-pool.md), [Speicherpool](concept-storage-pool.md) oder [Computepool](concept-compute-pool.md).

#### <a name="endpoint-settings"></a>Endpunkteinstellungen

Die Standardendpunkte wurden vorab ausgefüllt. Sie können jedoch nach Bedarf geändert werden.

   ![Endpunkteinstellungen](media/notebooks-deploy/endpoint-settings.png)

#### <a name="storage-settings"></a>Speichereinstellungen

Die Speichereinstellungen umfassen die Speicherklasse und die Anspruchsgröße für Daten und Protokolle. Die Einstellungen können auf den Speicherpool, Datenpool und SQL Server-Masterpool angewendet werden.

   ![Speichereinstellungen](media/notebooks-deploy/storage-settings.png)

#### <a name="advanced-storage-settings"></a>Erweiterte Speichereinstellungen

Sie können unter **Erweiterte Speichereinstellungen** zusätzliche Speichereinstellungen hinzufügen.

* Speicherpool (HDFS)
* Datenpool
* SQL Server Master

   ![Erweiterte Speichereinstellungen](media/notebooks-deploy/advanced-storage-settings.png)

### <a name="summary"></a>Zusammenfassung

Auf diesem Bildschirm ist eine Zusammenfassung der gesamten Eingabe, die für die Bereitstellung des Big Data-Clusters von SQL Server 2019 bereitgestellt wurde, ersichtlich. Die Konfigurationsdateien können über die Schaltfläche **Save config files** (Konfigurationsdateien speichern) heruntergeladen werden. Wählen Sie **Script to Notebook** (Skript in Notebook schreiben) aus, um ein Skript für die gesamte Bereitstellungskonfiguration in einem Notebook zu schreiben. Wählen Sie **Run Cells** (Zellen ausführen) aus, sobald das Notebook geöffnet ist, um mit der Bereitstellung des Big Data-Clusters für SQL Server 2019 für das ausgewählte Ziel zu beginnen.

   ![Zusammenfassung](media/notebooks-deploy/deploy-sql-server-big-data-cluster-on-a-new-AKS-cluster.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung finden Sie unter [Bereitstellungsanleitung für Big Data-Cluster für SQL Server](deployment-guidance.md).