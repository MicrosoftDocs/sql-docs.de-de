---
description: Neuigkeiten in Integration Services in SQL Server 2017
title: Neuigkeiten in Integration Services in SQL Server 2017 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e26d7884-e772-46fa-bfdc-38567fe976a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b955aa61a4e196fc4c419a9b34e186590006df42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345673"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2017"></a>Neuigkeiten in Integration Services in SQL Server 2017

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Artikel werden die Features beschrieben, die in [!INCLUDE[ssSQLv14_md](../includes/sssql17-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] hinzugefügt oder aktualisiert wurden.

> [!NOTE]
> SQL Server 2017 enthält ebenfalls die Funktionen von SQL Server 2016 sowie die Funktionen, die in Updates für SQL Server 2016 hinzugefügt wurden. Informationen zu den neuen Funktionen von SSIS in SQL Server 2016 finden Sie unter [Neuigkeiten in Integration Services in SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="highlights-of-this-release"></a>Highlights dieses Releases

Im Folgenden finden Sie die wichtigsten neuen Funktionen von Integration Services in SQL Server 2017.

-   **Scale Out**. Verteilen Sie die Ausführung von SSIS-Paketen einfacher über mehrere Workercomputer, und verwalten Sie Ausführungen und Worker von einem einzelnen Mastercomputer aus. Weitere Informationen finden Sie unter [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md).

-   **Integration Services unter Linux**. Führen Sie SSIS-Pakete auf Linux-Computern aus. Weitere Informationen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

-   **Verbesserung der Konnektivität**. Stellen Sie mithilfe der aktualisierten OData-Komponenten eine Verbindung mit den OData-Feeds von Microsoft Dynamics AX Online und Microsoft Dynamics CRM Online her. 

## <a name="new-in-azure-data-factory"></a>Neues in Azure Data Factory

In der öffentlichen Vorschauversion von Azure Data Factory Version 2, die seit September 2017 zur Verfügung steht, ist nun Folgendes möglich:
-   Bereitstellen von Paketen in der SSIS-Katalogdatenbank (SSISDB) für Azure SQL-Datenbank
-   Ausführen von Paketen, die in Azure in Azure SSIS Integration Runtime bereitgestellt wurden. Hierbei handelt es sich um eine Komponente von Azure Data Factory Version 2.

Weitere Informationen finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift und Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Diese neuen Funktionen benötigen SQL Server Data Tools (SSDT) Version 17.2 oder höher, nicht jedoch SQL Server 2017 oder SQL Server 2016. Wenn Sie Pakete in Azure bereitstellen, aktualisiert der Assistent für die Paketbereitstellung die Pakete immer auf das aktuelle Paketformat.

## <a name="new-in-the-azure-feature-pack"></a>Neues in Azure Feature Pack

Zusätzlich zur Verbesserung der Konnektivität in SQL Server wurde mit Integration Services Feature Pack für Azure die Unterstützung für Azure Data Lake Store hinzugefügt. Weitere Informationen finden Sie im Blogbeitrag [New Azure Feature Pack Release Strengthening ADLS Connectivity (Stärkung der ADLS-Konnektivität durch das Release von Azure Feature Pack)](https://blogs.msdn.microsoft.com/ssis/2017/08/29/new-azure-feature-pack-release-strengthening-adls-connectivity/). Weitere Informationen finden Sie ebenfalls unter [Azure Feature Pack for Integration Services (SSIS) (Azure Feature Pack für Integration Services (SSIS))](azure-feature-pack-for-integration-services-ssis.md).

## <a name="new-in-sql-server-data-tools-ssdt"></a>Neues in SQL Server Data Tools (SSDT)

Sie können nun SSIS-Projekte und -Pakete für SQL Server 2012 bis 2017 in Visual Studio 2017 oder Visual Studio 2015 entwickeln. Weitere Informationen finden Sie unter [Herunterladen von SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

## <a name="new-in-ssis-in-sql-server-2017-rc1"></a>Neues in SSIS in SQL Server 2017 RC1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Neue und geänderte Funktionen in Scale Out für SSIS

-   Scale Out-Master unterstützt jetzt Hochverfügbarkeit. Sie können Always On für SSISDB aktivieren und das Windows Server-Failoverclustering für den Server einrichten, der den Dienst „Scale Out-Master“ hostet. Indem Sie diese Änderung auf den Scale Out-Master anwenden, vermeiden Sie eine einzelne Fehlerquelle und ermöglichen Hochverfügbarkeit für die gesamte Scale Out-Bereitstellung.
-   Die Failoverbehandlung der Ausführungsprotokolle aus Scale-Out-Workern wurde verbessert. Die Ausführungsprotokolle werden auf dem lokalen Datenträger beibehalten, falls der Scale Out-Worker unerwartet beendet wird. Bei einem Neustart des Workers werden die beibehaltenen Protokolle erneut geladen und in SSISDB gespeichert.
-   Der Parameter *runincluster* der gespeicherten Prozedur **[catalog].[create_execution]** wird hinsichtlich Konsistenz und Lesbarkeit in *runinscaleout* umbenannt. Die Änderung des Parameternamens hat folgende Auswirkungen:
    -   Wenn Skripts zum Ausführen von Paketen in Scale Out vorhanden sind, müssen Sie den Parameternamen von *runincluster* in *runinscaleout* ändern, damit die Skripts in RC1 funktionieren.
    -   SQL Server Management Studio (SSMS) 17.1 und frühere Versionen können die Ausführung des Pakets in Scale Out in RC1 nicht auslösen. Die Fehlermeldung lautet: " *@runincluster* is not a parameter for procedure **create_execution** (ist kein Parameter für die Prozedur create_execution)." Dieses Problem wurde in der nächsten Version von SSMS, (Version 17.2) behoben. Version 17.2 und spätere SSMS-Versionen unterstützen den neuen Parameternamen und die Paketausführung in Scale Out. Bis SSMS Version 17.2 verfügbar ist, können Sie als Problemumgehung Ihre vorhandene Version von SSMS verwenden, um das Paketausführungsskript zu generieren. Ändern Sie anschließend im Skript den Namen des Parameters *runincluster* in *runinscaleout*, und führen Sie das Skript aus.
-   Der SSIS-Katalog verfügt über eine neue globale Eigenschaft, um den Standardmodus für das Ausführen von SSIS-Paketen anzugeben. Diese neue Eigenschaft wird angewendet, wenn Sie die gespeicherte Prozedur **[catalog].[create_execution]** mit dem auf NULL festgelegten Parameter *runinscaleout* aufrufen. Dieser Modus wird auch auf SSIS SQL-Agent-Aufträge angewendet. Sie können die neue globale Eigenschaft für den SSISDB-Knoten in SSMS im Dialogfeld „Eigenschaften“ oder mit folgendem Befehl festlegen:
    ```sql
    EXEC [catalog].[configure_catalog] @property_name=N'DEFAULT_EXECUTION_MODE', @property_value=1
    ```

## <a name="new-in-ssis-in-sql-server-2017-ctp-21"></a>Neues in SSIS in SQL Server 2017 CTP 2.1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Neue und geänderte Funktionen in Scale Out für SSIS

-   Sie können nun den Parameter **Use32BitRuntime** verwenden, wenn Sie die Ausführung in Scale Out auslösen.
-   Die Leistung der Protokollierung in SSISDB für die Ausführungen von Paketen in Scale Out wurde verbessert. Die Protokolle „Ereignismeldung“ und „Meldungskontext“ werden in SSISDB nun im Batchmodus statt einzeln geschrieben. Im Folgenden finden Sie einige zusätzliche Anmerkungen zu dieser Verbesserung:        
    - Einige Berichte in der aktuellen Version von SQL Server Management Studio (SSMS) zeigen diese Protokolle für Ausführungen in Scale Out derzeit nicht an. Dies soll im nächsten Release von SSMS unterstützt werden. Zu den betroffenen Berichten gehören die Berichte *Alle Verbindungen* und *Fehlerkontext* sowie der Abschnitt *Verbindungsinformationen* im Integration Services-Dashboard.
    - Die neue Spalte **event_message_guid** wurde hinzugefügt. Verwenden Sie diese Spalte, um die Sichten [catalog].[event_message_context] und [catalog].[event_messages] zu verknüpfen, anstatt **event_message_id** zu verwenden, wenn Sie diese Ausführungsprotokolle in Scale Out abfragen.
-   Laden Sie [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 17.1 oder höher herunter, um die Verwaltungsanwendung für SSIS Scale Out zu erhalten.

## <a name="new-in-ssis-in-sql-server-2017-ctp-20"></a>Neues in SSIS in SQL Server 2017 CTP 2.0

In SQL Server 2017 CTP 2.0 gibt es keine neuen SSIS-Funktionen.

## <a name="new-in-ssis-in-sql-server-2017-ctp-14"></a>Neues in SSIS in SQL Server 2017 CTP 1.4

In SQL Server 2017 CTP 1.4 gibt es keine neuen SSIS-Funktionen.

## <a name="new-in-ssis-in-sql-server-2017-ctp-13"></a>Neues in SSIS in SQL Server 2017 CTP 1.3

In SQL Server 2017 CTP 1.3 gibt es keine neuen SSIS-Funktionen.

## <a name="new-in-ssis-in-sql-server-2017-ctp-12"></a>Neues in SSIS in SQL Server 2017 CTP 1.2

In SQL Server 2017 CTP 1.2 gibt es keine neuen SSIS-Funktionen.

## <a name="new-in-ssis-in-sql-server-2017-ctp-11"></a>Neues in SSIS in SQL Server 2017 CTP 1.1

In SQL Server 2017 CTP 1.1 gibt es keine neuen SSIS-Funktionen.

## <a name="new-in-ssis-in-sql-server-2017-ctp-10"></a>Neues in SSIS in SQL Server 2017 CTP 1.0

### <a name="scale-out-for-ssis"></a>Horizontale Hochskalierung für SSIS

Die Funktion zum horizontalen Skalieren macht es viel einfacher, [!INCLUDE[ssIS_md](../includes/ssis-md.md)] auf mehreren Computern auszuführen. 
   
Nach dem Installieren des Masters und der Worker für horizontales Hochskalieren kann das Paket automatisch für die Ausführung auf verschiedenen Workern verteilt werden. Wenn die Ausführung unerwartet abbricht, wird sie automatisch wiederholt. Ferner können alle Ausführungen und Worker zentral mithilfe des Masters verwaltet werden.
   
Weitere Informationen finden Sie unter [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md).
   
### <a name="support-for-microsoft-dynamics-online-resources"></a>Unterstützung für Onlineressourcen von Microsoft Dynamics

Die OData-Quelle und der OData-Verbindungs-Manager unterstützen jetzt Verbindungen mit den OData-Feeds von Microsoft Dynamics AX Online und Microsoft Dynamics CRM Online.