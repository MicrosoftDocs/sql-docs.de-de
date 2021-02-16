---
description: IS_SRVROLEMEMBER (Transact-SQL)
title: IS_SRVROLEMEMBER (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_SRVROLEMEMBER_TSQL
- IS_SRVROLEMEMBER
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_SRVROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 3241a44a-6958-415b-b8b7-2a1207c36ab3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4a3832b4f9e250c2ff3368d0a94dbf77843f16a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350397"
---
# <a name="is_srvrolemember-transact-sql"></a>IS_SRVROLEMEMBER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt an, ob eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung Element der angegebenen Serverrolle ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
IS_SRVROLEMEMBER ( 'role' [ , 'login' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 **'** *Rolle* **'**  
 Der Name der zu überprüfenden Serverrolle. *role* weist den Typ **sysname** auf.  
  
 Gültige Werte für *role* sind benutzerdefinierte Serverrollen sowie die folgenden festen Serverrollen:  

- Serverrollen
- serveradmin
- dbcreator
- setupadmin  
- bulkadmin
- securityadmin  
- diskadmin
- öffentlich  
- processadmin
  
 **'** *login* **'**  
 Der Name der zu überprüfenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung. *login* ist vom Datentyp **sysname** und hat den Standardwert NULL. Wenn kein Wert angegeben wird, basiert das Ergebnis auf dem aktuellen Ausführungskontext. Wenn der Parameter das Wort NULL enthalten ist, wird NULL zurückgegeben.  
  
## <a name="return-types"></a>Rückgabetypen  
 **int**  
  
|Rückgabewert|BESCHREIBUNG|  
|------------------|-----------------|  
|0|*login* ist kein Mitglied von *role*.<br /><br /> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt diese Anweisung immer 0 (null) zurück.|  
|1|*login* ist ein Mitglied von *role*.|  
|NULL|*role* oder *login* ist nicht gültig, oder Sie verfügen über keine Berechtigung zum Anzeigen der Rollenmitgliedschaft.|  
  
## <a name="remarks"></a>Bemerkungen  
 Mit IS_SRVROLEMEMBER legen Sie fest, ob der aktuelle Benutzer eine Aktion ausführen kann, für die die Berechtigungen der Serverrolle erforderlich sind.  
  
 Wenn eine Windows-Anmeldung wie Contoso\Mary5 für *login* angegeben wird, gibt **IS_SRVROLEMEMBER** den Wert **NULL** zurück, falls der Anmeldung der direkte Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht erteilt oder verweigert wurde.  
  
 Wenn der optionale *login*-Parameter nicht bereitgestellt wird und *login* eine Windows-Domänenanmeldung ist, kann sie aufgrund der Mitgliedschaft in einer Windows-Gruppe Mitglied einer festen Serverrolle sein. Um derartige indirekte Mitgliedschaften aufzulösen, fordert IS_SRVROLEMEMBER Informationen zu Windows-Gruppenmitgliedschaften vom Domänencontroller an. Wenn nicht auf den Domänencontroller zugegriffen werden kann oder der Domänencontroller nicht reagiert, berücksichtigt **IS_SRVROLEMEMBER** beim Zurückgeben der Informationen zur Rollenmitgliedschaft nur den Benutzer und die lokalen Gruppen. Wenn der angegebene Benutzer nicht der aktuelle Benutzer ist, kann sich der durch IS_SRVROLEMEMBER zurückgegebene Wert vom letzten Datenupdate des Authentifikators (z. B. Active Directory) für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterscheiden.  
  
 Wenn der optionale Anmeldeparameter bereitgestellt wird, muss die Windows-Anmeldung, die abgefragt wird, in sys.server_principals vorhanden sein, oder IS_SRVROLEMEMBER gibt NULL zurück. Dies gibt an, dass die Anmeldung ungültig ist.  
  
 Wenn der Anmeldeparameter eine Domänenanmeldung ist oder auf einer Windows-Gruppe basiert und auf den Domänencontroller nicht zugegriffen werden kann, schlagen Aufrufe von IS_SRVROLEMEMBER fehl und können falsche oder unvollständige Daten zurückgeben.  
  
 Wenn der Domänencontroller nicht verfügbar ist, gibt der Aufruf von IS_SRVROLEMEMBER genaue Informationen zurück, wenn der Windows-Prinzipal lokal authentifiziert werden kann, z. B. ein lokales Windows-Konto oder eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung.  
  
 **IS_SRVROLEMEMBER** gibt immer 0 (null) zurück, wenn eine Windows-Gruppe als Anmeldeargument verwendet wird. Diese Windows-Gruppe ist ein Element einer anderen Windows-Gruppe, die wiederum ein Element der angegebenen Serverrolle ist.  
  
 Die Einstellung „Benutzerkontensteuerung“ kann dazu führen, dass andere Ergebnisse zurückgegeben werden. Dies hängt davon ab, ob der Benutzer auf den Server als Mitglied einer Windows-Gruppe oder als ein bestimmter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer zugreift.  
  
 Diese Funktion wertet die Rollenmitgliedschaft aus, nicht die zugrunde liegende Berechtigung. Die feste Serverrolle **sysadmin** besitzt z.B. die **CONTROL SERVER**-Berechtigung. Wenn der Benutzer die **CONTROL SERVER**-Berechtigung besitzt, aber nicht Mitglied der Rolle ist, meldet diese Funktion ordnungsgemäß, dass der Benutzer nicht Mitglied der **sysadmin**-Rolle ist, obwohl der Benutzer dieselben Berechtigungen besitzt.  
  
## <a name="related-functions"></a>Verwandte Funktionen  
 Verwenden Sie [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md), um zu ermitteln, ob der aktuelle Benutzer ein Mitglied der angegebenen Windows-Gruppe oder der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbankrolle ist. Zum Bestimmen, ob eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung ein Mitglied einer Datenbankrolle ist, verwenden Sie [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DEFINITION-Berechtigung für die Serverrolle.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt an, ob die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung für den aktuellen Benutzer Mitglied der festen Serverrolle `sysadmin` ist.  
  
```sql  
IF IS_SRVROLEMEMBER ('sysadmin') = 1  
   print 'Current user''s login is a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') = 0  
   print 'Current user''s login is NOT a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') IS NULL  
   print 'ERROR: The server role specified is not valid.';  
```  
  
 Das folgende Beispiel gibt an, ob die Domänenanmeldung Pat Mitglied der festen Serverrolle **diskadmin** ist.  
  
```sql  
SELECT IS_SRVROLEMEMBER('diskadmin', 'Contoso\Pat');  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
