---
description: sp_removedistpublisherdbreplication (Transact-SQL)
title: sp_removedistpublisherdbreplication (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_removedistpublisherdbreplication_TSQL
- sp_removedistpublisherdbreplication
helpviewer_keywords:
- sp_removedistpublisherdbreplication
ms.assetid: 9bfe002a-25b5-4226-bcfb-feb2060d6b4a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0a2f8b85702db26764b08b7c62b8db8ff1dda1d0
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89538589"
---
# <a name="sp_removedistpublisherdbreplication-transact-sql"></a>sp_removedistpublisherdbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Löscht Veröffentlichungsmetadaten, die zu einer bestimmten Veröffentlichung auf dem Verteiler gehören. Diese gespeicherte Prozedur wird auf dem Verteiler für die Verteilungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_removedistpublisherdbreplication [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verleger Servers. *Publisher* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standard.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_removedistpublisherdbreplication** wird von der Transaktions-und der Momentaufnahme Replikation verwendet.  
  
 **sp_removedistpublisherdbreplication** wird verwendet, wenn eine veröffentlichte Datenbank neu erstellt werden muss, ohne auch die Verteilungs Datenbank zu löschen. Es werden folgende Metadaten entfernt:  
  
-   Alle Veröffentlichungsmetadaten.  
  
-   Metadaten für alle Artikel, die zur Veröffentlichung gehören.  
  
-   Metadaten aller Abonnements für die Veröffentlichung.  
  
-   Metadaten für alle Replikations-Agent-Aufträge, die zur Veröffentlichung gehören.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** auf dem Verteiler oder Mitglieder der festen Daten Bank Rolle **db_owner** in der Verteilungs Datenbank können **sp_removedistpublisherdbreplication**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
