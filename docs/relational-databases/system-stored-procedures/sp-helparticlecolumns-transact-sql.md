---
description: sp_helparticlecolumns (Transact-SQL)
title: sp_helparticlecolumns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helparticlecolumns
- sp_helparticlecolumns_TSQL
helpviewer_keywords:
- sp_helparticlecolumns
ms.assetid: 9ea55df3-2e99-4683-88ad-bde718288bc7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 10382cd2daa15848bfc8454000ac23762f52e272
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89543303"
---
# <a name="sp_helparticlecolumns-transact-sql"></a>sp_helparticlecolumns (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Gibt alle Spalten in der zugrunde liegenden Tabelle zurück. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt. Für Oracle-Verleger wird diese gespeicherte Prozedur auf dem Verteiler auf jeder Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helparticlecolumns [ @publication = ] 'publication'   
        , [ @article = ] 'article'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, die den Artikel enthält. *Publication* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @article = ] 'article'` Der Name des Artikels, dessen Spalten zurückgegeben werden. der *Artikel* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @publisher = ] 'publisher'` Gibt einen nicht-- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger an. *Publisher* ist vom **Datentyp vom Datentyp sysname**und hat den Standardwert NULL.  
  
> [!NOTE]  
>  der *Verleger* sollte nicht angegeben werden, wenn der angeforderte Artikel von einem Verleger veröffentlicht wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Spalten, die nicht veröffentlicht werden) oder **1** (veröffentlichte Spalten)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**Spalten-ID**|**int**|Bezeichner der Spalte.|  
|**column**|**sysname**|Name der Spalte.|  
|**enes**|**bit**|Gibt an, ob die Spalte veröffentlicht wird:<br /><br /> **0** = Nein<br /><br /> **1** = Ja|  
|**Verlegertyp**|**sysname**|Datentyp der Spalte auf dem Verleger.|  
|**Abonnenten Typen**|**sysname**|Datentyp der Spalte auf dem Abonnenten.|  
  
## <a name="remarks"></a>Hinweise  
 **sp_helparticlecolumns** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
 **sp_helparticlecolumns** ist hilfreich beim Überprüfen einer vertikalen Partition.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** , der festen Daten Bank Rolle **db_owner** oder der Veröffentlichungs Zugriffsliste für die aktuelle Veröffentlichung können **sp_helparticlecolumns**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Definieren und Ändern eines Spalten Filters](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [sp_addarticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
