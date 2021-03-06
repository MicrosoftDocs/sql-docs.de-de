---
description: CREATE FULLTEXT CATALOG (Transact-SQL)
title: CREATE FULLTEXT CATALOG (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CATALOG_TSQL
- CREATE_FULLTEXT_TSQL
- FULLTEXT_TSQL
- FULLTEXT CATALOG
- CREATE FULLTEXT CATALOG
- CREATE_FULLTEXT_CATALOG_TSQL
- CATALOG
- FULLTEXT_CATALOG_TSQL
- CREATE FULLTEXT
- FULLTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- CREATE FULLTEXT CATALOG statement
ms.assetid: d7a8bd93-e2d7-4a40-82ef-39069e65523b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0166a4c24664172e3c101e9495a250835f29158a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159136"
---
# <a name="create-fulltext-catalog-transact-sql"></a>CREATE FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Erstellt einen Volltextkatalog für eine Datenbank. Ein Volltextkatalog kann mehrere Volltextindizes besitzen, ein Volltextindex kann jedoch nur Teil eines Volltextkatalogs sein. Jede Datenbank kann keinen oder mehrere Volltextkataloge enthalten.  
  
 In den Datenbanken **master**, **model** oder **tempdb** kann kein Volltextkatalog erstellt werden.  
  
> [!IMPORTANT]  
>  Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] stellt ein Volltextkatalog ein virtuelles Objekt dar, das keiner Dateigruppe angehört. Ein Volltextkatalog ist ein logisches Konzept, das auf eine Gruppe von Volltextindizes verweist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
CREATE FULLTEXT CATALOG catalog_name  
     [ON FILEGROUP filegroup ]  
     [IN PATH 'rootpath']  
     [WITH <catalog_option>]  
     [AS DEFAULT]  
     [AUTHORIZATION owner_name ]  
  
<catalog_option>::=  
     ACCENT_SENSITIVITY = {ON|OFF}  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *catalog_name*  
 Der Name des neuen Katalogs. Der Katalogname muss für alle Katalognamen in der aktuellen Datenbank eindeutig sein. Zudem muss der Name der Datei, die dem Volltextkatalog entspricht (siehe ON FILEGROUP), für alle Dateien in der Datenbank eindeutig sein. Wenn der Name des Katalogs bereits für einen anderen Katalog in der Datenbank verwendet wird, wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Fehler zurückgegeben.  
  
 Der Katalogname darf maximal 120 Zeichen enthalten.  
  
 ON FILEGROUP *filegroup*  
 Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hat diese Klausel keine Auswirkungen.  
  
 IN PATH **'** _rootpath_ **'**  
 > [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hat diese Klausel keine Auswirkungen.  
  
 ACCENT_SENSITIVITY = {ON|OFF}  
 Gibt an, ob für den Katalog bei der Volltextindizierung nach Akzent unterschieden wird. Bei einer Änderung dieser Eigenschaft muss der Index neu erstellt werden. Standardmäßig wird die in der Datenbanksortierung angegebene Unterscheidung nach Akzent verwendet. Die Datenbanksortierung kann in der **sys.databases**-Katalogsicht angezeigt werden.  
  
 Verwenden Sie die FULLTEXTCATALOGPROPERTY-Funktion mit dem **accentsensitivity**-Eigenschaftswert für *catalog_name*, um die aktuelle Eigenschaftseinstellung für die Unterscheidung nach Akzent eines Volltextkatalogs zu bestimmen. Wird '1' zurückgegeben, unterscheidet der Volltextkatalog nach Akzent. Wird '0' zurückgegeben, unterscheidet der Katalog nicht nach Akzent.  
  
 AS DEFAULT  
 Gibt an, dass der Katalog als Standardkatalog verwendet wird. Wenn beim Erstellen von Volltextindizes nicht explizit ein Volltextkatalog angegeben wird, wird der Standardkatalog verwendet. Falls ein vorhandener Volltextkatalog bereits mit AS DEFAULT gekennzeichnet ist, wird durch das Festlegen von AS DEFAULT für den neuen Katalog dieser Katalog als standardmäßiger Volltextkatalog verwendet.  
  
 AUTHORIZATION *owner_name*  
 Legt den Namen eines Datenbankbenutzers oder einer Datenbankrolle als Besitzer des Volltextkatalogs fest. Wenn für *owner_name* eine Rolle angegeben ist, muss dies der Name einer Rolle sein, deren Mitglied der aktuelle Benutzer ist, oder der Benutzer, der die Anweisung ausführt, muss der Datenbankbesitzer oder Systemadministrator sein.  
  
 Wenn ein Benutzername für *owner_name* angegeben ist, muss es sich um einen der folgenden Benutzernamen handeln:  
  
-   Den Namen des Benutzers, der die Anweisung ausführt.  
  
-   Den Namen eines Benutzers, für den der Benutzer, der den Befehl ausführt, IMPERSONATE-Berechtigungen besitzt.  
  
-   Oder der Benutzer, der den Befehl ausführt, muss der Datenbankbesitzer oder Systemadministrator sein.  
  
 *owner_name* muss zudem über die TAKE OWNERSHIP-Berechtigung im angegebenen Volltextkatalog verfügen.  
  
## <a name="remarks"></a>Hinweise  
 Volltextkatalog-IDs beginnen bei 00005 und werden mit jedem neu erstellten Katalog um eins erhöht.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss über die CREATE FULLTEXT CATALOG-Berechtigung für die Datenbank verfügen oder ein Mitglied der festen Datenbankrolle **db_owner** oder **db_ddladmin** sein.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden ein Volltextkatalog und ein Volltextindex erstellt.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume) KEY INDEX PK_JobCandidate_JobCandidateID;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Volltextsuche](../../relational-databases/search/full-text-search.md)   
 
  
  
