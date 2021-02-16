---
title: Analysis Services mit Verfügbarkeitsgruppen
description: Wenn Sie Always On-Verfügbarkeitsgruppen als Hochverfügbarkeitslösung verwenden, können Sie in einer tabellarischen oder mehrdimensionalen Analysis Services-Lösung in dieser Gruppe eine Datenbank als Datenquelle verwenden.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 14d16bfd-228c-4870-b463-a283facda965
author: cawrites
ms.author: chadam
manager: erikre
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1bcb05458c8e8792975606382e7a1cd24de628a3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338488"
---
# <a name="analysis-services-with-always-on-availability-groups"></a>Analysis Services mit Always On-Verfügbarkeitsgruppen

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Eine Always On-Verfügbarkeitsgruppe ist eine vordefinierte Sammlung relationaler SQL Server-Datenbanken, die gemeinsam ein Failover ausführen, wenn Bedingungen in einer der Datenbanken ein Failover auslösen. Dabei werden Anforderungen an eine gespiegelte Datenbank in einer anderen Instanz in der gleichen Verfügbarkeitsgruppe umgeleitet. Wenn Sie als Hochverfügbarkeitslösung Verfügbarkeitsgruppen verwenden, können Sie in einer tabellarischen oder mehrdimensionalen Analysis Services-Lösung eine Datenbank in dieser Gruppe als Datenquelle verwenden. Alle folgenden Analysis Services-Vorgänge werden bei Verwendung einer Verfügbarkeitsdatenbank wie erwartet ausgeführt: das Verarbeiten oder Importieren von Daten, das direkte Abfragen von relationalen Daten (im ROLAP-Speichermodus oder DirectQuery-Modus) und das Rückschreiben.  
  
 Das Verarbeiten und Abfragen sind schreibgeschützte Arbeitsauslastungen. Sie können die Leistung verbessern, indem Sie diese Arbeitsauslastungen in ein lesbares sekundäres Replikat auslagern. Dieses Szenario erfordert zusätzliche Konfiguration. Stellen Sie mithilfe der Prüfliste in diesem Thema sicher, dass Sie alle Schritte ausführen.  
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Voraussetzungen  
 Sie benötigen auf allen Replikaten eine SQL Server-Anmeldung. Verfügbarkeitsgruppen, Listener und Datenbanken müssen von einem Mitglied der **sysadmin** -Rolle konfiguriert werden. Benutzer benötigen jedoch für den Zugriff auf die Datenbank von einem Analysis Services-Client aus lediglich **db_datareader** -Berechtigungen.  
  
 Verwenden Sie einen Datenanbieter, der das TDS (Tabular Data Stream)-Protokoll, Version 7.4 oder höher, unterstützt, z. B. SQL Server Native Client 11.0 oder den Datenanbieter für SQL Server in .NET Framework 4.02.  
  
 **(Für schreibgeschützte Arbeitsauslastungen)** . Die sekundäre Replikatrolle muss für schreibgeschützte Verbindungen konfiguriert sein, die Verfügbarkeitsgruppe muss über eine Routingliste verfügen, und die Verbindung in der Analysis Services-Datenquelle muss den Verfügbarkeitsgruppenlistener angeben. In diesem Thema finden Sie Anweisungen.  
  
##  <a name="checklist-use-a-secondary-replica-for-read-only-operations"></a><a name="bkmk_UseSecondary"></a> Prüfliste: Verwenden eines sekundären Replikats für schreibgeschützte Vorgänge  
 Wenn die Analysis Services-Lösung kein Rückschreiben umfasst, können Sie eine Datenquellenverbindung zum Verwenden eines lesbaren sekundären Replikats konfigurieren. Wenn Sie über eine schnelle Netzwerkverbindung verfügen, weist das sekundäre Replikat eine sehr geringe Datenlatenz auf und stellt nahezu die gleichen Daten wie das primäre Replikat bereit. Mit dem sekundären Replikat für Analysis Services-Vorgänge können Sie Lese-/Schreibkonflikte auf dem primären Replikat reduzieren und eine bessere Auslastung sekundärer Replikate in der Verfügbarkeitsgruppe erzielen.  
  
 Standardmäßig sind sowohl der Lese-/Schreibzugriff als auch der Zugriff für beabsichtigte Lesevorgänge für das primäre Replikat zulässig, und es werden keine Verbindungen mit sekundären Replikaten zugelassen. Das Einrichten einer schreibgeschützten Clientverbindung mit einem sekundären Replikat erfordert zusätzliche Konfiguration. Bei der Konfiguration müssen Eigenschaften auf dem sekundären Replikat festgelegt und ein T-SQL-Skript ausgeführt werden, das eine schreibgeschützte Routingliste definiert. Stellen Sie mithilfe der folgenden Verfahren sicher, dass Sie beide Schritte ausgeführt haben.  
  
> [!NOTE]  
>  Für die folgenden Schritte werden eine vorhandene Always On-Verfügbarkeitsgruppe und vorhandene Datenbanken vorausgesetzt. Wenn Sie eine neue Gruppe konfigurieren, verwenden Sie den Assistenten für neue Verfügbarkeitsgruppen, um die Gruppe zu erstellen und die Datenbanken zu verknüpfen. Der Assistent überprüft, ob die Voraussetzungen erfüllt sind, bietet Anleitungen für die einzelnen Schritte und führt die Erstsynchronisierung aus. Weitere Informationen finden Sie unter [Verwenden des Assistenten für Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
#### <a name="step-1-configure-access-on-an-availability-replica"></a>Schritt 1: Konfigurieren des Zugriffs auf ein Verfügbarkeitsreplikat  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
    > [!NOTE]  
    >  Diese Schritte sind unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md) beschrieben. Dort finden Sie weitere Informationen und alternative Anweisungen zum Ausführen dieser Aufgabe.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie auf die Verfügbarkeitsgruppe, deren Replikat geändert werden soll. Erweitern Sie **Verfügbarkeitsreplikate**.  
  
4.  Klicken Sie mit der rechten Maustaste auf das sekundäre Replikat, und klicken Sie auf **Eigenschaften**.  
  
5.  Ändern Sie im Dialogfeld **Eigenschaften des Verfügbarkeitsreplikats** den Verbindungszugriff für die sekundäre Rolle wie folgt:  
  
    -   Wählen Sie in der Dropdownliste **Lesbare sekundäre Rolle** den Eintrag **Nur beabsichtigte Lesevorgänge** aus.  
  
    -   Wählen Sie in der Dropdownliste **Verbindungen in primärer Rolle** den Eintrag **Alle Verbindungen zulassen** aus. Dies ist die Standardoption.  
  
    -   Optional können Sie in der Dropdownliste **Verfügbarkeitsmodus** den Eintrag **Synchroner Commit** auswählen. Dieser Schritt ist nicht erforderlich, jedoch wird durch das Festlegen dieser Option Datenparität zwischen dem primären und sekundären Replikat sichergestellt.  
  
         Diese Eigenschaft ist auch eine Voraussetzung für geplantes Failover. Wenn Sie zu Testzwecken ein geplantes manuelles Failover ausführen möchten, legen Sie für das primäre und das sekundäre Replikat **Verfügbarkeitsmodus** auf **Synchroner Commit** fest.  
  
#### <a name="step-2-configure-read-only-routing"></a>Schritt 2: Konfigurieren von schreibgeschütztem Routing  
  
1.  Stellen Sie eine Verbindung mit dem primären Replikat her.  
  
    > [!NOTE]  
    >  Diese Schritte sind unter [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md) beschrieben. Dort finden Sie weitere Informationen und alternative Anweisungen zum Ausführen dieser Aufgabe.  
  
2.  Öffnen Sie ein Abfragefenster, und fügen Sie das folgende Skript ein. Dieses Skript führt drei Aufgaben aus: Es aktiviert lesbare Verbindungen mit einem sekundären Replikat (standardmäßig deaktiviert), legt die URL für das schreibgeschützte Routing fest und erstellt die Routingliste, in der die Priorisierung der Weiterleitung von Verbindungsanforderungen festgelegt ist.  Die erste Anweisung, die lesbare Verbindungen zulässt, ist redundant, wenn Sie die Eigenschaften bereits in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]festgelegt haben, sie wird jedoch der Vollständigkeit halber aufgeführt.  
  
    ```  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
    GO  
    ```  
  
3.  Ändern Sie das Skript, und ersetzen Sie die Platzhalter durch gültige Werte für Ihre Bereitstellung:  
  
    -   Ersetzen Sie 'COMPUTER01' durch den Namen der Serverinstanz, die das primäre Replikat hostet.  
  
    -   Ersetzen Sie 'COMPUTER02' durch den Namen der Serverinstanz, die das sekundäre Replikat hostet.  
  
    -   Ersetzen Sie 'contoso.com' durch den Namen der Domäne, oder lassen Sie den Namen aus dem Skript aus, wenn sich alle Computer in der gleichen Domäne befinden. Behalten Sie die Portnummer bei, wenn der Listener den Standardport verwendet. Der tatsächlich vom Listener verwendete Port ist auf der Eigenschaftenseite in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]aufgeführt.  
  
4.  Führen Sie das Skript aus.  
  
     Erstellen Sie anschließend eine Datenquelle in einem Analysis Services-Modell, die eine Datenbank aus der soeben von Ihnen konfigurierten Gruppe verwendet.  
  
##  <a name="create-an-analysis-services-data-source-using-an-always-on-availability-database"></a><a name="bkmk_ssasAODB"></a> Erstellen einer Analysis Services-Datenquelle mithilfe einer Always On-Verfügbarkeitsdatenbank  
 In diesem Abschnitt wird das Erstellen einer Analysis Services-Datenquelle beschrieben, die eine Verbindung mit einer Datenbank in einer Verfügbarkeitsgruppe herstellt. Sie können mithilfe dieser Anweisungen eine Verbindung mit einem primären Replikat (Standard) oder einem lesbaren sekundären Replikat konfigurieren, das Sie mit Schritten in einem vorherigen Abschnitt konfiguriert haben. Always On-Konfigurationseinstellungen sowie die im Client festgelegten Verbindungseigenschaften bestimmen, ob ein primäres oder sekundäres Replikat verwendet wird.  
  
1.  Klicken Sie in [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)]in einem Analysis Services-Projekt für mehrdimensionale und Data Mining-Modelle mit der rechten Maustaste auf **Datenquellen** , und wählen Sie **Neue Datenquelle** aus. Klicken Sie auf **Neu** , um eine neue Datenquelle zu erstellen.  
  
     Alternativ können Sie für ein tabellarisches Modellprojekt auf das Menü „Modell“ und anschließend auf **Aus Datenquelle importieren** klicken.  
  
2.  Wählen Sie im Verbindungs-Manager unter "Anbieter" einen Anbieter aus, der das TDS (Tabular Data Stream)-Protokoll unterstützt. Dieses Protokoll wird von SQL Server Native Client 11.0 unterstützt.  
  
3.  Geben Sie im Verbindungs-Manager unter „Servername“ den Namen des *Verfügbarkeitsgruppenlisteners* ein, und wählen Sie anschließend eine Datenbank aus, die in der Gruppe verfügbar ist.  
  
     Der Verfügbarkeitsgruppenlistener leitet für Lese-/Schreibanforderungen eine Clientverbindung an ein primäres Replikat um. Wenn Sie in der Verbindungszeichenfolge beabsichtigte Lesevorgänge angeben, erfolgt die Umleitung an ein sekundäres Replikat. Da sich Replikatrollen während eines Failovers (bei dem das primäre Replikat zum sekundären Replikat und das sekundäre Replikat zum primären Replikat wird) ändern, sollten Sie immer den Listener angeben, damit die Clientverbindung entsprechend umgeleitet wird.  
  
     Sie können einen Datenbankadministrator fragen oder eine Verbindung mit einer Instanz in der Verfügbarkeitsgruppe herstellen und die entsprechende Always On-Verfügbarkeitskonfiguration anzeigen, um den Namen des Verfügbarkeitsgruppenlisteners zu bestimmen.   
  
4.  Klicken Sie im Verbindungs-Manager im linken Navigationsbereich auf **Alle** , um das Eigenschaftenraster des Datenanbieters anzuzeigen.  
  
     Legen Sie **Anwendungszweck** auf **READONLY** fest, wenn Sie eine schreibgeschützte Clientverbindung mit einem sekundären Replikat konfigurieren. Behalten Sie andernfalls die Standardeinstellung **READWRITE** bei, um die Verbindung an das primäre Replikat umzuleiten.  
  
5.  Wählen Sie in „Identitätswechselinformationen“ die Option **Bestimmter Windows-Benutzername und bestimmtes Kennwort** aus, und geben Sie dann ein Windows-Domänenbenutzerkonto ein, das mindestens über **db_datareader** -Berechtigungen für die Datenbank verfügt.  
  
     Lassen Sie die Optionen **Anmeldeinformationen des aktuellen Benutzers** und **Erben** unbedingt deaktiviert. Sie können **Dienstkonto** auswählen, jedoch nur, wenn dieses Konto über Leseberechtigungen für die Datenbank verfügt.  
  
     Stellen Sie die Datenquelle fertig, und schließen Sie den Datenquellen-Assistenten.  
  
6.  Fügen Sie der Verbindungszeichenfolge **MultiSubnetFailover=Yes** hinzu, damit der aktive Server schneller erkannt wird und schneller eine Verbindung mit ihm hergestellt wird. Weitere Informationen zu dieser Eigenschaft finden Sie unter [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
     Diese Eigenschaft wird im Eigenschaftenraster nicht angezeigt. Klicken Sie mit der rechten Maustaste auf die Datenquelle, und wählen Sie **Code anzeigen** aus, um die Eigenschaft hinzuzufügen. Fügen Sie der Verbindungszeichenfolge `MultiSubnetFailover=Yes` hinzu.  
  
 Die Datenquelle ist jetzt definiert. Sie können jetzt ein Modell erstellen, wobei Sie mit der Datenquellensicht beginnen. Für tabellarische Modelle beginnen Sie mit dem Erstellen von Beziehungen. Wenn Daten aus der Verfügbarkeitsdatenbank abgerufen werden müssen (wenn Sie z. B. die Lösung verarbeiten oder bereitstellen möchten), können Sie die Konfiguration testen, um zu überprüfen, ob Zugriff auf Daten aus dem sekundären Replikat erfolgt.  
  
##  <a name="test-the-configuration"></a><a name="bkmk_test"></a> Testen der Konfiguration  
 Nachdem Sie das sekundäre Replikat konfiguriert und in Analysis Services eine Datenquellenverbindung erstellt haben, können Sie überprüfen, ob Verarbeitungs- und Abfragebefehle an das sekundäre Replikat umgeleitet werden. Sie können auch ein geplantes manuelles Failover ausführen, um den Wiederherstellungsplan für dieses Szenario zu überprüfen.  
  
#### <a name="step-1-confirm-the-data-source-connection-is-redirected-to-the-secondary-replica"></a>Schritt 1: Überprüfen, ob die Datenquellenverbindung an das sekundäre Replikat umgeleitet wird  
  
1.  Starten Sie SQL Server Profiler, und stellen Sie eine Verbindung mit der SQL Server-Instanz her, die das sekundäre Replikat hostet.  
  
     Bei der Ausführung der Ablaufverfolgung werden mit den Ereignissen **SQL:BatchStarting** und **SQL:BatchCompleting** die von Analysis Services ausgegebenen Abfragen angezeigt, die in der Datenbank-Engine-Instanz ausgeführt werden. Diese Ereignisse sind standardmäßig ausgewählt, daher müssen Sie lediglich die Ablaufverfolgung starten.  
  
2.  Öffnen Sie in [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)]das Analysis Services-Projekt oder die Analysis Services-Lösung, das bzw. die eine Datenquellenverbindung enthält, die Sie testen möchten. Stellen Sie sicher, dass die Datenquelle den Verfügbarkeitsgruppenlistener und nicht eine Instanz in der Gruppe angibt.  
  
     Dieser Schritt ist wichtig. Wenn Sie einen Serverinstanznamen angeben, erfolgt kein Routing zum sekundären Replikat.  
  
3.  Ordnen Sie die Anwendungsfenster so an, dass Server Profiler und [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] nebeneinander angezeigt werden.  
  
4.  Stellen Sie die Lösung bereit, und beenden Sie nach Abschluss der Bereitstellung die Ablaufverfolgung.  
  
     Im Ablaufverfolgungsfenster sollten Ereignisse aus der Anwendung **Microsoft SQL Server Analysis Services** angezeigt werden. Es sollten **SELECT** -Anweisungen angezeigt werden, mit denen Daten aus einer Datenbank auf der Serverinstanz abgerufen werden, die das sekundäre Replikat hostet. Dies bedeutet, dass die Verbindung mit dem sekundären Replikat über den Listener hergestellt wurde.  
  
#### <a name="step-2-perform-a-planned-failover-to-test-the-configuration"></a>Schritt 2: Ausführen eines geplanten Failovers zum Testen der Konfiguration  
  
1.  Überprüfen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] das primäre und sekundäre Replikat, um sicherzustellen, dass beide für den Modus mit synchronem Commit konfiguriert sind und gegenwärtig synchronisiert sind.  
  
     In den folgenden Schritten wird davon ausgegangen, dass ein sekundäres Replikat für synchronen Commit konfiguriert ist.  
  
     Öffnen Sie eine Verbindung mit jeder Instanz, die das primäre und sekundäre Replikat hostet, erweitern Sie den Ordner „Datenbanken“, und stellen Sie sicher, dass in jedem Replikat **(Synchronisiert)** und **(Wird synchronisiert)** an den Namen der Datenbank angefügt ist, um die Synchronisierung zu überprüfen.  
  
    > [!NOTE]  
    >  Diese Schritte sind unter [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md) beschrieben. Dort finden Sie weitere Informationen und alternative Anweisungen zum Ausführen dieser Aufgabe.  
  
2.  Starten Sie in SQL Server Profiler Ablaufverfolgungen für jedes Replikat, und zeigen Sie die Ablaufverfolgungen nebeneinander an. In den folgenden Schritten vergleichen Sie Ablaufverfolgungen und vergewissern sich, dass die zum Verarbeiten oder Abfragen verwendeten SQL-Abfragen aus Analysis Services zwischen den Replikaten wechseln.  
  
3.  Führen Sie in Analysis Services einen Verarbeitungs- oder Abfragebefehl aus. Da Sie die Datenquelle für eine schreibgeschützte Verbindung konfiguriert haben, sollte angezeigt werden, dass der Befehl auf dem sekundären Replikat ausgeführt wird.  
  
4.  Stellen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]eine Verbindung mit dem sekundären Replikat her.  
  
5.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
6.  Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, für die ein Failover ausgeführt werden soll, und wählen Sie den Befehl **Failover** aus. Dadurch wird der Assistent für das Failover von Verfügbarkeitsgruppen gestartet. Wählen Sie mithilfe des Assistenten aus, welches Replikat als neues primäres Replikat verwendet werden soll.  
  
7.  Vergewissern Sie sich, dass das Failover erfolgreich ausgeführt wurde:  
  
    -   Erweitern Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]die Verfügbarkeitsgruppen, um die Bezeichnungen für das primäre und sekundäre Replikat anzuzeigen. Die Instanz, die zuvor ein primäres Replikat war, sollte jetzt ein sekundäres Replikat sein.  
  
    -   Zeigen Sie das Dashboard an, um zu bestimmen, ob Integritätsprobleme gefunden wurden. Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und wählen Sie **Dashboard anzeigen** aus.  
  
8.  Warten Sie ein oder zwei Minuten lang, bis das Failover auf dem Back-End abgeschlossen wurde.  
  
9. Wiederholen Sie den Verarbeitungs- oder Abfragebefehl in der Analysis Services-Lösung, und beobachten Sie dann in SQL Server Profiler die Ablaufverfolgungen nebeneinander. Es sollte zu erkennen sein, dass die Verarbeitung auf der anderen Instanz erfolgt, die jetzt das neue sekundäre Replikat ist.  
  
##  <a name="what-happens-after-a-failover-occurs"></a><a name="bkmk_whathappens"></a> Vorgänge nach einem Failover  
 Während eines Failovers geht ein sekundäres Zielreplikat in die primäre Rolle über, und das frühere primäre Replikat geht in die sekundäre Rolle über. Alle Clientverbindungen werden beendet, der Besitz am Verfügbarkeitsgruppenlistener wechselt mit der primären Replikatrolle zu einer neuen SQL Server-Instanz, und der Listenerendpunkt wird an die virtuellen IP-Adressen und TCP-Ports der neuen Instanz gebunden. Weitere Informationen finden Sie unter [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
 Wenn während der Verarbeitung ein Failover erfolgt, wird in Analysis Services in der Protokolldatei oder im Ausgabefenster der folgende Fehler angezeigt „OLE DB-Fehler: OLE DB- oder ODBC-Fehler: Kommunikationslinkfehler; 08S01; TPC-Anbieter: Eine vorhandene Verbindung wurde vom Remotehost geschlossen. ; 08S01.“  
  
 Dieser Fehler sollte behoben sein, wenn Sie eine Minute lang warten und es dann erneut versuchen. Sofern die Verfügbarkeitsgruppe ordnungsgemäß für ein lesbares sekundäres Replikat konfiguriert ist, wird die Verarbeitung auf dem neuen sekundären Replikat fortgesetzt, wenn Sie die Verarbeitung erneut starten.  
  
 Persistente Fehler treten wahrscheinlich aufgrund eines Konfigurationsproblems auf. Sie können versuchen, das T-SQL-Skript erneut auszuführen, um Probleme mit der Routingliste, mit URLs für das schreibgeschützte Routing und mit beabsichtigten Lesevorgängen auf dem sekundären Replikat zu beheben. Sie sollten außerdem überprüfen, ob das primäre Replikat alle Verbindungen zulässt.  
  
##  <a name="writeback-when-using-an-always-on-availability-database"></a><a name="bkmk_writeback"></a> Rückschreiben mithilfe einer Always On-Verfügbarkeitsdatenbank  
 Rückschreiben ist eine Analysis Services-Funktion, die Was-wäre-wenn-Analysen in Excel unterstützt. Sie wird auch häufig für Budgetierungs- und Prognosetasks in benutzerdefinierten Anwendungen verwendet.  
  
 Die Unterstützung für Rückschreiben erfordert eine READWRITE-Clientverbindung. Wenn Sie in Excel versuchen, in eine schreibgeschützte Verbindung zurückzuschreiben, tritt der folgende Fehler auf: „Daten konnten nicht aus der externen Datenquelle abgerufen werden“. „Daten konnten nicht aus der externen Datenquelle abgerufen werden.“  
  
 Wenn Sie eine Verbindung ausschließlich für den Zugriff auf ein lesbares sekundäres Replikat konfiguriert haben, müssen Sie jetzt eine neue Verbindung als READWRITE-Verbindung mit dem primären Replikat konfigurieren.  
  
 Erstellen Sie hierzu in einem Analysis Services-Modell eine zusätzliche Datenquelle, um die Lese-/Schreibverbindung zu unterstützen. Wenn Sie die zusätzliche Datenquelle erstellen, verwenden Sie den gleichen Listenernamen und die gleiche Datenbank, die Sie in der schreibgeschützten Verbindung angegeben haben. Behalten Sie jedoch die Standardeinstellung bei, die READWRITE-Verbindungen unterstützt, statt **Anwendungszweck** zu ändern. Jetzt können Sie der Datenquelle neue Fakten- oder Dimensionstabellen hinzufügen, die auf der Datenquelle mit Lese-/Schreibzugriff basieren, und dann für die neuen Tabellen Rückschreiben aktivieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Aktive sekundäre Replikate: Lesbare sekundäre Replikate (Always On-Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Always On Policies for Operational Issues with Always On Availability Groups (SQL Server) (Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen (SQL Server))](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Erstellen einer Datenquelle (SSAS: mehrdimensional)](/analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional)   
 [Rückschreiben von Dimensionen aktivieren](/analysis-services/multidimensional-models/bi-wizard-enable-dimension-writeback)  
  
