---
description: sp_restoredbreplication (Transact-SQL)
title: sp_restoredbreplication (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_restoredbreplication
- sp_restoredbreplication_TSQL
helpviewer_keywords:
- sp_restoredbreplication
ms.assetid: a2c5ee32-e6d9-46e9-8031-8ff13c20acf7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b64a39661fdceefade15d605ccc8e1c083ede0e6
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541549"
---
# <a name="sp_restoredbreplication-transact-sql"></a>sp_restoredbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Entfernt Replikationseinstellungen, wenn eine Datenbank auf einem Server, in einer Datenbank oder auf einem System wiederhergestellt wird, bei denen es sich nicht um die Ausgangsobjekte handelt oder die aus anderen Gründen nicht in der Lage sind, Replikationsprozesse auszuführen. Wenn eine replizierte Datenbank auf einem Server oder in einer Datenbank wiederhergestellt wird, von dem bzw. der die Sicherung nicht erstellt wurde, dann können die Replikationseinstellungen nicht beibehalten werden. Bei der Wiederherstellung ruft der Server **sp_restoredbreplication** direkt auf, um die Replikations Metadaten automatisch aus der wiederhergestellten Datenbank zu entfernen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_restoredbreplication [ @srv_orig = ] 'original_server_name'  
        , [ @db_orig = ] 'original_database_name'  
    [ , [ @keep_replication = ] keep_replication ]  
    [ , [ @perform_upgrade = ] perform_upgrade ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @srv_orig = ] 'original_server_name'`  
 Der Name des Servers, auf dem die Sicherung erstellt wurde. *original_server_name* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @db_orig = ] 'original_database_name'`  
 Name der Datenbank, die gesichert wurde. *original_database_name* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @keep_replication = ] keep_replication`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @perform_upgrade = ] perform_upgrade`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_restoredbreplication** wird bei allen Replikations Typen verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder **dbcreator** oder des **dbo** -Datenbankschemas können **sp_restoredbreplication**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
