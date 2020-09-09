---
description: sp_copymergesnapshot (Transact-SQL)
title: sp_copymergesnapshot (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_copymergesnapshot
- sp_copymergesnapshot_TSQL
helpviewer_keywords:
- sp_copymergesnapshot
ms.assetid: eaecd6e0-8486-4e5d-ace7-8ae75768c0a8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4f763c28087802df2dd3922b54829f6dee888545
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536661"
---
# <a name="sp_copymergesnapshot-transact-sql"></a>sp_copymergesnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Kopiert den Momentaufnahme Ordner der angegebenen Veröffentlichung in den Ordner, der in der ** \@ destination_folder**aufgeführt ist. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_copymergesnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, deren Momentaufnahme Inhalt kopiert werden soll. *Publication* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @destination_folder = ] 'destination_folder'` Der Name des Ordners, in den der Inhalt der Veröffentlichungs Momentaufnahme kopiert werden soll. *destination_folder*ist vom Datentyp **nvarchar (255)** und hat keinen Standardwert. Der *destination_folder* kann ein alternativer Speicherort sein, z. b. auf einem anderen Server, auf einem Netzlaufwerk oder auf einem Wechselmedium (z. b. CD-ROMs oder Wechsel Datenträger).  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_copymergesnapshot** wird bei der Mergereplikation verwendet. Abonnenten, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Version 7,0 und früher ausführen, können den alternativen Momentaufnahme Speicherort nicht verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_copymergesnapshot**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Alternative Speicherorte für Momentaufnahme Ordner](../../relational-databases/replication/snapshot-options.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
