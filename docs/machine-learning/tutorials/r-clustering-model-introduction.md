---
title: 'Tutorial: Bereitstellen eines Clusteringmodells in R'
titleSuffix: SQL Machine Learning
description: Im dieser vierteiligen Tutorialreihe entwickeln Sie ein Modell, um das Clustering in R mit SQL Machine Learning durchzuführen.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6a02b31b6427659b93e28c666fb0efdb471a1fcf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348113"
---
# <a name="tutorial-develop-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutorial: Erstellen eines Clusteringmodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In dieser vierteiligen Tutorialreihe verwenden Sie R zum Entwickeln und Bereitstellen eines K-Means-Clustermodells in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) oder in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) zum Kategorisieren von Kundendaten.
::: moniker-end
::: moniker range="=sql-server-2017"
In dieser vierteiligen Tutorialreihe verwenden Sie R zum Entwickeln und Bereitstellen eines K-Means-Clustermodells in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) zum Clustern von Kundendaten.
::: moniker-end
::: moniker range="=sql-server-2016"
In dieser vierteiligen Tutorialreihe verwenden Sie R zum Entwickeln und Bereitstellen eines K-Means-Clustermodells in [SQL Server R Services](../r/sql-server-r-services.md) zum Clustern von Kundendaten.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In dieser vierteiligen Tutorialreihe verwenden Sie R zum Entwickeln und Bereitstellen eines K-Means-Clustermodells in [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) zum Clustern von Kundendaten.
::: moniker-end

Im ersten Teil dieser Reihe richten Sie die Voraussetzungen für das Tutorial ein und stellen dann ein Beispieldataset für eine Datenbank wieder her. Im zweiten und dritten Teil entwickeln Sie R-Skripts in einem Azure Data Studio-Notebook, um diese Beispieldaten zu analysieren und vorzubereiten sowie um ein Machine Learning-Modell zu trainieren. Im vierten Teil führen Sie diese R-Skripts in einer Datenbank mithilfe gespeicherter Prozeduren aus.

*Clustering* kann als Organisieren von Daten in Gruppen beschrieben werden, in denen Mitglieder einer Gruppe in irgendeiner Weise ähnlich sind. Stellen Sie sich für diese Tutorialreihe vor, dass Sie ein Einzelhandelsgeschäft besitzen. Sie verwenden den **K-Means**-Algorithmus zum Durchführen des Clusterings von Kunden in einem Dataset von Produktkäufen und -rückgaben. Durch das Clustern von Kunden können Sie Ihre Marketingmaßnahmen effektiver auf bestimmte Gruppen ausrichten. K-Means-Clustering ist ein *nicht überwachter Lernalgorithmus*, der auf der Grundlage von Ähnlichkeiten nach Mustern in Daten sucht.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Wiederherstellen einer Beispieldatenbank

In [Teil 2](r-clustering-model-prepare-data.md) lernen Sie, wie Sie die Daten aus einer Datenbank für das Clustering vorbereiten.

In [Teil 3](r-clustering-model-build.md) erfahren Sie, wie Sie ein K-Means-Clustermodell in R erstellen und trainieren.

In [Teil 4](r-clustering-model-deploy.md) erfahren Sie, wie Sie eine gespeicherte Prozedur in einer Datenbank erstellen, die Clustering auf der Grundlage neuer Daten in R durchführen kann.

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) mit der Python-Programmiersprachoption: Befolgen Sie die Installationsanweisungen im [Windows-Installationshandbuch](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationshandbuch](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). Sie können auch [Machine Learning Services in Big Data-Clustern unter SQL Server aktivieren](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) mit der R-Programmiersprachoption: Befolgen Sie die Installationsanweisungen im [Windows-Installationshandbuch](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md), um die Beispieldatenbank in Azure SQL Managed Instance wiederherzustellen
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) Für SQL verwenden Sie ein Notebook in Azure Data Studio. Weitere Informationen zu Notebooks finden Sie unter [Verwenden von Notebooks in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* R-IDE: In diesem Tutorial wird [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) verwendet.

* RODBC: Dieser Treiber wird in den R-Skripts verwendet, die Sie in diesem Tutorial entwickeln. Installieren Sie ihn mithilfe des R-Befehls `install.packages("RODBC")`, sofern er noch nicht installiert ist. Weitere Informationen zu RODBC finden Sie unter [CRAN – RODBC-Paket](https://CRAN.R-project.org/package=RODBC).

## <a name="restore-the-sample-database"></a>Wiederherstellen der Beispieldatenbank

Das in diesem Tutorial verwendete Beispieldataset wurde in einer **BAK**-Datenbanksicherungsdatei gespeichert, die Sie herunterladen und verwenden können. Dieses Dataset wird aus dem [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp)-Dataset abgeleitet, das von [TPC (Transaction Processing Performance Council)](http://www.tpc.org/) bereitgestellt wird.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Wenn Sie Machine Learning Services in Big Data-Clustern verwenden, finden Sie Informationen zum Wiederherstellen unter [Wiederherstellen einer Datenbank in der Masterinstanz eines Big Data-Clusters für SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Laden Sie die Datei [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak) herunter.

1. Befolgen Sie die Anweisungen unter [Wiederherstellen einer Datenbank aus einer Sicherungsdatei](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) in Azure Data Studio, und verwenden Sie hierzu die folgenden Details:

   * Importieren Sie aus der heruntergeladenen Datei **tpcxbb_1gb.bak**.
   * Geben Sie der Zieldatenbank den Namen „tpcxbb_1gb“.

1. Nach dem Wiederherstellen der Datenbank können Sie überprüfen, ob das Dataset vorhanden ist, indem Sie die Tabelle **dbo.customer** abfragen:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Laden Sie die Datei [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak) herunter.

1. Befolgen Sie die Anweisungen unter [Schnellstart: Wiederherstellen einer Datenbank in Azure SQL Managed Instance mit SSMS](/azure/sql-database/sql-database-managed-instance-get-started-restore) für die Ausführung in SQL Server Management Studio. Verwenden Sie hierzu die folgenden Details:

   * Importieren Sie aus der heruntergeladenen Datei **tpcxbb_1gb.bak**.
   * Geben Sie der Zieldatenbank den Namen „tpcxbb_1gb“.

1. Nach dem Wiederherstellen der Datenbank können Sie überprüfen, ob das Dataset vorhanden ist, indem Sie die Tabelle **dbo.customer** abfragen:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „tpcxbb_1gb“.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Installieren der Voraussetzungen
* Wiederherstellen einer Beispieldatenbank

Fahren Sie mit dem zweiten Teil dieser Tutorialreihe fort, um die Daten für das Machine Learning-Modell vorzubereiten:

> [!div class="nextstepaction"]
> [Vorbereiten von Daten zum Ausführen von Clustering](r-clustering-model-prepare-data.md)