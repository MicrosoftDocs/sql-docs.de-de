---
title: Durchführen einer SQL Server Migrations Bewertung
titleSuffix: Data Migration Assistant
description: Erfahren Sie, wie Sie Datenmigrations-Assistent zur Bewertung eines lokalen SQL Server vor der Migration zu einem anderen SQL Server oder zu Azure SQL-Datenbank verwenden.
ms.date: 01/15/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 300d88b66c2105235ab04ff616d9fcf81b24e944
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247332"
---
# <a name="perform-a-sql-server-migration-assessment-with-data-migration-assistant"></a>Durchführen einer SQL Server-Migrationsbewertung mit dem Datenmigrations-Assistenten

Die folgenden schrittweisen Anweisungen unterstützen Sie bei der ersten Bewertung der Migration zu lokalen SQL Server, SQL Server, die auf einer Azure-VM ausgeführt werden, oder Azure SQL-Datenbank mithilfe von Datenmigrations-Assistent.

Datenmigrations-Assistent v 5.0 führt die Unterstützung für die Analyse von Datenbankverbindungen und eingebetteten SQL-Abfragen im Anwendungscode ein. Weitere Informationen finden Sie im Blogbeitrag [using Datenmigrations-Assistent, um die Datenzugriffs Ebene einer Anwendung zu bewerten](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Using-Data-Migration-Assistant-to-assess-an-application-s-data/ba-p/990430).

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Erstellen einer Bewertung

1. Wählen Sie das Symbol **neu** (+) aus, und wählen Sie dann den Projekttyp **Bewertung** aus.

2. Legen Sie den Typ von Quell- und Zielserver fest.

    Wenn Sie die lokale SQL Server Instanz auf eine moderne lokale SQL Server Instanz oder auf eine auf einem virtuellen Azure-Computer gehostete SQL Server aktualisieren, legen Sie den Quell-und den Ziel Servertyp auf **SQL Server** fest. Wenn Sie zu Azure SQL-Datenbank migrieren, legen Sie stattdessen den Ziel Servertyp auf **Azure SQL-Datenbank** fest.

3. Klicken Sie auf **Erstellen**.

   ![Erstellen einer Bewertung](../dma/media/dma-assesssqlonprem/new-assessment.png)

## <a name="choose-assessment-options"></a>Bewertungs Optionen auswählen

1. Wählen Sie die Ziel SQL Server Version aus, zu der migriert werden soll.

2. Wählen Sie den Berichtstyp aus.

   Wenn Sie Ihre Quelle SQL Server-Instanz für die Migration zu einem lokalen SQL Server oder zu SQL Server, die auf Azure-VM-Zielen gehostet werden, bewerten, können Sie einen oder beide der folgenden Bewertungsberichts Typen auswählen:

    - **Kompatibilitätsprobleme**
    - **Empfehlung zu neuen Features**

   ![Wählen Sie einen Bewertungs Berichtstyp für SQL Server Ziel aus.](../dma/media/dma-assesssqlonprem/assessment-types.png)

   Wenn Sie die Quell SQL Server Instanz für die Migration zu Azure SQL-Datenbank bewerten, können Sie einen oder beide der folgenden Bewertungsberichts Typen auswählen:

    - **Check database compatibility (Datenbankkompatibilität prüfen)**
    - **Check feature parity (Featureparität prüfen)**

    ![Bewertungs Berichtstyp für SQL-Daten Bank Ziel auswählen](../dma/media/dma-assesssqlonprem/assessment-types-azure.png)

## <a name="add-databases-and-extended-events-trace-to-assess"></a>Datenbanken und Ablauf Verfolgung für erweiterte Ereignisse zur Bewertung hinzufügen

1. Wählen Sie **Quellen hinzufügen** aus, um das Menü Verbindung Flyout zu öffnen.

2. Geben Sie den SQL Server-Instanznamen ein, wählen Sie den Authentifizierungstyp, legen Sie die richtigen Verbindungs Eigenschaften fest, und wählen Sie dann **verbinden**

3. Wählen Sie die zu überprüfen Datenbanken aus, und wählen Sie dann **Hinzufügen**

    > [!NOTE]
    > Wenn Sie die UMSCHALT-oder STRG-Taste gedrückt halten, können Sie mehrere Datenbanken entfernen, indem Sie Sie auswählen und dann auf **Quellen entfernen** klicken. Sie können auch Datenbanken aus mehreren SQL Server Instanzen hinzufügen, indem Sie **Quellen hinzufügen** auswählen.

4. Wenn Sie Ad-hoc-oder dynamische SQL-Abfragen oder DML-Anweisungen haben, die über die Anwendungsdaten Schicht initiiert werden, geben Sie den Pfad zu dem Ordner ein, in dem Sie alle gesammelten Sitzungs Dateien für erweiterte Ereignisse abgelegt haben, um die Arbeitsauslastung auf dem Quell SQL Server zu erfassen.

     Im folgenden Beispiel wird gezeigt, wie Sie eine Sitzung für erweiterte Ereignisse auf der Quell SQL Server erstellen, um die Arbeitsauslastung der Anwendungsdaten Schicht zu erfassen.  Erfassen Sie die Arbeitsauslastung für die Dauer, die ihre Spitzen Auslastung darstellt.

    ```
    DROP EVENT SESSION [DatalayerSession] ON SERVER
    go
    CREATE EVENT SESSION [DatalayerSession] ON SERVER  
    ADD EVENT sqlserver.sql_batch_completed( 
        ACTION (sqlserver.sql_text,sqlserver.client_app_name,sqlserver.client_hostname,sqlserver.database_id))
    ADD TARGET package0.asynchronous_file_target(SET filename=N'C:\temp\Demos\DataLayerAppassess\DatalayerSession.xel')  
    WITH (MAX_MEMORY=2048 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=3 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
    go
    ---Start the session
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = START;
    ---Wait for few minutes
    
    ---Query events
        
        SELECT 
        object_name,
        CAST(event_data as xml) as event_data,
        file_name, 
        file_offset
    FROM sys.fn_xe_file_target_read_file('C:\temp\Demos\DataLayerAppassess\DatalayerSession*xel', 
                'C:\\temp\\Demos\\DataLayerAppassess\\DatalayerSession*xem', 
                null,
                null)
    ---Stop the session after capturing the peak load.
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = STOP;
        
        go
    ```

5. Klicken Sie auf **Weiter**, um die Bewertung zu starten.

    ![Quellen hinzufügen und Bewertung starten](../dma/media/dma-assesssqlonprem/select-database1.png)

> [!NOTE]
> Sie können mehrere Bewertungen gleichzeitig durchführen und den Status der Bewertungen anzeigen, indem Sie die Seite **Alle Bewertungen** öffnen.

## <a name="view-results"></a>Anzeigen der Ergebnisse

Die Dauer der Bewertung hängt von der Anzahl der hinzugefügten Datenbanken und der Größe der Schemas der einzelnen Datenbanken ab. Die Ergebnisse für die einzelnen Datenbanken werden angezeigt, sobald sie verfügbar sind.

1. Wählen Sie die Datenbank aus, die die Bewertung abgeschlossen hat, und wechseln Sie dann mit dem Switcher zwischen **Kompatibilitätsproblemen** und **Funktions Empfehlungen** .

2. Überprüfen Sie die Kompatibilitätsprobleme für alle Kompatibilitäts Grade, die **von der Ziel** SQL Server Version unterstützt werden.

Sie können Kompatibilitätsprobleme überprüfen, indem Sie das betroffene Objekt, die Details und ggf. eine Korrektur für jedes Problem, das unter wichtige **Änderungen**, **Verhaltensänderungen** und **Veraltete Features** identifiziert wird, analysieren.

![Bewertungsergebnisse anzeigen](../dma/media/dma-assesssqlonprem/review-results.png)

Auf ähnliche Weise können Sie die Funktions Empfehlung über **Leistungs**-, **Speicher**-und **Sicherheits** Bereiche hinweg überprüfen.

Funktions Empfehlungen behandeln verschiedene Arten von Features wie In-Memory OLTP, columnstore, Stretch Database, always encrypted, dynamische Datenmaskierung und transparent Data Encryption.

![Funktions Empfehlungen anzeigen](../dma/media/dma-assesssqlonprem/feature-recommendations.png)

Für Azure SQL-Datenbank bieten die Bewertungen Migrations Blockierungs Probleme und featureparitäts Probleme. Überprüfen Sie die Ergebnisse für beide Kategorien, indem Sie die spezifischen Optionen auswählen.

- Die Kategorie **SQL Server Featureparität** bietet eine umfassende Reihe von Empfehlungen, alternative Ansätze in Azure und Maßnahmen zur Minderung. Sie hilft Ihnen bei der Planung dieses Aufwands in ihren Migrationsprojekten.

  ![Anzeigen von Informationen zur SQL Server Featureparität](../dma/media/dma-assesssqlonprem/sql-feature-parity.png)

- Die Kategorie **Kompatibilitätsprobleme** bietet teilweise unterstützte oder nicht unterstützte Features, die die Migration von lokalen SQL Server Datenbanken zu Azure SQL-Datenbanken blockieren. Anschließend werden Empfehlungen bereitstellen, die Ihnen helfen, diese Probleme zu beheben.

  ![Kompatibilitätsprobleme anzeigen](../dma/media/dma-assesssqlonprem/compatibility-issues.png)

## <a name="assess-a-data-estate-for-target-readiness"></a>Bewerten eines Daten Estate für die Ziel Bereitschaft

Wenn Sie diese Bewertungen weiter auf den gesamten Datenbestand ausweiten und die relative Bereitschaft SQL Server Instanzen und Datenbanken für die Migration zu Azure SQL-Datenbank finden möchten, laden Sie die Ergebnisse Azure migrate in Azure migrate hoch, indem Sie auf **hochladen** klicken.

Auf diese Weise können Sie die konsolidierten Ergebnisse für das Azure migrate Hub-Projekt anzeigen.

Ausführliche Schritt-für-Schritt-Anleitungen für die Ziel Bereitschafts Bewertungen finden Sie [hier](./dma-assess-sql-data-estate-to-sqldb.md).

   ![Ergebnisse in Azure migrate hochladen](../dma/media/dma-assesssqlonprem/upload-to-azure-migrate.png)

## <a name="export-results"></a>Exportieren von Ergebnissen

Nachdem alle Datenbanken die Bewertung abgeschlossen haben, klicken Sie auf **Bericht exportieren** , um die Ergebnisse entweder in eine JSON-Datei oder in eine CSV-Datei zu exportieren. Anschließend können Sie die Daten in ihrer eigenen Weise analysieren.

## <a name="save-and-load-assessments"></a>Speichern und Laden von Bewertungen

Zusätzlich zum Exportieren der Ergebnisse einer Bewertung können Sie Bewertungs Details in einer Datei speichern und eine Bewertungs Datei zur späteren Überprüfung laden.  Weitere Informationen finden Sie im Artikel [Speichern und Laden von Bewertungen mit Datenmigrations-Assistent](../dma/dma-save-load-assessments.md).
