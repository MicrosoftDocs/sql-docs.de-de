---
title: MSmerge_conflict_publication_article (T-SQL)
description: Beschreibt die gespeicherte Prozedur MSmerge_conflict_publication_article, die Informationen zu Zeilen mit Konflikten oder Zeilen Änderungen enthält, die rückgängig gemacht wurden, um Daten Konvergenz zu erreichen.
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_conflict_publication_article_TSQL
- MSmerge_conflict_publication_article
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflict_publication_article system table
ms.assetid: dc4490b4-02d8-4dfc-98f5-0cf8de8e11be
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b336049c2ed0c32f9d5e325c4184a9698424de4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212264"
---
# <a name="msmerge_conflict_publication_article-transact-sql"></a>MSmerge_conflict_publication_article (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_conflict_publication_article** Tabelle enthält Informationen zu Zeilen mit Konflikten oder Zeilen Änderungen, die rückgängig gemacht wurden, um Daten Konvergenz zu erreichen. Eine Konflikttabelle besteht für jede replizierte Tabelle in einer Veröffentlichung, wobei der Name der Konflikttabelle mit der Veröffentlichung und dem Artikelnamen angefügt wird. Diese artikelspezifischen Konflikttabellen sind in der für die Konfliktprotokollierung verwendeten Datenbank gespeichert. Dies ist in der Regel die Verlegerdatenbank, kann aber auch die Abonnementdatenbank sein, wenn die Konfliktprotokollierung dezentralisiert erfolgt.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**_Name der Artikel \_ Spalte \__**|**variable**|Stellt eine Spalte in einer replizierten Tabelle dar. Diese Systemtabelle enthält eine Spalte für jede Spalte im Tabellenartikel.|  
|**ROWGUID**|**uniqueidentifier**|Der Zeilenbezeichner für die Konfliktzeile.|  
|**ModifiedDate**|**datetime**|Der Zeitpunkt, zu dem der Konflikt aufgetreten ist.|  
|**\_DataSource- \_ ID des Ursprungs**|**uniqueidentifier**|Das Abonnement, für das die Zeilenänderung rückgängig gemacht wurde oder das den Konflikt verloren hat.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
