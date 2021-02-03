---
title: Statistikaktualisierung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Abfrageoptimierungsstatistiken einer Tabelle oder einer indizierten Sicht in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL aktualisieren.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- updating statistics
- statistics [SQL Server], updating
ms.assetid: 4b97c0b4-03ff-4cfb-9c3f-3b33290b7a2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e503301eef971cc5eec21ddf4e96caf2389923dc
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250245"
---
# <a name="update-statistics"></a>Statistikaktualisierung
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Sie können Abfrageoptimierungsstatistiken für eine Tabelle oder indizierte Sicht in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]aktualisieren. Standardmäßig nimmt der Abfrageoptimierer erforderliche Updates der Statistiken automatisch vor, um den Abfrageplan zu verbessern. In einigen Fällen können Sie die Abfrageleistung mit UPDATE STATISTICS oder der gespeicherten Prozedur `sp_updatestats` verbessern, um Statistiken häufiger zu aktualisieren, als von der Standardeinstellung vorgegeben.  
  
 Durch das Update von Statistiken wird sichergestellt, dass Abfragen anhand aktueller Statistiken kompiliert werden. Dies führt jedoch dazu, dass Abfragen neu kompiliert werden. Es empfiehlt sich, Statistiken nicht zu oft zu aktualisieren und die Vorteile optimierter Abfragepläne gegen den Zeitaufwand für die Neukompilierung von Abfragen abzuwägen. Die Entscheidung hängt von der verwendeten Anwendung ab. UPDATE STATISTICS-Vorgänge können mithilfe von tempdb die Stichprobenzeilen zum Erstellen von Statistiken sortieren.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So aktualisieren Sie ein Statistikobjekt mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Bei der Verwendung von UPDATE STATISTICS oder beim Vornehmen von Änderungen mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ist für die Tabelle oder Sicht die ALTER-Berechtigung erforderlich. Wenn Sie `sp_updatestats`verwenden, ist die Mitgliedschaft in der festen Serverrolle **sysadmin** oder der Besitz der Datenbank (**dbo**) erforderlich.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-update-a-statistics-object"></a>So aktualisieren Sie ein Statistikobjekt  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um die Datenbank zu erweitern, in der Sie die Statistik aktualisieren möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Tabellen** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um die Tabelle zu erweitern, in der Sie die Statistik aktualisieren möchten.  
  
4.  Klicken Sie auf das Pluszeichen, um den Ordner **Statistik** zu erweitern.  
  
5.  Klicken Sie mit der rechten Maustaste auf das Statistikobjekt, das Sie aktualisieren möchten, und wählen Sie **Eigenschaften**.  
  
6.  Aktivieren Sie im Dialogfeld **Statistikeigenschaften –** _Name\_der_Statistik_ das Kontrollkästchen **Statistiken für diese Spalten aktualisieren**, und klicken Sie dann auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
### <a name="to-update-a-specific-statistics-object"></a>So aktualisieren Sie ein bestimmtes Statistikobjekt  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- The following example updates the statistics for the AK_SalesOrderDetail_rowguid index of the SalesOrderDetail table.   
    UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;   
    GO  
    ```  
  
### <a name="to-update-all-statistics-in-a-table"></a>So aktualisieren Sie alle Statistiken in einer Tabelle  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    -- The following example updates the statistics for all indexes on the SalesOrderDetail table.   
    UPDATE STATISTICS Sales.SalesOrderDetail;   
    GO  
    ```  
  
Weitere Informationen finden Sie unter [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)erstellt wurden.  
  
### <a name="to-update-all-statistics-in-a-database"></a>So aktualisieren Sie alle Statistiken in einer Datenbank  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    -- The following example updates the statistics for all tables in the database.   
    EXEC sp_updatestats;  
    ```  

Weitere Informationen finden Sie unter [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md).   

### <a name="automatic-index-and-statistics-management"></a>Automatische Verwaltung von Index und Statistiken
Nutzen Sie Lösungen wie [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), um die Indexdefragmentierung und das Aktualisieren der Statistiken für eine oder mehrere Datenbanken automatisch zu verwalten. Dieser Vorgang entscheidet unter anderem anhand des Fragmentierungsgrads automatisch, ob ein Index neu organisiert oder neu erstellt wird und aktualisiert Statistiken mit einem linearen Schwellenwert.

