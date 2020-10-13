---
description: Sichern und Wiederherstellen von Volltextkatalogen und Indizes
title: Sichern und Wiederherstellen von Volltextkatalogen und Indizes
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], backing up
- full-text search [SQL Server], back up and restore
- recovery [full-text search]
- backups [SQL Server], full-text indexes
- full-text indexes [SQL Server], restoring
- restore operations [full-text search]
ms.assetid: 6a4080d9-e43f-4b7b-a1da-bebf654c1194
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: b8b6708493f92712fddb22582fe94382d882f0a2
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869196"
---
# <a name="back-up-and-restore-full-text-catalogs-and-indexes"></a>Sichern und Wiederherstellen von Volltextkatalogen und Indizes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird erläutert, wie Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellte Volltextindizes sichern und wiederherstellen. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ist der Volltextkatalog ein logisches Konzept und befindet sich nicht in einer Dateigruppe. Um in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]einen Volltextkatalog zu sichern, müssen Sie daher jede Dateigruppe identifizieren, die einen Volltextindex enthält, der zum Katalog gehört. Dann müssen Sie diese Dateigruppen einzeln sichern.  
  
> [!IMPORTANT]  
>  Beim Aktualisieren einer [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Datenbank ist es möglich, Volltextkataloge zu importieren. Jeder importierte Volltextkatalog ist eine Datenbankdatei in einer eigenen Dateigruppe. Um einen importierten Katalog zu sichern, können Sie einfach die entsprechende Dateigruppe sichern. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Volltextkatalogen]()in der [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Onlinedokumentation.  
  
##  <a name="backing-up-the-full-text-indexes-of-a-full-text-catalog"></a><a name="backingup"></a> Sichern der Volltextindizes eines Volltextkatalogs  
  
###  <a name="finding-the-full-text-indexes-of-a-full-text-catalog"></a><a name="Find_FTIs_of_a_Catalog"></a> Suchen der Volltextindizes eines Volltextkatalogs  
 Sie können die Eigenschaften der Volltextindizes abrufen, indem Sie die folgende [SELECT](../../t-sql/queries/select-transact-sql.md) -Anweisung verwenden. Diese Anweisung wählt Spalten aus den Katalogsichten [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) und [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) aus.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @TableID int;  
SET @TableID = (SELECT OBJECT_ID('AdventureWorks2012.Production.Product'));  
SELECT object_name(@TableID), i.is_enabled, i.change_tracking_state,   
   i.has_crawl_completed, i.crawl_type, c.name as fulltext_catalog_name   
   FROM sys.fulltext_indexes i, sys.fulltext_catalogs c   
   WHERE i.fulltext_catalog_id = c.fulltext_catalog_id;  
GO  
```  
  
  
###  <a name="finding-the-filegroup-or-file-that-contains-a-full-text-index"></a><a name="Find_FG_of_FTI"></a> Suchen der Dateigruppe oder der Datei, die einen Volltextindex enthält  
 Wenn ein Volltextindex erstellt wird, wird dieser an einem der folgenden Speicherorte gespeichert:  
  
-   In einer vom Benutzer angegebenen Dateigruppe  
  
-   In derselben Dateigruppe als Basistabelle oder Sicht für eine nicht partitionierte Tabelle  
  
-   In der primären Dateigruppe für eine partitionierte Tabelle  
  
> [!NOTE]  
>  Informationen zum Erstellen eines Volltextindex finden Sie unter [Erstellen und Verwalten von Volltextindizes](../../relational-databases/search/create-and-manage-full-text-indexes.md)[CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
 Um die Dateigruppe eines Volltextindex für eine Tabelle oder Sicht zu finden, verwenden Sie die folgende Abfrage, in der *object_name* für den Namen der Tabelle oder Sicht steht:  
  
```  
SELECT name FROM sys.filegroups f, sys.fulltext_indexes i   
   WHERE f.data_space_id = i.data_space_id   
      and i.object_id = object_id('object_name');  
GO  
  
```  
  
  
###  <a name="backing-up-the-filegroups-that-contain-full-text-indexes"></a><a name="Back_up_FTIs_of_FTC"></a> Sichern der Dateigruppen, die Volltextindizes enthalten  
 Nachdem Sie die Dateigruppen gefunden haben, die die Indizes eines Volltextkatalogs enthalten, müssen Sie die einzelnen Dateigruppen sichern. Während der Sicherung können Volltextkataloge nicht gelöscht oder hinzugefügt werden.  
  
 Die erste Sicherung einer Dateigruppe muss eine vollständige Dateisicherung sein. Nachdem Sie eine vollständige Dateisicherung für eine Dateigruppe erstellt haben, können Sie bei Bedarf nur die Änderungen in einer Dateigruppe sichern, indem Sie eine oder mehrere differenzielle Dateisicherungen erstellen, die auf dieser vollständigen Dateisicherung basieren.  
  
 **So sichern Sie Dateien und Dateigruppen**  
  
-   [Sichern von Dateien und Dateigruppen &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
  
##  <a name="restoring-a-full-text-index"></a><a name="Restore_FTI"></a> Wiederherstellen eines Volltextindexes  
 Beim Wiederherstellen einer gesicherten Dateigruppe werden die Volltextindexdateien und die anderen Dateien der Dateigruppe wiederhergestellt. Standardmäßig wird die Dateigruppe an dem Speicherort auf einem Datenträger wiederhergestellt, an dem die Dateigruppe gesichert wurde.  
  
 Wenn bei der Erstellung der Sicherung eine Tabelle mit Volltextindizierung online war und eine Auffüllung ausgeführt wurde, wird die Auffüllung nach der Wiederherstellung fortgesetzt.  
  
 **So stellen Sie eine Dateigruppe wieder her**  
  
-   [Wiederherstellen von Dateien und Dateigruppen &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Wiederherstellen von Dateien und Dateigruppen über vorhandene Dateien &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Wiederherstellen von Dateien an einem neuen Speicherort &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten und Überwachen der Volltextsuche auf einer Serverinstanz](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)   
 [Upgrade der Volltextsuche](../../relational-databases/search/upgrade-full-text-search.md)  
  
