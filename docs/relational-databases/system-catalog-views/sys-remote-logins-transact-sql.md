---
description: sys.remote_logins (Transact-SQL)
title: sys.remote_logins (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.remote_logins_TSQL
- remote_logins
- remote_logins_TSQL
- sys.remote_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_logins catalog view
ms.assetid: 38477e91-d084-4df7-b1de-b930c5580189
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b66a0dc17660d4ff5c379dbedc9ebd629aa8c371
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206848"
---
# <a name="sysremote_logins-transact-sql"></a>sys.remote_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro Zuordnung von Remoteanmeldenamen zurück. Diese Katalogsicht wird zum Zuordnen von eingehenden lokalen Anmeldenamen verwendet, die vorgeben, von einem entsprechenden Server zu stammen und an einen tatsächlichen lokalen Anmeldenamen gerichtet zu sein.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID des Servers in **sys.servers**. Dieser Name wird von der Verbindung des so genannten Remoteservers bereitgestellt.|  
|**remote_name**|**sysname**|Der zuzuordnende Anmeldename, der von der Verbindung geliefert wird. Bei einem Wert von NULL der Anmeldename, der in der verwendeten Verbindung angegeben ist.|  
|**local_principal_id**|**int**|ID des Serverprinzipals, dem der Anmeldename zugeordnet wird. Bei einem Wert von 0 wird der Remoteanmeldename der Anmeldung mit demselben Namen zugeordnet.|  
|**modify_date**|**datetime**|Datum, an dem die verknüpfte Anmeldung zuletzt geändert wurde.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verknüpfte Server-Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
