---
title: 'Transportsicherheit: Verfügbarkeitsgruppen und Datenbankspiegelung'
description: In diesem Artikel erfahren Sie, wie Sie Nachrichten während der Übertragung zwischen Datenbanken schützen, die Mitglieder einer AlwaysOn-Verfügbarkeitsgruppe oder einer Datenbankspiegelungssitzung sind, die in SQL Server gehostet wird.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- cryptography [SQL Server], database mirroring
- certificates [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- transport security
- database mirroring [SQL Server], security
ms.assetid: 49239d02-964e-47c0-9b7f-2b539151ee1b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 97a3171a4028a28c6da5fb6afae37144db3d2727
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352198"
---
# <a name="transport-security---database-mirroring---always-on-availability"></a>Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die Transportsicherheit schließt die Authentifizierung und optional die Verschlüsselung der Nachrichten ein, die zwischen den Datenbanken ausgetauscht werden. Für die Datenbankspiegelung und [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]werden Authentifizierung und Verschlüsselung am Datenbankspiegelungs-Endpunkt konfiguriert. Eine Einführung für die Datenbankspiegelungs-Endpunkte finden Sie unter [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 **In diesem Thema:**  
  
-   [Authentifizierung](#Authentication)  
  
-   [Datenverschlüsselung](#DataEncryption)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="authentication"></a><a name="Authentication"></a> Authentifizierung  
 Unter Authentifizierung versteht man den Prozess, mit dem überprüft wird, ob es sich bei einem Benutzer wirklich um die Person handelt, die der Benutzer angeblich ist. Verbindungen zwischen Datenbank-Spiegelungsendpunkten erfordern die Authentifizierung. Verbindungsanforderungen von einem Partner oder ggf. einem Zeugen müssen authentifiziert werden.  
  
 Der für Datenbankspiegelung oder [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] von einer Serverinstanz verwendete Authentifizierungstyp ist eine Eigenschaft des Endpunkts der Datenbankspiegelung. Für Datenbankspiegelungs-Endpunkte sind zwei Arten von Transportsicherheit verfügbar: Windows-Authentifizierung (die Security Support Provider Interface (SSPI)) oder zertifikatbasierte Authentifizierung.  
  
### <a name="windows-authentication"></a>Windows-Authentifizierung  
 Bei der Windows-Authentifizierung erfolgt die Anmeldung jeder Serverinstanz bei der anderen Seite mithilfe der Windows-Anmeldeinformationen des Windows-Benutzerkontos, unter dem der Prozess ausgeführt wird. Die Windows-Authentifizierung erfordert eventuell eine manuelle Konfiguration von Anmeldekonten, je nach den folgenden Bedingungen:  
  
-   Wenn die Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Dienste unter dem gleichen Domänenkonto ausgeführt werden, ist keine zusätzliche Konfiguration erforderlich.  
  
-   Wenn die Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Dienste unter unterschiedlichen Domänenkonten ausgeführt werden (in der gleichen oder einer vertrauenswürdigen Domäne), müssen die Anmeldeinformationen jedes Kontos in **master** auf jeder der anderen Serverinstanzen erstellt werden. Dieser Anmeldung müssen CONNECT-Berechtigungen für den Endpunkt gewährt werden.  
  
-   Wenn die Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Netzwerkdienstkonto ausgeführt werden, muss die Anmeldung zu jedem Hostcomputer-Konto (_DomainName_ **\\** _ComputerName$_ ) in **master** auf jedem der anderen Server erstellt werden. Dieser Anmeldung müssen CONNECT-Berechtigungen für den Endpunkt gewährt werden. Das liegt daran, dass eine Serverinstanz, die unter dem Netzwerkdienstkonto ausgeführt wird, mit dem Domänenkonto des Hostcomputers authentifiziert wird.  
  
> [!NOTE]  
>  Ein Beispiel für das Einrichten einer Datenbankspiegelungs-Sitzung mithilfe der Windows-Authentifizierung finden Sie unter [Beispiel: Einrichten der Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
  
### <a name="certificates"></a>Zertifikate  
 Unter bestimmten Umständen – z. B. wenn sich Serverinstanzen nicht in vertrauenswürdigen Domänen befinden oder wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als lokaler Dienst ausgeführt wird – steht die Windows-Authentifizierung nicht zur Verfügung. In solchen Fällen werden zum Authentifizieren der Verbindungsanforderungen anstelle der Benutzeranmeldeinformationen Zertifikate benötigt. Der Spiegelungsendpunkt jeder Serverinstanz muss mit ihrem eigenen lokal erstellten Zertifikat konfiguriert werden.  
  
 Die Verschlüsselungsmethode wird beim Erstellen des Zertifikats festgelegt. Weitere Informationen finden Sie unter [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Gehen Sie bei der Verwaltung der von Ihnen verwendeten Zertifikate mit großer Vorsicht vor.  
  
 Eine Serverinstanz verwendet den privaten Schlüssel ihres eigenen Zertifikats, um bei einem Verbindungsaufbau ihre Identität einzurichten. Die Serverinstanz, die die Verbindungsanforderung empfängt, verwendet den öffentlichen Schlüssel des Absenderzertifikats, um die Identität des Absenders zu authentifizieren. Angenommen es gibt z. B. zwei Serverinstanzen, Server_A und Server_B. Von Server_A wird ein privater Schlüssel zur Verschlüsselung des Verbindungsheaders verwendet, bevor eine Verbindungsanforderung an Server_B gesendet wird. Von Server_B wird der öffentliche Schlüssel des Zertifikats von Server_A verwendet, um den Verbindungsheader zu entschlüsseln. Wenn der entschlüsselte Header richtig ist, weiß Server_B, dass der Header von Server_A verschlüsselt wurde, und die Verbindung wird authentifiziert. Wenn der entschlüsselte Header nicht richtig ist, erkennt Server_B, dass die Verbindungsanforderung nicht authentisch ist, und verweigert den Verbindungsaufbau.  
  
##  <a name="data-encryption"></a><a name="DataEncryption"></a> Datenverschlüsselung  
 Standardmäßig setzt ein Datenbank-Spiegelungsendpunkt die Verschlüsselung der über Spiegelungsverbindungen gesendeten Daten voraus. In diesem Fall kann der Endpunkt nur Verbindungen mit Endpunkten herstellen, die ebenfalls mit Verschlüsselung arbeiten. Sofern Sie nicht garantieren können, dass Ihr Netzwerk sicher ist, wird empfohlen, das Verschlüsseln bei Verbindungen zur Datenbankspiegelung vorauszusetzen. Allerdings können Sie die Verschlüsselung auch deaktivieren oder festlegen, dass die Verschlüsselung zwar unterstützt wird, jedoch nicht erforderlich ist. Bei deaktivierter Verschlüsselung werden die Daten niemals verschlüsselt, und der Endpunkt kann keine Verbindung mit einem Endpunkt herstellen, der die Verschlüsselung erfordert. Wenn die Verschlüsselung unterstützt wird, werden die Daten nur dann verschlüsselt, wenn der gegenüberliegende Endpunkt die Verschlüsselung unterstützt oder erfordert.  
  
> [!NOTE]  
>  Alle durch [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] erstellten Spiegelungsendpunkte werden entweder mit erforderlicher Verschlüsselung oder mit deaktivierter Verschlüsselung erstellt. Zum Ändern der Verschlüsselungseinstellung zu SUPPORTED verwenden Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung ALTER ENDPOINT. Weitere Informationen finden Sie unter [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 Sie können auch die Verschlüsselungsalgorithmen steuern, die von einem Endpunkt verwendet werden können, indem Sie einen der folgenden Werte für die ALGORITHM-Option in einer CREATE ENDPOINT- oder ALTER ENDPOINT-Anweisung angeben:  
  
|ALGORITHM-Wert|BESCHREIBUNG|  
|---------------------|-----------------|  
|RC4|Gibt an, dass der Endpunkt den RC4-Algorithmus verwenden muss. Dies ist die Standardoption.<br /><br /> <strong>\*\* Warnung \*\*</strong> Der RC4-Algorithmus ist veraltet. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Stattdessen wird die Verwendung von AES empfohlen.|  
|AES|Gibt an, dass der Endpunkt den AES-Algorithmus verwenden muss.|  
|AES RC4|Gibt an, dass die beiden Endpunkte einen Verschlüsselungsalgorithmus aushandeln, wobei dieser Endpunkt dem AES-Algorithmus den Vorzug gibt.|  
|RC4 AES|Gibt an, dass die beiden Endpunkte einen Verschlüsselungsalgorithmus aushandeln, wobei dieser Endpunkt dem RC4-Algorithmus den Vorzug gibt.|  
  
 Wenn das Verbinden der Endpunkte beide Algorithmen angibt, jedoch in unterschiedlicher Reihenfolge, wird die Einstellung des Endpunkts verwendet, der die Verbindung annimmt.  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
>   
>  Obwohl deutlich schneller als AES, ist RC4 ein relativ schwacher Algorithmus, während AES ein relativ starker Algorithmus ist. Deshalb wird empfohlen, den AES-Algorithmus zu verwenden.  
  
 Informationen zur [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax zum Angeben der Verschlüsselung finden Sie unter [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So konfigurieren Sie die Transportsicherheit für einen Datenbankspiegelung-Endpunkt**  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [DROP ENDPOINT (Transact-SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)   
 [Problembehandlung für die Datenbankspiegelungskonfiguration (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
