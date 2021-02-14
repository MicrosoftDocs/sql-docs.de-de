---
description: Aktivieren von Stretch Database für eine Tabelle
title: Aktivieren von Stretch Database für eine Tabelle
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling table
- enabling table for Stretch Database
ms.assetid: de4ac0c5-46ef-4593-a11e-9dd9bcd3ccdc
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 29d4e1d6e2de7c575b9549f13dc325add4f5ca88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081106"
---
# <a name="enable-stretch-database-for-a-table"></a>Aktivieren von Stretch Database für eine Tabelle
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Wählen Sie **Stretch &gt; Aktivieren** aus, um eine Tabelle für Stretch Database zu konfigurieren. So wird in SQL Server Management Studio für eine Tabelle der Assistent zum **Aktivieren der Tabelle für Stretch** geöffnet. Sie können Transact-SQL auch verwenden, um Stretch Database für eine vorhandene Tabelle zu aktivieren oder um eine neue Tabelle zu erstellen, in der Stretch Database aktiviert ist.  
  
-   Wenn Sie kalte Daten in einer separaten Tabelle speichern, können Sie die gesamte Tabelle migrieren.  
  
-   Wenn Ihre Tabelle sowohl heiße als auch kalte Daten enthält, können Sie eine Filterfunktion zum Auswählen der zu migrierenden Zeilen angeben.    
 
 **Voraussetzungen** Wenn Sie **Stretch | Aktivieren** für eine Tabelle auswählen, Stretch Database bisher jedoch noch nicht für die Datenbank aktiviert war, konfiguriert der Assistent zuerst die Datenbank für Stretch Database. Führen Sie die Schritte unter [Erste Schritte durch Ausführen des Assistenten zum Aktivieren von Stretch für eine Datenbank](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md) anstelle der in diesem Artikel aufgeführten Schritte durch.  
  
 **Berechtigungen** Um Stretch Database für eine Datenbank oder eine Tabelle zu aktivieren, benötigen Sie "db_owner"-Berechtigungen. Zum Aktivieren von Stretch Database für eine Tabelle sind auch ALTER-Berechtigungen für die Tabelle erforderlich.  

 > [!NOTE]
 > Bedenken Sie später beim Deaktivieren von Stretch Database Folgendes: Wenn Sie Stretch Database für eine Tabelle oder eine Datenbank deaktivieren, wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals löschen. Die Remoteobjekte erzeugen weiterhin Azure-Kosten, bis Sie die Objekte manuell löschen.
 
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-table"></a><a name="EnableWizardTable"></a> Verwenden des Assistenten zum Aktivieren von Stretch Database für eine Tabelle  
 **Starten des Assistenten**  
 1.  Wählen Sie im Objekt-Explorer in SQL Server Management Studio die Tabelle aus, für die Sie Stretch aktivieren möchten.  
  
2.  Klicken Sie mit der rechten Maustaste, wählen Sie **Stretch**, und wählen Sie anschließend **Aktivieren** aus, um den Assistenten zu starten.  
  
 **Introduction (Einführung)**  
 Überprüfen Sie den Zweck des Assistenten und die Voraussetzungen.  
  
 **Auswählen von Datenbanktabellen**  
 Vergewissern Sie sich, dass die Tabelle, die Sie aktivieren möchten, angezeigt wird und ausgewählt ist.  
  
 Sie können eine ganze Tabelle migrieren oder eine einfache Filterfunktion im Assistenten angeben. Wenn Sie zum Auswählen zu migrierender Zeilen eine andere Art von Filterfunktion verwenden möchten, führen Sie einen der folgenden Schritte aus.  
  
-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung aus, um Stretch für die Tabelle zu aktivieren und eine Filterfunktion anzugeben.  
  
-   Führen Sie die ALTER TABLE-Anweisung aus, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten beendet haben. Die erforderlichen Schritte finden Sie unter [Hinzufügen einer Filterfunktion nach Ausführen des Assistenten](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
 Die ALTER TABLE-Syntax wird im weiteren Verlauf dieses Artikels beschrieben.  
  
 **Zusammenfassung**  
 Überprüfen Sie die im Assistenten eingegebenen Werte und ausgewählten Optionen. Wählen Sie dann **Fertig stellen** aus, um Stretch zu aktivieren.  
  
 **Ergebnisse**  
 Überprüfen Sie die Ergebnisse.  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-table"></a><a name="EnableTSQLTable"></a> Verwenden von Transact-SQL zum Aktivieren von Stretch Database für eine Tabelle  
 Sie können Stretch Database mithilfe von Transact-SQL für eine vorhandene Tabelle aktivieren oder eine neue Tabelle erstellen, in der Stretch Database aktiviert ist.  
  
### <a name="options"></a>Optionen  
 Verwenden Sie die folgenden Optionen beim Ausführen von CREATE TABLE oder ALTER TABLE, um Stretch Database für eine Tabelle zu aktivieren.  
  
-   Verwenden Sie optional die `FILTER_PREDICATE = <function>` -Klausel, um eine Funktion zum Auswählen der zu migrierenden Zeilen anzugeben, wenn die Tabelle sowohl heiße als auch kalte Daten enthält. Das Prädikat muss eine Inline-Tabellenwertfunktion aufrufen. Weitere Informationen hierzu finden Sie unter [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Wenn Sie keine Filterfunktion angeben, wird die gesamte Tabelle migriert.  
  
    > [!IMPORTANT]  
    > Wenn Sie eine schwache Filterfunktion angeben, wird die Datenmigration ebenfalls unzureichend ausgeführt. Stretch Database wendet die Filterfunktion mithilfe des CROSS APPLY-Operators auf die Tabelle an.  
  
-   Geben Sie `MIGRATION_STATE = OUTBOUND` an, um sofort mit der Datenmigration zu beginnen, oder  `MIGRATION_STATE = PAUSED` , um den Beginn der Datenmigration zu verschieben.  
  
### <a name="enable-stretch-database-for-an-existing-table"></a>Aktivieren von Stretch Database für eine vorhandene Tabelle  
 Um eine vorhandene Tabelle für Stretch Database zu konfigurieren, führen Sie den Befehl ALTER TABLE aus.  
  
 Nachstehend sehen Sie ein Beispiel, das die gesamte Tabelle migriert und sofort mit der Datenmigration beginnt.  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 Es folgt ein Beispiel, das nur die Zeilen migriert, die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` identifiziert wurden, und das die Datenmigration verschiebt. Weitere Informationen zur Filterfunktion finden Sie unter [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion (Stretch-Datenbank)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```  
  
 Weitere Informationen hierzu finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
### <a name="create-a-new-table-with-stretch-database-enabled"></a>Erstellen einer neuen Tabelle, für die Stretch Database aktiviert ist  
 Führen Sie zum Erstellen einer neuen Tabelle, für die Stretch Database aktiviert ist, den Befehl CREATE TABLE aus.  
  
 Nachstehend sehen Sie ein Beispiel, das die gesamte Tabelle migriert und sofort mit der Datenmigration beginnt.  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 Es folgt ein Beispiel, das nur die Zeilen migriert, die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` identifiziert wurden, und das die Datenmigration verschiebt. Weitere Informationen zur Filterfunktion finden Sie unter [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion (Stretch-Datenbank)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name> 
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```  
  
 Weitere Informationen hierzu finden Sie unter [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
