---
description: Dynamische Verwaltungs Sichten für die Volltext-und semantische Suche-Funktionen
title: Dynamische Verwaltungs Sichten für die Volltext-und semantische Suche-Funktionen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 3f2d7a48bfbccf174b26a08ec3a887f44247a457
ms.sourcegitcommit: 32135463a8494d9ed1600a58f51819359e3c09dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91834296"
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Dynamische Verwaltungs Sichten für die Volltext-und semantische Suche-Funktionen
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Dieser Abschnitt enthält die folgenden dynamischen Verwaltungssichten und -funktionen für die Volltextsuche und semantische Suche.  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>Dynamische Verwaltungssichten und -funktionen für die Volltextsuche  
 [sys.dm_fts_active_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 Gibt Informationen zu den Volltextkatalogen zurück, für die zurzeit Auffüllungsaktivitäten auf dem Server ausgeführt werden.  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 Gibt Informationen über die aktuelle Aktivität des oder der Filterdaemonhosts auf der Serverinstanz zurück.  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 Gibt Informationen zum Inhalt eines Volltextindex für die angegebene Tabelle zurück.  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 Gibt Informationen zum Inhalt auf Dokumentebene eines Volltextindex für die angegebene Tabelle zurück. Ein Schlüsselwort kann in mehreren Dokumenten angezeigt werden.  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 Gibt alle eigenschaftsbezogenen Inhalte im Volltextindex einer angegebenen Tabelle zurück. Dies schließt alle Daten ein, die zu Eigenschaften gehören, die von der diesem Volltextindex zugeordneten Sucheigenschaftenliste registriert wurden.  
  
 sys.dm_fts_index_keywords_position_by_document  
 Gibt die Position von Schlüsselwörtern in einem Dokument zurück.  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 Gibt Informationen zu den aktuell ausgeführten Volltextindexauffüllungen zurück.  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 Gibt Informationen zu Speicherpuffern zurück, die einem bestimmten Speicherpool angehören, der im Rahmen einer Volltextdurchforstung oder eines Volltext-Durchforstungsbereichs verwendet wird.  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 Gibt Informationen zu den Shared Memory-Pools zurück, die für die Volltext-Gatherer-Komponente für einen Volltext-Durchforstungsvorgang oder einen Volltext-Durchforstungsbereich zur Verfügung stehen.  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 Gibt Informationen zu den einzelnen Volltext-Indizierungsbatches zurück.  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 Gibt das endgültige Tokenisierungsergebnis nach Anwendung einer gegebenen Kombination aus Wörtertrennung, Thesaurus und Stoppliste auf eine eingegebene Abfragezeichenfolge an. Diese Ausgabe entspricht der Ausgabe der angegebenen Abfragezeichenfolge an die Volltext-Engine.  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 Gibt Informationen zu den spezifischen Bereichen im Zusammenhang mit einer zurzeit ausgeführten Volltextindexauffüllung zurück.  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>Dynamische Verwaltungssichten und -funktionen für die semantische Suche  
 [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 Gibt eine Zeile mit Statusinformationen über die Auffüllung des Dokumentähnlichkeitsindex für jeden Ähnlichkeitsindex in jeder Tabelle zurück, der ein semantischer Index zugeordnet ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [System Sichten &#40;Transact-SQL-&#41;](../../t-sql/language-reference.md)  
  
