---
title: Dialogfeld „Neue Verfügbarkeitsgruppe“ in SSMS
description: Erfahren Sie, wie Sie über das Dialogfeld „Neue Verfügbarkeitsgruppe“ in SQL Server Management Studio eine Always On-Verfügbarkeitsgruppe konfigurieren.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 1b0a6421-fbd4-4bb4-87ca-657f4782c433
author: cawrites
ms.author: chadam
ms.openlocfilehash: ed9c9ec058062dd378bf29f78e1c60c49a654669
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355125"
---
# <a name="use-the-new-availability-group-dialog-box-sql-server-management-studio"></a>Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Dieses Thema enthält Informationen zum Verwenden des Dialogfelds **Neue Verfügbarkeitsgruppe** von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , um auf Instanzen von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] , die für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen. Eine *Verfügbarkeitsgruppe* definiert einen Satz von Benutzerdatenbanken, für die als eine einzelne Einheit ein Failover ausgeführt wird, sowie einen Satz von Failoverpartnern, die als *Verfügbarkeitsreplikate* bezeichnet werden, die Failover unterstützen.  
  
> [!NOTE]  
>  Eine Einführung zu Verfügbarkeitsgruppen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)erstellt und konfiguriert wird.  
   
> [!NOTE]  
>  Informationen zu alternativen Möglichkeiten zum Erstellen einer Verfügbarkeitsgruppe finden Sie in [Verwandte Aufgaben](#RelatedTasks)später in diesem Thema.  
  
  
##  <a name="prerequisites"></a><a name="PrerequisitesRestrictions"></a> Voraussetzungen  
  
-   Überprüfen Sie vor dem Erstellen einer Verfügbarkeitsgruppe, ob sich die Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die Verfügbarkeitsreplikate hosten, auf verschiedenen WSFC-Konten (Windows Server Failover Clustering) des gleichen WSFC-Failoverclusters befinden. Stellen Sie außerdem sicher, dass alle Serverinstanzen für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktiviert sind und alle anderen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Voraussetzungen erfüllen. Für weitere Informationen empfehlen wir Ihnen dringend [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
-   Vor dem Erstellen einer Verfügbarkeitsgruppe muss gewährleistet sein, dass jede Serverinstanz, die ein Verfügbarkeitsreplikat hostet, einen voll funktionsfähigen Datenbankspiegelungs-Endpunkt besitzt. Weitere Informationen finden Sie unter [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
-   Zur Verwendung des Dialogfelds **Neue Verfügbarkeitsgruppe** müssen Sie die Namen der Serverinstanzen kennen, die Verfügbarkeitsreplikate hosten. Zudem müssen Sie die Namen sämtlicher Datenbanken kennen, die Sie der neuen Verfügbarkeitsgruppe hinzufügen möchten, und gewährleisten, dass diese Datenbanken die Voraussetzungen und Einschränkungen der Verfügbarkeitsdatenbank erfüllen, die unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen. Wenn Sie ungültige Werte eingeben, funktioniert die neue Verfügbarkeitsgruppe nicht.  
  
## <a name="limitations"></a><a name="Limitations"></a> Einschränkungen  
 Das Dialogfeld **Neue Verfügbarkeitsgruppe** führt folgende Aktionen nicht durch:  
  
-   Verfügbarkeitsgruppenlistener erstellen    
-   Verknüpfen sekundärer Replikate mit der Verfügbarkeitsgruppe    
-   Ausführen einer anfänglichen Datensynchronisierung  
  
 Informationen zu diesen Konfigurationsaufgaben finden Sie unter [Nachverfolgung: Nach dem Erstellen einer Verfügbarkeitsgruppe](#FollowUp) weiter unten in diesem Thema.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.  
  
##  <a name="using-the-new-availability-group-dialog-box-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe (SQL Server Management Studio)  
 **So erstellen Sie eine Verfügbarkeitsgruppe**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung zur Serverinstanz her, die das primäre Replikat hostet, und klicken Sie auf den Servernamen.  
  
2.  Erweitern Sie den Knoten **Always On High Availability** (Always On Hochverfügbarkeit).  
  
3.  Klicken Sie mit der rechten Maustaste auf den Knoten **Verfügbarkeitsgruppen** , und wählen Sie den Befehl **Neue Verfügbarkeitsgruppe** aus.  
  
4.  Dieser Befehl erschließt das Dialogfeld **Neue Verfügbarkeitsgruppe** .  
  
5.  Verwenden Sie auf der Seite **Allgemein** das Feld **Name der Verfügbarkeitsgruppe** zur Eingabe eines Namens für die neue Verfügbarkeitsgruppe. Dieser Name muss ein gültiger SQL Server-Bezeichner und in allen Verfügbarkeitsgruppen im WSFC-Cluster eindeutig sein. Die maximale Länge eines Verfügbarkeitsgruppennamens beträgt 128 Zeichen.  
  
6.  Klicken Sie im Raster **Verfügbarkeitsdatenbanken** auf **Hinzufügen** , und geben Sie den Namen einer lokalen Datenbank ein, die zu dieser Verfügbarkeitsgruppe gehören soll. Wiederholen Sie dies für jede hinzuzufügende Datenbank. Wenn Sie auf **OK** klicken, überprüft das Dialogfeld, ob die angegebene Datenbank die Voraussetzungen dafür erfüllt, dass sie einer Verfügbarkeitsgruppe angehört. Informationen zur diesen Voraussetzungen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
7.  Klicken Sie im Raster **Verfügbarkeitsdatenbanken** auf **Hinzufügen** , und geben Sie den Namen einer Serverinstanz ein, um ein sekundäres Replikat zu hosten. Das Dialogfeld versucht nicht, eine Verbindung zu diesen Instanzen herzustellen. Wenn Sie einen falschen Servernamen angeben, wird zwar ein sekundäres Replikat hinzugefügt, Sie können aber keine Verbindung mit diesem Replikat herstellen.  
  
    > [!TIP]  
    >  Wenn Sie ein Replikat hinzugefügt haben und keine Verbindung zur Hostserverinstanz herstellen können, können Sie das Replikat entfernen und ein neues hinzufügen. Weitere Informationen finden Sie unter [Entfernen eines sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) und [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
8.  Klicken Sie im Bereich **Seite auswählen** des Dialogfelds auf **Sicherungseinstellungen**. Geben Sie dann auf der Seite **Sicherungseinstellungen** auf Grundlage der Replikatrolle an, wo die Sicherungen erstellt werden sollen, weisen Sie den einzelnen Serverinstanzen, die ein Verfügbarkeitsreplikat für diese Verfügbarkeitsgruppe hosten, Sicherungsprioritäten zu. Weitere Informationen finden Sie unter [Eigenschaften von Verfügbarkeitsgruppen: Neue Verfügbarkeitsgruppe &#40;Seite „Sicherungseinstellungen“&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
9. Klicken Sie auf **OK**, um die Verfügbarkeitsgruppe zu erstellen. Dadurch überprüft das Dialogfeld, ob die angegebenen Datenbanken die erforderlichen Komponenten erfüllen.  
  
     Um das Dialogfeld zu beenden, ohne die Verfügbarkeitsgruppe zu erstellen, klicken Sie auf **Abbrechen**.  
  
##  <a name="follow-up-after-using-the-new-availability-group-dialog-box-to-create-an-availability-group"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Erstellen einer Verfügbarkeitsgruppe mithilfe des Dialogfelds „Neue Verfügbarkeitsgruppe“  
  
-   Sie wiederum müssen eine Verbindung zu jeder Serverinstanz herstellen, die ein sekundäres Replikat für die Verfügbarkeitsgruppe hostet, und die folgenden Schritte ausführen:  
  
    1.  Verknüpfen Sie das sekundäre Replikat mit der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)mit einer Always On-Verfügbarkeitsgruppe verknüpft wird.  
  
    2.  Stellen Sie aktuelle Sicherungen jeder primären Datenbank und ihres Transaktionsprotokolls (mithilfe von RESTORE WITH NORECOVERY) wieder her. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
  
    3.  Verknüpfen Sie jede neu vorbereitete sekundäre Datenbank sofort mit der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
-   Wir empfehlen das Erstellen eines Verfügbarkeitsgruppenlisteners für die neue Verfügbarkeitsgruppe. Dazu müssen Sie mit der Serverinstanz verbunden sein, auf der das aktuelle primäre Replikat gehostet wird. Weitere Informationen finden Sie unter [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So konfigurieren Sie Verfügbarkeitsgruppen- und Replikateigenschaften**  
  
-   [Ändern des Verfügbarkeitsmodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Failovermodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Konfigurieren der flexiblen Failoverrichtlinie zum Steuern der Bedingungen für ein automatisches Failover &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Konfigurieren der Sicherung auf Verfügbarkeitsreplikaten &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **So schließen Sie die Konfiguration von Verfügbarkeitsgruppen ab**  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Alternative Möglichkeiten zum Erstellen einer Verfügbarkeitsgruppe**  
  
-   [Verwenden des Assistenten für Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **So aktivieren Sie Always On-Verfügbarkeitsgruppen**  
  
-   [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **So konfigurieren Sie einen Datenbankspiegelungs-Endpunkt**  
  
-   [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Problembehandlung für die Always On-Verfügbarkeitsgruppenkonfiguration**  
  
-   [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
