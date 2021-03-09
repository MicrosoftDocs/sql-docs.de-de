---
description: Katalog Sichten für die Datenbank-Spiegelungs Zeugen-sys.database_mirroring_witnesses
title: sys.database_mirroring_witnesses (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_witnesses
- sys.database_mirroring_witnesses_TSQL
- database_mirroring_witnesses
- database_mirroring_witnesses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_witnesses catalog view
- witness [SQL Server], sys.database_mirroring_witnesses catalog view
ms.assetid: 0dd5b794-733b-4a3c-b5a4-62f9f1f0f22d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63599e8dfbeafc6136e2d4904a546e5ebbd5dd96
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465098"
---
# <a name="database-mirroring-witness-catalog-views---sysdatabase_mirroring_witnesses"></a>Katalog Sichten für die Datenbank-Spiegelungs Zeugen-sys.database_mirroring_witnesses
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Zeugenrolle, die ein Server in einer Datenbankspiegelungspartnerschaft spielt. 
  
  Bei einer Sitzung zur Datenbankspiegelung ist ein Zeugenserver für das automatische Failover erforderlich. Im Idealfall befindet sich der Zeuge auf einem anderen Computer als die Prinzipal- und Spiegelserver. Der Zeuge dient nicht der Datenbank. Stattdessen überwacht er den Status der Prinzipal- und Spiegelserver. Wenn der Prinzipal Server ausfällt, kann der Zeuge ein automatisches Failover zum Spiegel Server initiieren. 
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Name der beiden Datenbankkopien in der Datenbankspiegelungssitzung.|  
|**principal_server_name**|**sysname**|Name des Partnerservers, dessen Datenbankkopie derzeit die Prinzipaldatenbank ist.|  
|**mirror_server_name**|**sysname**|Name des Partnerservers, dessen Datenbankkopie derzeit die Spiegeldatenbank ist.|  
|**safety_level**|**tinyint**|Transaktionssicherheitseinstellung für Updates der Spiegeldatenbank:<br /><br /> 0 = Unbekannter Status<br /><br /> 1 = Aus (asynchron)<br /><br /> 2 = Vollständig (synchron)<br /><br /> Die Verwendung eines Zeugen für das automatische Failover setzt die vollständige Transaktionssicherheit (Standardeinstellung) voraus.|  
|**safety_level_desc**|**nvarchar(60)**|Beschreibung der Sicherheitsgarantie für Updates der Spiegeldatenbank.<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL|  
|**safety_sequence_number**|**int**|Aktualisieren Sie die Sequenznummer für Änderungen an **safety_level**.|  
|**role_sequence_number**|**int**|Updatesequenznummer für Änderungen an Prinizipal-/Spiegelrollen der Spiegelungspartner.|  
|**mirroring_guid**|**uniqueidentifier**|Bezeichner der Spiegelungspartnerschaft.|  
|**family_guid**|**uniqueidentifier**|Bezeichner der Sicherungsfamilie für die Datenbank. Wird zur Erkennung übereinstimmender Wiederherstellungsstatus verwendet.|  
|**is_suspended**|**bit**|Daten Bank Spiegelung wurde angehalten.|  
|**is_suspended_sequence_number**|**int**|Die Sequenznummer für das Festlegen **is_suspended**.|  
|**partner_sync_state**|**tinyint**|Synchronisierungsstatus der Spiegelungssitzung:<br /><br /> 5 = die Partner sind synchronisiert. Failover ist eventuell möglich. Weitere Informationen zu den Anforderungen für Failover finden Sie unter [Rollenwechsel während einer Datenbank-Spiegelungs Sitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> 6 = die Partner sind nicht synchronisiert. Failover ist jetzt nicht möglich.|  
|**partner_sync_state_desc**|**nvarchar(60)**|Beschreibung des Synchronisierungsstatus der Spiegelungssitzung:<br /><br /> SYNCHRONIZED<br /><br /> UNSYNCHRONIZED|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-Spiegelungszeuge](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [sys.database_mirroring &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
