---
title: Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe mit dem Assistenten für Verfügbarkeitsgruppen
description: Hier erfahren Sie, wie Sie mit dem Assistenten für Verfügbarkeitsgruppen in SQL Server Management Studio eine Datenbank zu einer Always On-Verfügbarkeitsgruppe hinzufügen.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.adddatabasewizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], databases
ms.assetid: 81e5e36d-735d-4731-8017-2654673abb88
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0147846a3f7ec66295895812ac828a12c99d5632
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340900"
---
# <a name="add-a-database-to-an-always-on-availability-group-with-the-availability-group-wizard"></a>Hinzufügen einer Datenbank zu einer Always On-Verfügbarkeitsgruppe mit dem Assistenten für Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Verwenden Sie den Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen, um einer vorhandenen AlwaysOn-Verfügbarkeitsgruppe eine oder mehrere Datenbank hinzuzufügen.  
  
> [!NOTE]  
>  Informationen zum Hinzufügen einer Datenbank mithilfe von [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder PowerShell finden Sie unter [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Falls Sie einer Verfügbarkeitsgruppe noch nie eine Datenbank hinzugefügt haben, lesen Sie den Abschnitt „Verfügbarkeitsdatenbanken“ unter [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> Voraussetzungen, Einschränkungen und Empfehlungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, auf der das aktuelle primäre Replikat gehostet wird.  
  
-   **Voraussetzungen für die Verwendung der vollständigen anfänglichen Datensynchronisierung**  
  
    -   Alle Datenbankdateipfade müssen auf allen Serverinstanzen, die ein Replikat für die neue Verfügbarkeitsgruppe hosten, identisch sein.  
  
    -   Primäre Datenbanknamen können nicht auf Serverinstanzen vorhanden sein, die ein sekundäres Replikat hosten. Daher kann noch keine der neuen sekundären Datenbanken vorhanden sein.  
  
    -   Sie müssen eine Netzwerkfreigabe angeben, damit der Assistent Sicherungen erstellen und auf Sicherungen zugreifen kann. Für das primäre Replikat kann das zum Starten von [!INCLUDE[ssDE](../../../includes/ssde-md.md)] verwendete Konto über Lese- und Schreibberechtigungen für das Dateisystem in einer Netzwerkfreigabe verfügen. Bei sekundären Replikaten muss das Konto über eine Leseberechtigung für die Netzwerkfreigabe verfügen.  
  
     Wenn Sie nicht den Assistenten für eine vollständige anfängliche Datensynchronisierung verwenden können, müssen Sie die sekundären Datenbanken manuell vorbereiten. Dies ist vor oder nach dem Ausführen des Assistenten möglich. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="use-the-new-availability-group-wizard"></a>Verwenden des Assistenten für neue Verfügbarkeitsgruppen
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Replikat der Verfügbarkeitsgruppe hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, der Sie eine Datenbank hinzufügen, und wählen Sie den Befehl **Datenbank hinzufügen** aus. Dieser Befehl startet den Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen.  
  
4.  Wählen Sie auf der Seite **Datenbanken auswählen** mindestens eine Datenbank aus. Weitere Informationen finden Sie unter [Seite „Datenbanken auswählen“ &#40;Assistent für neue Verfügbarkeitsgruppen und Assistent zum Hinzufügen von Datenbanken&#41;](../../../database-engine/availability-groups/windows/select-databases-page-new-availability-group-wizard-and-add-database-wizard.md).  
  
     Wenn die Datenbank einen Datenbank-Hauptschlüssel enthält, geben Sie das Kennwort für den Datenbank-Hauptschlüssel in die Spalte **Kennwort** ein.  
  
5.  Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** aus, wie die neuen sekundären Datenbanken erstellt und mit der Verfügbarkeitsgruppe verknüpft werden sollen. Wählen Sie eine der folgenden Optionen aus:  

    - **Automatisches Seeding**
      
      Wählen Sie diese Option aus, um das automatische Seeding zu verwenden. Das automatische Seeding verwendet die Übermittlung durch Protokollstream, um die Sicherung mit VDI für jede Datenbank der Verfügbarkeitsgruppe mit konfigurierten Endpunkten an das sekundäre Replikat zu streamen. Damit wird die Sicherung der Datenbank auf dem sekundären Replikat wiederhergestellt, ohne dass dafür manuelle Schritte erforderlich sind. Weitere Informationen zum automatischen Seeding finden Sie unter [Automatisches Seeding](automatic-seeding-secondary-replicas.md).
  
    -   **Vollständig**  
  
         Aktivieren Sie diese Option, wenn Ihre Umgebung die Anforderungen zum automatischen Starten der anfänglichen Datensynchronisierung erfüllt. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen, Einschränkungen und Empfehlungen](#Prerequisites).  
  
         Wenn Sie **Vollständig** auswählen, versucht der Assistent, nach der Erstellung der Verfügbarkeitsgruppe alle primären Datenbanken und ihre Transaktionsprotokolle auf einer Netzwerkfreigabe zu sichern und die Sicherungen auf allen Serverinstanzen wiederherzustellen, die ein sekundäres Replikat hosten. Der Assistent verknüpft anschließend alle sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
         Legen Sie im Feld zum **Angeben eines freigegebenen Netzwerkspeicherorts, auf den von allen Replikaten zugegriffen werden kann** , eine Sicherungsfreigabe fest, für die alle Serverinstanzen, die Replikate hosten, Lese-/Schreibzugriff besitzen. Die Protokollsicherungen sind Teil der Protokollsicherungskette. Speichern Sie die Protokollsicherungsdateien ordnungsgemäß.  
  
        > [!IMPORTANT]  
        >  Informationen zu den erforderlichen Dateisystemberechtigungen finden Sie weiter oben in diesem Thema unter [Erforderliche Komponenten](#Prerequisites).  
  
    -   **Nur verknüpfen**  
  
         Wenn Sie sekundäre Datenbanken auf den Serverinstanzen, die die sekundären Replikate hosten, manuell vorbereitet haben, können Sie diese Option aktivieren. Der Assistent verknüpft die vorhandenen sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
    -   **Anfängliche Datensynchronisierung überspringen**  
  
         Aktivieren Sie diese Option, wenn Sie eigene Datenbank- und Protokollsicherungen der primären Datenbanken verwenden möchten. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
     Weitere Informationen finden Sie unter [Seite „Anfängliche Datensynchronisierung auswählen“ &#40;AlwaysOn-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md).  
  
6.  Klicken Sie auf der Seite **Mit vorhandenen sekundären Replikaten verbinden** auf [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Alle verbinden **, wenn die Instanzen von**, die die Verfügbarkeitsreplikate für diese Verfügbarkeitsgruppe hosten, als ein Dienst unter demselben Benutzerkonto ausgeführt werden. Wenn eine der Serverinstanzen als Dienst unter verschiedenen Konten ausgeführt wird, klicken Sie rechts neben jedem Serverinstanznamen jeweils auf die Schaltfläche **Verbinden** .  
  
     Weitere Informationen finden Sie unter [Mit vorhandenen sekundären Replikaten verbinden &#40;Seite, Assistent zum Hinzufügen von Replikaten/Assistent zum Hinzufügen von Datenbanken&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
7.  Auf der Seite **Überprüfung** wird überprüft, ob die in diesem Assistenten angegebenen Werte die Anforderungen des Assistenten für neue Verfügbarkeitsgruppen erfüllen. Um eine Änderung vorzunehmen, können Sie auf **Zurück** klicken, um zu einer vorherigen Assistentenseite zurückzukehren und Werte zu ändern. Klicken Sie anschließend auf **Weiter** , um auf die Seite **Überprüfung** zurückzukehren, und klicken Sie auf **Überprüfung erneut ausführen**.  
  
     Weitere Informationen finden Sie unter [Seite „Überprüfung“ &#40;AlwaysOn-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/validation-page-always-on-availability-group-wizards.md).  
  
8.  Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen für die neue Verfügbarkeitsgruppe. Um eine Änderung vorzunehmen, klicken Sie auf **Zurück** , um zu der relevanten Seite zurückzukehren. Nachdem Sie die Änderung vorgenommen haben, klicken Sie auf **Weiter** , um zur Seite **Zusammenfassung** zurückzukehren.  
  
     Weitere Informationen finden Sie unter [Seite „Zusammenfassung“ &#40;AlwaysOn-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/summary-page-always-on-availability-group-wizards.md).  
  
     Wenn Sie mit der Auswahl zufrieden sind, klicken Sie optional auf "Skript", um ein Skript der Schritte zu erstellen, die der Assistent ausführt. Klicken Sie dann zum Erstellen und Konfigurieren der neuen Verfügbarkeitsgruppe auf **Fertig stellen**.  
  
9. Auf der Seite **Status** wird der Status der Schritte zum Erstellen der Verfügbarkeitsgruppe angezeigt (Konfigurieren von Endpunkten, Erstellen der Verfügbarkeitsgruppe und Hinzufügen des sekundären Replikats zu der Gruppe).  
  
     Weitere Informationen finden Sie unter [Seite „Status“ &#40;AlwaysOn-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/progress-page-always-on-availability-group-wizards.md).  
  
10. Wenn diese Schritte abgeschlossen sind, wird auf der Seite **Ergebnisse** das Ergebnis der einzelnen Schritte angezeigt. Wenn all diese Schritte erfolgreich sind, ist die neue Verfügbarkeitsgruppe vollständig konfiguriert. Wenn einer der Schritte zu einem Fehler führt, müssen Sie die Konfiguration möglicherweise manuell abschließen. Klicken Sie in der Spalte **Ergebnis** auf den zugehörigen Fehlerlink, um weitere Informationen zur Ursache eines bestimmten Fehlers zu erhalten.  
  
     Klicken Sie nach Abschluss des Assistenten auf **Schließen** , um den Assistenten zu beenden.  
  
     Weitere Informationen finden Sie unter [Seite „Ergebnisse“ &#40;Always On-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md)ausgeführt wird.  
  
11. Wenn die anfängliche Datensynchronisierung nicht automatisch auf allen sekundären Datenbanken gestartet wurde, müssen Sie noch nicht verknüpfte Datenbanken konfigurieren. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)   
 [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)   
 [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
  
