---
description: DBCC FLUSHAUTHCACHE (Transact-SQL)
title: DBCC FLUSHAUTHCACHE (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC FLUSHAUTHCACHE
- FLUSHAUTHCACHE
- DBCC_FLUSHAUTHCACHE_TSQL
- FLUSHAUTHCACHE_TSQL
helpviewer_keywords:
- DBCC FLUSHAUTHCACHE
ms.assetid: 681ef31d-ceb9-4da5-86bf-bf1240df950f
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 0927bb62d78df87d007874e21dccadab9b8ddf4d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468251"
---
# <a name="dbcc-flushauthcache-transact-sql"></a>DBCC FLUSHAUTHCACHE (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Leert den Cache für die Datenbankauthentifizierung mit Informationen zu Anmeldungen und Firewallregeln für die aktuelle Benutzerdatenbank in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Diese Anweisung gilt nicht für die logische Masterdatenbank, weil die Masterdatenbank den physischen Speicher für Informationen zu Anmeldungen und Firewallregeln enthält. Für den Benutzer, der die Anweisung ausführt, und andere zum aktuellen Zeitpunkt verbundene Benutzer wird weiterhin keine Verbindung hergestellt. (DBCC FLUSHAUTHCACHE wird derzeit für [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] nicht unterstützt.)
 
![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Artikellinksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DBCC FLUSHAUTHCACHE [ ; ]  
```

## <a name="arguments"></a>Argumente  
Keine.
  
## <a name="remarks"></a>Bemerkungen  
Der Authentifizierungscache erstellt eine Kopie von Anmeldungen und Serverfirewallregeln, die in der Masterdatenbank gespeichert sind, und überträgt diese in den Arbeitsspeicher der Benutzerdatenbank.  Da Informationen zu Benutzern eigenständiger Datenbanken bereits in der Benutzerdatenbank gespeichert sind, werden diese Benutzer nicht im Authentifizierungscache gespeichert.
Ständig aktive Verbindungen mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] erfordern alle 10 Stunden eine erneute Authentifizierung, die von [!INCLUDE[ssDE](../../includes/ssde-md.md)] durchgeführt. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] versucht, eine erneute Authentifizierung mit dem ursprünglich übermittelten Kennwort durchzuführen. Dabei ist keine Eingabe des Benutzers erforderlich. Aus Leistungsgründen wird die Verbindung nicht erneut authentifiziert, wenn ein Kennwort in der [!INCLUDE[ssSDS](../../includes/sssds-md.md)] zurückgesetzt wird. Dies ist auch nicht der Fall, wenn die Verbindung aufgrund von Verbindungspooling zurückgesetzt wird. Dieses Verhalten unterscheidet sich von dem Verhalten einer lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz. Wenn das Kennwort nach der ersten Authentifizierung der Verbindung geändert wurde, muss diese Verbindung beendet und unter Verwendung des neuen Kennworts eine neue Verbindung hergestellt werden. Ein Benutzer mit der KILL DATABASE CONNECTION-Berechtigung kann eine Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] explizit beenden, wenn er den Befehl [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md) verwendet.
  
## <a name="permissions"></a>Berechtigungen  
Erfordert das [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Administratorkonto.
  
## <a name="example"></a>Beispiel  
Die folgende Anweisung entfernt den Authentifizierungscache für die aktuelle Datenbank.
  
```sql
DBCC FLUSHAUTHCACHE;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
