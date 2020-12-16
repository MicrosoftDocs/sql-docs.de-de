---
description: Ändern einer Partitionsfunktion
title: Ändern einer Partitionsfunktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae5bfc09-f27a-4ea9-9518-485278b11674
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8174cdaae012631ff834392d7bb664c82971ca3d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464841"
---
# <a name="modify-a-partition-function"></a>Ändern einer Partitionsfunktion
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Sie können Tabellen- oder Indexpartitionen in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] ändern, indem Sie mit [!INCLUDE[tsql](../../includes/tsql-md.md)]die angegebene Partitionsanzahl in Einerschritten in der Partitionsfunktion der partitionierten Tabelle bzw. des Indexes hinzufügen oder abziehen. Beim Hinzufügen einer Partition "teilen" Sie eine vorhandene Partition "auf" und definieren die Partitionsbegrenzungen erneut. Wenn Sie eine Partition löschen, "führen" Sie die Begrenzungen von zwei Partitionen "zusammen". Diese Aktion füllt eine Partition neu und belässt die andere Partition ohne Zuordnung.  
  
> [!CAUTION]  
>  Mehrere Tabellen oder Indizes können dieselbe Partitionsfunktion verwenden. Wenn Sie eine Partitionsfunktion ändern, wirkt sich dies auf alle Funktionen einer einzigen Transaktion aus. Überprüfen Sie die Abhängigkeiten der Partitionsfunktion, bevor Sie sie ändern.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So ändern Sie eine Partitionsfunktion mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   ALTER PARTITION FUNCTION kann nur verwendet werden, um eine Partition in zwei aufzuteilen bzw. um zwei Partitionen in eine zusammenzuführen. Um die Partitionierung von Tabellen bzw. Indizes zu ändern (beispielsweise von 10 in 5 Partitionen), können Sie eine der folgenden Optionen verwenden:  
  
    -   Erstellen Sie eine neue partitionierte Tabelle mit der gewünschten Partitionsfunktion, und fügen Sie dann die Daten aus der alten Tabelle in die neue Tabelle ein, indem Sie entweder eine INSERT INTO ... SELECT FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung oder den **Assistenten zum Verwalten von Partitionen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]verwenden.  
  
    -   Erstellen Sie einen partitionierten gruppierten Index für einen Heap.  
  
        > [!NOTE]  
        >  Das Löschen eines partitionierten gruppierten Index ergibt einen partitionierten Heap.  
  
    -   Verwenden Sie die CREATE INDEX-Anweisung von [!INCLUDE[tsql](../../includes/tsql-md.md)] mit der DROP EXISTING = ON-Klausel, um einen vorhandenen partitionierten Index zu löschen und neu zu erstellen.  
  
    -   Führen Sie eine Abfolge von ALTER PARTITION FUNCTION-Anweisungen aus.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird die Replikation beim Ändern einer Partitionsfunktion nicht unterstützt. Wenn Sie Änderungen an Partitionsfunktionen in der Veröffentlichungsdatenbank vornehmen möchten, müssen Sie diese manuell in der Abonnementdatenbank durchführen.  
  
-   Alle von ALTER PARTITION FUNCTION betroffenen Dateigruppen müssen online sein.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die folgenden Berechtigungen können zum Ausführen von ALTER PARTITION FUNCTION verwendet werden:  
  
-   ALTER ANY DATASPACE-Berechtigung. Diese Berechtigung gilt standardmäßig für Mitglieder der festen Serverrolle **sysadmin** und für Mitglieder der festen Datenbankrollen **db_owner** und **db_ddladmin** .  
  
-   Die Berechtigung CONTROL oder ALTER für die Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
-   Die Berechtigung CONTROL SERVER oder ALTER ANY DATABASE auf dem Server der Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So ändern Sie eine Partitionsfunktion:**  
  
 Diese spezielle Aktion kann nicht mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ausgeführt werden. Um eine Partitionsfunktion zu ändern, müssen Sie zuerst die Funktion löschen und dann mit dem Assistenten zum Erstellen von Partitionen eine neue Funktion mit den gewünschten Eigenschaften erstellen. Weitere Informationen finden Sie unter  
  
#### <a name="to-delete-a-partition-function"></a>So löschen Sie eine Partitionsfunktion  
  
1.  Erweitern Sie die Datenbank mit der zu löschenden Partitionsfunktion und dann den Ordner **Speicher** .  
  
2.  Erweitern Sie den Ordner **Partitionsfunktionen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Partitionsfunktion, die Sie löschen möchten, und klicken Sie dann auf **Löschen**.  
  
4.  Stellen Sie im Dialogfeld **Objekt löschen** sicher, dass die richtige Partitionsfunktion ausgewählt ist, und klicken Sie dann auf **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-split-a-single-partition-into-two-partitions"></a>So teilen Sie eine einzelne Partition in zwei Partitionen auf  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Split the partition between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    ```  
  
#### <a name="to-merge-two-partitions-into-one-partition"></a>So führen Sie zwei Partitionen zu einer Partition zusammen  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Merge the partitions between boundary_values 1 and 100  
    --and between boundary_values 100 and 1000 to create one partition  
    --between boundary_values 1 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    MERGE RANGE (100);  
    ```  
  
 Weitere Informationen finden Sie unter [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
  
