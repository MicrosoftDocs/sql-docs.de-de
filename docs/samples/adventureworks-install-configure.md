---
title: AdventureWorks-Beispiel Datenbanken
description: Befolgen Sie diese Anweisungen, um AdventureWorks-Beispiel Datenbanken mithilfe von Transact-SQL (T-SQL), SQL Server Management Studio (SSMS) oder Azure Data Studio in SQL Server herunterzuladen und zu installieren.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: d706b9ef0ed5d95c411f00d426a8b9daf1415074
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340028"
---
# <a name="adventureworks-sample-databases"></a>AdventureWorks-Beispiel Datenbanken
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Dieser Artikel enthält direkte Links zum Herunterladen von AdventureWorks-Beispiel Datenbanken sowie Anweisungen für die Wiederherstellung in SQL Server und Azure SQL-Datenbank. 

Weitere Informationen zu Beispielen finden Sie im [GitHub-Repository "Beispiele](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases)". 

## <a name="prerequisites"></a>Voraussetzungen

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) oder [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) oder [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>Sicherungsdateien herunterladen 

Verwenden Sie diese Links, um die entsprechende Beispieldatenbank für Ihr Szenario herunterzuladen. 

- **OLTP** -Daten sind für die meisten typischen Online Transaktionsverarbeitungs-Workloads vorgesehen. 
- **Data Warehouse (DW)** -Daten sind für Data Warehousing-Arbeits Auslastungen vorgesehen. 
- Bei **Lightweight (lt)** -Daten handelt es sich um eine vereinfachte und abgefragte Version des **OLTP** -Beispiels. 

Wenn Sie nicht sicher sind, was Sie benötigen, beginnen Sie mit der OLTP-Version, die mit Ihrer SQL Server Version übereinstimmt. 

|**OLTP** |**Data Warehouse** |**Einfache**|
|---------|---------|---------|
|[AdventureWorks2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| Nicht zutreffend |
|[AdventureWorks2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2. bak](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | Nicht zutreffend |

Weitere Dateien finden Sie direkt auf GitHub: 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 und 2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>In SQL Server wiederherstellen 

Sie können die- `.bak` Datei verwenden, um die Beispieldatenbank auf Ihrer SQL Server-Instanz wiederherzustellen. Verwenden Sie hierzu den Befehl [Restore (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) , oder verwenden Sie die grafische Benutzeroberfläche (GUI) in [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) oder [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

Wenn Sie mit der Verwendung von SQL Server Management Studio (SSMS) nicht vertraut sind, finden Sie unter [Connect & Query](../ssms/quickstarts/ssms-connect-query-sql-server.md) to Started. 

Führen Sie die folgenden Schritte aus, um die Datenbank in SQL Server Management Studio wiederherzustellen:

1. Laden Sie die entsprechende `.bak` Datei von einem der Links im Abschnitt [Herunterladen von Sicherungsdateien](#download-backup-files) herunter.
2. Verschieben Sie die `.bak` Datei an den SQL Server Sicherungs Speicherort. Dies variiert abhängig vom Installationsort, dem Instanznamen und der Version von SQL Server. Der Standard Speicherort für eine Standard Instanz von SQL Server 2019 lautet wie folgt:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. Öffnen Sie SQL Server Management Studio (SSMS), und stellen Sie eine Verbindung mit Ihrem SQL Server in her. 
4. Klicken Sie mit der rechten Maustaste auf **Datenbanken** **Objekt-Explorer**  >  **Restore Database...** , um den **Wiederherstellungs** -Assistenten zu starten. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="Screenshot, der zeigt, wie Sie die Datenbank wiederherstellen, indem Sie in Objekt-Explorer mit der rechten Maustaste auf Datenbanken und anschließend auf Datenbank wiederherstellen klicken":::


1. Wählen Sie **Gerät** aus, und wählen Sie dann die Auslassungs Punkte **(...)** aus, um ein Gerät auszuwählen. 
1. Wählen Sie **Hinzufügen** aus, und wählen Sie dann die `.bak` Datei aus, die Sie vor kurzem in diesen Wenn Sie die Datei an diesen Speicherort verschoben haben, Sie aber nicht im Assistenten sehen können, weist dies in der Regel auf ein Berechtigungsproblem hin. SQL Server oder der Benutzer, der sich bei SQL Server angemeldet hat, verfügt nicht über die Berechtigung für diese Datei in diesem Ordner. 
1. Wählen Sie **OK** aus, um die Auswahl der Datenbanksicherung zu bestätigen und das Fenster **Sicherungsmedien auswählen** zu schließen. 
1. Überprüfen Sie die Registerkarte " **Dateien** ", um zu bestätigen, dass die **Wiederherstellung als** Speicherort und Dateinamen **mit dem gewünschten** Speicherort und den Dateinamen im Assistenten 
1. Klicken Sie auf **OK**, um die Datenbank wiederherzustellen. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="Screenshot, der das Fenster Datenbank wiederherstellen mit hervorgehobener Sicherung und der Option OK anzeigt.":::

Weitere Informationen zum Wiederherstellen einer SQL Server Datenbank finden [Sie unter Wiederherstellen einer Datenbanksicherung mit SSMS](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

Sie können die Beispieldatenbank mithilfe von Transact-SQL (T-SQL) wiederherstellen. Im folgenden finden Sie ein Beispiel für die Wiederherstellung von AdventureWorks2019. der Datenbankname und der Installations Dateipfad können jedoch je nach Umgebung variieren. 

Um AdventureWorks2019 auf **Windows** wiederherzustellen, ändern Sie die Werte entsprechend Ihrer Umgebung, und führen Sie dann den folgenden Transact-SQL-Befehl (T-SQL) aus:


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

Um AdventureWorks2019 auf **Linux** wiederherzustellen, ändern Sie den Pfad des Windows-Dateisystems in Linux, und führen Sie dann den folgenden Transact-SQL-Befehl (T-SQL) aus: 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

Wenn Sie mit [Azure Data Studio Studio](../azure-data-studio/download-azure-data-studio.md)nicht vertraut sind, finden Sie unter [Connect & Query](../azure-data-studio/quickstart-sql-server.md) to Started

Führen Sie die folgenden Schritte aus, um die Datenbank in Azure Data Studio wiederherzustellen:

1. Laden Sie die entsprechende `.bak` Datei von einem der Links im Abschnitt [Herunterladen von Sicherungsdateien](#download-backup-files) herunter.
1. Verschieben Sie die `.bak` Datei an den SQL Server Sicherungs Speicherort. Dies variiert abhängig vom Installationsort, dem Instanznamen und der Version von SQL Server. Der Standard Speicherort für eine Standard Instanz von SQL Server 2019 lautet wie folgt:

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. Öffnen Sie Azure Data Studio Studio, und stellen Sie eine Verbindung mit Ihrer SQL Server Instanz her
1. Klicken Sie mit der rechten Maustaste auf Ihren Server, und wählen Sie **Verwalten**.

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="Screenshot, der Azure Data Studio mit hervorgehobener und aufgerufener Option &quot;verwalten&quot; anzeigt.":::

1. Auswählen der **Wiederherstellung**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="Wählen Sie im oberen Menü wiederherstellen aus, um die Datenbank wiederherzustellen.":::

1. Geben Sie auf der Registerkarte **Allgemein** die unter **Quelle** aufgeführten Werte ein.
    1. Wählen Sie unter **Wiederherstellen von** die Option *Sicherungsdatei* aus.
    1. Wählen Sie unter **Pfad der Sicherungsdatei** den Speicherort aus, an dem Sie die BAK-Datei gespeichert haben. 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="Wählen Sie den Pfad der Sicherungsdatei.":::
    
    Mit diesem Vorgang werden die restlichen Felder automatisch aufgefüllt, z. b. **Datenbank**, **Zieldatenbank** und **Wiederherstellung**. 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="Nachdem Sie einen Pfad für die Sicherungsdatei ausgewählt haben, wird der Rest der Felder unter":::

1. Wählen Sie **Wiederherstellen** aus, um die Datenbank wiederherzustellen. 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="Wenn Sie fertig sind, wählen Sie wiederherstellen aus, um die Datenbank wiederherzustellen.":::

---

## <a name="deploy-to-azure-sql-database"></a>In Azure SQL-Datenbank bereitstellen

Sie haben zwei Möglichkeiten, Beispiel Daten der Azure SQL-Datenbank anzuzeigen. Sie können ein Beispiel verwenden, wenn Sie eine neue Datenbank erstellen, oder Sie können eine Datenbank aus SQL Server direkt in Azure bereitstellen, indem Sie SQL Server Management Studio (SSMS) verwenden.

Informationen zum Anzeigen von Beispiel Daten für Azure SQL-verwaltete Instanz finden Sie unter [Wiederherstellen von weltweiten Import Programmen für SQL verwaltete Instanz](/azure/azure-sql/managed-instance/restore-sample-database-quickstart). 

### <a name="deploy-new-sample-database"></a>Neue Beispieldatenbank bereitstellen

Wenn Sie eine neue Datenbank in Azure SQL-Datenbank erstellen, haben Sie die Möglichkeit, eine leere Datenbank oder eine Beispieldatenbank zu erstellen. 

Führen Sie die folgenden Schritte aus, um eine-Beispieldatenbank zum Erstellen einer neuen Datenbank zu verwenden: 

1. Stellen Sie eine Verbindung mit Ihrem Azure-Portal her
1. Wählen Sie oben links im Navigationsbereich **Ressource erstellen** aus. 
1. Wählen Sie **Datenbanken** und dann **SQL-Datenbank** aus. 
1. Geben Sie die angeforderten Informationen ein, um die Datenbank zu erstellen. 
1. Wählen Sie auf der Registerkarte **zusätzliche Einstellungen** die Option **Sample** als vorhandene Daten unter **Datenquelle** aus: 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="Wählen Sie Beispiel als Datenquelle auf der Registerkarte zusätzliche Einstellungen in der Azure-Portal aus, wenn Sie die Azure SQL-Datenbank erstellen.":::

1. Wählen Sie **Erstellen** aus, um die neue SQL-Datenbank zu erstellen. Dies ist die wiederhergestellte Kopie der AdventureWorksLT-Datenbank. 


### <a name="deploy-database-from-sql-server"></a>Datenbank aus SQL Server bereitstellen

SQL Server Management Studio bietet die Möglichkeit, eine Datenbank direkt in Azure SQL-Datenbank bereitzustellen. Diese Methode stellt derzeit keine Datenüberprüfung bereit, sodass Sie für Entwicklung und Tests vorgesehen ist und nicht für die Produktion verwendet werden sollte. 

Führen Sie die folgenden Schritte aus, um eine Beispieldatenbank aus SQL Server in Azure SQL-Datenbank bereitzustellen:

1. Stellen Sie in SQL Server Management Studio eine Verbindung mit Ihrem SQL Server her. 
1. Wenn Sie dies noch nicht getan haben, stellen Sie [die-Beispieldatenbank in SQL Server wieder her](#restore-to-sql-server). 
1. Klicken Sie mit der rechten Maustaste auf die wiederhergestellte Datenbank in **Objekt-Explorer**  >  **Tasks**  >  **Datenbank in Microsoft Azure SQL-Datenbank** bereitstellen.... 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="Wählen Sie die Bereitstellung der Datenbank auf Microsoft Azure SQL-Datenbank aus, indem Sie mit der rechten Maustaste auf die Datenbank klicken und Aufgaben":::

1. Befolgen Sie den Assistenten, um eine Verbindung mit Azure SQL-Datenbank herzustellen und die Datenbank bereitzustellen. 


## <a name="creation-scripts"></a>Erstellungs Skripts

Anstatt eine Datenbank wiederherzustellen, können Sie alternativ Skripts verwenden, um die AdventureWorks-Datenbanken unabhängig von der jeweiligen Version zu erstellen. 

Die folgenden Skripts können verwendet werden, um die gesamte AdventureWorks-Datenbank zu erstellen: 

- [ZIP-Skripts für AdventureWorks-OLTP](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [ZIP-Skripts für AdventureWorks DW](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

Weitere Informationen zur Verwendung der Skripts finden Sie auf [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Beispieldatenbank wieder hergestellt haben, verwenden Sie die folgenden Tutorials für die ersten Schritte mit SQL Server: 


[Tutorials für SQL Server Datenbank-Engine](../relational-databases/database-engine-tutorials.md)   
[Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[Verbinden und Abfragen mit Azure Data Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)