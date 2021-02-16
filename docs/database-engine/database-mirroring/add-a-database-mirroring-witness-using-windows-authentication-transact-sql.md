---
title: Zeugenkonfiguration
description: 'In diesem Artikel wird beschrieben, wie ein Zeuge für die Datenbankspiegelung mit der Windows-Authentifizierung mithilfe von Transact-SQL konfiguriert wird. '
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- Windows authentication [SQL Server]
- database mirroring [SQL Server], witness
ms.assetid: bf5e87df-91a4-49f9-ae88-2a6dcf644510
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5702769994efd65abeeb330205a2a83362fee6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353253"
---
# <a name="add-a-database-mirroring-witness-using-windows-authentication-transact-sql"></a>Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Um einen Zeugen für eine Datenbank einzurichten, weist der Datenbankbesitzer einer Instanz der Datenbank-Engine die Rolle des Zeugenservers zu. Die Zeugenserverinstanz kann auf demselben Computer wie die Prinzipal- oder Spiegelserverinstanz ausgeführt werden. Hierdurch wird jedoch die Zuverlässigkeit des automatischen Failovers erheblich reduziert.  
  
 Wir raten dringend, den Zeugen auf einem separaten Computer zu platzieren. Ein Server kann an mehreren gleichzeitigen Datenbank-Spiegelungssitzungen mit den gleichen oder anderen Partnern teilnehmen. Ein Server kann bei manchen Sitzungen als Partnerserver und bei anderen Sitzungen als Zeugenserver dienen.  
  
 Der Zeugenserver ist ausschließlich für den Modus für hohe Sicherheit mit automatischem Failover gedacht. Bevor Sie einen Zeugen festlegen, sollten Sie unbedingt sicherstellen, dass die SAFETY-Eigenschaft auf FULL festgelegt ist.  
  
> [!IMPORTANT]  
>  Es empfiehlt sich, die Konfiguration der Datenbankspiegelung außerhalb der Spitzenbetriebszeiten durchzuführen, da sich die Konfiguration auf die Leistung auswirken kann.  
  
## <a name="establish-a-witness"></a>Einrichten eines Zeugen  
  
1.  Stellen Sie für die Zeugenserverinstanz sicher, dass ein Endpunkt für die Datenbankspiegelung vorhanden ist. Unabhängig von der Anzahl von Spiegelungssitzungen, die unterstützt werden sollen, darf die Serverinstanz nur einen Endpunkt der Datenbankspiegelung aufweisen. Wenn Sie diese Serverinstanz ausschließlich als Zeugen bei Datenbank-Spiegelungssitzungen verwenden möchten, weisen Sie dem Endpunkt die Rolle des Zeugen zu (ROLE **=** WITNESS). Wenn Sie diese Serverinstanz als Partner bei mindestens einer Datenbank-Spiegelungssitzung verwenden möchten, weisen Sie die Rolle des Endpunkts als ALL zu.  
  
     Zum Ausführen einer SET WITNESS-Anweisung muss die Datenbank-Spiegelungssitzung bereits gestartet sein (zwischen den Partnern), und als STATE muss für den Endpunkt des Zeugen STARTED festgelegt sein.  
  
     Zur Feststellung, ob die Zeugenserverinstanz einen Endpunkt der Datenbankspiegelung aufweist, und zur Feststellung von deren Rolle und Status verwenden Sie in dieser Instanz die folgende Transact-SQL-Anweisung:  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Wenn ein Endpunkt für die Datenbankspiegelung vorhanden ist und bereits verwendet wird, empfiehlt es sich, diesen Endpunkt auf der Serverinstanz für jede Sitzung zu verwenden. Durch Löschen eines verwendeten Endpunkts werden die Verbindungen der vorhandenen Sitzungen getrennt. Wenn für eine Sitzung ein Zeuge festgelegt wurde, kann das Löschen des Endpunkts für die Datenbankspiegelung dazu führen, dass der Prinzipalserver der Sitzung das Quorum verliert. In diesem Fall wird die Datenbank offline geschaltet und die Verbindung mit den Benutzern getrennt. Weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Wenn für einen Zeugen kein Endpunkt vorhanden ist, informieren Sie sich unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
2.  Wenn die Partnerinstanzen unter unterschiedlichen Domänenbenutzerkonten ausgeführt werden, müssen Sie in der Master-Datenbank jeder Instanz eine Anmeldung für die einzelnen Konten erstellen. Weitere Informationen finden Sie unter [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
3.  Stellen Sie anhand der folgenden Anweisung eine Verbindung mit dem Prinzipalserver her:  
  
     ALTER DATABASE *<Datenbankname>* SET WITNESS **=** _<Servernetzwerkadresse>_  
  
     Dabei ist *<Datenbankname>* der Name der zu spiegelnden Datenbank (dieser Name ist auf beiden Partnern gleich), und *<Servernetzwerkadresse>* ist die Servernetzwerkadresse der Zeugenserverinstanz.  
  
     Die Syntax für eine Server-Netzwerkadresse lautet folgendermaßen:  
  
     TCP <b>://</b> _\<system-address>_ <b>:</b> _\<port>_  
  
     Dabei ist \<*system-address>* eine Zeichenfolge, die das Zielcomputersystem eindeutig identifiziert, und \<*port>* ist die vom Spiegelungsendpunkt der Partnerserverinstanz verwendete Portnummer. Weitere Informationen finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Beispiel: Auf der Prinzipalserverinstanz wird mit folgender ALTER DATABASE-Anweisung der Zeuge festgelegt. Der Datenbankname ist **AdventureWorks**, die Systemadresse lautet DBSERVER3, der Name des Zeugensystems, und der vom Datenbankspiegelungsendpunkt des Zeugen verwendete Port ist `7022`:  
  
    ```  
    ALTER DATABASE AdventureWorks   
      SET WITNESS = 'TCP://DBSERVER3:7022'  
    ```  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel erstellt einen Datenspiegelungszeugen. Auf der Zeugenserverinstanz (Standardinstanz auf `WITNESSHOST4`):  
  
1.  Erstellen Sie einen Endpunkt für diese Serverinstanz für die WITNESS-Rolle, die ausschließlich Port `7022`verwendet.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    ```  
  
2.  Erstellen Sie eine Anmeldung für das Domänenbenutzerkonto der Partnerinstanzen, sofern dieses unterschiedlich ist. Wenn z. B. der Zeuge als `SOMEDOMAIN\witnessuser`ausgeführt wird, die Partner aber als `MYDOMAIN\dbousername`. Erstellen Sie die Anmeldung für Partner wie folgt:  
  
    ```  
    --Create a login for the partner server instances,  
    --which are both running as MYDOMAIN\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [MYDOMAIN\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [MYDOMAIN\dbousername];  
    GO  
    ```  
  
3.  Erstellen Sie auf jeder Serverinstanz eine Anmeldung für die Zeugenserverinstanz:  
  
    ```  
    --Create a login for the witness server instance,  
    --which is running as SOMEDOMAIN\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [SOMEDOMAIN\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [SOMEDOMAIN\witnessuser];  
    GO  
    ```  
  
4.  Legen Sie auf dem Prinzipalserver den Zeugen fest (der sich auf `WITNESSHOST4`befindet):  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4:7022'  
    GO  
    ```  
  
> [!NOTE]  
>  Die Servernetzwerkadresse gibt die Zielserverinstanz über die Portnummer an, die auf den Spiegelungsendpunkt der Instanz verweist.  
  
 Ein vollständiges Beispiel für das Einrichten der Sicherheitskomponenten, das Vorbereiten der Spiegeldatenbank, das Einrichten der Partner und das Hinzufügen eines Zeugen finden Sie unter [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)   
 [Entfernen des Zeugen aus einer Datenbank-Spiegelungssitzung (SQL Server)](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)   
 [Datenbank-Spiegelungszeuge](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
