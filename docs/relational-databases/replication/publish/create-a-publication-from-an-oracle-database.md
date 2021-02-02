---
description: Erstellen einer Veröffentlichung aus einer Oracle-Datenbank
title: Erstellen einer Veröffentlichung aus einer Oracle-Datenbank | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], Oracle databases
- Oracle publishing [SQL Server replication], configuring
ms.assetid: b3812746-14b0-4b22-809e-b4a95e1c8083
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cc7660d1990f556ae3ccfb8d34d66519dd2ecd76
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076778"
---
# <a name="create-a-publication-from-an-oracle-database"></a>Erstellen einer Veröffentlichung aus einer Oracle-Datenbank
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]eine Veröffentlichung aus einer Oracle-Datenbank erstellt wird.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Voraussetzungen](#Prerequisites)  
  
-   **So erstellen Sie eine Veröffentlichung aus einer Oracle-Datenbank mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Voraussetzung für das Erstellen einer Veröffentlichung aus einer Oracle-Datenbank ist, dass die Oracle-Software auf dem [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verteiler installiert ist und die Oracle-Datenbank konfiguriert wurde. Weitere Informationen finden Sie unter [Konfigurieren eines Oracle-Verlegers](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Sie können mithilfe des Assistenten für neue Veröffentlichung eine Momentaufnahme- bzw. eine Transaktionsveröffentlichung aus einer Oracle-Datenbank erstellen.  
  
 Wenn Sie zum ersten Mal eine Veröffentlichung aus einer Oracle-Datenbank erstellen, müssen Sie den Oracle-Verleger auf dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Verteiler identifizieren (für nachfolgende Veröffentlichungen aus derselben Datenbank ist dies nicht mehr erforderlich). Der Oracle-Verleger kann über den Assistenten für neue Veröffentlichung oder das Dialogfeld **Verteilereigenschaften – \<Distributor>** identifiziert werden. Im Folgenden wird das Identifizieren anhand der Vorgehensweise im Dialogfeld **Verteilereigenschaften – \<Distributor>** beschrieben.  
  
#### <a name="to-identify-the-oracle-publisher-at-the-sql-server-distributor"></a>So identifizieren Sie den Oracle-Verleger auf dem SQL Server-Verteiler  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]eine Verbindung mit der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz her, die der Oracle-Verleger als Verteiler verwenden wird. Erweitern Sie dann den Serverknoten.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Replikation** , und klicken Sie dann auf **Verteilereigenschaften**.  
  
3.  Klicken Sie auf der Seite **Verleger** des Dialogfelds **Verteilereigenschaften – \<Distributor>** auf **Hinzufügen** und anschließend auf **Oracle-Verleger hinzufügen**.  
  
4.  Klicken Sie im Dialogfeld **Verbindung mit Server herstellen** auf die Schaltfläche **Optionen** .  
  
5.  Gehen Sie auf der Registerkarte **Anmeldung** wie folgt vor:  
  
    1.  Geben Sie den Namen der Oracle-Datenbankinstanz ein, oder wählen Sie im Kombinationsfeld **Serverinstanz** den Eintrag **Suche fortsetzen** aus.  
  
    2.  Wählen Sie **Oracle-Standardauthentifizierung** (empfohlen) oder **Windows-Authentifizierung** aus.  
  
         Wenn Sie die **Windows-Authentifizierung** wählen: Der Oracle-Server muss so konfiguriert werden, dass Verbindungen mithilfe von Windows-Anmeldeinformationen möglich sind (weitere Informationen dazu finden Sie in der Oracle-Dokumentation). Darüber hinaus müssen Sie aktuell mit demselben [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows-Konto angemeldet sein, das Sie für das Schema des administrativen Replikationsbenutzers angegeben haben.  
  
    3.  Wenn Sie sich für **Oracle-Standardauthentifizierung** entschieden haben, geben Sie den Anmeldenamen und das Kennwort für das Schema für den administrativen Replikationsbenutzer ein, das Sie beim Konfigurieren auf dem Oracle-Verleger erstellt haben.  
  
6.  Wählen Sie auf der Registerkarte **Verbindungseigenschaften** als Verlegertyp **Gateway** oder **Vollständig** aus.  
  
     Mithilfe der Option **Vollständig** kann für Momentaufnahme- und Transaktionsveröffentlichungen der vollständige Satz an unterstützten Funktionen für Oracle-Veröffentlichungen verfügbar gemacht werden. Die Option **Gateway** bietet spezielle Designoptimierungen, um die Leistung in Fällen zu verbessern, in denen die Replikation als Gateway zwischen Systemen verwendet wird. Sie können die Option **Gateway** nicht verwenden, wenn Sie planen, dieselbe Tabelle in mehreren Transaktionsveröffentlichungen zu veröffentlichen. Wenn Sie **Gateway** auswählen, kann eine Tabelle höchstens in einer Transaktionsveröffentlichung und in einer beliebigen Zahl an Momentaufnahmeveröffentlichungen erscheinen.  
  
7.  Klicken Sie auf **Verbinden**. Es wird eine Verbindung mit dem Oracle-Verleger hergestellt, und die Verbindung wird für die Replikation konfiguriert. Das Dialogfeld **Verbindung mit Server herstellen** wird geschlossen, und es wird wieder das Dialogfeld **Verteilereigenschaften – \<Distributor>** angezeigt.  
  
    > [!NOTE]  
    >  Falls bei der Netzwerkkonfiguration Probleme auftreten, wird jetzt eine Fehlermeldung angezeigt. Wenn Sie Probleme haben, eine Verbindung mit der Oracle-Datenbank herzustellen, finden Sie entsprechende Informationen in dem Abschnitt in [Troubleshooting Oracle Publishers](../../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md), der sich mit Verbindungsproblemen zwischen dem SQL Server-Verteiler und der Oracle-Datenbankinstanz beschäftigt.  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-create-a-publication-from-an-oracle-database"></a>So erstellen Sie eine Veröffentlichung aus einer Oracle-Datenbank  
  
1.  Stellen Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz her, die der Oracle-Verleger als Verteiler verwenden wird. Erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** .  
  
3.  Klicken Sie mit der rechten Maustaste auf den Ordner **Lokale Veröffentlichungen** , und klicken Sie dann auf **Neue Oracle-Veröffentlichung**.  
  
4.  Wählen Sie auf der Seite **Oracle-Verleger** des Assistenten für neue Veröffentlichung den Oracle-Verleger aus. Wenn der Oracle-Verleger nicht angezeigt wird, klicken Sie auf **Oracle-Verleger hinzufügen**. Führen Sie dann die in der vorherigen Prozedur beschriebenen Schritte aus.  
  
5.  Wählen Sie auf der Seite **Veröffentlichungstyp** entweder **Momentaufnahmeveröffentlichung** oder **Transaktionsveröffentlichung** aus.  
  
6.  Wählen Sie auf der Seite **Artikel** die Datenbankobjekte aus, die Sie veröffentlichen möchten.  
  
     Optional können Sie auch Tabellenspalten herausfiltern, indem Sie eine Tabelle erweitern und dann die Kontrollkästchen für die Spalten deaktivieren, die nicht einbezogen werden sollen. Klicken Sie auf **Artikeleigenschaften** , um die Artikeleigenschaften anzuzeigen und zu ändern und um bei Bedarf alternative Datentypzuordnungen anzugeben. Weitere Informationen zu Datentypzuordnungen finden Sie unter [Angeben von Datentypzuordnungen für einen Oracle-Verleger](../../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md).  
  
7.  Wenden Sie bei Bedarf auf der Seite **Tabellenzeilen filtern** Filter an, um nur bestimmte Daten aus einer oder mehreren Tabellen zu veröffentlichen.  
  
8.  Deaktivieren Sie auf der Seite **Momentaufnahme-Agent** nur dann die Option **Momentaufnahme sofort erstellen** , wenn Sie alle Objekte erstellt und alle erforderlichen Daten der Abonnementdatenbank hinzugefügt haben.  
  
9. Geben Sie auf der Seite **Agentsicherheit** die Anmeldeinformationen für den Momentaufnahme-Agent (für alle Veröffentlichungen) und den Protokolllese-Agent (für Transaktionsveröffentlichungen) an. Die Agents werden ausgeführt und stellen mithilfe des von Ihnen angegebenen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Windows-Kontos Verbindungen zum [!INCLUDE[msCoName](../../../includes/msconame-md.md)] -Verteiler her. Für die Verbindungen zur Oracle-Datenbank verwenden die Agents den Kontext des Kontos, das Sie als Schema für den administrativen Replikationsbenutzer angegeben haben. Weitere Informationen finden Sie unter [Konfigurieren eines Oracle-Verlegers](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
     Weitere Informationen zu den erforderlichen Berechtigungen für die einzelnen Agents finden Sie unter [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) und [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
10. Erstellen Sie optional auf der Seite **Aktionen des Assistenten** ein Skript für die Veröffentlichung. Weitere Informationen finden Sie unter [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
11. Geben Sie auf der Seite **Assistenten abschließen** einen Namen für die Veröffentlichung an.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Nachdem eine Oracle-Datenbank als Verleger konfiguriert wurde, können Sie mit gespeicherten Systemprozeduren auf die gleiche Weise wie bei Verwendung eines [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verlegers eine Transaktions- oder Momentaufnahmenveröffentlichung erstellen.  
  
#### <a name="to-create-an-oracle-publication"></a>So erstellen Sie eine Oracle-Veröffentlichung  
  
1.  Konfigurieren Sie die Oracle-Datenbank als Verleger. Weitere Informationen finden Sie unter [Konfigurieren eines Oracle-Verlegers](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
2.  Wenn kein Remoteverteiler vorhanden ist, konfigurieren Sie den Remoteverteiler. Weitere Informationen finden Sie unter [Configure Publishing and Distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
3.  Führen Sie auf dem Remoteverteiler, der vom Oracle-Verleger verwendet wird, [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) aus. Geben Sie den TNS-Namen (Transparent Network Substrate) der Oracle-Datenbankinstanz für **\@publisher** und den Wert **ORACLE** oder **ORACLE GATEWAY** für **\@publisher_type** an. `Specify` : Geben Sie den beim Herstellen der Verbindung vom Oracle-Verleger zum Remote- [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Verteiler verwendeten Sicherheitsmodus auf eine der folgenden Arten an:  
  
    -   Geben Sie für die Standardeinstellung „Oracle-Standardauthentifizierung“ für **\@security_mode** den Wert **0**, für **\@login** den Anmeldenamen für das Schema des administrativen Replikationsbenutzers, das Sie während der Konfiguration auf dem Oracle-Verleger erstellt haben, und für **\@password** das Kennwort an.  
  
        > [!IMPORTANT]  
        >  Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Anmeldeinformationen zur Laufzeit anzugeben. Wenn Sie Anmeldeinformationen in einer Skriptdatei speichern, müssen Sie die Datei schützen, um unberechtigtem Zugriff vorzubeugen.  
  
    -   Geben Sie für **\@ security_mode** den Wert **1** an, um die Windows-Authentifizierung zu verwenden.  
  
        > [!NOTE]  
        >  Für die Verwendung der Windows-Authentifizierung muss der Oracle-Server so konfiguriert werden, dass Verbindungen mithilfe von Windows-Anmeldeinformationen möglich sind (weitere Informationen dazu finden Sie in der Oracle-Dokumentation). Darüber hinaus müssen Sie aktuell mit demselben Microsoft Windows-Konto angemeldet sein, das Sie für das Schema für den administrativen Replikationsbenutzer angegeben haben.  
  
4.  Erstellen Sie einen Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank.  
  
    -   Wenn Sie sich nicht sicher sind, ob ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank vorhanden ist, dann führen Sie [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) auf dem vom Oracle-Verleger verwendeten Verteiler für die Verteilerdatenbank aus. Geben Sie für **\@publisher** den Namen des Oracle-Verlegers an. Wenn das Resultset leer ist, muss ein Protokolllese-Agentauftrag erstellt werden.  
  
    -   Wenn ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank bereits vorhanden ist, fahren Sie mit Schritt 5 fort.  
  
    -   Führen Sie dem vom Oracle-Verleger verwendeten Verteiler für die Verteilungsdatenbank [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) aus. Geben Sie für **\@job_login** und **\@job_password** die Windows-Anmeldeinformationen an, unter denen der Agent ausgeführt wird.  
  
        > [!NOTE]  
        >  Der Parameter **\@job_login** muss mit den in Schritt 3 angegebenen Anmeldeinformationen übereinstimmen. Geben Sie keine Sicherheitsinformationen zum Verleger an. Der Protokolllese-Agent stellt mit den in Schritt 3 bereitgestellten Sicherheitsinformationen eine Verbindung mit dem Verleger her.  
  
5.  Führen Sie auf dem Verteiler für die Verteilerdatenbank [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) aus, um die Veröffentlichung zu erstellen. Weitere Informationen finden Sie unter [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
6.  Führen Sie auf dem Verteiler für die Verteilerdatenbank [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)aus. Geben Sie für **\@publication** den in Schritt 4 verwendeten Veröffentlichungsnamen und für **\@job_name** und **\@password** die Windows-Anmeldeinformationen an, unter denen der Momentaufnahme-Agent ausgeführt wird. Wenn zum Herstellen der Verbindung mit dem Verleger die Oracle-Standardauthentifizierung verwendet werden soll, müssen Sie außerdem für **\@publisher_security_mode** den Wert **0** und für **\@publisher_login** und **\@publisher_password** die Oracle-Anmeldeinformationen angeben. Dadurch wird ein Auftrag des Momentaufnahme-Agents für die Veröffentlichung erstellt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren eines Oracle-Verlegers](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Veröffentlichen von Daten und Datenbankobjekten](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Konfigurieren des Transaktionssatz-Auftrags für einen Oracle-Verleger &#40;Replikationsprogrammierung mit Transact-SQL&#41;](../../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Oracle Publishing Overview](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)   
 [Skript zum Erteilen von Oracle-Berechtigungen](../../../relational-databases/replication/non-sql/script-to-grant-oracle-permissions.md)  
  
  
