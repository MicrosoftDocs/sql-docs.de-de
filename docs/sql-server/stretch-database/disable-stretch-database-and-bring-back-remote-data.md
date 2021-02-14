---
description: Deaktivieren von Stretch Database und Zurückbringen von Remotedaten
title: Deaktivieren von Stretch Database und Zurückbringen von Remotedaten
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: c1bbb24e-47e3-46aa-b786-fcadf9fb65ce
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 315cfed40b1ccdadf349d9b371d6baac5928d3af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081025"
---
# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Deaktivieren von Stretch Database und Zurückbringen von Remotedaten
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Wählen Sie zum Deaktivieren von Stretch Database für eine Tabelle in SQL Server Management Studio **Stretch** für eine Tabelle aus. Wählen Sie anschließend eine der folgenden Optionen aus:  
  
-   **Deaktivieren | Daten aus Azure zurückbringen**. Kopieren Sie die Remotedaten für die Tabelle aus Azure, und fügen Sie sie wieder in SQL Server ein. Deaktivieren Sie dann Stretch Database für die Tabelle. Dieser Vorgang verursacht Datenübertragungskosten und kann nicht abgebrochen werden.  
  
-   **Deaktivieren | Daten in Azure lassen**. Deaktivieren Sie Stretch Database für die Tabelle.  Verwerfen Sie die Remotedaten für die Tabelle in Azure.  
  
 Sie können auch Transact-SQL verwenden, um Stretch Database für eine Tabelle oder eine Datenbank zu deaktivieren.  
  
 Nach dem Deaktivieren von Stretch Database für eine Tabelle wird die Datenmigration beendet und die Abfrageergebnisse enthalten nicht mehr die Ergebnisse aus der Remotetabelle.  
  
 Wenn Sie die Datenmigration einfach anhalten möchten, finden Sie Informationen dazu unter [Anhalten und Fortsetzen der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
> [!NOTE]
> Durch das Deaktivieren von Stretch Database für eine Tabelle oder eine Datenbank wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals löschen. Die Remoteobjekte erzeugen weiterhin Azure-Kosten, bis Sie die Objekte löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-table"></a>Deaktivieren von Stretch Database für eine Tabelle  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Verwenden von SQL Server Management Studio zum Deaktivieren von Stretch Database für eine Tabelle  
  
1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Tabelle aus, für die Stretch Database deaktiviert werden soll.  
  
2.  Klicken Sie mit der rechten Maustaste auf **Stretch**, und wählen Sie eine der folgenden Optionen aus.  
  
    -   **Deaktivieren | Daten aus Azure zurückbringen**. Kopieren Sie die Remotedaten für die Tabelle aus Azure, und fügen Sie sie wieder in SQL Server ein. Deaktivieren Sie dann Stretch Database für die Tabelle. Dieser Befehl kann nicht abgebrochen werden.  
  
        > [!NOTE]
        > Das Kopieren der Remotedaten der Tabelle von Azure zurück zu SQL Server generiert Datenübertragungskosten. Weitere Informationen finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
         Nachdem die Remotedaten von Azure zurück zu SQL Server kopiert wurden, wird Stretch für die Tabelle deaktiviert.  
  
    -   **Deaktivieren | Daten in Azure lassen**. Deaktivieren Sie Stretch Database für die Tabelle.  Verwerfen Sie die Remotedaten für die Tabelle in Azure.  
  
    > [!NOTE]
    > Durch das Deaktivieren von Stretch Database für eine Tabelle werden die Remotedaten oder die Remotetabelle nicht gelöscht. Wenn Sie die Remotetabelle löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals löschen. Die Remotetabelle erzeugt weiterhin Kosten in Azure, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Verwenden von Transact-SQL zum Deaktivieren von Stretch Database für eine Tabelle  
  
-   Führen Sie den folgenden Befehl aus, um Stretch für eine Tabelle zu deaktivieren und die Remotedaten einer Tabelle aus Azure zurück zu SQL Server zu kopieren. Nachdem die Remotedaten von Azure zurück zu SQL Server kopiert wurden, wird Stretch für die Tabelle deaktiviert.

    Dieser Befehl kann nicht abgebrochen werden.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ; 
    GO 
    ```  
  
    > [!NOTE]
    > Das Kopieren der Remotedaten der Tabelle von Azure zurück zu SQL Server generiert Datenübertragungskosten. Weitere Informationen finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).    
  
-   Führen Sie den folgenden Befehl aus, um Stretch für eine Tabelle zu deaktivieren und die Remotedaten zu verwerfen.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ; 
    GO
    ```  
  
> [!NOTE]
> Durch das Deaktivieren von Stretch Database für eine Tabelle werden die Remotedaten oder die Remotetabelle nicht gelöscht. Wenn Sie die Remotetabelle löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals löschen. Die Remotetabelle erzeugt weiterhin Kosten in Azure, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-database"></a>Deaktivieren von Stretch Database für eine Datenbank  
 Vor dem Deaktivieren von Stretch Database für eine Datenbank müssen Sie Stretch Database für die einzelnen Stretch-fähigen Tabellen in der Datenbank deaktivieren.  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Verwenden von SQL Server Management Studio zum Deaktivieren von Stretch Database für eine Datenbank  
  
1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Datenbank aus, für die Stretch Database deaktiviert werden soll.  
  
2.  Klicken Sie mit der rechten Maustaste auf **Aufgaben**, **Stretch** und anschließend auf **Deaktivieren**.  
  
> [!NOTE]
> Durch das Deaktivieren von Stretch Database für eine Datenbank wird die Remotedatenbank nicht gelöscht. Wenn Sie die Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals entfernen. Die Remotedatenbank erzeugt weiterhin Kosten in Azure, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Verwenden von Transact-SQL zum Deaktivieren von Stretch Database für eine Datenbank  
 Führen Sie den folgenden Befehl aus.  
  
```sql  
ALTER DATABASE <Stretch-enabled database name>  
    SET REMOTE_DATA_ARCHIVE = OFF ;  
GO 
```  
  
> [!NOTE]
> Durch das Deaktivieren von Stretch Database für eine Datenbank wird die Remotedatenbank nicht gelöscht. Wenn Sie die Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals entfernen. Die Remotedatenbank erzeugt weiterhin Kosten in Azure, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Database – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Anhalten und Fortsetzen der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
  
