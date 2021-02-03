---
description: SYSTEM_USER (Transact-SQL)
title: SYSTEM_USER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSTEM_USER_TSQL
- SYSTEM_USER
dev_langs:
- TSQL
helpviewer_keywords:
- current user names
- system-supplied user names [SQL Server]
- users [SQL Server], logins
- logins [SQL Server], identification name
- current system user names
- SYSTEM_USER function
- inserting system user name into table
- system usernames [SQL Server]
- users [SQL Server], names
ms.assetid: 565984cd-60c6-4df7-83ea-2349b838ccb2
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3432cf3a1ba598f86ee5ac52b28922128171121e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197824"
---
# <a name="system_user-transact-sql"></a>SYSTEM_USER (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Ermöglicht das Einfügen eines vom System bereitgestellten Werts für den aktuellen Anmeldenamen in eine Tabelle, wenn kein Standardwert angegeben ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SYSTEM_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Hinweise  
 Sie können die SYSTEM_USER-Funktion mit DEFAULT-Einschränkungen in den Anweisungen CREATE TABLE und ALTER TABLE verwenden. Sie können sie auch wie jede beliebige Standardfunktion verwenden.  
  
 Unterscheiden sich Benutzer- und Anmeldename, gibt SYSTEM_USER den Anmeldenamen zurück.  
  
 Wenn der aktuelle Benutzer über die Windows-Authentifizierung bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angemeldet ist, gibt SYSTEM_USER den Windows-Anmeldenamen im folgenden Format zurück: *DOMAIN*\\*user_login_name*. Wenn der aktuelle Benutzer jedoch über die SQL Server-Authentifizierung bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angemeldet ist, gibt SYSTEM_USER den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen zurück, z. B. `WillisJo` für einen als `WillisJo` angemeldeten Benutzer.  
  
 SYSTEM_USER gibt den Namen des zurzeit ausgeführten Kontexts zurück. Wenn der Kontext mithilfe der EXECUTE AS-Anweisung gewechselt wurde, gibt SYSTEM_USER den Namen des Kontexts zurück, dessen Identität angenommen wurde.  

 EXECUTE AS kann nicht mit der SYSTEM_USER-Funktion verwendet werden.

## <a name="examples"></a>Beispiele  
  
### <a name="a-using-system_user-to-return-the-current-system-user-name"></a>A. Verwenden von SYSTEM_USER zur Rückgabe des aktuellen Systembenutzernamens  
 Im folgenden Beispiel wird eine `char`-Variable deklariert, der aktuelle Wert von `SYSTEM_USER` in der Variablen gespeichert und dann der in der Variablen gespeicherte Wert ausgegeben.  
  
```sql
DECLARE @sys_usr CHAR(30);  
SET @sys_usr = SYSTEM_USER;  
SELECT 'The current system user is: '+ @sys_usr;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
----------------------------------------------------------
The current system user is: WillisJo

(1 row(s) affected)
 ```  
  
### <a name="b-using-system_user-with-default-constraints"></a>B. Verwenden von SYSTEM_USER mit DEFAULT-Einschränkungen  
 Im folgenden Beispiel wird eine Tabelle mit `SYSTEM_USER` als `DEFAULT`-Einschränkung für die `SRep_tracking_user`-Spalte erstellt.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.Sales_Tracking  
(  
    Territory_id INT IDENTITY(2000, 1) NOT NULL,  
    Rep_id INT NOT NULL,  
    Last_sale DATETIME NOT NULL DEFAULT GETDATE(),  
    SRep_tracking_user VARCHAR(30) NOT NULL DEFAULT SYSTEM_USER  
);  
GO  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (151);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (293, '19980515');  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (27882, '19980620');  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (21392);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (24283, '19981130');  
GO  
```  
  
 Mit der folgenden Abfrage werden alle Informationen aus der `Sales_Tracking`-Tabelle ausgewählt:  
  
```sql
SELECT * FROM Sales_Tracking ORDER BY Rep_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Territory_id Rep_id Last_sale            SRep_tracking_user
-----------  ------ -------------------- ------------------
2000         151    Mar 4 1998 10:36AM   ArvinDak
2001         293    May 15 1998 12:00AM  ArvinDak
2003         21392  Mar 4 1998 10:36AM   ArvinDak
2004         24283  Nov 3 1998 12:00AM   ArvinDak
2002         27882  Jun 20 1998 12:00AM  ArvinDak
  
(5 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-system_user-to-return-the-current-system-user-name"></a>C: Verwenden von SYSTEM_USER zur Rückgabe des aktuellen Systembenutzernamens  
 Im folgenden Beispiel wird der aktuelle Wert von `SYSTEM_USER` zurückgegeben.  
  
```sql
SELECT SYSTEM_USER;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [Systemfunktionen &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [USER &#40;Transact-SQL&#41;](../../t-sql/functions/user-transact-sql.md)  
  
  

