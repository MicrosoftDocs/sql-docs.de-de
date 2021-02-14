---
description: Verwalten von Stretch Database und Behandeln von Problemen
title: Verwaltung und Problembehandlung
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, managing
- Stretch Database, troubleshooting
- managing Stretch Database
- troubleshooting Stretch Database
ms.assetid: 6334db3e-9297-44df-8d53-211187a95520
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 346bc660bc1fa1fd9a6e75a91caa182ff25e6c53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081029"
---
# <a name="manage-and-troubleshoot-stretch-database"></a>Verwalten von Stretch Database und Behandeln von Problemen
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Verwenden Sie die in diesem Artikel beschriebenen Tools und Methoden, um Stretch Database zu verwalten und Probleme zu behandeln.  
## <a name="manage-local-data"></a>Verwalten von lokalen Daten  
  
###  <a name="get-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a> Abrufen von Informationen zu lokalen Datenbanken und Tabellen, die für Stretch Database aktiviert sind  
 Öffnen Sie die Katalogsichten **sys.databases** und **sys.tables** , um Informationen über SQL Server-Datenbanken und -Tabellen zu finden, die für Stretch aktiviert sind. Weitere Informationen finden Sie unter [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) und [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md).  
 
 Führen Sie die folgende Anweisung aus, um zu ermitteln, wie viel Speicherplatz eine Stretch-aktivierte Tabelle in SQL Server verwendet.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
   
## <a name="manage-data-migration"></a>Verwalten der Datenmigration  
  
### <a name="check-the-filter-function-applied-to-a-table"></a>Überprüfen der auf eine Tabelle angewendeten Filterfunktion  
 Öffnen Sie die Katalogsicht **sys.remote_data_archive_tables** , und überprüfen Sie den Wert der **filter_predicate** -Spalte, um die Funktion zu identifizieren, die von Stretch Database verwendet wird, um die zu migrierenden Zeilen auszuwählen. Wenn der Wert null ist, ist die gesamte Tabelle für eine Migration berechtigt. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md) und [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
###  <a name="check-the-status-of-data-migration"></a><a name="Migration"></a> Überprüfen des Status der Datenmigration  
 Wählen Sie **Aufgaben | Stretch | Überwachung** für eine Datenbank in SQL Server Management Studio, um die Datenmigration im Stretch Database-Monitor zu überwachen. Weitere Informationen finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 Oder öffnen Sie die dynamische Verwaltungssicht **sys.dm_db_rda_migration_status** , um anzuzeigen, wie viele Batches und Datenzeilen migriert wurden.  
  
###  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> Problembehandlung der Datenmigration  
 Weitere Empfehlungen zur Problembehandlung finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
## <a name="manage-remote-data"></a>Verwalten von Remotedaten  
  
###  <a name="get-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a> Abrufen von Informationen zu Remotedatenbanken und Tabellen, die von Stretch Database verwendet werden  
 Öffnen Sie die Katalogsichten **sys.remote_data_archive_databases** und **sys.remote_data_archive_tables** , um Informationen über die Remotedatenbanken und Tabellen zu finden, in denen migrierte Daten gespeichert sind. Weitere Informationen finden Sie unter [sys.remote_data_archive_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md) und [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
 
Führen Sie die folgende Anweisung aus, um zu ermitteln, wie viel Speicherplatz eine Stretch-aktivierte Tabelle in Azure verwendet.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Löschen von migrierten Daten  
Wenn Sie bereits zu Azure migrierte Daten löschen möchten, befolgen Sie die in [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)beschriebenen Schritte.  
  
## <a name="manage-table-schema"></a>Verwalten von Tabellenschemas

### <a name="dont-change-the-schema-of-the-remote-table"></a>Verändern Sie das Schema der Remotetabelle nicht  
 Ändern Sie nicht das Schema einer Azure-Remotetabelle, die mit einer SQL Server-Tabelle verknüpft ist, die für Stretch Database konfiguriert wurde. Ändern Sie insbesondere nicht den Namen oder den Datentyp einer Spalte. Für die Stretch Database-Funktion gelten verschiedene Annahmen hinsichtlich des Schemas einer Remotetabelle in Bezug auf das Schema der SQL Server-Tabelle. Wenn Sie das Remoteschema ändern, funktioniert Stretch Database nicht mehr für die geänderte Tabelle.  

### <a name="reconcile-table-columns"></a>Abstimmen von Tabellenspalten  
Wenn Sie Spalten aus der Remotetabelle versehentlich gelöscht haben, führen Sie **sp_rda_reconcile_columns** aus, um Spalten zur Remotetabelle hinzuzufügen, die in der Stretch-aktivierten SQL Server-Tabelle, aber nicht in der Remotetabelle vorhanden sind. Weitere Informationen finden Sie unter [sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md).  
  
  > [!IMPORTANT]
  > Wenn **sp_rda_reconcile_columns** Spalten neu erstellt, die Sie versehentlich aus der Remotetabelle gelöscht haben, dann werden nicht die zuvor in den gelöschten Spalten enthaltenen Daten wiederhergestellt.
  
**sp_rda_reconcile_columns** löscht keine Spalten aus der Remotetabelle, die in der Remotetabelle, aber nicht in der Stretch-aktivierten SQL Server-Tabelle vorhanden sind. Wenn die Azure-Remotetabelle Spalten enthält, die in der Stretch-fähigen SQL Server-Tabelle nicht mehr vorhanden sind, verhindern diese zusätzlichen Spalten nicht die normale Funktionsweise von Stretch Database. Sie können die zusätzlichen Spalten optional manuell entfernen.  
 
## <a name="manage-performance-and-costs"></a>Verwalten von Leistung und Kosten  
  
### <a name="troubleshoot-query-performance"></a>Problembehandlung der Abfrageleistung  
  Bei Abfragen, die Stretch-aktivierte Tabellen enthalten, wird erwartet, dass sie langsamer ausgeführt werden, als zum Zeitpunkt bevor die Tabellen für Stretch aktiviert wurden. Falls die Abfrageleistung erheblich beeinträchtigt wird, überprüfen Sie die folgenden möglichen Probleme.  
  
-   Befindet sich Ihr Azure-Server in einer anderen geografischen Region als Ihr SQL Server? Konfigurieren Sie Ihren Azure-Server dahingehend, dass er sich in der gleichen geografischen Region wie Ihr SQL Server befindet, um die Netzwerklatenz zu reduzieren.  
  
-   Möglicherweise haben sich Ihre Netzwerkbedingungen verschlechtert. Wenden Sie sich an Ihren Netzwerkadministrator, um Informationen über aktuelle Probleme oder Ausfälle zu erhalten.  
  
### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Erhöhen der Azure-Leistungsstufe für ressourcenintensive Vorgänge wie z.B. Indizierung  
 Wenn Sie einen Index für eine große Tabelle, die für Stretch Database konfiguriert ist, erstellen, neu erstellen oder neu organisieren, und während des Vorgangs umfangreiche Abfragen der migrierten Daten in Azure erwarten, sollten Sie die Steigerung der Leistung der entsprechenden Azure-Remotedatenbank für die Dauer des Vorgangs in Betracht ziehen. Weitere Informationen zu Leistungsstufen und Preisen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Sie können den SQL Server Stretch Database-Dienst in Azure nicht anhalten.  
 Stellen Sie sicher, dass Sie die entsprechenden Leistungs- und Preisstufen auswählen. Wenn Sie die Leistungsstufe für einen ressourcenintensiver Vorgang vorübergehend erhöhen, stellen Sie die vorherige Stufe nach Abschluss des Vorgangs wieder her. Weitere Informationen zu Leistungsstufen und Preisen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
   
 ## <a name="change-the-scope-of-queries"></a>Ändern des Bereichs von Abfragen  
 Abfragen für Stretch-aktivierte Tabellen geben standardmäßig sowohl lokale als auch Remotedaten zurück. Sie können den Bereich von Abfragen für alle Abfragen von allen Benutzern oder nur für eine einzelne Abfrage von einem Administrator ändern.  
   
 ### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Ändern des Bereichs von Abfragen für alle Abfragen von allen Benutzern  
 Um den Bereich aller Abfragen von allen Benutzern zu ändern, führen Sie die gespeicherte Prozedur **sys.sp_rda_set_query_mode** aus. Sie können den Bereich eingrenzen, sodass nur lokale Daten abgefragt werden, und Sie können alle Abfragen deaktivieren oder die Standardeinstellung wiederherstellen. Weitere Informationen finden Sie unter [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md).  
   
 ### <a name="change-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>Ändern des Abfragebereichs für eine einzelne Abfrage durch einen Administrator  
 Zum Ändern des Bereichs einer einzelnen Abfrage von einem Mitglied der Rolle „db_owner“ fügen Sie den Abfragehinweis ***WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE =* value)** zur SELECT-Anweisung hinzu. Der REMOTE_DATA_ARCHIVE_OVERRIDE-Abfragehinweis kann die folgenden Werte enthalten.  
 -   **LOCAL_ONLY**. Es werden nur lokale Daten abgefragt.  
   
 -   **REMOTE_ONLY**. Es werden nur Remotedaten abgefragt.  
   
 -   **STAGE_ONLY**. Es werden nur die Daten in der Tabelle abgefragt, in der Stretch Database für die Migration berechtigte Zeilen bereitstellt und migrierte Zeilen für den angegebenen Zeitraum nach der Migration beibehält. Dieser Abfragehinweis ist die einzige Möglichkeit zum Abfragen der Stagingtabelle.  
  
Die folgende Abfrage gibt z. B. nur lokale Ergebnisse zurück.  
  
 ```sql  
USE <Stretch-enabled database name>;
GO
SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
GO
```  
   
 ## <a name="make-administrative-updates-and-deletes"></a><a name="adminHints"></a>Ausführen administrativer Updates und Löschvorgänge  
 Die Befehle UPDATE oder DELETE können standardmäßig nicht für zur Migration berechtigte Zeilen oder für bereits migrierte Zeilen in einer Stretch-fähigen Tabelle ausgeführt werden. Wenn Sie ein Problem beheben müssen, kann ein Mitglied der Rolle „db_owner“ einen UPDATE- oder DELETE-Vorgang durch Hinzufügen des Abfragehinweises ***WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE =* value)** zur Anweisung ausführen. Der REMOTE_DATA_ARCHIVE_OVERRIDE-Abfragehinweis kann die folgenden Werte enthalten.  
 -   **LOCAL_ONLY**. Aktualisieren oder löschen Sie nur lokale Daten.  
   
 -   **REMOTE_ONLY**. Aktualisieren oder löschen Sie nur Remotedaten.  
   
 -   **STAGE_ONLY**. Es werden nur die Daten in der Tabelle aktualisiert oder gelöscht, in der Stretch Database für die Migration berechtigte Zeilen bereitstellt und migrierte Zeilen für den angegebenen Zeitraum nach der Migration beibehält.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachung und Problembehandlung bei der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)   
[Sichern von Stretch-aktivierten Datenbanken (Stretch Database)](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
[Wiederherstellen von Stretch-aktivierten Datenbanken (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
  
