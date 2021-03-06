---
title: Bewerten der SQL Server Bereitschaft für die Migration zu Azure SQL-Datenbank
titleSuffix: Data Migration Assistant
description: Erfahren Sie, wie Sie Datenmigrations-Assistent zum Migrieren eines SQL Server Data Estate für die Migration zu Azure SQL-Datenbank verwenden.
ms.date: 12/19/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 1094f10eedc819123cd446c0933ec14df511ac75
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247345"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>Bewerten Sie die Bereitschaft einer SQL Server-Datenbankmigration zu Azure SQL-Datenbank mithilfe des Datenmigrations-Assistent

Das Migrieren von Hunderten von SQL Server Instanzen und Tausenden von Datenbanken zu Azure SQL-Datenbank oder Azure SQL verwaltete Instanz, unsere Platform-as-a-Service-Angebote (PAS), ist eine beträchtliche Aufgabe. Um den Prozess so weit wie möglich zu optimieren, müssen Sie sich auf die relative Bereitschaft der Migration verlassen. Die Identifizierung von Obst Früchten, einschließlich der Server und Datenbanken, die vollständig bereit sind oder nur minimalen Aufwand zur Vorbereitung der Migration erfordern, vereinfacht und beschleunigt Ihre Anstrengungen.

Dieser Artikel enthält Schritt-für-Schritt-Anleitungen für die Verwendung der [Datenmigrations-Assistent](./dma-overview.md) , um Bereitschafts Ergebnisse zusammenzufassen und auf dem [Azure migrate](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) Hub zu nutzen.

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>Erstellen eines Projekts und Hinzufügen eines Tools

Richten Sie ein neues Azure migrate-Projekt in einem Azure-Abonnement ein, und fügen Sie dann ein Tool hinzu.

Ein Azure Migrate-Projekt wird zum Speichern von Ermittlungs-, Bewertungs- und Migrationsmetadaten verwendet, die in der von Ihnen bewerteten oder migrierten Umgebung gesammelt werden. Außerdem verwenden Sie ein Projekt, um ermittelte Objekte zu verfolgen und die Bewertung und Migration zu orchestrieren.

1. Melden Sie sich beim Azure-Portal an, wählen Sie **alle Dienste** aus, und suchen Sie dann nach Azure migrate.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.

   ![Azure migrate Dienst auswählen](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. Wählen Sie auf der Seite **Übersicht** die Option **Datenbanken bewerten und Migrieren** aus.

   ![Azure migrate Initiieren der Bewertung](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. Wählen Sie unter " **Datenbanken**" unter " **Getting Started**" **(e) Tools hinzufügen** aus.

   ![Tools zum Azure migrate hinzufügen](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. Wählen Sie auf der Registerkarte **Projekt migrieren** Ihr Azure-Abonnement und ihre Ressourcengruppe aus (wenn Sie noch nicht über eine Ressourcengruppe verfügen, erstellen Sie eine).
6. Geben Sie unter **Project Details (Projekt Details**) den Projektnamen und den geography an, in dem Sie das Projekt erstellen möchten.

    ![Azure migrate-Dialogfeld "Tool hinzufügen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    Sie können Azure Migrate-Projekte in den unten angegebenen geografischen Regionen erstellen.

    | **Geografie**  | **Region des Speicherorts** |
    | ------------- | ------------- |
    | Asia | „Asien, Südosten“ und „Asien, Osten“ |
    | Europa | Europa, Süden oder Europa, Westen |
    | United Kingdom | „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“ |
    | USA | „USA, Mitte“ oder „USA, Westen 2“ |

    Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. Bei der tatsächlichen Migration kann eine beliebige Zielregion ausgewählt werden.

7. Wählen Sie **weiter** aus, und fügen Sie dann ein Bewertungstool hinzu.

   > [!NOTE]
   > Wenn Sie ein Projekt erstellen, müssen Sie mindestens ein Bewertungs-oder Migrationstool hinzufügen.

8. Auf der Registerkarte **Bewertungstool auswählen** **Azure migrate: Daten Bank Bewertung** wird als Bewertungstool angezeigt, das hinzugefügt werden soll. Wenn Sie zurzeit kein Bewertungstool benötigen, aktivieren Sie das Kontrollkästchen **Hinzufügen eines Assessment-Tools für jetzt** . Wählen Sie **Weiter** aus.

    ![Azure migrate-Registerkarte "Bewertungstool auswählen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. Auf der Registerkarte **Migrationstool auswählen** wird **Azure migrate: die Datenbankmigration** wird als Migrationstool angezeigt, das hinzugefügt werden soll. Wenn Sie derzeit kein Migrationstool benötigen, wählen Sie das **Tool zum Hinzufügen eines Migrationstools aus**. Wählen Sie **Weiter** aus.

    ![Registerkarte "Migrationstool Azure migrate auswählen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. Überprüfen Sie die Einstellungen, und wählen **Sie Tools hinzu** **fügen** aus.

    ![Registerkarte "Azure migrate-Überprüfung + Tool (s) hinzufügen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    Nachdem Sie das Projekt erstellt haben, können Sie zusätzliche Tools für Bewertung und Migration von Servern, Workloads, Datenbanken und Web-Apps auswählen.

## <a name="assess-and-upload-assessment-results"></a>Bewerten und Hochladen der Bewertungsergebnisse

Nachdem Sie erfolgreich ein Migrationsprojekt erstellt haben, können Sie unter **Bewertungs Tools** im Feld **Azure migrate: Daten Bank Bewertung** die Anweisungen zum herunterladen und Verwenden der Anzeige des Datenmigrations-Assistent Tools anzeigen.

   ![Azure migrate Assessment-Tool hinzugefügt](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. Laden Sie Datenmigrations-Assistent über den bereitgestellten Link herunter, und installieren Sie ihn auf einem Computer mit Zugriff auf die Quell SQL Server Instanzen.
2. Öffnen Sie den Datenmigrations-Assistenten.

### <a name="create-an-assessment"></a>Erstellen einer Bewertung

1. Wählen Sie auf der linken Seite das **+** Symbol aus, und wählen Sie dann den **Projekttyp** Bewertung aus.
2. Geben Sie den Projektnamen an, und wählen Sie dann die Typen Quell Server und Zielserver aus.

    Wenn Sie die lokale SQL Server Instanz auf eine höhere Version von SQL Server oder auf SQL Server, die auf einer Azure-VM gehostet wird, aktualisieren, legen Sie den Quell-und den Ziel Servertyp auf **SQL Server** fest. Legen Sie den Ziel Servertyp auf **Azure SQL-verwaltete Instanz** für eine Ziel Bereitschafts Bewertung von Azure SQL-Datenbank (PAS) fest.

3. Klicken Sie auf **Erstellen**.

   ![Azure migrate-Datenmigrations-Assistent-Schnittstelle](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>Bewertungs Optionen auswählen

1. Wählen Sie den Berichtstyp aus.

    Sie können einen oder beide der folgenden Berichts Typen auswählen:
    * Check database compatibility (Datenbankkompatibilität prüfen)
    * Check feature parity (Featureparität prüfen)

   ![Bildschirm "Azure migrate-Datenmigrations-Assistent-Bewertungs Optionen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. Wählen Sie **Weiter** aus.

### <a name="add-databases-to-assess"></a>Hinzufügen zu bewertender Datenbanken

1. Wählen Sie **Quellen hinzufügen** aus, um das Menü mit der Verbindung zu öffnen.
2. Geben Sie den SQL Server-Instanznamen ein, wählen Sie den Authentifizierungstyp, legen Sie die richtigen Verbindungs Eigenschaften fest, und wählen Sie dann **verbinden**
3. Wählen Sie die zu überprüfen Datenbanken aus, und wählen Sie dann **Hinzufügen**

   > [!NOTE]
   > Wenn Sie die UMSCHALT-oder STRG-Taste gedrückt halten, können Sie mehrere Datenbanken entfernen, indem Sie Sie auswählen und dann auf Quellen entfernen klicken. Mithilfe der Schaltfläche Quellen hinzufügen können Sie auch Datenbanken aus mehreren SQL Server Instanzen hinzufügen.

4. Wählen Sie **Weiter** aus, um die Bewertung zu starten.

   ![Bildschirm "Azure migrate-Datenmigrations-Assistent-Quellen auswählen"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. Nachdem die Bewertung abgeschlossen ist, wählen Sie **Hochladen in Azure migrate** aus.

   ![Screenshot, der die Datenmigrations-Assistent anzeigt, bei der die Option Upload to Azure migrate aufgerufen wird.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. Melden Sie sich beim Azure-Portal an.

   ![Screenshot der Datenmigrations-Assistent, die das Azure-Portal Anmeldefenster anzeigt.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. Wählen Sie das Abonnement und Azure migrate Projekt aus, in das Sie die Bewertungsergebnisse hochladen möchten, und wählen Sie dann **hochladen** aus.

   Warten Sie, bis die bestätigungsbestätigung bestätigt wurde.

## <a name="view-target-readiness-assessment-results"></a>Ergebnisse der Ziel Bereitschafts Bewertung anzeigen

1. Melden Sie sich beim Azure-Portal an, suchen Sie nach Azure migrieren, und wählen Sie **Azure migrate** aus.

   ![Azure migrate-Azure-Portal-Dienst Suche](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. Wählen Sie **Datenbanken bewerten und Migrieren** aus, um die Bewertungsergebnisse zu erhalten.

   ![Bewertungsergebnisse Azure migrate Review](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    Sie können die Zusammenfassung der SQL Server Bereitschaft anzeigen, sicherstellen, dass Sie das richtige Migrationsprojekt verwenden. andernfalls verwenden Sie die Option "ändern", um ein anderes Migrationsprojekt auszuwählen.

    Jedes Mal, wenn Sie die Bewertungsergebnisse in das Azure-Migrationsprojekt aktualisieren, konsolidiert der Azure-Migrations-Hub alle Ergebnisse und stellt den Zusammenfassungs Bericht bereit.  Sie können mehrere Datenmigrations-Assistent Bewertungen parallel ausführen und die Ergebnisse in das einzelne Migrationsprojekt hochladen, um den konsolidierten Bereitschafts Bericht zu erhalten.

   ![Azure migrate Überprüfungs Ergebnisse](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **Bewertete Datenbankinstanzen**: die Anzahl der bisher bewerteten SQL Server Instanzen.
    **Bewertete Datenbanken**: die Gesamtanzahl von Datenbanken, die für eine oder mehrere SQL Server Instanzen bewertet wurden, die **für die SQL-Datenbank bereit** sind: die Anzahl von Datenbanken, die für die Migration zu Azure SQL-Datenbank ("
    **Für Azure SQL-VM geeignete Datenbanken**: die Anzahl der Datenbanken besteht aus mindestens einem Migrations Blockierer in Azure SQL-Datenbank (Azure SQL-Datenbank), aber bereit für die Migration zu Azure SQL Server virtuellen Computern.

3. Wählen Sie **bewertete Datenbankinstanzen** aus, um zur Ansicht SQL Server Instanzebene zu gelangen.

   ![Azure migrate Überprüfen der instanzbereitschaft](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    Sie können den Status der prozentualen Bereitstellung für jede SQL Server Instanz ermitteln, die zu Azure SQL-Datenbank (PAS) migriert wird.

4. Wählen Sie eine bestimmte-Instanz aus, um zur Ansicht der Daten Bank Bereitschaft zu gelangen.

   ![Daten Bank Bereitschaft Azure migrate überprüfen](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    In der obigen Ansicht wird die Anzahl der Migrations Blockierer pro Datenbank angezeigt. Dies ist das empfohlene Ziel für jede Datenbank.

5. Überprüfen Sie die DMA-Bewertungsergebnisse, um weitere Details zu den Migrations blockatoren zu erhalten.

   ![Migrations Blockierer Azure migrate überprüfen](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>Weitere Informationen

* [Datenmigrations-Assistent (DMA)](../dma/dma-overview.md)
* [Datenmigrations-Assistent: Konfigurationseinstellungen](../dma/dma-configurationsettings.md)
* [Datenmigrations-Assistent: bewährte Methoden](../dma/dma-bestpractices.md)
