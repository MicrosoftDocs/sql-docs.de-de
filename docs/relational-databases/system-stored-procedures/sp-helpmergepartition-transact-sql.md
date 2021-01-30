---
description: sp_helpmergepartition (Transact-SQL)
title: sp_helpmergepartition (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepartition
- sp_helpmergepartition_TSQL
helpviewer_keywords:
- sp_helpmergepartition
ms.assetid: 184188cc-f519-445d-97ce-aae38f1eb550
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 06994bb4f0e606f0d67e797603e6f4a23899076b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185729"
---
# <a name="sp_helpmergepartition-transact-sql"></a>sp_helpmergepartition (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Partitionsinformationen für die angegebene Mergeveröffentlichung zurück. Diese gespeicherte Prozedur wird auf dem Verleger für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helpmergepartition [ @publication= ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]  
    [ , [ @host_name = ] 'host_name' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @suser_sname = ] 'suser_sname'` Der SUSER_SNAME Wert, der verwendet wird, um eine Partition zu definieren. *SUSER_SNAME* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Geben Sie diesen Parameter an, um das Resultset auf die Partitionen einzuschränken, bei denen SUSER_SNAME zu dem angegebenen Wert aufgelöst wird.  
  
> [!NOTE]  
>  Wenn *SUSER_SNAME* angegeben wird, muss *HOST_NAME* NULL sein.  
  
`[ @host_name = ] 'host_name'` Der HOST_NAME Wert, der verwendet wird, um eine Partition zu definieren. *HOST_NAME* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Geben Sie diesen Parameter an, um das Resultset auf die Partitionen einzuschränken, bei denen HOST_NAME zu dem angegebenen Wert aufgelöst wird.  
  
> [!NOTE]  
>  Wenn *SUSER_SNAME* angegeben wird, muss *HOST_NAME* NULL sein.  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**partition**|**int**|Identifiziert die Abonnentenpartition.|  
|**host_name**|**sysname**|Der Wert, der beim Erstellen der Partition für ein Abonnement verwendet wird, das nach dem Wert der [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) -Funktion auf dem Abonnenten gefiltert wird.|  
|**suser_sname**|**sysname**|Der Wert, der beim Erstellen der Partition für ein Abonnement verwendet wird, das nach dem Wert der [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) -Funktion auf dem Abonnenten gefiltert wird.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Speicherort der gefilterten Datenmomentaufnahme für die Abonnentenpartition.|  
|**date_refreshed**|**datetime**|Das Datum, an dem der Momentaufnahmeauftrag zuletzt ausgeführt wurde, um eine gefilterte Datenmomentaufnahme für die Partition zu generieren.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifiziert den Auftrag, der die gefilterte Datenmomentaufnahme für eine Partition erstellt.|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_helpmergepartition** wird bei der Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** und der festen Daten Bank Rolle **db_owner** können **sp_helpmergepartition** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addmergepartition &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_dropmergepartition &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)  
  
  
