---
description: MSpeer_originatorid_history (Transact-SQL)
title: MSpeer_originatorid_history (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_originatorid_history_TSQL
- MSpeer_originatorid_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_originatorid_history
ms.assetid: c1f53d0f-4080-43ff-be38-2b10395c68c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 05ba5720a45bc8d448fe1ed24d6bdc950dcf290d
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545520"
---
# <a name="mspeer_originatorid_history-transact-sql"></a>MSpeer_originatorid_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Absender-ID, die in der Topologie definiert wurde. Dies beinhaltet IDs für Knoten, die nicht mehr aktiv sind. Die Tabelle wird verwendet, wenn Sie einen neuen Knoten für die Konflikterkennung konfigurieren, um sicherzustellen, dass die angegebene Absender-ID noch nicht verwendet wurde. Diese Tabelle wird in der Veröffentlichungsdatenbank gespeichert. Weitere Informationen zur Konflikterkennung finden Sie unter [Konflikterkennung in der Peer-zu-Peer-Replikation](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|originator_publication|**sysname**|Veröffentlichung, für welche die Absender-ID angegeben wurde|  
|originator_id|**int**|Nummer, die jeden Knoten in der Topologie zum Zweck der Konflikterkennung kennzeichnet|  
|originator_node|**sysname**|Serverinstanz, für welche die Absender-ID angegeben wurde|  
|originator_db|**sysname**|Veröffentlichungsdatenbank, für welche die Absender-ID angegeben wurde|  
|originator_db_version|**int**|Kennzeichnet die Versionsnummer der ursprünglichen Datenbank.|  
|originator_version|**int**|Kennzeichnet die Versionsnummer des Verlegers|  
|inserted_date|**datetime**|Datum und Zeit, zu der die Zeile für die Absender-ID in diese Tabelle eingefügt wurde|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
