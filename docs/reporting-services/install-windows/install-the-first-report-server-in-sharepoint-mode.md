---
description: Installieren des ersten Berichtsservers im SharePoint-Modus
title: Installieren des ersten Berichtsservers im SharePoint-Modus | Microsoft-Dokumentation
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 3ccffdc3beca07d53302b7a7dceff0e30bbb6331
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891220"
---
# <a name="install-the-first-report-server-in-sharepoint-mode"></a>Installieren des ersten Berichtsservers im SharePoint-Modus

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Dieser Artikel leitet Sie durch die Installation eines einzelnen Reporting Services-Berichtsservers im SharePoint-Modus. In den Schritten führen Sie u. a. den Installations-Assistenten für SQL Server sowie Konfigurationsaufgaben unter Verwendung der SharePoint-Zentraladministration aus. Außerdem können Sie sich in diesem Artikel auch über einzelne Verfahren zum Aktualisieren einer vorhandenen Installation informieren, beispielsweise um eine Reporting Services-Dienstanwendung zu erstellen.  
  
> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.
  
 Informationen darüber, wie Sie einer vorhandenen Farm weitere Reporting Services-Server hinzufügen, finden Sie unter:  
  
-   [Hinzufügen eines zusätzlichen Berichtsservers zu einer Farm &#40;Horizontales Skalieren für SSRS&#41;](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md)  
  
-   [Add an Additional Reporting Services Web Front-end to a Farm (Hinzufügen eines zusätzlichen Reporting Services-Web-Front-Ends zu einer Farm)](../../reporting-services/install-windows/add-an-additional-reporting-services-web-front-end-to-a-farm.md)  
  
 Die Installation auf einem einzelnen Server ist für Entwicklungs- und Testszenarien geeignet, wird aber für Produktionsumgebungen nicht empfohlen.  
  
##  <a name="example-single-server-deployment"></a><a name="bkmk_singleserver"></a> Beispiel für die Bereitstellung auf einem Server

 Die Installation auf einem einzelnen Server ist für Entwicklungs- und Testszenarien geeignet, wird aber für Produktionsumgebungen nicht empfohlen. Eine Umgebung mit einem Server bezieht sich auf einen einzelnen Computer, für den SharePoint und Reporting Services-Komponenten auf demselben Computer installiert sind. Die horizontale Skalierung mit mehreren Reporting Services-Servern wird hier nicht thematisiert.  
  
 Das folgende Diagramm veranschaulicht die Komponenten, die Teil einer Reporting Services-Bereitstellung auf einem einzelnen Server sind.  
 
 > [!NOTE]
 > In SharePoint 2016 wurde Excel Services auf den Office Online Server verschoben und kann nicht in einer Bereitstellung mit einem einzelnen Server verwendet werden. Der Office Online Server muss auf einem anderen Server bereitgestellt werden. Weitere Informationen finden Sie unter [Übersicht über Office Online Server](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) und [Konfigurieren der Excel Online-Verwaltungseinstellungen](https://technet.microsoft.com/library/jj219698\(v=office.16\).aspx).
  
|||  
|-|-|  
|**(1)**|Mit einer SQL Server-Installation installierter SharePoint-Dienst. Sie können mindestens eine Reporting Services-Dienstanwendung erstellen.|  
|**(2)**|Das Reporting Services-Add-In für SharePoint-Produkte stellt die Benutzeroberflächenkomponenten auf den SharePoint-Servern bereit.|  
|**(3)**|Die Excel Services-Anwendung wird von Power View und [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]verwendet. Dies ist in einer Einzelserverbereitstellung für SharePoint 2016 nicht verfügbar. Es ist ein [Office Online Server](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) erforderlich.|  
|**(4)**|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Dienstanwendung zu erstellen.|  
  
 ![SharePoint-Modus von SSRS: Bereitstellung auf einem Server](../../reporting-services/install-windows/media/rs-sharepoint-1server-deployment.gif "SharePoint-Modus von SSRS: Bereitstellung auf einem Server")  
  
> [!TIP]  
>  Komplexere Bereitstellungsbeispiele finden Sie unter [Bereitstellungstopologien für SQL Server-BI-Funktionen in SharePoint](/previous-versions/sql/sql-server-2016/hh231674(v=sql.130)).  
  
##  <a name="setup-accounts"></a><a name="bkmk_setupaccounts"></a> Setupkonten

 In diesem Abschnitt werden die Konten und Berechtigungen beschrieben, die für die wesentlichen Schritte zur Bereitstellung von Reporting Services im SharePoint-Modus verwendet werden.  
  
 **Installieren und Registrieren des Reporting Services-Dienstes:**  
  
-   Das aktuelle Installationskonto (auch Setupkonto) von Reporting Services im SharePoint-Modus erfordert Administratorrechte auf dem lokalen Computer. Wenn Sie erst SharePoint und dann Reporting Services installieren und das Setupkonto außerdem ein Mitglied der Administratorgruppe der SharePoint-Farm ist, wird der Reporting Services-Dienst von der Reporting Services-Installation für Sie registriert. Wenn Sie erst Reporting Services und dann SharePoint installieren bzw. wenn das Setupkonto kein Mitglied der Administratorgruppe der Farm ist, registrieren Sie den Dienst manuell. Weitere Informationen finden Sie im Abschnitt [Schritt 2: Registrieren und Starten des SharePoint-Diensts für Reporting Services](#bkmk_install_SSRS_sharedservice).  
  
 **Erstellen einer Reporting Services-Dienstanwendung**  
  
-   Nach der Installation und Registrierung des Reporting Services-Dienstes erstellen Sie mindestens eine Reporting Services-Dienstanwendung. Das „Dienstkonto der SharePoint-Farm“ muss vorübergehend Mitglied der lokalen Administratorgruppe sein, damit die Reporting Services-Dienstanwendung erstellt werden kann. Weitere Informationen zu SharePoint 2013-Kontoberechtigungen finden Sie unter [Kontoberechtigungen und Sicherheitseinstellungen in SharePoint 2013](/SharePoint/install/account-permissions-and-security-settings-in-sharepoint-server-2016) (https://technet.microsoft.com/library/cc678863.aspx) ). Zu SharePoint 2016 finden Sie weitere Informationen unter [Kontoberechtigungen und Sicherheitseinstellungen in SharePoint 2016](https://technet.microsoft.com/library/cc678863\(v=office.16\).aspx).  
  
     Eine bewährte Sicherheitsmethode besteht darin, die Administratorkonten der SharePoint-Farm nicht gleichzeitig als Administratorkonten des lokalen Betriebssystems festzulegen. Wenn Sie der lokalen Administratorgruppe während der Installation ein Farmadministratorkonto hinzufügen, wird empfohlen, das Konto nach Ende der Installation aus der lokalen Administratorgruppe zu entfernen.  
  
##  <a name="step-1-install-reporting-services-report-server-in-sharepoint-mode"></a><a name="bkmk_install_SSRS"></a> Schritt 1: Installieren eines Reporting Services-Berichtsservers im SharePoint-Modus

 Mit diesem Schritt installieren Sie Reporting Services im SharePoint-Modus und das Reporting Services-Add-In für SharePoint-Produkte. Abhängig von bereits auf dem Computer installierten Komponenten werden einige der in den folgenden Schritten beschriebenen Installationsseiten u. U. nicht angezeigt.  
 
 > [!IMPORTANT]
 > Für SharePoint 2016 muss der SharePoint-Server, auf dem Reporting Services installiert wird, eine **benutzerdefinierte** Serverrolle besitzen. Reporting Services lässt sich auf einem SharePoint-Server, der keine **benutzerdefinierte** Rolle besitzt, erfolgreich bereitstellen, aber während des nächsten SharePoint-Wartungsfensters hält MinRole den Reporting Services-Dienst an, da erkannt wird, dass Reporting Services im in SharePoint integrierten Modus keine Unterstützung für eine der anderen SharePoint-Serverrollen bietet. Die Reporting Services-Dienstanwendung unterstützt nur die **benutzerdefinierte** Rolle.
 
 > [!NOTE]
 > Wenn Sie planen, auch den Power Pivot-Dienst mit SharePoint 2016 zu verwenden, installieren Sie diesen Dienst, bevor Sie Reporting Services installieren. Der Power Pivot-Dienst kann ausschließlich mit der **benutzerdefinierten** Rolle auf einem SharePoint-Server installiert werden.
 
 ### <a name="apply-the-custom-server-role-to-a-sharepoint-2016-server"></a>Anwenden der benutzerdefinierten Serverrolle auf einen SharePoint 2016-Server
 
 > [!NOTE]
 > Dies gilt nicht für SharePoint 2013.
 
 1. Melden Sie sich bei dem SharePoint-Server an, auf dem Sie [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].
 
 2. Starten Sie die **SharePoint 2016-Verwaltungsshell** als Administrator. 
  
    Sie können mit der rechten Maustaste auf die **SharePoint 2016-Verwaltungsshell** klicken und **Als Administrator ausführen**auswählen.

3. Führen Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl aus.

    > [!NOTE]
    > Stellen Sie sicher, dass Sie den richtigen Namen für den SharePoint-Server angeben.
    
    ```powershell
    Set-SPServer SERVERNAME -Role Custom
    ```

4. In einer Antwort sollte angezeigt werden, dass ein Zeitgeberauftrag geplant wurde. Sie müssen warten, bis der Auftrag ausgeführt wurde.

5. Verwenden Sie den folgenden Befehl, um die dem Server zugewiesene Rolle zu überprüfen.

    ```powershell
    Get-SPServer SERVERNAME 
    ```
 
 6. Unter **Rolle** sollte **Benutzerdefiniert**aufgeführt sein.
 
 ### <a name="install-reporting-services"></a>Installieren von Reporting Services
  
1.  Führen Sie den Installations-Assistenten für SQL Server (Setup.exe) aus.  
  
2.  Wählen Sie links im Assistenten die Option **Installation** , und wählen Sie anschließend **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**aus.  

3.  Falls die Seite **Product Key** angezeigt wird, geben Sie Ihren Product Key ein, oder übernehmen Sie den Standardwert der Enterprise Evaluation Edition.  
  
     Wählen Sie **Weiter** aus.  
  
4.  Lesen Sie die Seite mit den Lizenzbedingungen, falls diese angezeigt wird, und akzeptieren Sie die Bedingungen. Microsoft ist Ihnen dankbar, wenn Sie damit einverstanden sind, Daten zur Funktionsverwendung zu senden, damit wir Produktfunktionen und -support verbessern können.  
  
     Wählen Sie **Weiter** aus.  

5.  Es wird empfohlen, die Option **Mit Microsoft Update nach Updates suchen (empfohlen)** auszuwählen. Diese Eingabe ist optional.
  
     Wählen Sie **Weiter** aus.   
  
6.  Je nachdem, welche Komponenten bereits auf dem Computer installiert sind, kann auf der Seite **Setupdateien installieren** folgende Meldung angezeigt werden:  
  
    -   „Für mindestens eine betroffene Datei stehen Vorgänge aus. Sie müssen den Computer nach Abschluss des Setupvorgangs neu starten.“  
  
    -   Wählen Sie **Weiter** aus.  
  
7.  Möglicherweise wird die Seite **Installationsregeln** angezeigt. Überprüfen Sie alle Warnungen oder Probleme, die eine Blockierung verursachen. Wählen Sie **Weiter**aus.
 
8. Wählen Sie Folgendes auf der Seite **Funktionsauswahl** aus:  
  
    -   **Reporting Services – SharePoint**  
  
    -   **Reporting Services-Add-In für SharePoint-Produkte**.  
  
    -   Optional können Sie auch **Database Engine Services** auswählen, um eine vollständige Umgebung zu installieren. Sie müssen allerdings über eine SQL Server-Datenbankmodulinstanz verfügen, auf der die SharePoint-Datenbanken gehostet werden.  
  
     Wählen Sie **Weiter** aus.  
  
     ![rs_SetupFeatureSelection_SharePoint_with_circles](../../reporting-services/install-windows/media/rs-setupfeatureselection-sharepoint-with-circles.png)
  
9. Wenn Sie Datenbank-Engine-Dienste ausgewählt haben, akzeptieren Sie die Standardinstanz von **MSSQLSERVER** auf der Seite **Instanzkonfiguration** , und klicken Sie auf **Weiter**.  
  
     ![Hinweis](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "Hinweis") Die Architektur des SharePoint-Diensts von Reporting Services basiert nicht auf einer SQL Server-„Instanz“, wie es bei der vorherigen Reporting Services-Architektur der Fall war.  
  
10. Wenn die Seite **Serverkonfiguration** angezeigt wird, geben Sie die entsprechenden Anmeldeinformationen ein. Wenn Sie die Reporting Services-Datenwarn- oder -Abonnementfunktionen verwenden möchten, müssen Sie den **Starttyp** für den SQL Server-Agent in **Automatisch** ändern. Abhängig von den bereits auf dem Computer installierten Komponenten wird die Seite **Serverkonfiguration** möglicherweise nicht angezeigt.  
  
     Wählen Sie **Weiter** aus.  
  
11. Wenn Sie die Database Engine Services ausgewählt haben, wird die Seite **Datenbankmodulkonfiguration** angezeigt. Fügen Sie der Liste der SQL-Administratoren entsprechende Konten hinzu, und klicken Sie dann auf **Weiter**.  
  
12. Auf der Seite **Reporting Services-Konfiguration** sollten Sie sehen, dass die Option **Nur Installieren** aktiviert ist. Mit dieser Option werden die Berichtsserverdateien installiert. Die SharePoint-Umgebung für Reporting Services wird damit nicht konfiguriert.  
  
    > [!NOTE]
    > Nachdem die SQL Server-Installation abgeschlossen wurde, befolgen Sie die Anweisungen in den weiteren Abschnitten dieses Themas, um die SharePoint-Umgebung zu konfigurieren. Dies schließt die Installation des gemeinsamen Reporting Services-Dienstes und das Erstellen der Reporting Services-Dienstanwendungen ein.  
  
     ![ssRS-2016-setup-configuration](../../reporting-services/install-windows/media/ssrs-2016-setup-configuration.png)
  
13. Überprüfen Sie eventuell angezeigte Warnungen, und klicken Sie dann auf der Seite **Funktionskonfigurationsregeln** auf **Weiter** , wenn Sie die Konfiguration auf diese Seite beenden möchten.  
  
14. Lesen Sie auf der Seite **Installationsbereit** die Installationszusammenfassung. Die Zusammenfassung enthält einen untergeordneten Knoten **SharePoint-Modus für Reporting Services** , für den der Wert **SharePointFilesOnlyMode**angezeigt wird. Wählen Sie **Installieren** aus.  
  
15. Die Installation dauert mehrere Minuten. Die Seite **Abgeschlossen** wird mit einer Liste der Funktionen und dem Status der einzelnen Funktionen angezeigt. Möglicherweise werden Sie in einem Informationsdialogfeld darauf hingewiesen, dass der Computer neu gestartet werden muss.  
  
##  <a name="step-2-register-and-start-the-reporting-services-sharepoint-service"></a><a name="bkmk_install_SSRS_sharedservice"></a> Schritt 2: Registrieren und Starten des SharePoint-Dienstes für Reporting Services  
 ![PowerShell-Inhalt](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell-Inhalt")  
  
> [!NOTE]
> Wenn Sie in eine vorhandene SharePoint-Farm installieren, müssen Sie die Schritte in diesem Abschnitt nicht ausführen. Der Reporting Services SharePoint-Dienst wurde installiert und gestartet, als Sie entsprechend den Anweisungen im vorherigen Abschnitt dieses Dokuments den SQL Server-Installations-Assistenten ausgeführt haben.  
  
 Aus den folgenden Gründen ist die manuelle Registrierung des Reporting Services-Dienstes erforderlich.  
  
-   Reporting Services wurde vor der Installation von SharePoint im SharePoint-Modus installiert.  
  
-   Das Konto, das zur Installation von Reporting Services im SharePoint-Modus verwendet wurde, war kein Member der Administratorgruppe der SharePoint-Farm. Weitere Informationen finden Sie im Abschnitt [Setup accounts](#bkmk_setupaccounts).  
  
 Die notwendigen Dateien wurden als Teil des SQL Server-Installations-Assistenten installiert, die Dienste müssen jedoch in der SharePoint-Farm registriert werden.  
  
 Die folgenden Schritte zeigen Ihnen, wie Sie die SharePoint-Verwaltungsshell öffnen und PowerShell-Cmdlets ausführen:  
  
1.  Klicken Sie auf die Schaltfläche **Start** .  
  
2.  Wählen Sie die Gruppe **Microsoft SharePoint 2016-Produkte** oder **Microsoft SharePoint 2013-Produkte** aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf **SharePoint 2016-Verwaltungsshell**bzw. **SharePoint 2013-Verwaltungsshell**, und wählen Sie **Als Administrator ausführen**aus. 

    > [!NOTE]
    > Die SharePoint-Befehle werden im Windows PowerShell-Standardfenster nicht erkannt. Verwenden Sie die **SharePoint-Verwaltungsshell**.  
  
4.  Führen Sie den folgenden PowerShell-Befehl aus, um den Reporting Services SharePoint-Dienst zu installieren. Ein erfolgreicher Abschluss des Befehls zeigt eine neue Zeile in der Verwaltungsshell an. Wurde der Befehl erfolgreich ausgeführt, wird**keine Meldung** an die Verwaltungsshell zurückgegeben:  
  
    ```  
    Install-SPRSService  
    ```  
  
5.  Führen Sie den folgenden PowerShell-Befehl aus, um den Reporting Services-Dienstproxy zu installieren. Ein erfolgreicher Abschluss des Befehls zeigt eine neue Zeile in der Verwaltungsshell an. Wurde der Befehl erfolgreich ausgeführt, wird**keine Meldung** an die Verwaltungsshell zurückgegeben:  
  
    ```  
    Install-SPRSServiceProxy  
    ```  
  
6.  Führen Sie den folgenden PowerShell-Befehl aus, um den Dienst zu starten, oder starten Sie den Dienst von der SharePoint-Zentraladministration anhand der unten stehenden Anweisungen:  
  
    ```  
    get-spserviceinstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
    ```  
  
    > [!IMPORTANT]
    > Angenommen, eine Fehlermeldung mit etwa folgendem Wortlaut wird ausgegeben:  
    >   
    ```powershell
    >     Install-SPRSService : The term 'Install-SPRSService' **is not recognized** as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.  
    ```
    >
    > Entweder befinden Sie sich in Windows PowerShell anstatt in der SharePoint-Verwaltungsshell, oder Reporting Services ist nicht im SharePoint-Modus installiert. Weitere Informationen zu Reporting Services und PowerShell finden Sie unter [PowerShell-Cmdlets für SharePoint-Modus von Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
 Sie können den Dienst auch von der SharePoint-Zentraladministration starten anstatt den dritten PowerShell-Befehl auszuführen. Die folgenden Schritte sind auch nützlich, um sicherzustellen, dass der Dienst gestartet wurde.  
  
1.  Klicken Sie in der SharePoint-Zentraladministration in der Gruppe **Systemeinstellungen** auf **Dienste auf dem Server verwalten** .  
  
2.  Suchen Sie den **SQL Server Reporting Services-Dienst** , und klicken Sie in der Spalte Aktion auf **Starten** .  
  
3.  Der Status des Reporting Services-Diensts ändert sich von **Beendet** auf **Gestartet**. Installieren Sie den Reporting Services-Dienst mit PowerShell, wenn sich dieser nicht in der Liste befindet.  
  
    > [!NOTE]  
    >  Wenn der Reporting Services-Dienst im Status **Wird gestartet** bleibt und sich nicht in **Gestartet** ändert, stellen Sie sicher, dass der Dienst der SharePoint 2013-Administration im Windows Server-Manager gestartet wurde.  
  
##  <a name="step-3-create-a-reporting-services-service-application"></a><a name="bkmk_create_serrviceapplication"></a>Schritt 3: Erstellen einer Reporting Services-Dienstanwendung  
 Dieser Abschnitt enthält die Schritte zum Erstellen einer Dienstanwendung und eine Beschreibung der Eigenschaften, wenn Sie eine vorhandene Dienstanwendung überprüfen.  
  
1.  Klicken Sie in der SharePoint-Zentraladministration in der Gruppe **Anwendungsverwaltung** auf **Dienstanwendungen verwalten**.  
  
2.  Klicken Sie im SharePoint-Menüband auf die Schaltfläche **Neu** .  
  
3.  Klicken Sie im Menü „Neu“ auf **SQL Server Reporting Services-Dienstanwendung**.  
  
    > [!IMPORTANT]  
    >  Wenn die Option „Reporting Services“ nicht in der Liste angezeigt wird, bedeutet dies, dass **der freigegebene Reporting Services-Dienst nicht installiert ist**. Lesen Sie den vorherigen Abschnitt, in dem das Installieren des Reporting Services-Dienstes mithilfe von PowerShell-Cmdlets beschrieben wird.  
  
4.  Geben Sie auf der Seite **SQL Server Reporting Services-Dienstanwendung erstellen** einen Namen für die Anwendung ein. Wenn Sie mehrere Reporting Services-Dienstanwendungen erstellen, hilft ein aussagekräftiger Name oder eine Benennungskonvention bei der Organisation von Administrations- und Verwaltungsvorgängen.  
  
5.  Erstellen Sie im Abschnitt **Anwendungspool** einen neuen Anwendungspool für die Anwendung (empfohlen). Die Verwendung des gleichen Namens für den Anwendungspool und die Dienstanwendung kann Ihre laufenden Verwaltungsaufgaben vereinfachen. Die Wahl des Namens kann allerdings auch davon abhängen, wie viele Dienstanwendungen Sie erstellen und ob mehrere Anwendungen in einem einzelnen Anwendungspool verwendet werden müssen. Informieren Sie sich in der SharePoint Server-Dokumentation über Empfehlungen und Best Practices zur Verwaltung von Anwendungspools.  
  
     Wählen Sie ein Sicherheitskonto für den Anwendungspool aus, oder erstellen Sie es. Sie müssen ein Domänenbenutzerkonto angeben. Ein Domänenbenutzerkonto ermöglicht die Verwendung der in SharePoint verfügbaren Funktion "Verwaltetes Konto", mit der Sie Kennwörter und Kontoinformationen zentral aktualisieren können. Domänenkonten sind auch erforderlich, wenn Sie beabsichtigen, die Bereitstellung auf zusätzlichen Dienstinstanzen, die unter der gleichen Identität ausgeführt werden, zu skalieren.  
  
6.  Im **Datenbankserver**können Sie den aktuellen Server verwenden oder einen anderen SQL Server auswählen.  
  
7.  Der Standardwert von **Datenbankname** ist `ReportingService_<guid>`. Dies ist ein eindeutiger Datenbankname. Wenn Sie einen neuen Wert eingeben, geben Sie einen eindeutigen Wert ein. Dies ist die neue Datenbank, die speziell für die Dienstanwendung erstellt werden soll.  
  
8.  Der Standardwert unter **Datenbankauthentifizierung**lautet Windows-Authentifizierung. Wenn Sie **SQL-Authentifizierung**auswählen, finden Sie in der SharePoint-Dokumentation bewährte Methoden zur Verwendung dieses Authentifizierungstyps in einer SharePoint-Bereitstellung.  
  
9. Wählen Sie im Abschnitt **Webanwendungszuordnungen** die Webanwendung aus, die für Zugriff von der aktuellen Reporting Services-Dienstanwendung bereitgestellt werden soll. Sie können einer Webanwendung eine Reporting Services-Dienstanwendung zuordnen. Wenn alle aktuellen Webanwendungen bereits einer Reporting Services-Dienstanwendung zugeordnet sind, wird eine Warnmeldung angezeigt.  
  
10. Klicken Sie auf **OK**.  
  
11. Der Dienstanwendungserstellungsprozess dauert möglicherweise mehrere Minuten. Wenn es vollständig ist, sehen Sie eine Bestätigungsmeldung und einen Link zu einer Seite **Abonnements und Warnungen bereitstellen** . Führen Sie den Bereitstellungsschritt aus, wenn Sie die Datenwarn- oder Abonnementfunktion von Reporting Services verwenden möchten. Weitere Informationen finden Sie unter [Bereitstellen von Abonnements und Warnungen für SSRS-Dienstanwendungen](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
 ![PowerShell-Inhalt](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell-Inhalt") Informationen zur Verwendung von PowerShell zum Erstellen einer Reporting Services-Dienstanwendung finden Sie unter folgenden Links:  
  
-   Weitere Informationen finden Sie im Abschnitt [Windows PowerShell-Skript für die Schritte 1 bis 4](#bkmk_full_script).  
  
-   Thema [So erstellen Sie eine Reporting Services-Dienstanwendung mit PowerShell](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)  

##  <a name="step-4-activate-the-power-view-site-collection-feature"></a><a name="bkmk_powerview"></a> Schritt 4: Aktivieren des Power View-Websitesammlungsfeatures

 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] ist eine Funktion des SQL Server 2016 Reporting Services-Add-Ins für [!INCLUDE[msCoName](../../includes/msconame-md.md)]-SharePoint-Produkte und stellt eine Websitesammlungsfunktion dar. Die Funktion wird für Stammwebsitesammlungen und Websitesammlungen automatisch aktiviert, die nach der Installation des Reporting Services-Add-Ins erstellt wurden. Wenn Sie die Verwendung von [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]planen, sollten Sie sicherstellen, dass die Funktion aktiviert ist.  
  
 Wenn Sie nach der Installation des SharePoint-Servers das Reporting Services-Add-In für SharePoint-Produkte installieren, werden die Berichtsserver-Integrationsfunktion und die Power View-Integrationsfunktion nur für Stammwebsitesammlungen aktiviert. Für andere Websitesammlungen müssen Sie die Funktionen manuell aktivieren.  
  
#### <a name="to-activate-or-verify-the-power-view-site-collection-feature"></a>So aktivieren oder überprüfen Sie die Power View-Websitesammlungsfunktion  
  
1.  Bei folgenden Schritten wird davon ausgegangen, dass die SharePoint-Website für eine Umgebung mit der **Benutzeroberflächenversion**2013 für SharePoint 2013 konfiguriert ist.  
  
     Öffnen Sie die gewünschte SharePoint-Website in Ihrem Browser. Beispiel: https://\<servername>/sites/bi.  
  
2.  Wählen Sie **Einstellungen**![SharePoint-Einstellungen](/analysis-services/analysis-services/media/as-sharepoint2013-settings-gear.gif "SharePoint-Einstellungen") aus.  
  
3.  Wählen Sie **Websiteeinstellungen**aus.  
  
4.  Wählen Sie in der Gruppe **Websitesammlungsverwaltung** die Option **Websitesammlungsfeatures**aus.  
  
5.  Suchen Sie **Power View-Integrationsfunktion** in der Liste.  
  
6.  Wählen Sie **Aktivieren**aus. Der Funktionsstatus ändert sich in **Aktiv**.  
  
 Diese Prozedur wird pro Websitesammlung abgeschlossen. Weitere Informationen finden Sie unter [Aktivieren der Berichtsserver- und Power View-Integrationsfunktionen in SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md).  
  
##  <a name="windows-powershell-script-for-steps-1-4"></a><a name="bkmk_full_script"></a> Windows PowerShell-Skript für die Schritte 1 bis 4  
 Das PowerShell-Skript in diesem Abschnitt entspricht dem Abschluss der Schritte 1 bis 4 in den vorangegangenen Schritten. Das Skript führt folgende Schritte aus:  
  
-   Der Reporting Services-Dienst und der -Dienstproxy werden installiert, und der Dienst wird gestartet.  
  
-   Ein neuer Proxy namens „Reporting Services“ wird erstellt.  
  
-   Eine Reporting Services-Dienstanwendung mit der Bezeichnung „Reporting Services Application“ (Reporting Services-Anwendung) wird erstellt.  
  
-   Die [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] -Funktion für eine Websitesammlung wird aktiviert.  
  
 Parameter  
  
-   Aktualisieren Sie den Parameter **-Account** für den Dienstproxy. Dabei muss es sich um ein verwaltetes Dienstkonto in der SharePoint-Farm handeln. Weitere Informationen finden Sie im SharePoint-Thema [Planen von Administrator- und Dienstkonten in SharePoint 2013](/SharePoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts).  
  
-   Aktualisieren Sie den **-DatabaseServer**-Parameter für die Dienstanwendung. Dieser Parameter entspricht der Instanz der Datenbank-Engine.  
  
-   Aktualisieren Sie den **-url**-Parameter der Website, für die die [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]-Funktion aktiviert werden soll.  
  
 **So verwenden Sie das Skript**  
  
1.  Öffnen Sie Windows PowerShell mit Administratorrechten.  
  
2.  Kopieren Sie folgenden Code in das Skriptfenster.  
  
3.  Aktualisieren Sie die drei im vorherigen Abschnitt beschriebenen Parameter, und führen Sie dann das Skript aus.  
  
```  
#This script Configures SQL Server Reporting Services SharePoint mode  
  
$starttime=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
  
Write-Host -ForegroundColor Green "Import the SharePoint PowerShell snappin"  
Add-PSSnapin Microsoft.Sharepoint.Powershell -EA 0  
  
Write-Host -ForegroundColor Green "Install SSRS Service and Service Proxy, and start the service"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
  
    Write-Host -ForegroundColor Green "Install the Reporting Services Shared Service"  
    Install-SPRSService  
  
    Write-Host -ForegroundColor Green " Install the Reporting Services Service Proxy"  
    Install-SPRSServiceProxy  
  
    # Get the ID of the RS Service Instance and start the service   
    Write-Host -ForegroundColor Green "Start the Reporting Services Service"  
    $RS = Get-SPServiceInstance | Where {$_.TypeName -eq "SQL Server Reporting Services Service"}  
    Start-SPServiceInstance -Identity $RS.Id.ToString()  
  
    # Wait for the Reporting Services Service to start...  
    $Status = Get-SPServiceInstance $RS.Id.ToString()  
    While ($Status.Status -ne "Online")  
    {  
        Write-Host -ForegroundColor Green "SSRS Service Not Online...Current Status = " $Status.Status  
        Start-Sleep -Seconds 2  
        $Status = Get-SPServiceInstance $RS.Id.ToString()  
    }  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Create a new application pool and Reporting Services service application"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
Write-Host -ForegroundColor Green "Create a new application pool"  
#!!!! update "-Account" with an existing Managed Service Account  
New-SPServiceApplicationPool -Name "Reporting Services" -Account "<domain>\User name>"  
$appPool = Get-SPServiceApplicationPool "Reporting Services"  
  
Write-Host -ForegroundColor Green " Create the Reporting Services Service Application"  
#!!!! Update "-DatabaseServer", an instance of the SQL Server database engine   
$rsService = New-SPRSServiceApplication -Name "Reporting Services Application" -ApplicationPool $appPool -DatabaseName "Reporting_Services_Application" -DatabaseServer "<server name>"  
  
Write-Host -ForegroundColor Green "Create the Reporting Services Service Application Proxy"  
$rsServiceProxy = New-SPRSServiceApplicationProxy -Name "Reporting Services Application Proxy" -ServiceApplication $rsService  
  
Write-Host -ForegroundColor Green "Associate service application proxy to default web site and grant web applications rights to SSRS application pool"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"     
# Associate the Reporting Services Service Applicatoin Proxy to the default web site...  
Get-SPServiceApplicationProxyGroup -default | Add-SPServiceApplicationProxyGroupMember -Member $rsServiceProxy  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Enable the PowerView and reportserver site features"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
#!!!! update "-url"  of the site where you want the features enabled  
Enable-SPfeature -identity "powerview" -Url https://server/sites/bi  
Enable-SPfeature -identity "reportserver" -Url https://server/sites/bi  
  
####To Verify, you can run the following:  
#Get-SPRSServiceApplication  
#Get-SPServiceApplicationPool | where {$_.name -like "reporting*"}  
#Get-SPRSServiceApplicationProxy  
  
```  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional_config"></a> Zusätzliche Konfiguration  
 In diesem Abschnitt werden zusätzliche Konfigurationsschritte beschrieben, die in den meisten SharePoint-Bereitstellungen wichtig sind.  
  
###  <a name="configure-excel-services-and-power-pivot"></a><a name="bkmk_configure_ECS"></a> Konfigurieren von Excel Services und PowerPivot  
 Wenn Sie Power View-Berichte von [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] in einer Excel 2016- oder Excel 2013-Arbeitsmappe in SharePoint anzeigen möchten, muss Excel Services für die Verwendung eines [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Servers im Power Pivot-Modus konfiguriert werden. 
 
 Für SharePoint 2016 muss ein [Office Online Server](https://technet.microsoft.com/library/jj219456\(v=office.16\).aspx) konfiguriert werden, um Excel Services verwenden zu können. Weitere Informationen finden Sie in den folgenden Whitepapers.
 
 - [Bereitstellen von SQL Server 2016 Power Pivot und Power View in SharePoint 2016](/analysis-services/instances/install-windows/deploying-sql-server-2016-powerpivot-and-power-view-in-sharepoint-2016)
 
 - [Bereitstellen von SQL Server 2016 Power Pivot und Power View in einer Multi-Tier SharePoint 2016-Farm](/analysis-services/instances/install-windows/deploy-powerpivot-and-power-view-multi-tier-sharepoint-2016-farm)
 
 Für SharePoint 2016 müssen Sie eine Excel Services-Anwendung erstellen und konfigurieren. Weitere Informationen finden Sie unter  
  
-   Abschnitt „Konfigurieren von Excel Services für die Analysis Services-Integration“ in [Installieren von Analysis Services im Power Pivot-Modus](/analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode).  
  
-   [Verwalten von Einstellungen für das Excel Services-Datenmodell (SharePoint Server 2013)](/SharePoint/administration/manage-excel-services-data-model-settings).  

Darüber hinaus muss das von der Reporting Services-Dienstanwendung verwendete Sicherheitskonto für den Anwendungspool auf dem Analysis Services-Server Administratorrechte aufweisen.
  
###  <a name="provision-subscriptions-and-alerts"></a><a name="bkmk_provision_agent"></a> Bereitstellen von Abonnements und Warnungen  
 Reporting Services-Abonnements und -Datenwarnungen verlangen möglicherweise die Konfiguration von SQL Server-Agent-Berechtigungen. Wenn eine Fehlermeldung darauf hinweist, dass SQL Server-Agent erforderlich ist, Sie jedoch sichergestellt haben, dass SQL Server-Agent gestartet wurde, müssen Sie die Berechtigungen aktualisieren. Sie können auf der Seite mit der Erfolgsmeldung nach der Dienstanwendungserstellung auf den Link **Abonnements und Warnmeldungen bereitstellen** klicken, um eine andere Seite für die SQL Server-Agent-Bereitstellung aufzurufen. Der Bereitstellungsschritt ist erforderlich, wenn die Bereitstellung über mehrere Computer erfolgt (wenn sich z. B. die SQL Server-Datenbankinstanz auf einem anderen Computer befindet). Weitere Informationen finden Sie unter [Bereitstellen von Abonnements und Warnungen für SSRS-Dienstanwendungen](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
### <a name="configure-e-mail-for-ssrs-service-applications"></a>Konfigurieren von E-Mail-Einstellungen für SSRS-Dienstanwendungen  
 Die Reporting Services-Datenwarnungsfunktion sendet Warnungen in E-Mail-Nachrichten. Um E-Mails übermitteln zu können, müssen Sie unter Umständen Ihre Reporting Services-Dienstanwendung konfigurieren und die E-Mail-Übermittlungserweiterung für die Dienstanwendung ändern. Die E-Mail-Einstellungen sind erforderlich, wenn Sie beabsichtigen, die E-Mail-Übermittlungserweiterung für die Reporting Services-Abonnementfunktion zu verwenden. Weitere Informationen finden Sie unter [Configure E-mail for a Reporting Services Service Application (SharePoint 2013 and SharePoint 2016) (Konfigurieren von E-Mails für eine Reporting Services-Dienstanwendung (SharePoint 2013 und SharePoint 2016))](./configure-e-mail-for-a-reporting-services-service-application.md). 
  
### <a name="add-reporting-services-content-types-to-content-libraries"></a>Hinzufügen von Reporting Services-Inhaltstypen zu Inhaltsbibliotheken  
 Reporting Services stellt vordefinierte Inhaltstypen bereit, die zum Verwalten freigegebener Datenquellendateien (RSDS) und Berichts-Generator-Berichtsdefinitionsdateien (RDL) verwendet werden können. Wenn einer Bibliothek ein Inhaltstyp (**Berichts-Generator-Bericht** und **Berichtsdatenquelle**) hinzugefügt wird, wird der Befehl **Neu** aktiviert, sodass Sie neue Dokumente des betreffenden Typs erstellen können. Weitere Informationen finden Sie unter [Hinzufügen von Reporting Services-Inhaltstypen zu einer SharePoint-Bibliothek](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="activate-the-report-server-file-sync-feature"></a>Aktivieren des Features zur Berichtsserver-Dateisynchronisierung  
 Wenn Benutzer häufig veröffentlichte Berichtselemente direkt in SharePoint-Dokumentbibliotheken hochladen, ist das Feature zur **Berichtsserver-Dateisynchronisierung** auf Websiteebene hilfreich. Die Dateisynchronisierungsfunktion synchronisiert den Berichtsserverkatalog regelmäßiger mit Elementen in Dokumentbibliotheken. Weitere Informationen finden Sie unter [Aktivieren des Features zur Berichtsserver-Dateisynchronisierung in der SharePoint-Zentraladministration](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).  
  
##  <a name="verify-the-installation"></a><a name="bkmk_verify_installation"></a> Überprüfen der Installation  
 Im Folgenden werden Schritte und Verfahren vorgeschlagen, um die Reporting Services-Bereitstellung im SharePoint-Modus zu überprüfen.  
  
-   Weitere Informationen finden Sie im SharePoint-Abschnitt im Thema zur Bereitstellung [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
-   Erstellen Sie in einer SharePoint-Dokumentbibliothek einen einfachen Reporting Services-Bericht, der nur ein Textfeld enthält, z.B. einen Titel. Der Bericht enthält keine Datenquellen oder Datasets. Ziel ist es, zu überprüfen, ob Sie den Berichts-Generator öffnen und einen grundlegenden Bericht erstellen und in der Vorschau anzeigen können.  
  
     Speichern Sie den Bericht in der Dokumentbibliothek, und führen Sie den Bericht dann aus der Bibliothek heraus aus. Weitere Informationen zum Erstellen von Berichten mit dem Berichts-Generator finden Sie unter [Starten des Berichts-Generators (Berichts-Generator)](../report-builder/start-report-builder.md).  
  
## <a name="next-steps"></a>Nächste Schritte

[PowerShell-Cmdlets für SharePoint-Modus von Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)   
[Aktualisieren und Migrieren von Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
[Reporting Services-SharePoint-Dienst und -Dienstanwendungen](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)