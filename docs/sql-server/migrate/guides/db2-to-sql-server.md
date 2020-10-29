---
title: 'Migrationsleitfaden: DB2 zu SQL Server'
description: Befolgen Sie diesen Leitfaden, um Ihren DB2-Server zu SQL Server zu migrieren.
ms.custom: ''
ms.date: 08/17/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- processors [SQL Server], supported
- number of processors supported
- maximum number of processors supported
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4a4aa024d94908800c020fdc5d2362d48d03becd
ms.sourcegitcommit: b09f069c6bef0655b47e9953a4385f1b52bada2b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92734665"
---
# <a name="migration-guide-db2-to-sql-server"></a>Migrationsleitfaden: DB2 zu SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

In diesem Migrationsleitfaden erfahren Sie, wie Sie Ihre Benutzerdatenbanken mithilfe von SQL Server Migration Assistant for DB2 von DB2 zu SQL Server migrieren. 

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](https://datamigration.microsoft.com/). 


## <a name="prerequisites"></a>Voraussetzungen

Sie müssen zunächst die folgenden Schritte ausführen, um Ihre DB2-Datenbank zu SQL Server migrieren zu können:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird.
- [SQL Server Migration Assistant (SSMA) for DB2](https://www.microsoft.com/download/details.aspx?id=54254)



## <a name="pre-migration"></a>Vor der Migration

Wenn diese Voraussetzungen erfüllt sind, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit der Migration bewerten. 

### <a name="assess"></a>Bewerten 

Erstellen Sie mit SQL Server Migration Assistant (SSMA) eine Bewertung. 

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen:

1. Öffnen Sie SQL Server Migration Assistant (SSMA) for DB2. 
1. Klicken Sie auf **File** (Datei) und dann auf **New Project** (Neues Projekt). 
1. Geben Sie einen Projektnamen und einen Speicherort für das Projekt an, und wählen Sie dann in der Dropdownliste ein SQL Server-Migrationsziel aus. Klicken Sie auf **OK** . 

   :::image type="content" source="media/db2-to-sql-server/new-project.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::


1. Geben Sie im Dialogfeld **Connect to DB2** (Mit DB2 verbinden) die Werte für die DB2-Verbindungsdetails ein. 

   :::image type="content" source="media/db2-to-sql-server/connect-to-db2.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::


1. Klicken Sie mit der rechten Maustaste auf das DB2-Schema, das Sie migrieren möchten, und klicken Sie dann auf **Create report** (Bericht erstellen). Dadurch wird ein HTML-Bericht generiert. Alternativ können Sie nach dem Auswählen des Schemas in der Navigationsleiste auf **Create report** (Bericht erstellen) klicken. 

   :::image type="content" source="media/db2-to-sql-server/create-report.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um ein Inventar der DB2-Objekte sowie Informationen zum für die Durchführung von Schemakonvertierungen erforderlichen Aufwand zu erhalten. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects.

   Beispiel: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-server/report.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::


### <a name="validate-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie sie bei Bedarf basierend auf den Anforderungen. Gehen Sie dazu folgendermaßen vor: 

1. Klicken Sie im Menü auf **Tools** . 
1. Klicken auf **Project Settings** (Projekteinstellungen). 
1. Klicken Sie auf die Registerkarte **Type Mappings** (Typzuordnungen). 

   :::image type="content" source="media/db2-to-sql-server/type-mapping.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Sie können die Typzuordnung für die einzelnen Tabellen ändern, indem Sie die gewünschte Tabelle im **DB2 Metadata Explorer** (DB2-Metadaten-Explorer) auswählen. 

### <a name="convert-schema"></a>Schema konvertieren 

Führen Sie die folgenden Schritte aus, um das Schema zu konvertieren:

1. (Optional:) Fügen Sie Anweisungen dynamische Abfragen oder Ad-hoc-Abfragen hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten, und klicken Sie dann auf **Add statements** (Anweisungen hinzufügen). 
1. Klicken Sie auf **Connect to SQL Server** (Mit SQL Server verbinden). 
    1. Geben Sie die Verbindungsdetails ein, um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen. 
    1. Stellen Sie entweder eine Verbindung mit einer bereits vorhandenen Datenbank auf dem Zielserver her, oder geben Sie einen neuen Namen an, um eine neue Datenbank auf dem Zielserver zu erstellen. 
    1. Wählen Sie **Verbinden** aus. 

   :::image type="content" source="media/db2-to-sql-server/connect-to-sql-server.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::


1. Klicken Sie mit der rechten Maustaste auf das Schema, und klicken Sie dann auf **Convert Schema** (Schema konvertieren). Alternativ können Sie nach dem Auswählen des Schemas in der oberen Navigationsleiste auf **Convert Schema** (Schema konvertieren) klicken. 

   :::image type="content" source="media/db2-to-sql-server/convert-schema.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Vergleichen Sie nach Abschluss der Konvertierung die neue Schemastruktur mit der alten, und überprüfen Sie sie, um mögliche Probleme zu identifizieren und basierend auf den Empfehlungen zu behandeln. 

   :::image type="content" source="media/db2-to-sql-server/compare-review-schema-structure.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Klicken Sie im Menü **File** (Datei) auf **Save Project** (Projekt speichern). 


## <a name="migrate"></a>Migrate

Wenn Sie die Bewertung Ihrer Datenbanken und die Behandlung etwaiger Abweichungen abgeschlossen haben, besteht der nächste Schritt in der Durchführung des Migrationsprozesses.

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und Ihre Daten zu migrieren:

1. Veröffentlichen des Schemas: Klicken Sie im **SQL Server Metadata Explorer** (SQL Server-Metadaten-Explorer) im Knoten **Databases** (Datenbanken) mit der rechten Maustaste auf die Datenbank, und klicken Sie dann auf **Synchronize with Database** (Mit Datenbank synchronisieren).

   :::image type="content" source="media/db2-to-sql-server/synchronize-with-database.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Migrieren der Daten: Klicken Sie im **DB2 Metadata Explorer** (DB2-Metadaten-Explorer) mit der rechten Maustaste auf das Schema, und klicken Sie dann auf **Migrate Data** (Daten migrieren). 

   :::image type="content" source="media/db2-to-sql-server/migrate-data.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Geben Sie die Verbindungsdetails für die DB2- und die SQL Server-Instanz an. 
1. Zeigen Sie den **Datenmigrationsbericht** an. 

   :::image type="content" source="media/db2-to-sql-server/data-migration-report.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

1. Stellen Sie über das SQL Server Management Studio eine Verbindung mit Ihrer SQL Server-Instanz her, und überprüfen Sie die Migration durch Überprüfen der Daten und des Schemas. 

   :::image type="content" source="media/db2-to-sql-server/compare-schema-in-ssms.png" alt-text="Geben Sie die Projektdetails an, und klicken Sie zum Speichern auf „OK“.":::

## <a name="post-migration"></a>Nach der Migration 

Nach dem erfolgreichen Abschluss der Migrationsphase müssen Sie eine Reihe von Aufgaben nach der Migration ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen 

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests** : Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
1. **Einrichten der Testumgebung** : Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
1. **Ausführen der Validierungstests** : Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.
1. **Ausführen von Leistungstests** : Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

   > [!NOTE]
   > Für Unterstützung bei der Entwicklung und Ausführung von Validierungstests nach der Migration sollten Sie die Datenqualitätslösung des Partners [QuerySurge](https://www.querysurge.com/company/partners/microsoft) in Erwägung ziehen. 

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung erhalten Sie in Form der folgenden Ressourcen, die im Rahmen eines Auftrags für ein echtes Migrationsprojekt entwickelt wurden:

|Asset  |BESCHREIBUNG  |
|---------|---------|
|[Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Dieses Tool stellt für eine angegebene Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch Bereitstellung eines automatisierten und einheitlichen Zielplattform-Entscheidungsprozesses dabei helfen, umfangreiche Bewertungen zu beschleunigen.|
|[DB2 z/OS Data Assets Discovery and Assessment Package (Paket zur Ermittlung und Bewertung von DB2 z/OS-Datenressourcen)](https://github.com/Microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|Nach dem Ausführen des SQL-Skripts für eine-Datenbank können Sie die Ergebnisse in eine Datei im Dateisystem exportieren. Es werden verschiedene Dateiformate unterstützt, z. B. *.csv, damit Sie die Ergebnisse in externen Tools wie Tabellen erfassen können. Diese Methode kann nützlich sein, wenn Sie Ergebnisse einfach für Teams freigeben möchten, die die Workbench nicht installiert haben.|
|[IBM DB2 LUW Inventory Scripts and Artifacts (IBM DB2 LUW-Inventarskripts und -artefakte)](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Diese Ressource umfasst eine SQL-Abfrage für die Systemtabellen von Version 11.1 von IBM DB2 LUW und bietet eine Zählung von Objekten nach Schema und Objekttyp, eine grobe Schätzung für „Rohdaten“ in jedem Schema und die Dimensionierung von Tabellen in jedem Schema, wobei die Ergebnisse im CSV-Format gespeichert werden.|
|[Leitfaden für die Einrichtung von DB2 LUW pureScale in Azure](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Dieser Leitfaden dient als Ausgangspunkt für einen DB2-Implementierungsplan. Auch wenn Geschäftsanforderungen unterschiedlich sind, gilt dennoch dasselbe grundlegende Muster. Dieses Architekturmuster kann auch für OLAP-Anwendungen in Azure verwendet werden.|

Diese Ressourcen wurden im Rahmen des Data SQL Ninja-Programms entwickelt, das vom Azure Data Group-Entwicklungsteam gesponsert wird. Der Hauptzweck des Data SQL Ninja-Programms besteht darin, Hindernisse für komplexe Modernisierung zu beseitigen und Letztere zu beschleunigen sowie den Wettbewerb in Bezug auf Möglichkeiten zur Migration von Datenplattformen zur Azure-Datenplattform von Microsoft zu fördern. Wenn Sie denken, dass Ihre Organisation an einer Teilnahme am Data SQL Ninja-Programm interessiert wäre, wenden Sie sich an Ihr Kundenteam, und bitten Sie es, eine Nominierung einzureichen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich nach der Migration den [Leitfaden für die Überprüfung und Optimierung nach der Migration](../../../relational-databases/post-migration-validation-and-optimization-guide.md) an. 

Eine Übersicht über die verfügbaren Dienste und Tools von Microsoft und Drittanbietern, mit deren Hilfe Sie verschiedene Datenbank- und Datenmigrationsszenarios und Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für Datenmigrationsszenarios](/azure/dms/dms-tools-matrix).

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](https://datamigration.microsoft.com/). 

Videoinhalte finden Sie unter:
- [Verwenden des Leitfadens zur Azure-Datenbankmigration](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)
- [Übersicht über den Migrationsprozess und die für das Durchführen einer Bewertung und Migration empfohlenen Tools und Dienste](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
