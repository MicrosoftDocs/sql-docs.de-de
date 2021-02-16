---
title: Hinzufügen von Replikaten zu Verfügbarkeitsgruppen (SSMS)
description: Fügen Sie mithilfe des Assistenten in SQL Server Management Studio einer Always On-Verfügbarkeitsgruppe ein Replikat hinzu.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], wizards
ms.assetid: 60d962b6-2af4-4394-9190-61939a102bc0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9180f809dd1300a0c5c6a4c5b5d429c6054adc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340455"
---
# <a name="add-a-replica-to-your-always-on-availability-group-using-the-availability-group-wizard-in-sql-server-management"></a>Hier erfahren Sie, wie Sie mit dem Assistenten für Verfügbarkeitsgruppen in SQL Server Management Studio ein Replikat zu Ihrer Always On-Verfügbarkeitsgruppe hinzufügen.
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Fügen Sie mithilfe des **Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen** ein neues sekundäres Replikat zu einer vorhandenen Alway On-Verfügbarkeitsgruppe hinzu.  
  
> [!NOTE]  
>  Informationen zur Verwendung von [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder PowerShell zum Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe finden Sie unter [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Wenn Sie einer Verfügbarkeitsgruppe noch kein Verfügbarkeitsreplikat hinzugefügt haben, sollten Sie sich zuvor im Thema [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)in den Abschnitten „Serverinstanzen“ und „Verfügbarkeitsgruppen und -replikate“ informieren.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, auf der das aktuelle primäre Replikat gehostet wird.  
  
-   Stellen Sie vor dem Hinzufügen eines sekundären Replikats sicher, dass sich die Hostinstanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] im selben WSFC-Cluster (Windows Server Failover Cluster) wie die vorhandenen Replikate, jedoch auf einem anderen Clusterknoten befindet. Stellen Sie außerdem sicher, dass diese Serverinstanz alle anderen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Voraussetzungen erfüllt. Weitere Informationen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)zu unterstützen.  
  
-   Wenn eine Serverinstanz, die Sie zum Hosten eines Verfügbarkeitsreplikats auswählen, unter einem Domänenbenutzerkonto ausgeführt wird und noch keinen Datenbankspiegelungs-Endpunkt aufweist, kann der Assistent den Endpunkt erstellen und dem Dienstkonto der Serverinstanz die CONNECT-Berechtigung erteilen. Wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst jedoch als integriertes Konto, z. B. Lokales System, Lokaler Dienst oder Netzwerkdienst, oder als Nichtdomänenkonto ausgeführt wird, müssen Sie Zertifikate zur Endpunktauthentifizierung verwenden, und der Assistent kann keinen Datenbankspiegelungs-Endpunkt auf der Serverinstanz erstellen. In diesem Fall empfiehlt es sich, dass Sie die Datenbankspiegelungs-Endpunkte manuell erstellen, bevor Sie den Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen starten.  
  
     **So verwenden Sie Zertifikate für einen Datenbankspiegelungs-Endpunkt:**  
  
     [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
     [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   **Voraussetzungen für die Verwendung der vollständigen anfänglichen Datensynchronisierung**  
  
    -   Alle Datenbankdateipfade müssen auf allen Serverinstanzen, die ein Replikat für die neue Verfügbarkeitsgruppe hosten, identisch sein.  
  
    -   Primäre Datenbanknamen können nicht auf Serverinstanzen vorhanden sein, die ein sekundäres Replikat hosten. Daher kann noch keine der neuen sekundären Datenbanken vorhanden sein.  
  
    -   Sie müssen eine Netzwerkfreigabe angeben, damit der Assistent Sicherungen erstellen und auf Sicherungen zugreifen kann. Für das primäre Replikat kann das zum Starten von [!INCLUDE[ssDE](../../../includes/ssde-md.md)] verwendete Konto über Lese- und Schreibberechtigungen für das Dateisystem in einer Netzwerkfreigabe verfügen. Bei sekundären Replikaten muss das Konto über eine Leseberechtigung für die Netzwerkfreigabe verfügen.  
  
     Wenn Sie nicht den Assistenten für eine vollständige anfängliche Datensynchronisierung verwenden können, müssen Sie die sekundären Datenbanken manuell vorbereiten. Dies ist vor oder nach dem Ausführen des Assistenten möglich. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
   
## <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
 Außerdem ist die CONTROL ON ENDPOINT-Berechtigung erforderlich, wenn Sie dem Verfügbarkeitsgruppen-Assistenten zur Verwaltung des Datenbankspiegelungs-Endpunkts das Hinzufügen von Replikaten erlauben möchten.  
  
##  <a name="using-the-add-replica-to-availability-group-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwendung des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen (SQL Server Management Studio)  
 **So verwenden Sie den Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Replikat der Verfügbarkeitsgruppe hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, der Sie ein sekundäres Replikat hinzufügen, und wählen Sie den Befehl **Replikat hinzufügen** aus. Dies startet den Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen.  
  
4.  Verbinden Sie sich auf der Seite **Mit vorhandenen sekundären Replikaten verbinden** mit sämtlichen sekundären Replikaten in der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Seite „Mit vorhandenen sekundären Replikaten verbinden“ &#40;Assistent zum Hinzufügen von Replikaten: Assistent zum Hinzufügen von Datenbanken&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
5.  Auf der Seite **Replikate angeben** können Sie ein oder mehrere neue sekundäre Replikate für die Verfügbarkeitsgruppe angeben und konfigurieren. Diese Seite enthält drei Registerkarten. In der folgenden Tabelle werden diese Registerkarten eingeführt. Weitere Informationen finden Sie unter [Seite „Replikate angeben“ &#40;Assistent für neue Verfügbarkeitsgruppen: Assistent zum Hinzufügen von Replikaten&#41;](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md).  
  
    |Registerkarte|Kurzbeschreibung|  
    |---------|-----------------------|  
    |**Replikate**|Geben Sie mit dieser Registerkarte jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] an, die ein neues sekundäres Replikat hostet.|  
    |**Endpunkte**|Verwenden Sie diese Registerkarte, um den vorhandenen Datenbankspiegelungs-Endpunkt ggf. für jedes neue sekundäre Replikat zu überprüfen. Falls dieser Endpunkt auf einer Serverinstanz fehlt, deren Dienstkonten die Windows-Authentifizierung verwenden, wird vom Assistenten versucht, den Endpunkt automatisch zu erstellen.<br /><br /> <br /><br /> Hinweis: Wenn eine Serverinstanz unter einem Nicht-Domänenbenutzerkonto ausgeführt wird, müssen Sie eine manuelle Änderung an der Serverinstanz vornehmen, bevor Sie den Assistenten fortsetzen können. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen](#Prerequisites).|  
    |**Sicherungseinstellungen**|Geben Sie mit dieser Registerkarte die Sicherungseinstellungen für die Verfügbarkeitsgruppe als Ganzes, wenn Sie die aktuelle Einstellung ändern möchten, und die Sicherungsprioritäten für die einzelnen Verfügbarkeitsreplikate an.|  
  
6.  Wenn die ausgewählten Replikate Datenbanken enthalten, die über einen Datenbank-Hauptschlüssel verfügen, geben Sie die Kennwörter für die Datenbank-Hauptschlüssel in der Spalte **Kennwort** ein. Die Spalte **Status** gibt **Kennwort erforderlich** für Datenbanken an, die über einen Datenbank-Hauptschlüssel verfügen. **Weiter** ist so lange ausgegraut, bis das Kennwort in die Spalte **Kennwort** eingegeben wird. Nachdem Sie die Kennwörter eingegeben haben, klicken Sie auf **Aktualisieren**. Wenn Sie die Kennwörter richtig eingegeben haben, wird in der Status-Spalte **Kennwort eingegeben** angezeigt, und die Schaltfläche **Weiter** wird verfügbar.  
  
7.  Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** aus, wie die neuen sekundären Datenbanken erstellt und mit der Verfügbarkeitsgruppe verknüpft werden sollen. Wählen Sie eine der folgenden Optionen aus:  
  
    -   **Vollständig**  
  
         Aktivieren Sie diese Option, wenn Ihre Umgebung die Anforderungen zum automatischen Starten der anfänglichen Datensynchronisierung erfüllt. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen, Einschränkungen und Empfehlungen](#Prerequisites).  
  
         Wenn Sie **Vollständig** auswählen, werden vom Assistenten nach der Erstellung der Verfügbarkeitsgruppe alle primären Datenbanken und ihre Transaktionsprotokolle auf einer Netzwerkfreigabe gesichert und die Sicherungen auf allen Serverinstanzen wiederhergestellt, die ein neues sekundäres Replikat hosten. Der Assistent verknüpft anschließend alle neuen sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
         Legen Sie im Feld zum **Angeben eines freigegebenen Netzwerkspeicherorts, auf den von allen Replikaten zugegriffen werden kann** , eine Sicherungsfreigabe fest, für die alle Serverinstanzen, die Replikate hosten, Lese-/Schreibzugriff besitzen. Die Protokollsicherungen sind Teil der Protokollsicherungskette. Speichern Sie die Protokollsicherungsdateien ordnungsgemäß.  
  
        > [!IMPORTANT]  
        >  Informationen zu den erforderlichen Dateisystemberechtigungen finden Sie weiter oben in diesem Thema unter [Erforderliche Komponenten](#Prerequisites).  
  
    -   **Nur verknüpfen**  
  
         Wenn Sie sekundäre Datenbanken auf den Serverinstanzen, die die neuen sekundären Replikate hosten, manuell vorbereitet haben, können Sie diese Option aktivieren. Der Assistent verknüpft die neuen sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
    -   **Anfängliche Datensynchronisierung überspringen**  
  
         Aktivieren Sie diese Option, wenn Sie eigene Datenbank- und Protokollsicherungen der primären Datenbanken verwenden möchten. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
8.  Auf der Seite **Überprüfung** wird überprüft, ob die in diesem Assistenten angegebenen Werte die Anforderungen des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen erfüllen. Um eine Änderung vorzunehmen, klicken Sie auf **Zurück** , um zu einer vorherigen Assistentenseite zurückzukehren und Werte zu ändern. Klicken Sie anschließend auf **Weiter** , um auf die Seite **Überprüfung** zurückzukehren, und klicken Sie auf **Überprüfung erneut ausführen**.  
  
9. Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen für die neue Verfügbarkeitsgruppe. Um eine Änderung vorzunehmen, klicken Sie auf **Zurück** , um zu der relevanten Seite zurückzukehren. Nachdem Sie die Änderung vorgenommen haben, klicken Sie auf **Weiter** , um zur Seite **Zusammenfassung** zurückzukehren.  
  
     Wenn Sie mit der Auswahl zufrieden sind, klicken Sie optional auf "Skript", um ein Skript der Schritte zu erstellen, die der Assistent ausführt. Klicken Sie dann zum Erstellen und Konfigurieren der neuen Verfügbarkeitsgruppe auf **Fertig stellen**.  
  
10. Auf der Seite **Status** wird der Status der Schritte zum Erstellen der Verfügbarkeitsgruppe angezeigt (Konfigurieren von Endpunkten, Erstellen der Verfügbarkeitsgruppe und Hinzufügen des sekundären Replikats zu der Gruppe).  
  
11. Wenn diese Schritte abgeschlossen sind, wird auf der Seite **Ergebnisse** das Ergebnis der einzelnen Schritte angezeigt. Wenn all diese Schritte erfolgreich sind, ist die neue Verfügbarkeitsgruppe vollständig konfiguriert. Wenn einer der Schritte zu einem Fehler führt, müssen Sie die Konfiguration möglicherweise manuell abschließen. Klicken Sie in der Spalte **Ergebnis** auf den zugehörigen Fehlerlink, um weitere Informationen zur Ursache eines bestimmten Fehlers zu erhalten.  
  
     Klicken Sie nach Abschluss des Assistenten auf **Schließen** , um den Assistenten zu beenden.  
  
> [!IMPORTANT]  
>  Lesen Sie nach dem Hinzufügen eines Replikats den Abschnitt „Nächster Schritt: Nach dem Hinzufügen eines sekundären Replikats“ unter [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
  
