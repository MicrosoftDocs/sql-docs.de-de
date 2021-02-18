---
title: 'Tutorial: Entwickeln eines Vorhersagemodells in R'
titleSuffix: SQL machine learning
description: In dieser vierteiligen Tutorialreihe entwickeln Sie Daten für das Training eines Vorhersagemodells in R mit SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5376d2b7876914c28d656574a57f5fc80294c63c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348043"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Entwickeln eines Vorhersagemodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In dieser vierteiligen Tutorialreihe verwenden Sie R und ein Machine Learning-Modell in [SQL Server-Machine Learning Services](../sql-server-machine-learning-services.md) oder in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) zur Vorhersage der Verleihzahlen für einen Skiverleih.
::: moniker-end
::: moniker range="=sql-server-2017"
In dieser vierteiligen Tutorialreihe verwenden Sie R und ein Machine Learning-Modell in [SQL Server-Machine Learning Services](../sql-server-machine-learning-services.md) zur Vorhersage von Verleihzahlen für einen Skiverleih.
::: moniker-end
::: moniker range="=sql-server-2016"
In dieser vierteiligen Tutorialreihe verwenden Sie R und ein Machine Learning-Modell in [SQL Server R Services](../r/sql-server-r-services.md) zur Vorhersage von Verleihzahlen für einen Skiverleih.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In dieser vierteiligen Tutorialreihe verwenden Sie R und ein Machine Learning-Modell in [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) zur Vorhersage von Verleihzahlen für einen Skiverleih.
::: moniker-end

Angenommen, Sie möchten für Ihren eigenen Skiverleih die Verleihzahlen für einen bestimmten Zeitpunkt in der Zukunft vorhersagen. Mit dieser Information können Sie den Bestand, die Mitarbeiter und die Räumlichkeiten besser vorausplanen.

Im ersten Teil der Reihe bereiten Sie die Voraussetzungen vor. Im zweiten und dritten Teil entwickeln Sie R-Skripts in einem Notebook zur Vorbereitung Ihrer Daten und zum Trainieren eines Machine Learning-Modells. In Teil 3 führen Sie diese R-Skripts dann mithilfe von gespeicherten T-SQL-Prozeduren in einer Datenbank aus.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Wiederherstellen einer Beispieldatenbank 

In [Teil 2](r-predictive-model-prepare-data.md) lernen Sie, wie Sie die Daten aus einer Datenbank in einen Python-Datenrahmen laden und in R vorbereiten.

In [Teil 3](r-predictive-model-train.md) erfahren Sie, wie Sie ein Machine Learning-Modell in R trainieren.

In [Teil 4](r-predictive-model-deploy.md) haben Sie gelernt, wie Sie das Modell in einer Datenbank speichern und gespeicherte Prozeduren aus den R-Skripts erstellen, die Sie in Teil 2 und 3 entwickelt haben. Die gespeicherten Prozeduren werden auf dem Server ausgeführt, um Vorhersagen basierend auf neuen Daten treffen zu können.

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* SQL Server Machine Learning Services: Informationen zur Installation von Machine Learning Services finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Sie können auch [Machine Learning Services in Big Data-Clustern unter SQL Server aktivieren](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* SQL Server-Machine Learning Services: Informationen zur Installation von Machine Learning Services finden Sie im [Windows-Installationsleitfaden](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
* SQL Server 2016 R Services. Informationen zur Installation von R Services finden Sie im [Windows-Installationsleitfaden](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) zum Wiederherstellen der Beispieldatenbank in Azure SQL Managed Instance
::: moniker-end

* R-IDE: In diesem Tutorial wird [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) verwendet.

* RODBC: Dieser Treiber wird in den R-Skripts verwendet, die Sie in diesem Tutorial entwickeln. Installieren Sie ihn mithilfe des R-Befehls `install.packages("RODBC")`, sofern er noch nicht installiert ist. Weitere Informationen zu RODBC finden Sie unter [CRAN – RODBC-Paket](https://CRAN.R-project.org/package=RODBC).

* SQL-Abfragetool: In diesem Tutorial wird davon ausgegangen, dass Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwenden. Weitere Informationen finden Sie unter [Verwenden von Notebooks in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

## <a name="restore-the-sample-database"></a>Wiederherstellen der Beispieldatenbank

Die in diesem Tutorial verwendete Beispieldatenbank wurde in einer **BAK**-Datenbanksicherungsdatei gespeichert, die Sie herunterladen und verwenden können.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Wenn Sie Machine Learning Services in Big Data-Clustern verwenden, finden Sie Informationen zum Wiederherstellen unter [Wiederherstellen einer Datenbank in der Masterinstanz eines Big Data-Clusters für SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Laden Sie die Datei [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak) herunter.

1. Befolgen Sie die Anweisungen unter [Wiederherstellen einer Datenbank aus einer Sicherungsdatei](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) in Azure Data Studio, und verwenden Sie hierzu die folgenden Details:

   * Importieren Sie aus der heruntergeladenen Datei **TutorialDB.bak**.
   * Geben Sie der Zieldatenbank den Namen „TutorialDB“.

1. Sie können überprüfen, ob die wiederhergestellte Datenbank vorhanden ist, indem Sie die Tabelle **dbo.rental_data** abfragen:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Laden Sie die Datei [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak) herunter.

1. Befolgen Sie die Anweisungen unter [Schnellstart: Wiederherstellen einer Datenbank in Azure SQL Managed Instance mit SSMS](/azure/sql-database/sql-database-managed-instance-get-started-restore) im SQL Server Management Studio, und verwenden Sie hierzu die folgenden Details:

   * Importieren Sie aus der heruntergeladenen Datei **TutorialDB.bak**.
   * Geben Sie der Zieldatenbank den Namen „TutorialDB“.

1. Sie können überprüfen, ob die wiederhergestellte Datenbank vorhanden ist, indem Sie die Tabelle **dbo.rental_data** abfragen:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „TutorialDB“.
## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Installieren der Voraussetzungen
* Wiederherstellen einer Beispieldatenbank

Fahren Sie mit dem zweiten Teil dieser Tutorialreihe fort, um die Daten für das Machine Learning-Modell vorzubereiten:

> [!div class="nextstepaction"]
> [Vorbereiten von Daten zum Trainieren eines Vorhersagemodells in R](r-predictive-model-prepare-data.md)