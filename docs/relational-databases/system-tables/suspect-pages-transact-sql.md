---
description: suspect_pages (Transact-SQL)
title: suspect_pages (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- suspect_page_table
- suspect_page_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- suspect_pages system table
- suspect pages [SQL Server]
ms.assetid: 119c8d62-eea8-44fb-bf72-de469c838c50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c601496075c71d70af89b0413e5bcbf3a6ba8b2
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89544478"
---
# <a name="suspect_pages-transact-sql"></a>suspect_pages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile pro Seite, die einen geringfügigen Fehler vom Typ 823 oder einen Fehler vom Typ 824 aufweist. Von den in dieser Tabelle aufgelisteten Seiten wird angenommen, dass sie fehlerhaft sind. Dies trifft jedoch möglicherweise nicht zu. Wenn eine fehlerverdächtige Seite repariert wird, wird ihr Status in der **event_type** -Spalte aktualisiert.  
  
 Die folgende Tabelle kann maximal 1.000 Zeilen umfassen und wird in der Datenbank **msdb** gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID der Datenbank, auf die sich diese Seite bezieht.|  
|**file_id**|**int**|ID der Datei in der Datenbank.|  
|**page_id**|**bigint**|ID der fehlerverdächtigen Seite. Jede Seite hat eine Seiten-ID, bei der es sich um einen 32-Bit-Wert handelt, die den Speicherort der Seite in der Datenbank identifiziert. **page_id** ist der Offset in die Datendatei der 8-KB-Seite. Jede Seiten-ID ist innerhalb einer Datei eindeutig.|  
|**event_type**|**int**|Einer der folgenden Fehlertypen:<br /><br /> 1 = Ein Fehler vom Typ 823, der eine fehlerverdächtige Seite verursacht (z. B. ein Datenträgerfehler) oder ein Fehler vom Typ 824, der außer einer fehlerhaften Prüfsumme oder einer zerrissenen Seite z. B. eine fehlerhafte Seiten-ID anzeigt.<br /><br /> 2 = Fehlerhafte Prüfsumme<br /><br /> 3 = Zerrissene Seite<br /><br /> 4 = Wiederhergestellt (die Seite wurde wiederhergestellt, nachdem sie als ungültig gekennzeichnet wurde)<br /><br /> 5 = Repariert (DBCC hat die Seite repariert)<br /><br /> 7 = Zuordnung durch DBCC aufgehoben|  
|**error_count**|**int**|Häufigkeit, mit der ein Fehler aufgetreten ist.|  
|**last_update_date**|**datetime**|Datums- und Zeitstempel des letzten Updates.|  
  
## <a name="permissions"></a>Berechtigungen  
 Jeder mit Zugriff auf **msdb** kann die Daten in der Tabelle **suspect_pages** lesen. Jeder mit UPDATE-Berechtigung für die suspect_pages-Tabelle kann ihre Datensätze aktualisieren. Mitglieder der festen Datenbankrolle **db_owner** auf **msdb** oder der festen Serverrolle **sysadmin** können Datensätze einfügen, aktualisieren und löschen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiederherstellung von Seiten &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Database verdächtige Data Page-Ereignisklasse](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)   
 [System Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [Verwalten der suspect_pages-Tabelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
