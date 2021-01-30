---
description: sp_enumcustomresolvers (Transact-SQL)
title: sp_enumcustomresolvers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumcustomresolvers
- sp_enumcustomresolvers_TSQL
helpviewer_keywords:
- sp_enumcustomresolvers
ms.assetid: 81bd0d3a-48dc-42b1-b662-c630f61fc630
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8972aa1aaa4e567c60efb9653a75e6fa196211b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156776"
---
# <a name="sp_enumcustomresolvers-transact-sql"></a>sp_enumcustomresolvers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Liste aller verfügbaren Geschäftslogikhandler und benutzerdefinierten Konfliktlöser zurück, die auf dem Verteiler registriert sind. Diese gespeicherte Prozedur wird auf dem Verleger für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_enumcustomresolvers [ [ @distributor =] 'distributor']  
```  
  
## <a name="arguments"></a>Argumente  
`[ @distributor = ] 'distributor'` Der Name des Verteilers, auf dem sich der benutzerdefinierte Konflikt Löser befindet. *Distributor* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. *Dieser Parameter ist als veraltet markiert und wird in einer zukünftigen Version entfernt.*  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**article_resolver**|**nvarchar(255)**|Anzeigename für den Geschäftslogikhandler oder Konfliktlöser|  
|**resolver_clsid**|**nvarchar(50)**|Die Klassen-ID (CLSID, Class ID) des COM-basierten Konfliktlösers. Für einen Geschäftslogikhandler gibt diese Spalte einen CLSID-Wert von Null zurück.|  
|**is_dotnet_assembly**|**bit**|Gibt an, ob die Registrierung für einen Geschäftslogikhandler ist.<br /><br /> **0** = com-basierter Konflikt Löser<br /><br /> **1** = Geschäftslogik Handler|  
|**dotnet_assembly_name**|**nvarchar(255)**|Der Name der [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework-Assembly, die den Geschäftslogikhandler implementiert.|  
|**dotnet_class_name**|**nvarchar(255)**|Der Name der Klasse, die <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> überschreibt, um den Geschäftslogikhandler zu implementieren|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_enumcustomresolvers** wird bei der Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** und der festen Daten Bank Rolle **db_owner** können **sp_enumcustomresolvers** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Implementieren eines Geschäftslogikhandlers für einen Mergeartikel](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Implementieren eines benutzerdefinierten Konflikt Lösers für einen Mergeartikel](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
