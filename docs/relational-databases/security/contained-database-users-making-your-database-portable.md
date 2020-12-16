---
title: Eigenständiger Benutzerzugriff auf eigenständige Datenbanken
description: Hier erfahren Sie, wie Sie den eigenständigen Benutzerzugriff auf eigenständige Datenbanken einrichten. Zudem werden die Unterschiede zu einem herkömmlichen Anmelde-/Benutzermodell erläutert.
ms.custom: seo-lt-2019
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- contained database, users
- user [SQL Server], about contained database users
ms.assetid: e57519bb-e7f4-459b-ba2f-fd42865ca91d
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9215ac429ff45d80a2ba1ea7d913a63ab49e34e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480951"
---
# <a name="contained-database-users---making-your-database-portable"></a>Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Verwenden Sie eigenständige Datenbankbenutzer, um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - und [!INCLUDE[ssSDS](../../includes/sssds-md.md)] -Verbindungen auf Datenbankebene zu authentifizieren. Eine eigenständige Datenbank ist eine Datenbank, die von anderen Datenbanken und der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/ [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (und der Masterdatenbank), der die Datenbank hostet, isoliert ist. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt eigenständige Datenbankbenutzer sowohl für die Windows- als auch für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung. Kombinieren Sie bei Verwendung von [!INCLUDE[ssSDS](../../includes/sssds-md.md)]eigenständige Datenbankbenutzer mit den Firewallregeln auf Datenbankebene. In diesem Thema werden die Unterschiede und Vorteile der Verwendung von einem eigenständigen Datenbankmodell im Vergleich zum herkömmlichen Anmelde-/Benutzermodell sowie zu Firewallregeln für Windows bzw. auf Serverebene vorgestellt. Bestimmte Szenarien, Verwaltbarkeit oder Anwendungsgeschäftslogik können dennoch den Einsatz des herkömmlichen Anmelde-/Benutzermodells und von Firewallregeln auf Serverebene erfordern.  
  
> [!NOTE]  
> Bei der Entwicklung des [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Diensts durch [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und dem Wechsel zu stärker garantierten SLAs, müssen Sie möglicherweise zum eigenständigen Datenbankbenutzermodell und den datenbankbezogenen Firewallregeln wechseln, um die SLA für höhere Verfügbarkeit sowie höhere maximale Anmelderaten für eine bestimmte Datenbank zu erreichen. [!INCLUDE[msCoName](../../includes/msconame-md.md)] ermutigt Sie, solche Änderungen noch heute zu berücksichtigen.  
  
## <a name="traditional-login-and-user-model"></a>Herkömmliches Anmelde- und Benutzermodell

 Beim herkömmlichen Verbindungsmodell stellen Windows-Benutzer oder Mitglieder der Windows-Gruppen eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] durch die Bereitstellung von Benutzer- oder Gruppenanmeldeinformationen her, die von Windows authentifiziert werden. Sie können auch sowohl einen Namen als auch ein Kennwort bereitstellen und die Verbindung über die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung herstellen. In beiden Fällen muss in der Masterdatenbank eine Anmeldung vorhanden sein, die den Anmeldeinformationen zur Verbindungsherstellung entspricht. Nachdem [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Anmeldeinformationen für die Windows-Authentifizierung bestätigt oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen authentifiziert, versucht die Verbindung in der Regel eine Verbindung zu einer Benutzerdatenbank herzustellen. Zum Herstellen einer Verbindung mit einer Benutzerdatenbank muss die Anmeldung einem Datenbankbenutzer in der Datenbank zugeordnet werden können. Die Verbindungszeichenfolge kann auch angeben, dass eine Verbindung mit einer bestimmten Datenbank hergestellt werden soll, was in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] optional und in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]erforderlich ist.  
  
 Das wichtige Prinzip ist, dass sowohl die Anmeldung (in der Masterdatenbank) als auch der Benutzer (in der Benutzerdatenbank) vorhanden sein müssen und miteinander verknüpft werden können. Dies bedeutet, dass die Verbindung mit der Benutzerdatenbank eine Abhängigkeit von der Anmeldung in der Masterdatenbank hat und dies die Möglichkeiten zum Verschieben der Datenbank auf einen anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - oder [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] -Hostingserver einschränkt. Wenn aus irgendeinem Grund eine Verbindung mit der Masterdatenbank nicht verfügbar ist (z. B. wenn ein Failover läuft), wird die Gesamtdauer der Verbindung erhöht oder es tritt ggf. ein Timeout auf. Dies kann folglich die Verbindungsskalierbarkeit beeinträchtigen.  
  
## <a name="contained-database-user-model"></a>Eigenständiges Datenbankbenutzermodell

 Die Anmeldung in der Masterdatenbank ist im eigenständigen Datenbankbenutzermodell nicht vorhanden. Stattdessen findet der Authentifizierungsprozess in der Benutzerdatenbank statt, und in der Masterdatenbank ist keine zugeordnete Anmeldung für den Datenbankbenutzer in der Benutzerdatenbank vorhanden. Das eigenständige Datenbankbenutzermodell unterstützt sowohl die Windows-Authentifizierung als auch die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung und kann sowohl in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als auch in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]verwendet werden. Um als eigenständiger Datenbankbenutzer eine Verbindung herzustellen, muss die Verbindungszeichenfolge immer einen Parameter für die Benutzerdatenbank enthalten, damit das [!INCLUDE[ssDE](../../includes/ssde-md.md)] weiß, welche Datenbank für die Verwaltung des Authentifizierungsprozesses verantwortlich ist. Die Aktivität des eigenständigen Datenbankbenutzers ist auf den Authentifizierungsserver beschränkt. Beim Herstellen einer Verbindung als eigenständiger Datenbankbenutzer muss das Datenbankbenutzerkonto also unabhängig in jeder Datenbank erstellt werden, die der Benutzer benötigt. Um die Datenbanken zu ändern, müssen [!INCLUDE[ssSDS](../../includes/sssds-md.md)] -Benutzer eine neue Verbindung erstellen. Eigenständige Datenbankbenutzer in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Datenbanken ändern, wenn ein identischer Benutzer in einer anderen Datenbank vorhanden ist.  
  
**Azure:** [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] und [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] unterstützen Azure Active Directory-Identitäten als Benutzer einer eigenständigen Datenbank. [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] unterstützt Benutzer einer eigenständigen Datenbank mit [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] aber [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] dies nicht. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview). Wird Azure Active Directory-Authentifizierung verwendet, können Verbindungen von SSMS über die universelle Active Directory-Authentifizierung hergestellt werden.  Administratoren können die universelle Authentifizierung so konfigurieren, dass Multi-Factor Authentication verwendet wird, bei der die Identität mit einem Telefonanruf, einer SMS, einer Smartcard mit PIN oder einer Benachrichtigung über mobile App überprüft wird. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und Azure Synapse Analytics](/azure/azure-sql/database/authentication-mfa-ssms-overview).  
  
 Für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]sind, weil der Datenbankname in der Verbindungszeichenfolge immer erforderlich ist, beim Wechseln vom traditionellen Modell auf das eigenständige Datenbankbenutzermodell keine Änderungen an der Verbindungszeichenfolge erforderlich. Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verbindungen muss der Name der Datenbank zur Verbindungszeichenfolge hinzugefügt werden, falls nicht bereits geschehen.  
  
> [!IMPORTANT]  
> Wenn Sie das traditionelle Modell verwenden, können die Rollen und Berechtigungen auf Serverebene den Zugriff auf alle Datenbanken einschränken. Wenn Sie das eigenständige Datenbankmodell verwenden, können Datenbankbesitzer und Datenbankbenutzer mit der ALTER ANY USER-Berechtigung Zugriff auf die Datenbank erteilen. Die Zugriffssteuerung für hoch privilegierten Serveranmeldungen reduziert und erweitert die Zugriffssteuerung, um hoch privilegierten Datenbankbenutzer zu enthalten.  
  
## <a name="firewalls"></a>Firewalls  
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 Windows-Firewall-Regeln gelten für alle Verbindungen und haben die gleichen Auswirkungen auf Anmeldungen (Verbindungen nach dem traditionellen Modell) und eigenständige Datenbankbenutzer. Weitere Informationen zur Windows-Firewall finden Sie unter [Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
### <a name="sssds-firewalls"></a>[!INCLUDE[ssSDS](../../includes/sssds-md.md)] Firewalls

 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ermöglicht separate Firewallregeln für Verbindungen auf Serverebene (Anmeldenamen) und für Verbindungen auf Datenbankebene (eigenständige Datenbankbenutzer). Bei der Verbindung mit einer Benutzerdatenbank werden zuerst die Datenbank-Firewallregeln überprüft. Wenn es keine Regel gibt, die den Zugriff auf die Datenbank ermöglicht, werden die Firewallregeln auf Serverebene geprüft, sodass der Zugriff auf eine SQL-Datenbankserver-Masterdatenbank erforderlich ist. Firewallregeln auf Datenbankebene können zusammen mit eigenständigen Datenbankbenutzern die Notwendigkeit beseitigen, während der Verbindung auf die Masterdatenbank des Servers zuzugreifen, was eine verbesserte Verbindungsskalierbarkeit bietet.  
  
 Weitere Informationen zu [!INCLUDE[ssSDS](../../includes/sssds-md.md)] -Firewall-Regeln finden Sie unter den folgenden Themen:  
  
- [Firewall für die Azure SQL-Datenbank](/previous-versions/azure/ee621782(v=azure.100))  
- [Vorgehensweise: Konfigurieren von Firewalleinstellungen (Azure SQL-Datenbank)](/previous-versions/azure/jj553530(v=azure.100))  
- [sp_set_firewall_rule &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
- [sp_set_database_firewall_rule &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
  
## <a name="syntax-differences"></a>Syntaxunterschiede  
  
|Herkömmliches Modell|Eigenständiges Datenbankbenutzermodell|  
|-----------------------|-----------------------------------|  
|Bei Verbindung mit der Masterdatenbank:<br /><br /> `CREATE LOGIN login_name  WITH PASSWORD = 'strong_password';`<br /><br /> Und bei anschließender Verbindung mit einer Benutzerdatenbank:<br /><br /> `CREATE USER 'user_name' FOR LOGIN 'login_name';`|Bei Verbindung mit einer Benutzerdatenbank:<br /><br /> `CREATE USER user_name  WITH PASSWORD = 'strong_password';`|  
  
|Herkömmliches Modell|Eigenständiges Datenbankbenutzermodell|  
|-----------------------|-----------------------------------|  
|So ändern Sie das Kennwort im Kontext der Masterdatenbank:<br /><br /> `ALTER LOGIN login_name  WITH PASSWORD = 'strong_password';`|So ändern Sie das Kennwort im Kontext der Benutzerdatenbank:<br /><br /> `ALTER USER user_name  WITH PASSWORD = 'strong_password';`|  

### <a name="managed-instance"></a>SQL-Datenbank-Instanz

Azure SQL Managed Instance verhält sich im Kontext von eigenständigen Datenbanken wie lokale SQL Server-Instanzen. Stellen Sie sicher, dass Sie den Kontext Ihrer Datenbank von dem der Masterdatenbank in den der Benutzerdatenbank ändern, wenn Sie Ihren eigenständigen Benutzer erstellen. Außerdem sollten keine aktiven Verbindungen mit der Benutzerdatenbank bestehen, wenn Sie die Eigenständigkeit festlegen. 


Beispiel:

> [!WARNING]
> Bevor Sie das folgende Skript ausführen, sollten Sie dafür sorgen, dass keine anderen aktiven Verbindungen in der Datenbank Ihrer verwalteten Instanz vorhanden sind. Das Skript unterbricht möglicherweise andere Prozesse, die in der Datenbank ausgeführt werden.

```sql
Use MASTER;
GO 

ALTER DATABASE Test
SET RESTRICTED_USER
WITH ROLLBACK IMMEDIATE;

ALTER DATABASE Test
SET containment=partial;

ALTER DATABASE Test
SET MULTI_USER;

USE Test;  
GO 

CREATE USER Carlo  
WITH PASSWORD='Enterpwdhere*'  

SELECT containment_desc FROM sys.databases
WHERE name='Test'
```

  
## <a name="remarks"></a>Bemerkungen  
  
- In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]müssen eigenständige Datenbankbenutzer für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktiviert werden. Weitere Informationen finden Sie unter [Serverkonfigurationsoption für die Authentifizierung der eigenständigen Datenbank](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md).  
- Eigenständige Datenbankbenutzer und Anmeldungen mit nicht überlappenden Namen können in Ihren Anwendungen gemeinsam vorliegen.  
- Wenn es in der Masterdatenbank eine Anmeldung unter dem Namen **name1** gibt und Sie einen eigenständigen Datenbankbenutzer namens **name1** erstellen und ein Datenbankname in der Verbindungszeichenfolge angegeben wird, wird der Kontext des Datenbankbenutzers bei der Datenbankverbindung über den Anmeldekontext gewählt. D. h. eigenständige Datenbankbenutzer haben Vorrang vor Anmeldungen mit demselben Namen.  
- Der Name des eigenständigen Datenbankbenutzers darf in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] nicht mit dem Namen des Serveradmin-Kontos identisch sein.  
- Das [!INCLUDE[ssSDS](../../includes/sssds-md.md)] -Server-Administratorkonto darf nie ein eigenständiger Datenbankbenutzer sein. Der Serveradministrator verfügt über ausreichende Berechtigungen zum Erstellen und Verwalten von eigenständigen Datenbankbenutzern. Der Serveradministrator kann eigenständigen Datenbankbenutzern Berechtigungen für die Benutzerdatenbanken erteilen.  
- Da enthaltene Datenbankbenutzer Prinzipale auf Datenbankebene sind, müssen Sie eigenständige Datenbankbenutzer in jeder Datenbank erstellen, die Sie verwenden möchten. Die Identität ist auf die Datenbank beschränkt und ist in allen Aspekten von einem Benutzer mit demselben Namen und demselben Kennwort in einer anderen Datenbank auf demselben Server unabhängig.  
- Verwenden Sie Kennwörter derselben Stärke, wie Sie sie normalerweise für Anmeldenamen verwenden.  
  
## <a name="see-also"></a>Weitere Informationen

 [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)   
 [Bewährte Methoden für die Sicherheit eigenständiger Datenbanken](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview)  
