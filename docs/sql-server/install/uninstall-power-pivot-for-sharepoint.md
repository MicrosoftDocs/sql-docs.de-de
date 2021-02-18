---
title: Deinstallieren von Power Pivot für SharePoint | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie eine Power Pivot für SharePoint-Installation deinstallieren. Dabei handelt es sich um einen aus mehreren Schritten bestehenden Vorgang.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 3941a2f0-0d0c-4d1a-8618-7a6a7751beac
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 375f351c1d80e957fba29f81b9d2afc423c222ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336094"
---
# <a name="uninstall-power-pivot-for-sharepoint"></a>Deinstallieren von Power Pivot für SharePoint
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Die Deinstallation einer [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] -Installation ist ein Vorgang mit mehreren Schritten, der die Vorbereitung auf die Deinstallation, das Entfernen von Funktionen und Lösungen aus der Farm und das Entfernen von Programmdateien und Registrierungseinstellungen umfasst.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2013 | SharePoint 2010  
  
 **In diesem Artikel:**  
  
-   [Voraussetzungen](#prereq)  
  
-   [Schritt 1: Prüfliste vor der Deinstallation](#bkmk_before)  
  
-   [Schritt 2: Entfernen von Funktionen und Projektmappen aus SharePoint](#bkmk_remove)  
  
-   [Schritt 3: Ausführen des SQL Server-Setups zum Entfernen von Programmen vom lokalen Computer](#bkmk_uninstall)  
  
-   [Schritt 4: Deinstallieren des Power Pivot für SharePoint-Add-Ins](#bkmk_addin)  
  
-   [Schritt 5: Überprüfen der Deinstallation](#verify)  
  
-   [Schritt 6: Prüfliste nach der Deinstallation](#bkmk_post)  
  
##  <a name="prerequisites"></a><a name="prereq"></a> Voraussetzungen  
  
-   Sie müssen ein SharePoint-Farmadministrator oder ein Dienstanwendungsadministrator sein, um Funktionen und Lösungen in der Farm zu deinstallieren.  
  
-   Sie müssen SQL Server-Systemadministrator und Mitglied der lokalen Administratorengruppe sein, wenn Sie auch die Datenbank-Engine deinstallieren.  
  
-   Sie müssen Analysis Services-Systemadministrator und Mitglied der lokalen Administratorengruppe sein, um Analysis Services und [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]zu deinstallieren.  
  
##  <a name="step-1-pre-uninstall-checklist"></a><a name="bkmk_before"></a>Schritt 1: Prüfliste vor der Deinstallation  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Der Power Pivot-Datenzugriff wird deaktiviert, sobald die Software, die die Abfrage- und -Datenverarbeitung unterstützt, aus der Farm entfernt wird. Als ersten Schritt sollten Sie die Dateien und Bibliotheken, die nicht mehr funktionsfähig sein werden, präventiv löschen. Auf diese Weise können Sie Fragen oder Bedenken hinsichtlich „fehlender Daten“ bereits vor der Deinstallation der Software klären.  
  
1.  Löschen Sie alle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappen, -Dokumente und -Bibliotheken, die mit einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint-Installation verknüpft sind. Weder die Bibliotheken noch die Dokumente sind nach der Deinstallation der Software funktionsfähig.  
  
    -   [Löschen eines Power Pivot-Katalogs](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Löschen einer Power Pivot-Datenfeedbibliothek](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
2.  Löschen Sie Excel-Arbeitsmappen oder Reporting Services-Berichte in den anderen Bibliotheken, die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Daten enthalten oder darauf verweisen.  
  
3.  Entfernen Sie jedes Webpart in einem PerformancePoint-Dashboard, das auf [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Daten verweist.  
  
4.  Prüfen Sie die SharePoint-Berechtigungen für vorhandene Websites und Bibliotheken, um zu ermitteln, ob sie geändert werden müssen. Für einige [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Datenzugriffsszenarien sind Leseberechtigungen erforderlich, insbesondere für den sekundären Datenzugriff auf [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Daten in einer anderen Arbeitsmappe über eine URL-Verbindungszeichenfolge. Dies ist eine höhere Berechtigungsstufe als die Anzeigeberechtigungen, die SharePoint-Benutzern normalerweise zugeordnet werden, die nur eine Website besuchen. Wenn Sie keine Leseberechtigungen mehr benötigen, können Sie die Berechtigungen entsprechend herabstufen.  
  
5.  Optional können Sie die Dienste beenden und mehrere Tage warten, bevor Sie die Software deinstallieren. Dieser Schritt ist zur Deinstallation zwar nicht erforderlich, bietet aber die Möglichkeit, den Dienst temporär fortzusetzen. Dies ist nützlich, wenn Sie an Problemlösungen in Bezug auf die Datenmigration oder den Technologieaustausch arbeiten, die Sie ggf. übersehen haben.  
  
##  <a name="step-2-remove-features-and-solutions-from-sharepoint"></a><a name="bkmk_remove"></a>Schritt 2: Entfernen von Funktionen und Projektmappen aus SharePoint  
 Verwenden Sie das [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Konfigurationstool, um [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Dienste und -Anwendungen aus SharePoint zu entfernen.  
  
-   Sie müssen ein Farmadministrator, ein Serveradministrator auf der Analysis Services-Instanz und Mitglied der Rolle **db_owner** der Konfigurationsdatenbank der Farm sein.  
  
-   Verwenden Sie die für die genutzte SharePoint-Version geeignete Version des Konfigurationstools. Verwenden Sie keines der beiden Tools mit Installationen von [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
-   Überprüfen Sie, ob der SharePoint-Verwaltungsdienst ausgeführt wird.  
  
1.  **Ausführen des Konfigurationstools:** Beachten Sie, dass die Konfigurationstools nur dann aufgeführt werden, wenn [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] auf dem lokalen Server installiert ist. Zeigen Sie im Menü **Start** auf **Alle Programme**, klicken Sie auf [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], auf **Konfigurationstools** und dann auf eine der folgenden Optionen:  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Konfiguration von Power Pivot für SharePoint 2013**  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Konfigurationstool**  
  
2.  Wählen Sie **Funktionen, Dienste, Anwendungen und Lösungen entfernen** aus, und klicken Sie dann auf **OK**.  
  
3.  Erweitern Sie optional das Fenster auf Vollbildgröße. Am unteren Rand des Fensters wird eine Schaltflächenleiste angezeigt, die die Befehle **Überprüfen**, **Ausführen** und **Beenden** enthält.  
  
4.  Überprüfen Sie alle Aktionen in der Taskliste, um nachzuvollziehen, was die einzelnen Aktionen bewirken.  
  
     Unter **Entfernen [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Service-Anwendungen entfernen** haben Sie die Option, die der Dienstanwendung zugeordneten Anwendungsdaten zu löschen. Die Anwendungsdaten bilden eine SQL Server-Datenbank, die mit der Dienstanwendung zum Speichern von Zeitplänen zur Datenaktualisierung, Informationen zur Datenbankinstanz, Verwendungsdaten und anderen von [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint verwendeten Daten erstellt wurde. Benutzerdateien, wie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappen, werden nicht gespeichert. Wenn Sie die Anwendungsdaten nicht aus einem bestimmten Grund speichern müssen (z. B. wenn auf Datenaktualisierung oder Datenzugriff bezogene Datenbeibehaltungsrichtlinien angewendet werden), können Sie die Anwendungsdatenbank löschen, ohne Dateien zu entfernen, die von SharePoint-Benutzern erstellt oder gespeichert wurden.  
  
     Wählen Sie zum Löschen der Datenbank **Entfernen [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Service-Anwendungen entfernen** aus, und wählen Sie dann die Option **Löschen der dieser Dienstanwendung zugeordneten Anwendungsdaten** zu deinstallieren.  
  
5.  Überprüfen Sie optional ausführliche Informationen auf der Registerkarte **Ausgabe** oder auf der Registerkarte **Skript** .  
  
     Die Registerkarte "Ausgabe" ist eine Zusammenfassung der Aktionen, die vom Tool ausgeführt werden. Diese Informationen werden unter folgendem Protokolldateipfad gespeichert:  
  
     `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`  
  
     Die Registerkarte "Skript" enthält die PowerShell-Cmdlets oder verweist auf die PowerShell-Skriptdateien, die vom Tool ausgeführt werden.  
  
6.  Klicken Sie auf **Überprüfen** , um zu überprüfen, ob jede Aktion gültig ist. Wenn **Überprüfen** nicht verfügbar ist, bedeutet das, dass alle Aktionen für das System gültig sind.  
  
7.  Klicken Sie auf **Ausführen** , um alle Aktionen auszuführen, die für diesen Task gültig sind. **Ausführen** ist nur verfügbar, nachdem die Überprüfung erfolgreich war. Wenn Sie auf **Ausführen** klicken, wird die folgende Warnung angezeigt und erinnert Sie, dass Aktionen im Batchmodus verarbeitet werden: „All of the configuration settings that are flagged as valid in the tool will be applied to the SharePoint farm. Möchten Sie den Vorgang fortsetzen?“  
  
8.  Klicken Sie zum Fortsetzen des Vorgangs auf **Ja** .  
  
 **Problembehandlung:**  
  
 Im Konfigurationstool können Sie Fehlerinformationen für jede Aktion im Bereich Parameter anzeigen. Überprüfen Sie bei Problemen mit der Bereitstellung bzw. Aufheben der Bereitstellung, ob der SharePoint-Administrationsdienst gestartet wurde. Dieser Dienst führt die Zeitgeberaufträge aus, die Konfigurationsänderungen in einer Farm auslösen. Wird der Dienst nicht ausgeführt, tritt beim Bereitstellen oder Zurückziehen der Lösung ein Fehler auf. Persistente Fehler weisen darauf hin, dass sich bereits ein Auftrag zum Bereitstellen oder Zurückziehen in der Warteschlange befindet und weitere Aktionen des Konfigurationstools blockiert. Anhand des folgenden PowerShell-Befehls können Sie überprüfen, ob der Dienst ausgeführt wird.  
  
```  
Get-Service | where {$_.displayname -like "*sharepoint* administration*"}  
```  
  
 Gehen Sie folgendermaßen vor, um einen Auftrag zum Bereitstellen oder Zurückziehen, der sich bereits in der Warteschlange befindet, zu suchen und zu entfernen:  
  
1.  Überprüfen Sie bei allen anderen Fehlern die ULS-Protokolle. Weitere Informationen finden Sie unter [Konfigurieren und Anzeigen der SharePoint-Protokolldateien und -Diagnoseprotokollierung &#40;Power Pivot für SharePoint&#41;](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging).  
  
2.  Starten Sie die SharePoint-Verwaltungsshell als Administrator, und führen Sie dann den folgenden Befehl aus, um Aufträge in der Warteschlange anzuzeigen:  
  
    ```  
    Stsadm -o enumdeployments  
    ```  
  
3.  Überprüfen Sie vorhandene Bereitstellungen auf die folgenden Informationen: **Typ** ist „Zurückziehung“ oder „Bereitstellung“, **Datei** ist „powerpivotwebapp.wsp“ oder „powerpivotfarm.wsp“.  
  
4.  Kopieren Sie bei Bereitstellungen oder Zurückziehungen von [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Lösungen den GUID-Wert für **JobId**, und fügen Sie ihn in den folgenden Befehl ein (verwenden Sie zum Kopieren der GUID die Befehle zum Markieren, Kopieren und Einfügen im Bearbeitungsmenü der Shell):  
  
    ```  
    Stsadm -o canceldeployment -id "<GUID>"  
    ```  
  
5.  Wiederholen Sie den Task im Konfigurationstool, indem Sie auf **Überprüfen** und **Ausführen** klicken.  
  
 Sie können alternativ Funktionen und Lösungen von der Farm mithilfe von PowerShell entfernen. Weitere Informationen finden Sie unter [PowerShell-Referenz für PowerPivot für SharePoint](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint).  
  
##  <a name="step-3-run-sql-server-setup-to-remove-programs-from-the-local-computer"></a><a name="bkmk_uninstall"></a>Schritt 3: Ausführen des SQL Server-Setups zum Entfernen von Programmen vom lokalen Computer  
 Programmdateien zu löschen erfordert, dass Sie SQL Server-Setup ausführen, um die Software zu deinstallieren. Durch das Deinstallieren werden die Dateien und die von Setup erstellten Registrierungseinträge entfernt. Sie können die Seite Programme und Funktionen verwenden, um die Software zu deinstallieren. Eine Installation von [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] ist Teil einer SQL Server-Installation.  
  
 Sie können einen Teil einer Installation deinstallieren, ohne dabei andere SQL Server-Instanzen (oder Funktionen in der gleichen Instanz) zu beeinflussen, die bereits installiert sind. Sie können z.B. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint deinstallieren, aber andere Komponenten wie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] oder die Datenbank-Engine, installiert lassen.  
  
1.  Wählen Sie **Microsoft SQL Server 2014 (64-Bit)** aus der Programmliste aus.  
  
2.  Klicken Sie auf **Deinstallieren/ändern**.  
  
3.  Klicken Sie auf **Entfernen**. Dies startet SQL Server-Setup.  
  
     Im Setup können Sie die **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** -Instanz und anschließend **Analysis Services** und **Analysis Services-SharePoint-Integration** auswählen, um nur diese Funktion zu entfernen und alle anderen Funktionen unverändert zu lassen.  
  
##  <a name="step-4-uninstall-the-power-pivot-for-sharepoint-add-in"></a><a name="bkmk_addin"></a> Schritt 4: Deinstallieren des Power Pivot für SharePoint-Add-Ins  
 Wenn Ihre [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] -Bereitstellung zwei oder mehr Server umfasst und Sie das [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] -Add-In installiert haben, deinstallieren Sie das [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] -Add-In von jedem Server, auf dem es installiert wurde, um alle [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] -Dateien vollständig zu deinstallieren. Weitere Informationen finden Sie unter [Installieren oder Deinstallieren des PowerPivot für SharePoint-Add-In &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013).  
  
##  <a name="step-5-verify-uninstall"></a><a name="verify"></a> Schritt 5: Überprüfen der Deinstallation  
  
1.  Stellen Sie in der Zentraladministration unter **Dienste auf dem Server verwalten** eine Verbindung mit dem Server her, von dem aus Sie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint deinstalliert haben.  
  
2.  -   Wenn Sie [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013 deinstalliert haben, vergewissern Sie sich, dass **SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Systemdienst** nicht mehr in der Liste aufgeführt wird.  
  
    -   Wenn Sie [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010 deinstalliert haben, vergewissern Sie sich, dass **SQL Server Analysis Services** und **SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Systemdienst** nicht mehr in der Liste aufgeführt werden.  
  
3.  Nachdem Sie den letzten [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint-Server in der Farm deinstalliert haben, gehen Sie wie folgt vor:  
  
    1.  Überprüfen Sie in der Anwendungsverwaltung unter **Dienstanwendungen verwalten**, ob die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Dienstanwendung nicht mehr in der Liste angezeigt wird.  
  
    2.  Überprüfen Sie in den Systemeinstellungen, unter **Farmfunktionen verwalten**, ob die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Integrationsfunktion nicht mehr auf der Seite angezeigt wird. Überprüfen Sie unter **Farmlösungen verwalten**, ob die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Lösungen nicht mehr auf der Seite angezeigt werden.  
  
    3.  Überprüfen Sie unter „Überwachung“, in **Diagnoseprotokollierung konfigurieren** und **Verwendungs- und Integritätsdatenerfassung konfigurieren**, ob [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Ereignisse und Ereigniskategorien nicht mehr angezeigt werden.  
  
    4.  Stellen Sie in „Allgemeine Anwendungseinstellungen“ sicher, dass **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Management-Dashboard** nicht mehr auf der Seite angezeigt wird.  
  
##  <a name="step-6-post-uninstall-checklist"></a><a name="bkmk_post"></a> Schritt 6: Prüfliste nach der Deinstallation  
 Verwenden Sie die folgende Liste, um Software und Dateien zu entfernen, die bei der Deinstallation nicht gelöscht wurden.  
  
1.  Löschen Sie alle Datendateien und Unterordner aus `C:\Program Files\Microsoft SQL Server\MSAS13.PowerPivot`, und löschen Sie dann den Ordner. Bei diesem Schritt werden auch zuvor zwischengespeicherte Dateien im DATA-Verzeichnis gelöscht.  
  
2.  Löschen Sie alle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappen, -Dokumente und -Bibliotheken, falls noch nicht geschehen.  
  
    -   [Löschen eines Power Pivot-Katalogs](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Löschen einer Power Pivot-Datenfeedbibliothek](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
3.  Löschen Sie in „Secure Store Service“ alle Zielanwendungen, die die von [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint verwendeten gespeicherten Anmeldeinformationen enthalten. Einige (aber nicht alle) Einträge in „Secure Store Service“ werden gelöscht, wenn [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint deinstalliert wird. Zielanwendungen, die für das unbeaufsichtigte Datenaktualisierungskonto für [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] erstellt wurden, sowie alle für die Datenaktualisierung erstellten Zielanwendungen sind noch vorhanden und müssen manuell gelöscht werden.  
  
     Im Gegensatz dazu werden einzelne Zielanwendungen, die vom [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Systemdienst automatisch generiert wurden, bei der Deinstallation von [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] automatisch gelöscht.  
  
4.  Klicken Sie in der Systemsteuerung auf **Programme** und dann auf **Programm deinstallieren** Deinstallieren Sie alle Analysis Services-Clientbibliotheken, die nicht mehr verwendet werden. Analysis Services-ADOMD.NET und Microsoft SQL Servers Analysis Management Objects werden bei der Deinstallation von [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint nicht entfernt. Da die Bibliotheken möglicherweise von anderen Programmen verwendet werden, die Analysis Services-Daten verwenden, werden diese beim SQL Server-Setup nicht automatisch deinstalliert. Sie müssen die Clientbibliotheken einzeln deinstallieren, wenn Sie sie nicht mehr benötigen.  
  
     Deinstallieren Sie das SQL Server Reporting Services SharePoint 2010-Add-In, sofern Sie keine Problembehandlungs- oder Installationsanweisungen befolgen und darin zur Deinstallation aufgefordert werden. Das Reporting Services-Add-In wird von Access Services verwendet. Es wird mit dem SharePoint Products-Produktvorbereitungstool installiert und sollte im System verbleiben, um die für SharePoint erforderlichen Funktionen zu unterstützen.  
  
     Deinstallieren Sie nicht den Analysis Services OLE DB-Anbieter. Bei der SharePoint-Installation wird der OLE DB-Anbieter als Voraussetzung für Excel-Arbeitsmappen installiert, die eine Verbindung mit Analysis Services-Datenbanken herstellen. [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] wird eine neuere Version installiert. Diese Version ist jedoch abwärts kompatibel und sollte daher im System bleiben, um spätere Datenverbindungsprobleme zu vermeiden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren oder Deinstallieren des PowerPivot für SharePoint-Add-Ins &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Power Pivot-Konfigurationstools](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
