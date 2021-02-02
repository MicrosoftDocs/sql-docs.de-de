---
title: Festlegen des Kompatibilitätsgrads von Mergeveröffentlichungen
description: In diesem Artikel erfahren Sie, wie Sie den Kompatibilitätsgrad für Mergeveröffentlichungen mithilfe von SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL) festlegen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], replication
- backward compatibility [SQL Server], replication
- publications [SQL Server replication], backward compatibility
ms.assetid: db47ac73-948b-4d77-b272-bb3565135ea5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7dd5765ed93648b0bdd1e6c7108f8c77a47c3143
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076628"
---
# <a name="set-the-compatibility-level-for-merge-publications"></a>Festlegen des Kompatibilitätsgrads von Mergeveröffentlichungen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie der Kompatibilitätsgrad für Mergeveröffentlichungen in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]festgelegt wird. Bei der Mergereplikation wird anhand des Kompatibilitätsgrades der Veröffentlichung bestimmt, welche Funktionen von Veröffentlichungen in der jeweiligen Datenbank verwendet werden können.  
  
 **In diesem Thema**  
  
-   **So legen Sie den Kompatibilitätsgrad von Mergeveröffentlichungen fest mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Der Kompatibilitätsgrad wird auf der Seite **Abonnententypen** des Assistenten für neue Veröffentlichung festgelegt. Weitere Informationen zum Zugreifen auf diesen Assistenten finden Sie unter [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)festgelegt wird. Nach dem Erstellen einer Veröffentlichungsmomentaufnahme kann der Kompatibilitätsgrad zwar erhöht, nicht aber gesenkt werden. Sie können den Kompatibilitätsgrad auf der Seite **Allgemein** des Dialogfelds **Veröffentlichungseigenschaften – \<Publication>** erhöhen. Weitere Informationen zum Zugreifen auf dieses Dialogfeld finden Sie unter [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Wenn Sie den Veröffentlichungskompatibilitätsgrad erhöhen, können alle vorhandenen Abonnements auf Servern, auf denen eine Version vor diesem Kompatibilitätsgrad ausgeführt wird, nicht mehr synchronisiert werden.  
  
> [!NOTE]  
>  Da der Kompatibilitätsgrad Auswirkungen auf andere Veröffentlichungseigenschaften und darauf hat, welche Artikeleigenschaften gültig sind, dürfen Sie den Kompatibilitätsgrad nicht gleichzeitig mit anderen Eigenschaften im Dialogfeld ändern. Die Momentaufnahme für die Veröffentlichung sollte nach dem Ändern der Eigenschaften neu generiert werden.  
  
#### <a name="to-set-the-publication-compatibility-level"></a>So legen Sie den Veröffentlichungskompatibilitätsgrad fest  
  
-   Wählen Sie auf der Seite **Abonnementtypen** des Assistenten für neue Veröffentlichung die Abonnententypen aus, die die Veröffentlichung unterstützen soll.  
  
#### <a name="to-increase-the-publication-compatibility-level"></a>So erhöhen Sie den Veröffentlichungskompatibilitätsgrad  
  
-   Wählen Sie auf der Seite **Allgemein** des Dialogfelds **Veröffentlichungseigenschaften – \<Publication>** die Option **Kompatibilitätsgrad** aus.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Der Kompatibilitätsgrad einer Mergeveröffentlichung kann entweder programmgesteuert während der Erstellung der Veröffentlichung festgelegt oder zu einem späteren Zeitpunkt programmgesteuert geändert werden. Sie können gespeicherte Replikationsprozeduren verwenden, um diese Veröffentlichungseigenschaft festzulegen oder zu ändern.  
  
#### <a name="to-set-the-publication-compatibility-level-for-a-merge-publication"></a>So legen Sie den Veröffentlichungskompatibilitätsgrad einer Mergeveröffentlichung fest  
  
1.  Führen Sie auf dem Verleger [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) unter Angabe eines Werts für `@publication_compatibility_level` aus, damit die Veröffentlichung mit älteren Versionen von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kompatibel ist. Weitere Informationen finden Sie unter [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  

#### <a name="to-change-the-publication-compatibility-level-of-a-merge-publication"></a>So ändern Sie den Veröffentlichungskompatibilitätsgrad einer Mergeveröffentlichung  
  
1.  Führen Sie [Sp_changemergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) unter Angabe von **publication_compatibility_level** für `@property` und des entsprechenden Veröffentlichungskompatibilitätsgrads für `@value` aus.  
  
#### <a name="to-determine-the-publication-compatibility-level-of-a-merge-publication"></a>So bestimmen Sie den Veröffentlichungskompatibilitätsgrad einer Mergeveröffentlichung  
  
1.  Führen Sie [Sp_helpmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) unter Angabe der gewünschten Veröffentlichung aus.  
  
2.  Suchen Sie den Veröffentlichungskompatibilitätsgrad im Resultset in der **backward_comp_level** -Spalte.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 In diesem Beispiel wird eine Mergeveröffentlichung erstellt und der Veröffentlichungskompatibilitätsgrad festgelegt.  
  
```sql  
-- To avoid storing the login and password in the script file, the values   
-- are passed into SQLCMD as scripting variables. For information about   
-- how to use scripting variables on the command line and in SQL Server  
-- Management Studio, see the "Executing Replication Scripts" section in  
-- the topic "Programming Replication Using System Stored Procedures".  
  
--Add a new merge publication.  
DECLARE @publicationDB AS sysname;  
DECLARE @publication AS sysname;  
DECLARE @login AS sysname;  
DECLARE @password AS sysname;  
SET @publicationDB = N'AdventureWorks2012';   
SET @publication = N'AdvWorksSalesOrdersMerge'   
SET @login = $(Login);  
SET @password = $(Password);  
  
-- Create a new merge publication.   
USE [AdventureWorks2012]  
EXEC sp_addmergepublication   
@publication = @publication,   
-- Set the compatibility level to SQL Server 2014.  
@publication_compatibility_level = '120RTM';   
  
-- Create the snapshot job for the publication.  
EXEC sp_addpublication_snapshot   
@publication = @publication,  
@job_login = @login,  
@job_password = @password;  
GO  
```  
  
 In diesem Beispiel wird der Veröffentlichungskompatibilitätsgrad einer Mergeveröffentlichung geändert.  
  
> [!NOTE]  
>  Wenn in der Veröffentlichung Funktionen verwendet werden, die einen bestimmten Kompatibilitätsgrad erfordern, darf der Veröffentlichungskompatibilitätsgrad möglicherweise nicht geändert werden. Weitere Informationen finden Sie unter [Abwärtskompatibilität von Replikationen](../../../relational-databases/replication/replication-backward-compatibility.md).  
  
```sql  
DECLARE @publication AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge' ;  
  
-- Change the publication compatibility level to   
-- SQL Server 2008 or later.  
EXEC sp_changemergepublication   
@publication = @publication,   
@property = N'publication_compatibility_level',   
@value = N'100RTM';  
GO  
  
```  
  
 In diesem Beispiel wird der aktuelle Veröffentlichungskompatibilitätsgrad einer Mergeveröffentlichung zurückgegeben.  
  
```sql  
DECLARE @publication AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge' ;  
EXEC sp_helpmergepublication   
@publication = @publication;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Veröffentlichung](../../../relational-databases/replication/publish/create-a-publication.md)  
  
  
