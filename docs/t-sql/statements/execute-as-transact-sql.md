---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 56bf90c0d3022ca15030faadeef731aa1e5bc982
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464898"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Legt den Ausführungskontext einer Sitzung fest.  
  
 Standardmäßig beginnt eine Sitzung, wenn sich ein Benutzer anmeldet, und sie endet mit dem Abmelden. Alle Vorgänge während einer Sitzung hängen von Berechtigungsüberprüfungen für diesen Benutzer ab. Bei Ausführung einer **EXECUTE AS**-Anweisung wird der Ausführungskontext der Sitzung zum angegebenen Anmeldenamen oder Benutzernamen gewechselt. Nach dem Kontextwechsel werden Berechtigungen anhand der Sicherheitstoken des Anmeldenamens und Benutzers für dieses Konto überprüft, anstatt für die Person, die die **EXECUTE AS**-Anweisung aufgerufen hat. Im Wesentlichen wird die Identität des Benutzer- oder Anmeldekontos für die Dauer der Sitzung oder der Modulausführung übernommen, oder der Kontextwechsel wird explizit zurückgesetzt.  
  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 LOGIN  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt an, dass es sich bei dem Ausführungskontext, dessen Identität übernommen werden soll, um einen Anmeldenamen handelt. Der Bereich des Identitätswechsels liegt auf Serverebene.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank oder in SQL-Datenbank oder [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] nicht verfügbar.  
  
 USER  
 Gibt an, dass der Kontext, der als Identität angenommen werden soll, ein Benutzer in der aktuellen Datenbank ist. Der Identitätswechselbereich ist auf die aktuelle Datenbank beschränkt. Bei einem Kontextwechsel zu einem Datenbankbenutzer werden die Berechtigungen auf Serverebene dieses Benutzers nicht geerbt.  
  
> [!IMPORTANT]  
>  Für die Dauer des Kontextwechsels zum Datenbankbenutzer schlägt die Anweisung bei jedem Versuch, auf Ressourcen außerhalb der Datenbank zuzugreifen, fehl. Dies schließt USE-*Datenbankanweisungen*, verteilte Abfragen und Abfragen ein, die auf eine andere Datenbank mit drei- oder vierteiligen Bezeichnern verweisen.  
  
 *name* ist ein gültiger Benutzer- oder Anmeldename. *name* muss ein Mitglied der festen Serverrolle **sysadmin** sein oder als Prinzipal in [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) bzw. [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) vorhanden sein.  
  
 *name* kann als lokale Variable angegeben werden.  
  
 *name* muss ein einzelnes Konto und kann keine Gruppe, Rolle, kein Zertifikat, Schlüssel oder integriertes Konto sein, wie z.B. NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService oder NT AUTHORITY\LocalSystem.  
  
 Weitere Informationen finden Sie unter [Angeben eines Benutzer- oder Anmeldenamens](#_user) weiter unten in diesem Thema.  
  
 NO REVERT  
 Gibt an, dass der Kontextwechsel nicht auf den vorherigen Kontext zurückgesetzt werden kann. Die **NO REVERT**-Option kann nur auf der Ad-hoc-Ebene verwendet werden.  
  
 Weitere Informationen zum Zurücksetzen auf den vorherigen Kontext finden Sie unter [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md).  
  
 COOKIE INTO @*varbinary_variable*  
 Gibt an, dass der Ausführungskontext nur auf den vorherigen Kontext zurückgesetzt werden kann, wenn die aufrufende REVERT WITH COOKIE-Anweisung den richtigen @*varbinary_variable*-Wert enthält. [!INCLUDE[ssDE](../../includes/ssde-md.md)] übergibt das Cookie an @*varbinary_variable*. Die **COOKIE INTO**-Option kann nur auf der Ad-hoc-Ebene verwendet werden.  
  
 @*varbinary_variable* entspricht **varbinary(8000)** .  
  
> [!NOTE]  
>  Der **OUTPUT**-Cookieparameter ist zurzeit als **varbinary(8000)** dokumentiert, was der korrekten maximalen Länge entspricht. Die aktuelle Implementierung gibt jedoch **varbinary(100)** zurück. Anwendungen müssen **varbinary(8000)** reservieren, damit die Anwendung weiterhin ordnungsgemäß ausgeführt wird, falls die Rückgabegröße des Cookies in einem zukünftigen Release erhöht wird.  
  
 CALLER  
 Bei Verwendung innerhalb eines Moduls gibt dieser Wert an, dass die Anweisungen innerhalb des Moduls im Kontext des Modulaufrufers ausgeführt werden.
Bei Verwendung außerhalb eines Moduls weist die Anweisung keine Aktion auf.
 > [!NOTE]  
>  Diese Option steht in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]nicht zur Verfügung.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Änderung des Ausführungskontexts bleibt wirksam, bis eine der folgenden Bedingungen auftritt:  
  
-   Eine andere EXECUTE AS-Anweisung wird ausgeführt.  
  
-   Eine REVERT-Anweisung wird ausgeführt.  
  
-   Die Sitzung wird gelöscht.  
  
-   Die gespeicherte Prozedur oder der Trigger, für die bzw. den der Befehl ausgeführt wurde, wird beendet.  
  
Sie können einen Ausführungskontextstapel erstellen, indem Sie die EXECUTE AS-Anweisung mehrmals für mehrere Prinzipale aufrufen. Bei Aufruf wechselt die REVERT-Anweisung den Kontext zu dem Anmelde- oder Benutzernamen in der nächsten Ebene des Kontextstapels. Eine Demonstration dieses Verhaltens finden Sie unter [Beispiel A](#_exampleA).  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Angeben eines Benutzer- oder Anmeldenamens  
 Der in EXECUTE AS \<context_specification> angegebene Benutzer- oder Anmeldename muss als Prinzipal entsprechend in **sys.database_principals** oder **sys.server_principals** vorhanden sein. Andernfalls schlägt die EXECUTE AS-Anweisung fehl. Zudem müssen für den Prinzipal IMPERSONATE-Berechtigungen erteilt worden sein. Falls der Aufrufer nicht der Datenbankbesitzer oder ein Mitglied der festen Serverrolle **sysadmin** ist, muss der Prinzipal auch dann vorhanden sein, wenn der Benutzer als Windows-Gruppenmitglied auf die Datenbank oder Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugreift. Stellen Sie sich z. B. folgende Bedingungen vor: 
  
-   Die **CompanyDomain\SQLUsers**-Gruppe verfügt über Zugriff auf die **Sales**-Datenbank.  
  
-   **CompanyDomain\SqlUser1** ist ein Element von **SQLUsers** und hat deshalb impliziten Zugriff auf die **Sales**-Datenbank.  
  
 Obwohl **CompanyDomain\SqlUser1** über die Mitgliedschaft in der **SQLUsers**-Gruppe Zugriff auf die Datenbank besitzt, schlägt die `EXECUTE AS USER = 'CompanyDomain\SqlUser1'`-Anweisung fehl, da `CompanyDomain\SqlUser1` nicht als Prinzipal in der Datenbank vorhanden ist.  
  
Wenn der Benutzer verwaist ist (der zugeordnete Anmeldename ist nicht mehr vorhanden), und der Benutzer nicht mit **WITHOUT LOGIN** erstellt wurde, erzeugt **EXECUTE AS** einen Fehler für den Benutzer.  
  
## <a name="best-practice"></a>Bewährte Methode  
 Geben Sie einen Anmelde- oder Benutzernamen an, der die geringsten Privilegien besitzt, die zum Ausführen der Vorgänge in der Sitzung erforderlich sind. Geben Sie z. B. keinen Anmeldenamen mit Berechtigungen auf Serverebene an, wenn nur Berechtigungen auf Datenbankebene erforderlich sind. Oder geben Sie nur dann ein Datenbankbesitzerkonto an, wenn diese Berechtigungen tatsächlich erforderlich sind.  
  
> [!CAUTION]  
>  Die EXECUTE AS-Anweisung kann erfolgreich sein, wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] den Namen auflösen kann. Wenn ein Domänenbenutzer vorhanden ist, kann Windows den Benutzer für [!INCLUDE[ssDE](../../includes/ssde-md.md)] möglicherweise auflösen, obwohl der Windows-Benutzer keinen Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat. Dies kann zu einer Bedingung führen, in der eine Anmeldung ohne Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anscheinend nicht angemeldet ist, obwohl die Anmeldung für den Identitätswechsel nur Berechtigungen besitzt, die public oder guest gewährt wurden.  
  
## <a name="using-with-no-revert"></a>Verwenden von WITH NO REVERT  
 Wenn die EXECUTE AS-Anweisung die optionale WITH NO REVERT-Klausel enthält, kann der Ausführungskontext einer Sitzung nicht mithilfe von REVERT oder durch Ausführen einer anderen EXECUTE AS-Anweisung zurückgesetzt werden. Der von der Anweisung festgelegte Kontext bleibt aktiv, bis die Sitzung gelöscht wird.  
  
 Wenn die WITH NO REVERT COOKIE = @*varbinary_variable*-Klausel angegeben wird, übergibt [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] den Cookiewert an @*varbinary_variable*. Der von dieser Anweisung festgelegte Ausführungskontext kann nur auf einen vorherigen Kontext zurückgesetzt werden, wenn die aufrufende REVERT WITH COOKIE = @*varbinary_variable*-Anweisung den gleichen *\@varbinary_variable*-Wert enthält.  
  
 Diese Option ist in einer Umgebung nützlich, in der Verbindungspools verwendet werden. Mithilfe von Verbindungspools wird eine Gruppe von Datenbankverbindungen verwaltet, die von Anwendungen auf einem Anwendungsserver wiederverwendet werden können. Da nur der Aufrufer der EXECUTE AS-Anweisung den an *\@varbinary_variable* übergebenen Wert kennt, kann der Aufrufer sicherstellen, dass der eingerichtete Ausführungskontext von keinem anderen Benutzer geändert werden kann.  
  
## <a name="determining-the-original-login"></a>Bestimmen des ursprünglichen Anmeldenamens  
 Verwenden Sie die [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md)-Funktion, um den Anmeldenamen zurückzugeben, der die Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz hergestellt hat. Mit dieser Funktion können Sie die Identität des ursprünglichen Anmeldenamens in Sitzungen zurückgeben, in denen zahlreiche explizite oder implizite Kontextwechsel vorkommen.  
  
## <a name="permissions"></a>Berechtigungen  
 Um **EXECUTE AS** für einen Anmeldenamen anzugeben, muss der Aufrufer über die **IMPERSONATE**-Berechtigung für den angegebenen Anmeldenamen verfügen, und ihm darf die **IMPERSONATE ANY LOGIN**-Berechtigung nicht verweigert werden. Um **EXECUTE AS** für einen Datenbankbenutzer anzugeben, benötigt der Aufrufer **IMPERSONATE**-Berechtigungen für den angegebenen Benutzernamen. Wenn **EXECUTE AS CALLER** angegeben ist, sind keine **IMPERSONATE**-Berechtigungen erforderlich.  
  
## <a name="examples"></a>Beispiele  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. Verwenden von EXECUTE AS und REVERT zum Wechseln des Kontexts  
 Im folgenden Beispiel wird ein Kontextausführungsstapel mit mehreren Prinzipalen erstellt. Mit der `REVERT`-Anweisung wird der Ausführungskontext anschließend auf den vorherigen Aufrufer zurückgesetzt. Die `REVERT`-Anweisung wird mehrmals ausgeführt und bewegt sich so durch den Stapel, bis der Ausführungskontext wieder auf den ursprünglichen Aufrufer festgelegt ist.  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Verwenden der WITH COOKIE-Klausel  
 Im folgenden Beispiel wird der Ausführungskontext einer Sitzung auf einen angegebenen Benutzer festgelegt und die WITH COOKIE INTO @*varbinary_variable*-Klausel angegeben. In der `REVERT`-Anweisung muss der an die `@cookie`-Variable in der `EXECUTE AS`-Anweisung übergebene Wert angegeben sein, damit der Kontext erfolgreich auf den Aufrufer zurückgesetzt werden kann. Zur Ausführung dieses Beispiels müssen der `login1`-Anmeldename und der `user1`-Benutzer, die in Beispiel A erstellt wurden, vorhanden sein.  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS-Klausel &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  

