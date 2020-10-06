---
description: sp_add_category (Transact-SQL)
title: sp_add_category (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_add_category
- sp_add_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_category
ms.assetid: 6cca32cd-d941-4378-aed6-a7c90cb7520a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 845801380e1047dc9c44bf597a5fbea21e9d8dc4
ms.sourcegitcommit: 968969b62bc158b9843aba5034c9d913519bc4a7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91753710"
---
# <a name="sp_add_category-transact-sql"></a>sp_add_category (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Fügt dem Server die angegebene Kategorie von Aufträgen, Warnungen oder Operatoren hinzu. Informationen zur alternativen Methode finden [Sie unter CREATE Job Category Using SQL Server Management Studio](../../ssms/agent/create-a-job-category.md).
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
 > [!IMPORTANT]  
 > In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_add_category   
     [ [ @class = ] 'class', ]   
     [ [ @type = ] 'type', ]   
     { [ @name = ] 'name' }  
```  
  
## <a name="arguments"></a>Argumente  
`[ @class = ] 'class'` Die Klasse der Kategorie, die hinzugefügt werden soll. die *Klasse* ist vom Datentyp **varchar (8)** und hat den Standardwert Job. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|JOB|Fügt eine Auftragskategorie hinzu|  
|ALERT|Fügt eine Warnungskategorie hinzu|  
|OPERATOR|Fügt eine Operatorkategorie hinzu|  
  
`[ @type = ] 'type'` Der Typ der Kategorie, die hinzugefügt werden soll. *Type ist vom Datentyp* **varchar (12)** und hat den Standardwert **local**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|LOCAL|Lokale Auftragskategorie|  
|MultiServer|Eine Multiserver-Auftrags Kategorie.|  
|Keine|Eine Kategorie für eine andere Klasse als Job **.**|  
  
`[ @name = ] 'name'` Der Name der Kategorie, die hinzugefügt werden soll. Der Name muss innerhalb der angegebenen Klasse eindeutig sein. *Name ist vom Datentyp* **vom Datentyp sysname**und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_add_category** müssen von der **msdb** -Datenbank aus ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_add_category**ausführen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die lokale Auftragskategorie `AdminJobs` erstellt.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_category  
    @class=N'JOB',  
    @type=N'LOCAL',  
    @name=N'AdminJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_delete_category &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_help_category &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-category-transact-sql.md)   
 [sp_update_category &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-update-category-transact-sql.md)   
 [dbo.sysAufträge &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)   
 [dbo.sysjobservers &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-sysjobservers-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
