---
description: sys.dm_os_stacks (Transact-SQL)
title: sys.dm_os_stacks (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_stacks
- dm_os_stacks_TSQL
- sys.dm_os_stacks
- sys.dm_os_stacks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_stacks dynamic management view
ms.assetid: a69b06c4-28f0-4535-8fa1-9f132db4d916
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e5c42d0a82ce7726883e46f2e633aacc624e4db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099720"
---
# <a name="sysdm_os_stacks-transact-sql"></a>sys.dm_os_stacks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Diese dynamische Verwaltungssicht wird intern von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für Folgendes verwendet:  
  
-   Nachverfolgen von Debugdaten wie z. B. ausstehenden Zuordnungen.  
  
-   Voraussetzen oder Überprüfen von Logik, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten an Stellen verwendet wird, an denen von einer Komponente vorausgesetzt wird, dass ein bestimmter Aufruf erfolgt ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**stack_address**|**varbinary(8)**|Eindeutige Adresse für diese Stapelzuordnung. Lässt keine NULL-Werte zu.|  
|**frame_index**|**int**|Jede Zeile stellt einen Funktionsaufruf dar, der bei Sortierung in aufsteigender Reihenfolge nach Rahmenindex für einen bestimmten **stack_address**-Wert die vollständige Aufrufliste zurückgibt. Lässt keine NULL-Werte zu.|  
|**frame_address**|**varbinary(8)**|Adresse des Funktionsaufrufes. Lässt keine NULL-Werte zu.|  
  
## <a name="remarks"></a>Bemerkungen  
 **sys.dm_os_stacks** erfordert, dass die Symbole des Servers und anderer Komponenten auf dem Server vorhanden sein müssen, damit die Informationen richtig angezeigt werden.  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken `Server admin` oder ein `Azure Active Directory admin` Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   


## <a name="see-also"></a>Weitere Informationen  
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
