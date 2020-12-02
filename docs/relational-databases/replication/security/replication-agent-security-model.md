---
title: Sicherheitsmodell des Replikations-Agents | Microsoft-Dokumentation
description: 'Das Sicherheitsmodell des Replikations-Agents in SQL Server ermöglicht die präzise Steuerung der Konten, unter denen Replikations-Agents ausgeführt werden und Verbindungen herstellen:'
ms.custom: ''
ms.date: 04/26/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Snapshot Agent, security
- agents [SQL Server replication], security
- Distribution Agent, security
- logins [SQL Server replication], permissions for agents
- security [SQL Server replication], agent security model
- Log Reader Agent, security
- Queue Reader Agent, security
- Merge Agent, security
- replication [SQL Server], agents and profiles
ms.assetid: 6d09fc8d-843a-4a7a-9812-f093d99d8192
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 41d77ae1b9c0763fedf2e53610bb3a0be5cd185c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "87823935"
---
# <a name="replication-agent-security-model"></a>Sicherheitsmodell des Replikations-Agents
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Das Sicherheitsmodell des Replikations-Agents ermöglicht die präzise Steuerung der Konten, unter denen Replikations-Agents ausgeführt werden und Verbindungen herstellen: Für jeden Agent kann ein anderes Konto angegeben werden. Weitere Informationen zum Angeben von Konten finden Sie unter [Identität und Zugriffssteuerung (Replikation)](../../../relational-databases/replication/security/identity-and-access-control-replication.md).  

Das Sicherheitsmodell des Replikations-Agents unterscheidet sich ein wenig für Azure SQL Managed Instance, da es keine Windows-Konten gibt, unter denen die Agents ausgeführt werden können. Stattdessen muss alles über SQL Server-Authentifizierung erfolgen. 
  
> [!IMPORTANT]  
>  Wenn ein Mitglied der festen Serverrolle **sysadmin** die Replikation konfiguriert, kann es die Replikations-Agents so konfigurieren, dass sie die Identität des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agentkontos annehmen. Dies geschieht, indem für den Replikations-Agent kein Anmeldename oder Kennwort angegeben wird. Dieser Ansatz ist jedoch nicht empfehlenswert. Sie sollten besser als bewährte Methode in Bezug auf die Sicherheit für jeden Agent ein Konto mit den im Abschnitt zu den für Agents erforderlichen Berechtigungen beschriebenen minimalen Privilegien angeben.  
  
 Replikations-Agents werden wie alle ausführbaren Dateien im Kontext eines Windows-Kontos ausgeführt. Die Agents stellen mithilfe dieses Kontos Verbindungen über die integrierte Sicherheit von Windows her. Unter welchem Konto der Agent ausgeführt wird, ist davon abhängig, wie er gestartet wird:  
  
-   Starten des Agents über einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Agent-Auftrag (Standardeinstellung): Wenn ein [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Agent-Auftrag zum Starten eines Replikations-Agents verwendet wird, wird dieser Agent im Kontext eines Kontos ausgeführt, das Sie während der Replikationskonfiguration angeben. Weitere Informationen zum [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent und der Replikation finden Sie im Abschnitt "Agentsicherheit mit SQL Server-Agent" weiter unten in diesem Thema. Informationen zu den Berechtigungen, die für das Konto erforderlich sind, unter dem der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Agent ausgeführt wird, finden Sie unter [Konfigurieren des SQL Server Agents](../../../ssms/agent/configure-sql-server-agent.md).  
  
-   Starten des Agents über eine MS-DOS-Befehlszeile (direkt oder über ein Skript): Der Agent wird im Kontext des Kontos des Benutzers ausgeführt, der den Agent über die Befehlszeile ausführt.  
  
-   Starten des Agents über eine Anwendung, die Replikationsverwaltungsobjekte (Replication Management Objects, RMO) oder ein ActiveX-Steuerelement verwendet: Der Agent wird im Kontext der Anwendung ausgeführt, die RMOs oder ActiveX-Steuerelemente aufruft.  
  
    > [!NOTE]  
    >  ActiveX-Steuerelemente sind als veraltet markiert.  
  
 Es empfiehlt sich, Verbindungen im Kontext der integrierten Sicherheit von Windows herzustellen. Aus Gründen der Abwärtskompatibilität kann auch die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Sicherheit verwendet werden. Weitere Informationen zu bewährten Methoden finden Sie unter [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="permissions-that-are-required-by-agents"></a>Für Agents erforderliche Berechtigungen  
 Die Konten, unter denen Agents ausgeführt werden und über die sie Verbindungen herstellen, erfordern verschiedene Berechtigungen. Eine Beschreibung zu diesen Berechtigungen finden Sie in der folgenden Tabelle. Es empfiehlt sich, jeden Agent unter einem gesonderten Windows-Konto auszuführen. Außerdem sollten den einzelnen Konten lediglich die erforderlichen Berechtigungen erteilt werden. Weitere Informationen zur Veröffentlichungszugriffsliste (Publication Access List oder PAL), die für eine Reihe von Agents relevant ist, finden Sie unter [Sichern des Verlegers](../../../relational-databases/replication/security/secure-the-publisher.md).  
  
> [!NOTE]  
>  Durch die Benutzerkontensteuerung (User Account Control, UAC) in einigen Windows-Betriebssystemen kann der Administratorzugriff auf die Momentaufnahmefreigabe verhindert werden. Sie müssen daher den vom Momentaufnahme-Agent, Verteilungs-Agent und Merge-Agent verwendeten Windows-Konten explizit Berechtigungen für die Momentaufnahmefreigabe erteilen. Dies ist auch dann erforderlich, wenn die Windows-Konten Mitglieder der Administratorengruppe sind. Weitere Informationen finden Sie unter [Schützen des Momentaufnahmeordners](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
|Agent|Berechtigungen|  
|-----------|-----------------|  
|Momentaufnahme-Agent|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Verteiler herstellt. Für dieses Konto ist Folgendes erforderlich:<br /><br /> – Es muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.<br /><br /> – Es muss über Lese-, Schreib- und Änderungsberechtigungen für die Momentaufnahmefreigabe verfügen.<br /><br /> <br /><br /> Beachten Sie, dass das zum Herstellen der *Verbindung* mit dem Verleger verwendete Konto zumindest Mitglied der festen Datenbankrolle **db_owner** in der Veröffentlichungsdatenbank sein muss.|  
|Protokolllese-Agent|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Verteiler herstellt. Dieses Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.<br /><br /> Das zum Herstellen der Verbindung mit dem Verleger verwendete Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Veröffentlichungsdatenbank sein.<br /><br /> Bei Auswahl der **sync_type** -Optionen *replication support only*, *initialize with backup* oder *initialize from lsn* muss der Protokolllese-Agent ausgeführt werden, nachdem **sp_addsubscription** ausgeführt wurde, damit die festgelegten Skripts in die Verteilungsdatenbank geschrieben werden. Der Protokolllese-Agent muss unter einem Konto ausgeführt werden, das Mitglied der festen Serverrolle **sysadmin** ist. Wenn die **sync_type** -Option auf *Automatic* festgelegt wird, sind keine speziellen Aktionen für den Protokollleser-Agent erforderlich.|  
|Verteilungs-Agent für Pushabonnements|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Verteiler herstellt. Für dieses Konto ist Folgendes erforderlich:<br /><br /> – Es muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.<br /><br /> – Es muss Mitglied der PAL sein.<br /><br /> – Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.<br /><br /> – Es muss über die Leseberechtigung für das Installationsverzeichnis des OLE DB-Anbieters für den Abonnenten verfügen, wenn das Abonnement für einen Nicht-SQL Server-Abonnenten vorgesehen ist.<br /><br /> – Bei der Replikation von LOB-Daten muss der Verteilungs-Agent über Schreibberechtigungen auf dem Replikationsordner **C:\Programme\Microsoft SQL Server\XX\COMfolder** verfügen, wobei „XX“ für die Instanz-ID steht.<br /><br /> <br /><br /> Beachten Sie, dass das zum Herstellen der *Verbindung* mit dem Abonnenten verwendete Konto mindestens Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein oder über vergleichbare Berechtigungen verfügen muss, wenn das Abonnement nicht SQL Server betrifft.<br /><br /> Beachten Sie außerdem, dass Sie bei Verwendung von `-subscriptionstreams >= 2` für den Verteilungs-Agent den Abonnenten auch die **View Server State** -Berechtigung erteilen müssen, damit Deadlocks erkannt werden können.|  
|Verteilungs-Agent für Pullabonnements|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Abonnenten herstellt. Dieses Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein. Für das zum Herstellen der Verbindung mit dem Verteiler verwendete Konto ist Folgendes erforderlich:<br /><br /> – Es muss Mitglied der PAL sein.<br /><br /> – Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.<br /><br /> – Bei der Replikation von LOB-Daten muss der Verteilungs-Agent über Schreibberechtigungen auf dem Replikationsordner **C:\Programme\Microsoft SQL Server\XX\COMfolder** verfügen, wobei „XX“ für die Instanz-ID steht.<br /><br /> <br /><br /> Beachten Sie, dass Sie bei Verwendung von `-subscriptionstreams >= 2` für den Verteilungs-Agent den Abonnenten auch die **View Server State** -Berechtigung erteilen müssen, damit Deadlocks erkannt werden können.|  
|Merge-Agent für Pushabonnements|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Verleger und dem Verteiler herstellt. Für dieses Konto ist Folgendes erforderlich:<br /><br /> – Es muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.<br /><br /> – Es muss Mitglied der PAL sein.<br /><br /> – Die Anmeldung muss mit einem Benutzer mit Lese-/Schreibberechtigungen in der Veröffentlichungsdatenbank verknüpft sein.<br /><br /> – Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.<br /><br /> <br /><br /> Beachten Sie, dass das zum Herstellen der *Verbindung* mit dem Abonnenten verwendete Konto zumindest Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein muss.|  
|Merge-Agent für Pullabonnements|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Abonnenten herstellt. Dieses Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein. Für das zum Herstellen der Verbindung mit dem Verleger und Verteiler verwendete Konto ist Folgendes erforderlich:<br /><br /> – Es muss Mitglied der PAL sein.<br /><br /> – Die Anmeldung muss mit einem Benutzer mit Lese-/Schreibberechtigungen in der Veröffentlichungsdatenbank verknüpft sein.<br /><br /> – Die Anmeldung muss mit einem Benutzer in der Verteilungsdatenbank verknüpft sein. Der Benutzer kann der **Guest** -Benutzer sein.<br /><br /> – Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.|  
|Warteschlangenlese-Agent|Das Windows-Konto, unter dem der Agent ausgeführt wird, wird verwendet, wenn er Verbindungen mit dem Verteiler herstellt. Dieses Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.<br /><br /> Das zum Herstellen der Verbindung mit dem Verleger verwendete Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Veröffentlichungsdatenbank sein.<br /><br /> Das zum Herstellen der Verbindung mit dem Abonnenten verwendete Konto muss zumindest Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein.|  
  
## <a name="agent-security-under-sql-server-agent"></a>Agentsicherheit mit SQL Server-Agent  
 Wenn Sie die Replikation mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Prozeduren oder RMO konfigurieren, wird standardmäßig ein [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agentauftrag für jeden Agent erstellt. Die Agents werden dann im Kontext eines Auftragsschritts ausgeführt, unabhängig davon, ob sie kontinuierlich, nach einem Zeitplan oder bei Bedarf ausgeführt werden. Diese Aufträge können im Ordner **Aufträge** in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]angezeigt werden. Die Auftragsnamen sind in der folgenden Tabelle aufgeführt.  
  
|Agent|Auftragsname|  
|-----------|--------------|  
|Momentaufnahme-Agent|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<integer>**|  
|Momentaufnahme-Agent für eine Mergeveröffentlichungspartition|**Dyn_\<Publisher>-\<PublicationDatabase>-\<Publication>-\<GUID>**|  
|Protokolllese-Agent|**\<Publisher>-\<PublicationDatabase>-\<integer>**|  
|Merge-Agent für Pullabonnements|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<integer>**|  
|Merge-Agent für Pushabonnements|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|Verteilungs-Agent für Pushabonnements|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|Verteilungs-Agent für Pullabonnements|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<GUID>**|  
|Verteilungs-Agent für Pushabonnements für Nicht-SQL Server-Abonnenten|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|Warteschlangenlese-Agent|**[\<Distributor>].\<integer>**|  
  
 \*Bei Pushabonnements für Oracle-Veröffentlichungen lautet der Auftragsname **\<Publisher>-\<Publisher**> anstelle von **\<Publisher>-\<PublicationDatabase>** .  
  
 \*\*Bei Pullabonnements für Oracle-Veröffentlichungen lautet der Auftragsname **\<Publisher>-\<DistributionDatabase**> anstelle von **\<Publisher>-\<PublicationDatabase>** .  
  
 Wenn Sie die Replikation konfigurieren, geben Sie Konten an, unter denen die Agents ausgeführt werden sollen. Sämtliche Auftragsschritte werden jedoch im Sicherheitskontext eines *Proxys* ausgeführt, somit führt die Replikation folgende Zuordnungen für die von Ihnen angegebenen Agentkonten intern aus:  
  
-   Das Konto wird zunächst mithilfe der [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung [CREATE CREDENTIAL](../../../t-sql/statements/create-credential-transact-sql.md) Anmeldeinformationen zugeordnet. Von[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent-Proxys werden Anmeldeinformationen zum Speichern von Informationen zu Windows-Benutzerkonten verwendet.  
  
-   Die gespeicherte Prozedur [sp_add_proxy](../../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md) wird aufgerufen, und die Anmeldeinformationen werden zum Erstellen eines Proxys verwendet.  
  
> [!NOTE]  
>  Diese Informationen werden bereitgestellt, damit Sie besser verstehen, was für die Ausführung von Agents im angemessenen Sicherheitskontext erforderlich ist. Sie müssen in der Regel nicht direkt mit den Anmeldeinformationen oder Proxys interagieren, die erstellt wurden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Sichern des Momentaufnahmeordners](../../../relational-databases/replication/security/secure-the-snapshot-folder.md)  
  
  
