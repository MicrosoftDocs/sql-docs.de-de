---
title: Power Query-Quelle | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Power Query-Quelle im SQL Server Integration Services-Datenfluss (SSIS) konfigurieren.
ms.date: 12/27/2019
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.powerqueryconnmgr.f1
- sql13.ssis.designer.powerquerysource.queries.f1
- sql13.ssis.designer.powerquerysource.connmgrs.f1
- sql14.ssis.designer.powerqueryconnmgr.f1
- sql14.ssis.designer.powerquerysource.queries.f1
- sql14.ssis.designer.powerquerysource.connmgrs.f1
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 43e02595cc57413eb18ff3e390606ea94493912a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336351"
---
# <a name="power-query-source-preview"></a>Power Query-Quelle (Preview)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Dieser Artikel beschreibt, wie Sie die Eigenschaften der Power Query-Quelle im SQL Server Integration Services-Datenfluss konfigurieren. Power Query ist eine Technologie, mit der Sie eine Verbindung mit verschiedenen Datenquellen herstellen und Daten mithilfe von Excel/Power BI Desktop transformieren können. Weitere Informationen finden Sie in dem Artikel [Power Query – Übersicht und Schulung](https://support.office.com/article/power-query-overview-and-learning-ed614c81-4b00-4291-bd3a-55d80767f81d). Das von Power Query generierte Skript kann kopiert und in die Power Query-Quelle im SSIS-Datenfluss eingefügt werden, um sie zu konfigurieren.
  
> [!NOTE]
> Für die aktuelle Vorschauversion können Power Query-Quellen nur in SQL Server 2017, SQL Server 2019 und in Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) verwendet werden. Sie können die neueste Power Query-Quelle für SQL Server 2017 und SQL Server 2019 [hier](https://www.microsoft.com/download/details.aspx?id=100619) herunterladen und installieren. Die Power Query-Quelle für Azure-SSIS IR ist bereits vorinstalliert. Informationen zum Bereitstellen von Azure-SSIS IR finden Sie unter [Bereitstellen von SSIS in ADF](/azure/data-factory/tutorial-deploy-ssis-packages-azure).

## <a name="configure-the-power-query-source"></a>Konfigurieren der Power Query-Quelle

Um den **Power Query-Quellen-Editor** in SSDT zu öffnen, ziehen Sie die **Power Query-Quelle** aus der SSIS-Toolbox, legen sie im Datenfluss-Designer ab und doppelklicken darauf.  

![PQ-Quelle](media/power-query-source/pq-source.png)

Auf der linken Seite werden drei Registerkarten angezeigt. Auf der Registerkarte **Abfragen** können Sie Ihren Abfragemodus aus dem Dropdownmenü auswählen.
-   Im Modus **Einzelne Abfrage** können Sie ein einzelnes Power Query-Skript aus Excel/Power BI Desktop kopieren und dann einfügen.
-   Im Modus **Einzelne Abfrage aus Variable** können Sie eine Zeichenfolgenvariable angeben, die Ihre auszuführende Abfrage enthält.

![„Einzeln“ auf der PQ-Quellen-Registerkarte „Abfragen“](media/power-query-source/pq-source-queries-tab-single.png)

Auf der Registerkarte **Verbindungs-Manager** können Sie Power Query-Verbindungs-Manager hinzufügen oder entfernen, die Anmeldeinformationen für den Datenquellenzugriff enthalten. Wenn Sie die Schaltfläche **Datenquelle erkennen** auswählen, wird die in Ihrer Abfrage referenzierte Quelle identifiziert für Sie aufgelistet, damit Sie die geeigneten, vorhandenen Power Query Verbindungs-Manager zuweisen oder neue erstellen können.

![„Erkennen“ auf der PQ-Quellen-Registerkarte „Verbindungs-Manager“](media/power-query-source/pq-source-connection-managers-tab-detect.png)

![„Hinzufügen“ auf der PQ-Quellen-Registerkarte „Verbindungs-Manager“](media/power-query-source/pq-source-connection-managers-tab-add.png)

Schließlich können Sie auf der Registerkarte **Spalten** die Ausgabespalteninformationen bearbeiten.

![PQ-Quellen-Registerkarte „Spalten“](media/power-query-source/pq-source-columns-tab.png)

## <a name="configure-the-power-query-connection-manager"></a>Konfigurieren des Power Query-Verbindungs-Managers

Wenn Sie Ihren Datenfluss mit Power Query-Quelle in SSDT entwerfen, können Sie einen neuen Power Query-Verbindungs-Manager auf folgende Arten erstellen:
- Indirektes Erstellen auf der Registerkarte **Verbindungs-Manager** von Power Query-Quelle, nachdem Sie die Schaltfläche **Hinzufügen**/**Datenquelle erkennen** ausgewählt haben und dann im Dropdownmenü wie oben beschrieben **<New connection...>** auswählen.
- Direktes Erstellen, indem Sie mit der rechten Maustaste auf den Bereich **Verbindungs-Manager** Ihres Pakets klicken und im Dropdownmenü **Neue Verbindung...**  auswählen.

![„Hinzufügen“ im PQ-Quellen-Bereich „Verbindungs-Manager“](media/power-query-source/pq-source-connection-managers-panel-add.png)

Doppelklicken Sie im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen** in der Liste mit Verbindungs-Manager-Typen auf **PowerQuery**.

![Dialogfeld „Hinzufügen“ im PQ-Quellen-Bereich „Verbindungs-Manager“](media/power-query-source/pq-source-connection-managers-panel-add-dialog.png)

Im **Power Query-Verbindungs-Manager-Editor** müssen Sie **Datenquellenart**, **Datenquellenpfad** und **Authentifizierungsart** angeben sowie die entsprechenden Anmeldeinformationen zuweisen. Als **Datenquellenart** können Sie zurzeit eine von 22 Arten aus dem Dropdownmenü auswählen.

![„Art“ im PQ-Quellen-Verbindungs-Manager-Editor](media/power-query-source/pq-source-connection-manager-editor-kind.png)

Einige dieser Quellen (**Oracle**, **DB2**, **MySQL**, **PostgreSQL**, **Teradata**, **Sybase**) erfordern zusätzliche Installationen von ADO.NET-Treibern, die Sie über den Artikel [Power Query-Voraussetzungen](/power-bi/desktop-data-source-prerequisites) abrufen können. Sie können die Schnittstelle für benutzerdefinierte Installationen verwenden, um sie in Ihrer Azure-SSIS IR zu installieren. Siehe hierzu den Artikel [Anpassen von Azure-SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Als **Datenquellenpfad** können Sie datenquellenspezifische Eigenschaften eingeben, die eine Verbindungszeichenfolge ohne die Authentifizierungsinformationen ergeben. Beispielsweise wird der den Pfad für die **SQL**-Datenquelle als `<Server>;<Database>` gebildet. Sie können die Schaltfläche **Bearbeiten** auswählen, um datenquellenspezifischen Eigenschaften Werte zuzuweisen, die den Pfad bilden.

![„Pfad“ im PQ-Quellen-Verbindungs-Manager-Editor](media/power-query-source/pq-source-connection-manager-editor-path.png)

Schließlich können Sie als **Authentifizierungsart** **Anonym**/**Windows-Authentifizierung**/**Benutzername Kennwort**/**Schlüssel** aus dem Dropdownmenü auswählen, die entsprechenden Anmeldeinformationen eingeben und die Schaltfläche **Testverbindung** auswählen, um sicherzustellen, dass die Power Query Quelle ordnungsgemäß konfiguriert wurde.

![„Authentifizierung“ im PQ-Quellen-Verbindungs-Manager-Editor](media/power-query-source/pq-source-connection-manager-editor-authentication.png)

### <a name="current-limitations"></a>Aktuelle Einschränkungen

-   Die Datenquelle **Oracle** kann zurzeit nicht in Azure-SSIS IR verwendet werden, da der Oracle-ADO.NET-Treiber nicht in Azure-SSIS IR installiert werden kann. Installieren Sie daher stattdessen den Oracle-ODBC-Treiber, und verwenden Sie vorerst die **ODBC**-Datenquelle, um eine Verbindung mit Oracle herzustellen. Weitere Informationen finden Sie im **ORACLE STANDARD ODBC**-Beispiel im Artikel [Anpassen von Azure-SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

-   Die Datenquelle **Web** kann zurzeit nicht mit einem benutzerdefinierten Setup in Azure-SSIS IR verwendet werden. Verwenden Sie diese daher vorerst ohne benutzerdefiniertes Setup in Azure-SSIS IR.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie SSIS-Pakete in der Azure-SSIS IR als Aktivitäten erster Klasse in ADF-Pipelines ausführen. Siehe hierzu im Artikel [Ausführen der SSIS-Paket-Aktivitätslaufzeit](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).