---
title: Anmelden bei SQL Server | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die verschiedenen Möglichkeiten, sich bei einer Instanz von SQL Server anzumelden. Hier erfahren Sie, welches Format für den Servernamen in verschiedenen Umgebungen verwendet werden muss.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server, logging in
- services [SQL Server], logging in
- TCP connection string
- connecting to the Database Engine
- logins [SQL Server], about logging in
- named pipe connection string
- log ins [SQL Server]
- shared memory connection string
- logging in [SQL Server]
- logins [SQL Server]
ms.assetid: 77158a9a-d638-4818-90a1-cb2eb57df514
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bb0fe13f5a15a726235c306c7b6b3c6b79bcef2c
ms.sourcegitcommit: 2f868a77903c1f1c4cecf4ea1c181deee12d5b15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91669603"
---
# <a name="logging-in-to-sql-server"></a>Anmelden an SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Sie können sich mithilfe eines beliebigen grafischen Administrationstools oder über die Eingabeaufforderung bei einer Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anmelden.  
  
 Wenn Sie sich mithilfe eines grafischen Administrationstools, wie beispielsweise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , bei einer Instanz von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]anmelden, werden Sie zum Angeben eines Servernamens, eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamens und gegebenenfalls eines Kennworts aufgefordert. Wenn Sie sich mithilfe der Windows-Authentifizierung bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anmelden, müssen Sie den SQL Server-Anmeldenamen nicht jedes Mal angeben, wenn Sie auf eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zugreifen. Stattdessen wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Ihr [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto zum automatischen Anmelden verwendet. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit Authentifizierung im gemischten Modus ausgeführt wird ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]- und Windows-Authentifizierungsmodus) und wenn Sie zum Anmelden die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwenden möchten, müssen Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen und ein Kennwort angeben. Verwenden Sie nach Möglichkeit die Windows-Authentifizierung.  
  
> [!NOTE]  
>  Wenn Sie beim Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]eine Sortierung ausgewählt haben, die zwischen Groß- und Kleinschreibung unterscheidet, wird bei der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung auch zwischen Groß- und Kleinschreibung unterschieden.  
  
## <a name="format-for-specifying-the-name-of-sql-server"></a>Format zum Angeben des Namens von SQL Server  
 Beim Herstellen einer Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] müssen Sie den Namen der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeben. Wenn es sich bei der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] um die Standardinstanz (eine unbenannte Instanz) handelt, geben Sie den Namen des Computers, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist, oder die IP-Adresse des Computers an. Wenn es sich bei der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] um eine benannte Instanz (z. B. SQLEXPRESS) handelt, geben Sie den Namen des Computers, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist, oder die IP-Adresse des Computers an, und fügen Sie einen Schrägstrich und den Instanznamen hinzu.  
  
 In den folgenden Beispielen wird eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt, die auf dem Computer APPHOST ausgeführt wird. Beim Angeben einer benannten Instanz wird im Beispiel der Instanzname SQLEXPRESS verwendet.  
  
 **Beispiele:**  
  
|Typ der Instanz|Eintrag für den Servernamen|  
|----------------------|-------------------------------|  
|Verbindung mit einer Standardinstanz unter Verwendung des Standardprotokolls.|APPHOST|  
|Verbindung mit einer benannten Instanz unter Verwendung des Standardprotokolls. |APPHOST\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz auf dem gleichen Computer unter Verwendung eines Punkts, der angibt, dass die Instanz auf dem lokalen Computer ausgeführt wird.|erforderlich.|  
|Verbindung mit einer benannten Instanz auf dem gleichen Computer unter Verwendung eines Punkts, der angibt, dass die Instanz auf dem lokalen Computer ausgeführt wird.|.\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz auf dem gleichen Computer unter Verwendung von "localhost", um anzugeben, dass die Instanz auf dem lokalen Computer ausgeführt wird.|localhost|  
|Verbindung mit einer benannten Instanz auf dem gleichen Computer unter Verwendung von "localhost", um anzugeben, dass die Instanz auf dem lokalen Computer ausgeführt wird.|localhost\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz auf dem gleichen Computer unter Verwendung von "(local)", um anzugeben, dass die Instanz auf dem lokalen Computer ausgeführt wird.|(local)|  
|Verbindung mit einer benannten Instanz auf dem gleichen Computer unter Verwendung von "(local)", um anzugeben, dass die Instanz auf dem lokalen Computer ausgeführt wird.|(local)\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz auf dem gleichen Computer unter Erzwingung einer Shared Memory-Verbindung.|lpc:APPHOST|  
|Verbindung mit einer benannten Instanz auf dem gleichen Computer unter Erzwingung einer Shared Memory-Verbindung.|lpc:APPHOST\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz, die an TCP-Adresse 192.168.17.28 lauscht, unter Verwendung einer IP-Adresse.|192.168.17.28|  
|Verbindung mit einer benannten Instanz, die an TCP-Adresse 192.168.17.28 lauscht, unter Verwendung einer IP-Adresse.|192.168.17.28\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz, die nicht am Standard-TCP-Port lauscht, unter Angabe des verwendeten Ports (in diesem Fall 2828). (Die Angabe einer Portnummer ist nicht notwendig, wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] am Standardport 1433 lauscht.)|APPHOST,2828|  
|Verbindung mit einer benannten Instanz auf einem festgelegten TCP-Port, in diesem Fall 2828. (Die Angabe einer Portnummer ist oft notwendig, wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Browserdienst nicht auf dem Hostcomputer ausgeführt wird.)|APPHOST,2828|  
|Verbindung mit einer Standardinstanz, die nicht am Standard-TCP-Port lauscht, unter Angabe der IP-Adresse und des verwendeten TCP-Ports (in diesem Fall 2828).|192.168.17.28,2828|  
|Verbindung mit einer benannten Instanz durch Angabe der IP-Adresse und des verwendeten TCP-Ports (in diesem Fall 2828).|192.168.17.28\SQLEXPRESS,2828|  
|Verbindung mit einer Standardinstanz nach Name unter Erzwingung einer TCP-Verbindung.|tcp:APPHOST|  
|Verbindung mit einer benannten Instanz nach Name unter Erzwingung einer TCP-Verbindung.|tcp:APPHOST\SQLEXPRESS|  
|Verbindung mit einer Standardinstanz unter Angabe eines Named Pipe-Namens.|\\\APPHOST\pipe\SQL\query|  
|Verbindung mit einer benannten Instanz unter Angabe eines Named Pipe-Namens.|\\\APPHOST\pipe\MSSQL$SQLEXPRESS\SQL\query|  
|Verbindung mit einer Standardinstanz nach Name unter Erzwingung einer Named Pipe-Verbindung.|np:APPHOST|  
|Verbindung mit einer benannten Instanz nach Name unter Erzwingung einer Named Pipe-Verbindung.|np:APPHOST\SQLEXPRESS|  
  
## <a name="verifying-your-connection-protocol"></a>Überprüfen Ihres Verbindungsprotokolls  
 Bei einer Verbindung mit [!INCLUDE[ssDE](../../includes/ssde-md.md)]gibt die folgende Abfrage das für die aktuelle Verbindung verwendete Protokoll sowie die Authentifizierungsmethode (NTLM oder Kerberos) zurück und gibt an, ob es sich um eine verschlüsselte Verbindung handelt:  
  
```sql  
SELECT net_transport, auth_scheme, encrypt_option   
FROM sys.dm_exec_connections   
WHERE session_id = @@SPID;  
```  
  
## <a name="related-tasks"></a>Related Tasks  
 [Anmelden an einer Instanz von SQL Server &#40;Befehlszeile&#41;](../../database-engine/configure-windows/log-in-to-an-instance-of-sql-server-command-prompt.md)  
  
 Die folgenden Ressourcen können beim Beheben von Verbindungsproblemen hilfreich sein:  
  
-   [So beheben Sie Verbindungsfehler mit der SQL Server-Datenbank-Engine](https://social.technet.microsoft.com/wiki/contents/articles/how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
-   [Schritte zum Beheben von SQL-Konnektivitätsproblemen](/archive/blogs/sql_protocols/steps-to-troubleshoot-sql-connectivity-issues)  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Auswählen eines Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md)  
  
 [Verwenden des Hilfsprogramms sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md)  
  
 [Erstellen einer Anmeldung](../../t-sql/lesson-2-configuring-permissions-on-database-objects.md)
  
