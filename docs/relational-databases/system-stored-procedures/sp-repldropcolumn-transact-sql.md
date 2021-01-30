---
description: sp_repldropcolumn (Transact-SQL)
title: sp_repldropcolumn (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repldropcolumn_TSQL
- sp_repldropcolumn
helpviewer_keywords:
- sp_repldropcolumn
ms.assetid: fdc1ec5f-f108-42b4-a2d8-f06a71913ab8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c5fdabf724c284e8a75244269321b92c81b46ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206423"
---
# <a name="sp_repldropcolumn-transact-sql"></a>sp_repldropcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Löscht eine Spalte aus einem vorhandenen Tabellenartikel, der veröffentlicht worden ist. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
> [!IMPORTANT]
>  Diese gespeicherte Prozedur wurde als veraltet markiert und wird hauptsächlich aus Gründen der Abwärtskompatibilität unterstützt. Er sollte nur mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Verlegern und wieder Veröffentlichungs Abonnenten verwendet werden [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . Diese Prozedur sollte nicht für Spalten mit Datentypen verwendet werden, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] oder höher eingeführt wurden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_repldropcolumn [ @source_object = ] 'source_object', [ @column = ] 'column'   
    [ , [ @from_agent = ] from_agent]   
    [ , [ @schema_change_script = ] 'schema_change_script' ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Argumente  
 [ @source_object =] '*source_object*'  
 Ist der Name des Tabellenartikels, der die zu löschende Spalte enthält. *source_object* ist vom Datentyp nvarchar (258) und hat keinen Standardwert.  
  
 [ @column =] '*Spalte*'  
 Entspricht dem Namen der Spalte in der zu löschenden Tabelle. *Column ist vom Datentyp* sysname und hat keinen Standardwert.  
  
 [ @from_agent =] *from_agent*  
 Gibt an, ob die gespeicherte Prozedur von einem Replikations-Agent ausgeführt wird. *from_agent* ist vom Datentyp int und hat den Standardwert 0. der Wert 1 wird verwendet, wenn diese gespeicherte Prozedur von einem Replikations-Agent ausgeführt wird. in jedem anderen Fall sollte der Standardwert 0 verwendet werden.  
  
 [ @schema_change_script =] '*schema_change_script*'  
 Gibt den Namen und Pfad eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Skripts an, das zum Ändern der systemgenerierten bzw. benutzerdefinierten gespeicherten Prozeduren dient. *schema_change_script* ist vom Datentyp nvarchar (4000) und hat den Standardwert NULL. Mithilfe der Replikation ist es möglich, mindestens eine der bei der Transaktionsreplikation verwendeten Standardprozeduren durch benutzerdefinierte gespeicherte Prozeduren zu ersetzen. *schema_change_script* wird ausgeführt, nachdem eine Schema Änderung an einem replizierten Tabellen Artikel mithilfe sp_repldropcolumn vorgenommen wurde, und kann verwendet werden, um eine der folgenden Aktionen auszuführen:  
  
-   Wenn benutzerdefinierte gespeicherte Prozeduren automatisch erneut generiert werden, können *schema_change_script* verwendet werden, um diese benutzerdefinierten gespeicherten Prozeduren zu löschen und durch benutzerdefinierte gespeicherte Prozeduren zu ersetzen, die das neue Schema unterstützen.  
  
-   Wenn benutzerdefinierte gespeicherte Prozeduren nicht automatisch erneut generiert werden, können *schema_change_script* zum erneuten Generieren dieser gespeicherten Prozeduren oder zum Erstellen benutzerdefinierter gespeicherter Prozeduren verwendet werden.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Aktiviert oder deaktiviert die Möglichkeit, eine Momentaufnahme für ungültig zu erklären. *force_invalidate_snapshot* ist ein Bit, der Standardwert ist 1.  
  
 Der Wert 1 gibt an, dass Änderungen am Artikel bewirken können, dass die Momentaufnahme ungültig wird. Wenn dies zutrifft, wird mit dem Wert 1 die Berechtigung für das Auftreten de neuen Momentaufnahme erteilt.  
  
 Der Wert 0 gibt an, dass Änderungen am Artikel nicht bewirken, dass die Momentaufnahme ungültig wird.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Aktiviert oder deaktiviert die Möglichkeit, das Abonnement erneut zu initialisieren. *force_reinit_subscription* ist ein Bit mit einem Standardwert von 0.  
  
 Der Wert 0 gibt an, dass Änderungen am Artikel nicht bewirken, dass das Abonnement erneut initialisiert wird.  
  
 Der Wert 1 gibt an, dass Änderungen am Artikel bewirken können, dass das Abonnement erneut initialisiert wird. Wenn dies zutrifft, wird mit dem Wert 1 die Berechtigung für das Auftreten der erneuten Initialisierung des Abonnements erteilt.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle sysadmin auf dem Verleger oder Mitglieder der festen Datenbankrollen db_owner oder db_ddladmin für die Veröffentlichungsdatenbank können sp_repldropcolumn ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Als veraltet markierte Funktionen in SQL Server-Replikation](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
