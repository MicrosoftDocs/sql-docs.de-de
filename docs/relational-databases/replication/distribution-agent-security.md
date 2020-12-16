---
description: Sicherheit für den Verteilungs-Agent
title: Sicherheit für den Verteilungs-Agent | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.security.DA.f1
helpviewer_keywords:
- Distribution Agent Security dialog box
ms.assetid: de40cc21-2e58-4464-9be7-b5b90c925e9b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5cfa887d79c8ecc2e4c0fa5b0fbea498bae176c3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484892"
---
# <a name="distribution-agent-security"></a>Sicherheit für den Verteilungs-Agent
::: moniker range=">=sql-server-2016"
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Im Dialogfeld **Sicherheit für den Verteilungs-Agent** können Sie das Windows-Konto angeben, unter dem der Verteilungs-Agent ausgeführt wird. Der Verteilungs-Agent wird für Pushabonnements auf dem Verteiler und für Pullabonnements auf dem Abonnenten ausgeführt. Das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto wird auch als *Prozesskonto* bezeichnet, da der Agentprozess unter diesem Konto ausgeführt wird. Abhängig davon, wie Sie auf dieses Dialogfeld zugreifen, stehen zusätzliche Optionen zur Verfügung:  
  
-   Wenn Sie das Dialogfeld über den Assistenten für neue Abonnements aufrufen, können Sie auch den Kontext angeben, unter dem der Verteilungs-Agent Verbindungen mit dem Abonnenten (bei Pushabonnements) bzw. dem Verteilungs-Agent (bei Pullabonnements) herstellt. Diese Verbindung kann hergestellt werden, indem die Identität des Windows-Kontos angenommen wird oder indem der Kontext eines [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Kontos verwendet wird, das Sie festlegen.  
  
-   Im Falle eines Zugriffs über das Dialogfeld **Abonnementeigenschaften** geben Sie den Kontext ein, unter dem der Verteilungs-Agent Verbindungen herstellen soll, indem Sie in der Zeile **Abonnentenverbindung** bzw. **Verteilerverbindung** dieses Dialogfelds auf die Schaltfläche mit den **drei Punkten** klicken. Weitere Informationen zum Zugriff auf das Dialogfeld **Abonnementeigenschaften** finden Sie unter [Anzeigen und Ändern der Eigenschaften von Pushabonnements](../../relational-databases/replication/view-and-modify-push-subscription-properties.md) und Vorgehensweise: [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 Alle Konten müssen gültig sein, und für jedes Konto muss das richtige Kennwort angegeben sein. Konten und Kennwörter werden erst bei der Ausführung eines Agents überprüft.  
  
## <a name="options"></a>Tastatur  
 **Process Account**  
 Geben Sie ein Windows-Konto ein, unter dem der Verteilungs-Agent ausgeführt wird:  
  
-   Bei Pushabonnements muss das Konto folgende Voraussetzungen erfüllen:  
  
    -   Es muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.  
  
    -   Es muss Mitglied der Veröffentlichungszugriffsliste (PAL) sein.  
  
    -   Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.  
  
    -   Wenn das Abonnement für einen Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten vorgesehen ist, muss das Konto über Lesezugriff auf das Installationsverzeichnis des OLE DB-Anbieters für den Abonnenten verfügen.  
  
-   Bei Pullabonnements muss das Konto mindestens Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein.  
  
 Zusätzliche Berechtigungen sind erforderlich, wenn beim Herstellen von Verbindungen die Identität des Prozesskontos angenommen wird. Siehe nachstehende Abschnitte **Verbindung mit dem Verteiler herstellen** und **Verbindung mit dem Abonnenten herstellen** .  
  
 Das **Prozesskonto** kann nicht für Pullabonnements für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] festgelegt werden, da der Verteilungs-Agent nicht auf [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]-Instanzen ausgeführt werden kann.  
  
 **Kennwort** und **Kennwort bestätigen**  
 In diese Felder geben Sie das Kennwort für das Windows-Konto ein.  
  
 **Verbindung mit dem Verteiler herstellen**  
 Bei Pushabonnements wird eine Verbindung mit dem Verteiler immer hergestellt, indem der Agent die Identität des im Textfeld **Prozesskonto** angegebenen Kontos annimmt.  
  
 Bei Pullabonnements müssen Sie auswählen, ob der Verteilungs-Agent die Verbindungen zum Verteiler entweder durch Annahme der Identität des im Textfeld **Prozesskonto** angegebenen Kontos oder mithilfe eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Kontos herstellt. Wenn Sie sich für ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto entscheiden, müssen Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Benutzernamen und ein Kennwort eingeben.  
  
> [!NOTE]  
>  Es wird empfohlen, kein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto zu verwenden und stattdessen festzulegen, dass der Agent die Identität des Windows-Kontos annimmt.  
  
 Das für die Verbindung verwendete Windows-Konto bzw. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto muss die folgenden Voraussetzungen erfüllen:  
  
-   Es muss Mitglied der Veröffentlichungszugriffsliste sein.  
  
-   Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.  
  
 **Verbindung mit dem Abonnenten herstellen**  
 Bei Pullabonnements wird eine Verbindung mit dem Abonnenten immer hergestellt, indem die Identität des im Textfeld **Prozesskonto** angegebenen Kontos angenommen wird.  
  
 Bei Pushabonnements gibt es für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten und Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten unterschiedliche Optionen:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten müssen auswählen, ob der Verteilungs-Agent die Verbindungen mit dem Abonnenten entweder durch Annahme der Identität des im Textfeld **Prozesskonto** angegebenen Kontos oder mithilfe eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Kontos herstellen soll. Wenn Sie sich für ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto entscheiden, müssen Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Benutzernamen und ein Kennwort eingeben.  
  
    > [!NOTE]  
    >  Es wird empfohlen, kein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto zu verwenden und stattdessen festzulegen, dass der Agent die Identität des Windows-Kontos annimmt.  
  
     Das für die Verbindung mit dem Abonnenten verwendete Windows-Konto bzw. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konto muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein.  
  
-   Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten müssen den Datenbank-Anmeldenamen angeben, den der Verteilungs-Agent beim Aufbau der Verbindung mit dem Abonnenten verwenden soll. Für den Anmeldenamen müssen ausreichende Zugriffsrechte zum Erstellen von Objekten in der Abonnementdatenbank konfiguriert sein. Weitere Informationen zum Konfigurieren von Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten finden Sie unter [Erstellen eines Abonnements für einen Nicht-SQL Server-Abonnenten](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  
  
 **Zusätzliche Verbindungsoptionen**  
 Nur für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten. Geben Sie alle Verbindungsoptionen für den Abonnenten in Form einer Verbindungszeichenfolge an. (Für Oracle sind keine zusätzlichen Optionen erforderlich.) Mehrere Optionen müssen durch Semikolons voneinander getrennt werden. Das folgende Beispiel verdeutlicht eine IBM DB2-Verbindungszeichenfolge (die Zeilenumbrüche wurde im Sinne der besseren Lesbarkeit eingefügt):  
  
```  
Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
Persist Security Info=False;Connection Pooling=True;  
```  
  
 Die meisten Optionen in der Zeichenfolge hängen von dem DB2-Server ab, den Sie konfigurieren. Allerdings sollte die Option **Process Binary as Character** immer auf **False** festgelegt sein. Zur Identifizierung der Abonnementdatenbank ist ein Wert für die Option **Anfangskatalog** erforderlich. Weitere Informationen finden Sie unter [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Identität und Zugriffssteuerung (Replikation)](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Replikations-Agents (Übersicht)](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)  
::: moniker-end
  
::: moniker range="azuresqldb-mi-current"
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
Im Dialogfeld **Sicherheit für den Verteilungs-Agent** können Sie das SQL-Authentifizierungskonto angeben, unter dem der Verteilungs-Agent ausgeführt wird. Der Verteilungs-Agent wird für Pushabonnements auf dem Verteiler und für Pullabonnements auf dem Abonnenten ausgeführt.  Abhängig davon, wie Sie auf dieses Dialogfeld zugreifen, stehen zusätzliche Optionen zur Verfügung:  
  
-   Wenn Sie das Dialogfeld über den Assistenten für neue Abonnements aufrufen, können Sie auch den Kontext angeben, unter dem der Verteilungs-Agent Verbindungen mit dem Abonnenten (bei Pushabonnements) bzw. dem Verteilungs-Agent (bei Pullabonnements) herstellt. Die Verbindung sollte mithilfe eines SQL Server-Authentifizierungskontos hergestellt werden. 
  
-   Im Falle eines Zugriffs über das Dialogfeld **Abonnementeigenschaften** geben Sie den Kontext ein, unter dem der Verteilungs-Agent Verbindungen herstellen soll, indem Sie in der Zeile **Abonnentenverbindung** bzw. **Verteilerverbindung** dieses Dialogfelds auf die Schaltfläche mit den **drei Punkten** klicken. Weitere Informationen zum Zugriff auf das Dialogfeld **Abonnementeigenschaften** finden Sie unter [Anzeigen und Ändern der Eigenschaften von Pushabonnements](../../relational-databases/replication/view-and-modify-push-subscription-properties.md) und Vorgehensweise: [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 Alle Konten müssen gültig sein, und für jedes Konto muss das richtige Kennwort angegeben sein. Konten und Kennwörter werden erst bei der Ausführung eines Agents überprüft.  
  
## <a name="options"></a>Tastatur  
 **Process Account**  
 Geben Sie ein SQL Server-Authentifizierungskonto ein, unter dem der Verteilungs-Agent ausgeführt wird:  
  
-   Bei Pushabonnements muss das Konto folgende Voraussetzungen erfüllen:  
  
    -   Es muss mindestens Mitglied der festen Datenbankrolle **db_owner** in der Verteilungsdatenbank sein.  
  
    -   Es muss Mitglied der Veröffentlichungszugriffsliste (PAL) sein.  
  
    -   Es muss über Leseberechtigungen für die Momentaufnahmefreigabe verfügen.  
  
    -   Wenn das Abonnement für einen Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten vorgesehen ist, muss das Konto über Lesezugriff auf das Installationsverzeichnis des OLE DB-Anbieters für den Abonnenten verfügen.  
  
-   Bei Pullabonnements muss das Konto mindestens Mitglied der festen Datenbankrolle **db_owner** in der Abonnementdatenbank sein.  
  
    
**Kennwort** und **Kennwort bestätigen**  
In diese Felder geben Sie das Kennwort für das Windows-Konto ein.  
  
**Verbindung mit dem Verteiler herstellen**  
Bei Pushabonnements wird eine Verbindung mit dem Verteiler immer hergestellt, indem der Agent die Identität des im Textfeld **Prozesskonto** angegebenen Kontos annimmt.  
  
Bei Pullabonnements müssen Sie auswählen, ob der Verteilungs-Agent die Verbindungen zum Verteiler entweder durch Annahme der Identität des im Textfeld **Prozesskonto** angegebenen Kontos oder mithilfe eines SQL Server-Authentifizierungskontos herstellt. 
  
  
 **Verbindung mit dem Abonnenten herstellen**  
 Bei Pullabonnements wird eine Verbindung mit dem Abonnenten immer hergestellt, indem die Identität des im Textfeld **Prozesskonto** angegebenen Kontos angenommen wird.  
  
 Bei Pushabonnements gibt es für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten und Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten unterschiedliche Optionen:

  
-   Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten müssen den Datenbank-Anmeldenamen angeben, den der Verteilungs-Agent beim Aufbau der Verbindung mit dem Abonnenten verwenden soll. Für den Anmeldenamen müssen ausreichende Zugriffsrechte zum Erstellen von Objekten in der Abonnementdatenbank konfiguriert sein. Weitere Informationen zum Konfigurieren von Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten finden Sie unter [Erstellen eines Abonnements für einen Nicht-SQL Server-Abonnenten](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  
  
 **Zusätzliche Verbindungsoptionen**  
 Nur für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten. Geben Sie alle Verbindungsoptionen für den Abonnenten in Form einer Verbindungszeichenfolge an. (Für Oracle sind keine zusätzlichen Optionen erforderlich.) Mehrere Optionen müssen durch Semikolons voneinander getrennt werden. Das folgende Beispiel verdeutlicht eine IBM DB2-Verbindungszeichenfolge (die Zeilenumbrüche wurde im Sinne der besseren Lesbarkeit eingefügt):  
  
```  
Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
Persist Security Info=False;Connection Pooling=True;  
```  
  
 Die meisten Optionen in der Zeichenfolge hängen von dem DB2-Server ab, den Sie konfigurieren. Allerdings sollte die Option **Process Binary as Character** immer auf **False** festgelegt sein. Zur Identifizierung der Abonnementdatenbank ist ein Wert für die Option **Anfangskatalog** erforderlich. Weitere Informationen finden Sie unter [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transaktionsreplikation mit Azure SQL-Datenbank](/azure/sql-database/sql-database-managed-instance-transactional-replication) [Konfigurieren der Replikation für Azure SQL Managed Instance](/azure/sql-database/replication-with-sql-database-managed-instance)
::: moniker-end


