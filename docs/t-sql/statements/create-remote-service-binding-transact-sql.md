---
description: CREATE REMOTE SERVICE BINDING (Transact-SQL)
title: CREATE REMOTE SERVICE BINDING (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE REMOTE SERVICE BINDING
- SERVICE_BINDING_TSQL
- CREATE REMOTE SERVICE
- REMOTE_TSQL
- CREATE_REMOTE_SERVICE_BINDING_TSQL
- CREATE_REMOTE_SERVICE_TSQL
- BINDING
- SERVICE BINDING
- BINDING_TSQL
- CREATE_REMOTE_TSQL
- REMOTE_SERVICE_TSQL
- CREATE REMOTE
- REMOTE SERVICE
- REMOTE_SERVICE_BINDING_TSQL
- REMOTE
- REMOTE SERVICE BINDING
dev_langs:
- TSQL
helpviewer_keywords:
- binding remote service [Service Broker]
- dialog security [Service Broker]
- end-to-end security [Service Broker]
- security [Service Broker], remote service bindings
- CREATE REMOTE SERVICE BINDING statement
- conversation security [Service Broker]
- remote service bindings [Service Broker], creating
ms.assetid: 4165c404-4d50-4063-9a6e-6e267d309376
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13abf5a16b800bc856d482fbc525a833b9d7e872
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188514"
---
# <a name="create-remote-service-binding-transact-sql"></a>CREATE REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erstellt eine Bindung, mit der die Sicherheitsanmeldeinformationen definiert werden, mit denen eine Konversation mit einem Remotedienst initiiert werden soll.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE REMOTE SERVICE BINDING binding_name   
   [ AUTHORIZATION owner_name ]   
   TO SERVICE 'service_name'   
   WITH  USER = user_name [ , ANONYMOUS = { ON | OFF } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *binding_name*  
 Der Name der zu erstellenden Remotedienstbindung. Server-, Datenbank- und Schemaname können nicht angegeben werden. *binding_name* muss einen gültigen **sysname**-Wert besitzen.  
  
 AUTHORIZATION *owner_name*  
 Legt den Besitzer der Bindung auf den angegebenen Datenbankbenutzer oder die angegebene Datenbankrolle fest. Ist der aktuelle Benutzer **dbo** oder **sa**, kann *owner_name* der Name eines beliebigen gültigen Benutzers bzw. einer beliebigen gültigen Rolle sein. Andernfalls muss *owner_name* der Name des aktuellen Benutzers, der Name eines Benutzers, für den der aktuelle Benutzer IMPERSONATE-Berechtigungen besitzt, oder der Name einer Rolle sein, der der aktuelle Benutzer angehört.  
  
 TO SERVICE '*service_name*'  
 Gibt den Remotedienst an, der an den in der WITH USER-Klausel identifizierten Benutzer gebunden werden soll.  
  
 USER = *user_name*  
 Gibt den Datenbankprinzipal an, der das Zertifikat besitzt, das dem von der TO SERVICE-Klausel identifizierten Remotedienst zugeordnet wird. Dieses Zertifikat wird für die Verschlüsselung und Authentifizierung von Nachrichten verwendet, die mit dem Remotedienst ausgetauscht werden.  
  
 ANONYMOUS  
 Gibt an, ob die anonyme Authentifizierung bei der Kommunikation mit dem Remotedienst verwendet wird. Wenn ANONYMOUS = ON angegeben ist, wird die anonyme Authentifizierung verwendet, und Vorgänge in der Remotedatenbank erscheinen als Mitglieder der festen Datenbankrolle **public**. Wenn ANONYMOUS = OFF angegeben ist, finden Vorgänge in der Remotedatenbank als bestimmte Benutzer dieser Datenbank statt. Wird diese Klausel nicht angegeben, ist die Standardeinstellung OFF.  
  
## <a name="remarks"></a>Bemerkungen  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] verwendet eine Remotedienstbindung, um nach dem für eine neue Konversation zu verwendenden Zertifikat zu suchen. Der öffentliche Schlüssel des Zertifikats, das *user_name* zugeordnet ist, wird zur Authentifizierung von Nachrichten an den Remotedienst und zur Verschlüsselung eines Sitzungsschlüssels verwendet, der dann zur Verschlüsselung der Konversation verwendet wird. Das Zertifikat für *user_name* muss dem Zertifikat für einen Benutzer in der Datenbank entsprechen, die als Host für den Remotedienst dient.  
  
 Eine Remotedienstbindung wird nur zum Initiieren von Diensten benötigt, die mit Zieldiensten außerhalb der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz kommunizieren. Eine Datenbank, die als Host für einen initiierenden Dienst dient, muss Remotedienstbindungen für alle Zieldienste außerhalb der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz enthalten. Eine Datenbank, die als Host für einen Zieldienst dient, muss keine Remotedienstbindungen für die initiierenden Dienste enthalten, die mit dem Zieldienst kommunizieren. Wenn sich der Initiator- und der Zieldienst in der gleichen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befinden, ist keine Dienstbindung erforderlich. Wenn hingegen eine Remotedienstbindung vorhanden ist, in der *service_name* für TO SERVICE dem Namen des lokalen Diensts entspricht, verwendet [!INCLUDE[ssSB](../../includes/sssb-md.md)] die Bindung.  
  
 Wenn ANONYMOUS = ON festgelegt wurde, stellt der initiierende Dienst eine Verbindung mit dem Zieldienst als Mitglied der festen Datenbankrolle **public** her. Standardmäßig haben Mitglieder dieser Rolle keine Berechtigung, eine Verbindung mit einer Datenbank herzustellen. Damit eine Nachricht erfolgreich gesendet wird, muss die Zieldatenbank der **public**-Rolle die CONNECT-Berechtigung für die Datenbank und die SEND-Berechtigung für den Zieldienst erteilen.  
  
 Besitzt ein Benutzer mehrere Zertifikate, wählt [!INCLUDE[ssSB](../../includes/sssb-md.md)] das Zertifikat mit dem aktuellsten Ablaufzeitpunkt aus den derzeit gültigen und als AVAILABLE FOR BEGIN_DIALOG gekennzeichneten Zertifikaten aus.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Erstellen einer Remotedienstbindung erhalten standardmäßig in der USER-Klausel benannte Benutzer, Mitglieder der festen Datenbankrollen **db_owner** und **db_ddladmin** sowie Mitglieder der festen Serverrolle **sysadmin**.  
  
 Der Benutzer, der die CREATE REMOTE SERVICE BINDING-Anweisung ausführt, muss berechtigt sein, die Identität des in der Anweisung angegebenen Prinzipals anzunehmen.  
  
 Eine Remotedienstbindung ist möglicherweise kein temporäres Objekt. Namen von Remotedienstbindungen, die mit **#** beginnen, sind zulässig. Sie sind jedoch dauerhafte Objekte.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-remote-service-binding"></a>A. Erstellen einer Remotedienstbindung  
 Im folgenden Beispiel wird eine Bindung für den Dienst `//Adventure-Works.com/services/AccountsPayable` erstellt. [!INCLUDE[ssSB](../../includes/sssb-md.md)] führt mit dem Zertifikat, das sich im Besitz des `APUser`-Datenbankprinzipals befindet, eine Authentifizierung gegenüber dem Remotedienst durch und tauscht den Verschlüsselungsschlüssel für die Sitzung mit dem Remotedienst aus.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser ;  
```  
  
### <a name="b-creating-a-remote-service-binding-using-anonymous-authentication"></a>B. Erstellen einer Remotedienstbindung mit anonymer Authentifizierung  
 Im folgenden Beispiel wird eine Bindung für den Dienst `//Adventure-Works.com/services/AccountsPayable` erstellt. [!INCLUDE[ssSB](../../includes/sssb-md.md)] tauscht mit dem Zertifikat, das sich im Besitz des `APUser`-Datenbankprinzipals befindet, den Verschlüsselungsschlüssel für die Sitzung mit dem Remotedienst aus. Es findet keine Authentifizierung von Service Broker beim Remotedienst statt. In der Datenbank, die als Host für den Remotedienst dient, werden Nachrichten als **guest**-Benutzer übermittelt.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser, ANONYMOUS=ON ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
