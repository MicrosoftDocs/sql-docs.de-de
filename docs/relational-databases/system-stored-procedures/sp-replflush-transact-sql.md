---
description: sp_replflush (Transact-SQL)
title: sp_replflush (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_replflush
- sp_replflush_TSQL
helpviewer_keywords:
- sp_replflush
ms.assetid: 20809f5f-941d-427f-8f0c-de7a6c487584
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cb5d459f1a9b89836a412450eb4215182b914ba8
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541190"
---
# <a name="sp_replflush-transact-sql"></a>sp_replflush (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Leert den Artikelcache. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
> [!IMPORTANT]  
>  Sie sollten diese Prozedur nicht manuell ausführen müssen. **sp_replflush** sollten nur für die Problembehandlung der Replikation verwendet werden, wie von einem erfahrenen Supportmitarbeiter unterstützt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_replflush  
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_replflush** wird bei der Transaktions Replikation verwendet.  
  
 Artikeldefinitionen werden aus Effizienzgründen im Cache gespeichert. **sp_replflush** wird von anderen gespeicherten Replikations Prozeduren verwendet, wenn eine Artikel Definition geändert oder gelöscht wird.  
  
 Auf jede Datenbank kann nur eine Clientverbindung Protokolllesezugriff haben. Wenn ein Client Protokoll Lesezugriff auf eine Datenbank hat, bewirkt das Ausführen von **sp_replflush** , dass der Client seinen Zugriff freigibt. Andere Clients können dann das Transaktionsprotokoll mithilfe von **sp_replcmds** oder **sp_replshowcmds**scannen.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_replflush**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_repltrans &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
