---
title: 'Upgrade: Installations-Assistent (Setup)'
description: Der SQL Server-Installations-Assistent enthält eine Featurestruktur, die ein direktes Upgrade von SQL Server-Komponenten auf die neueste Version von SQL Server ermöglicht.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- upgrading Database Engine
- Database Engine [SQL Server], upgrading
ms.assetid: cef118a5-a7ce-4bfa-8b9d-c81996284cfc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a5621b9444d56307d21855c9b07aefe736d382d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353851"
---
# <a name="upgrade-sql-server-using-the-installation-wizard-setup"></a>Upgrade von SQL Server mithilfe des Installations-Assistenten (Setup)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installations-Assistent enthält eine Funktionsstruktur zum direkten Upgrade von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten auf die neueste Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>[!WARNING]  
>Wenn Sie auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisieren, wird die frühere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überschrieben und ist auf dem Computer folglich nicht mehr vorhanden. 
>
>Sichern Sie vor dem Upgrade die SQL Server-Datenbanken und alle anderen der früheren SQL Server-Instanz zugeordneten Objekte.  
  
> [!CAUTION]  
> Für viele Produktions- und einige Entwicklungsumgebungen eignet sich ein Upgrade durch Neuinstallation oder ein paralleles Update besser als ein direktes Upgrade.  Weitere Informationen zu Upgrade-Methoden finden Sie unter:
> * [Wählen einer Upgrademethode für die Datenbank-Engine](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)
> * [Aktualisieren von Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)
> * [Upgrade von Integration Services](../../integration-services/install-windows/upgrade-integration-services.md)
> * [Aktualisieren von Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md)
> * [Aktualisieren und Migrieren von Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)
> * [Aktualisieren von Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md)
> * [Upgrade von PowerPivot für SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)  
  
## <a name="prerequisites"></a>Voraussetzungen  
Sie müssen Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat und ein lokaler Administrator ist.  
  
> [!WARNING]  
>  Sie können die zu aktualisierenden Funktionen nicht ändern und während des Aktualisierungsvorgangs keine Funktionen hinzufügen. Wenn Sie einer aktualisierten Instanz von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Funktionen hinzufügen möchten, nachdem der Upgradevorgang abgeschlossen wurde, finden Sie entsprechende Informationen unter [Add Features to an Instance of SQL Server (Setup) (Hinzufügen von Funktionen zu einer Instanz von SQL Server (Setup))](./add-features-to-an-instance-of-sql-server-setup.md).  
  
 Wenn Sie ein Upgrade von [!INCLUDE[ssDE](../../includes/ssde-md.md)]durchführen, lesen Sie zunächst [Planen und Testen des Upgradeplans für die Datenbank-Engine](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md) und führen Sie dann die folgenden Aufgaben je nach Umgebung durch:  
  
-   Sichern Sie alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankdateien der zu aktualisierenden Instanz, damit diese bei Bedarf wiederhergestellt werden können.  
  
-   Führen Sie die entsprechenden Datenbank-Konsolenbefehle (Database Console Commands, DBCC) für die zu aktualisierenden Datenbanken aus, um deren Konsistenz sicherzustellen.  
  
-   Schätzen Sie ab, wie viel Speicherplatz für das Upgrade von SQL Server-Komponenten sowie der Benutzerdatenbanken erforderlich ist. Informationen zum erforderlichen Speicherplatz für SQL Server-Komponenten finden Sie unter [Hardware and Software Requirements for Installing SQL Server (Hardware- und Softwareanforderungen für die Installation von SQL Server)](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Stellen Sie sicher, dass für vorhandene SQL Server-Systemdatenbanken – master, model, msdb und tempdb – die automatische Vergrößerung konfiguriert ist, und stellen Sie sicher, dass ausreichend Festplattenspeicherplatz verfügbar ist.  
  
-   Stellen Sie sicher, dass alle Datenbankserver über Anmeldeinformationen in der master-Datenbank verfügen. Dies ist wichtig für das Wiederherstellen einer Datenbank, da sich die Systemanmeldeinformationen in der master-Datenbank befinden.  
  
-   Deaktivieren Sie alle gespeicherten Startprozeduren, da im Rahmen des Upgradeprozesses auch Dienste auf der zu aktualisierenden SQL Server-Instanz beendet und gestartet werden. Der Upgradeprozess kann durch beim Start verarbeitete gespeicherte Prozeduren blockiert werden.  
  
-   Wenn Sie Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktualisieren, wo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent in MSX/TSX-Beziehungen eingetragen wird, aktualisieren Sie Zielserver vor den Masterservern. Wenn Sie Masterserver vor den Zielservern aktualisieren, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent keine Verbindung mit den Masterinstanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]herstellen.  
  
-   Beenden Sie alle Anwendungen sowie alle Dienste mit SQL Server-Abhängigkeiten. Wenn Verbindungen von lokalen Anwendungen mit der zu aktualisierenden Instanz bestehen, kann das Upgrade einen Fehler erzeugen.  
  
-   Stellen Sie sicher, dass die Replikation aktuell ist, und beenden Sie die Replikation.   
    Ausführliche Schritte zum Ausführen eines parallelen Upgrades in einer replizierten Umgebung finden Sie unter [Upgrade Replicated Databases (Aktualisieren von replizierten Datenbanken)](../../database-engine/install-windows/upgrade-replicated-databases.md).
  
## <a name="procedure"></a>Verfahren  
  
### <a name="to-upgrade-ssnoversion"></a>So führen Sie das Upgrade auf [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] aus  
  
1.  Legen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedium ein, und doppelklicken Sie im Stammordner auf Setup.exe. Wenn Sie eine Installation über eine Netzwerkfreigabe ausführen möchten, wechseln Sie in der Freigabe zum Stammordner, und doppelklicken Sie auf Setup.exe.  
  
2.  Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationscenter wird vom Installations-Assistenten ausgeführt. Um eine vorhandene Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu aktualisieren, klicken Sie im linken Navigationsbereich auf **Installation** und dann auf **Upgrade von...** vorherigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Aktivieren Sie auf der Seite für den Product Key eine Option, um anzugeben, ob Sie auf eine kostenlose Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisieren oder ob Sie über einen PID-Schlüssel für eine Produktionsversion des Produkts verfügen. Weitere Informationen finden Sie unter [Editionen und Komponenten von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) und unter [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
4.  Lesen Sie auf der Seite Lizenzbedingungen den Lizenzvertrag, und aktivieren Sie das Kontrollkästchen **Ich akzeptiere die Lizenzbedingungen** , wenn Sie diesen zustimmen. Klicken Sie anschließend auf **Weiter**. Falls Sie zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beitragen möchten, können Sie auch die Option zur Funktionsverwendung aktivieren und Berichte an [!INCLUDE[msCoName](../../includes/msconame-md.md)]senden.  
  
5.  Im Fenster Globale Regeln wechselt Setup automatisch weiter zum Fenster Produktupdates, sofern keine Regelfehler auftreten.  
  
6.  Die Seite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update wird als Nächstes angezeigt, wenn das Kontrollkästchen [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update in Systemsteuerung\Alle Systemsteuerungselemente\Windows Update\Einstellungen ändern nicht aktiviert ist. Wenn Sie die Seite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update mit einem Häkchen markieren, ändern sich die Computereinstellungen, und beim Suchen nach Windows Update werden die neuesten Updates angezeigt.  
  
7.  Auf der Seite für Produktupdates werden die neuesten verfügbaren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Produktupdates angezeigt. Wenn Sie die Updates nicht einschließen möchten, deaktivieren Sie das Kontrollkästchen **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produktupdates einschließen**. Wenn keine Produktupdates ermittelt wurden, zeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup diese Seite nicht an und geht automatisch zur Seite **Setupdateien installieren** über.  
  
8.  Auf der Seite "Setupdateien installieren" wird der Status angezeigt, während die Setupdateien heruntergeladen, extrahiert und installiert werden. Wenn ein Update für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup gefunden und angegeben wird, dass das Update eingeschlossen werden soll, wird dieses Update ebenfalls installiert.  
  
9. Im Fenster Upgradeegeln wechselt Setup automatisch weiter zum Fenster Instanz auswählen, sofern keine Regelfehler auftreten.  
  
10. Geben Sie auf der Seite Instanz auswählen die zu aktualisierende Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an. Zum Aktualisieren der Verwaltungstools und freigegebenen Funktionen wählen Sie **Nur freigegebene Funktionen aktualisieren**.  
  
11. Auf der Seite Funktionen auswählen sind die Funktionen, die aktualisiert werden sollen, bereits markiert. Nach Auswahl des Funktionsnamens wird im rechten Bereich eine Beschreibung für die einzelnen Komponentengruppen angezeigt.  
  
     Die erforderlichen Komponenten für die ausgewählten Funktionen werden im rechten Bereich angezeigt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert die erforderlichen Komponenten, die nicht bereits während des Installationsschritts installiert werden, der im weiteren Verlauf dieser Prozedur beschrieben wird.  
  
    > [!NOTE]  
    >  Wenn Sie sich für das Upgrade der freigegebenen Features entschieden haben, indem Sie **\<Upgrade shared features only>** auf der Seite **Instanz auswählen** ausgewählt haben, sind auf der Seite „Featureauswahl“ alle freigegebenen Features vorab ausgewählt. Alle freigegebenen Komponenten werden gleichzeitig aktualisiert.  
  
12. Geben Sie auf der Instanzkonfigurationsseite die Instanz-ID für die Instanz [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]an.  
  
     **Instanz-ID** – Standardmäßig wird der Instanzname als Instanz-ID verwendet. So werden Installationsverzeichnisse und Registrierungsschlüssel für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]identifiziert. Dies ist der Fall für Standardinstanzen und benannte Instanzen. Bei einer Standardinstanz lauten Instanzname und Instanz-ID MSSQLSERVER. Wenn Sie nicht die Standard-Instanz-ID verwenden möchten, geben Sie im Textfeld **Instanz-ID** einen Wert an.  
  
     Alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Service Packs und Updates werden für jede Komponente einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]übernommen.  
  
     **Installierte Instanzen**: Im Raster werden Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angezeigt, die sich auf dem Computer befinden, auf dem das Setup ausgeführt wird. Wenn bereits eine Standardinstanz auf dem Computer installiert ist, muss eine benannte Instanz von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]installiert werden.  
  
13. Der Ablauf für die weiteren Vorgänge dieses Artikels richtet sich nach den Funktionen, die Sie für die Installation angegeben haben. Je nach Auswahl werden möglicherweise nicht alle Seiten angezeigt.  
  
14. Auf der Seite „Serverkonfiguration – Dienstkonten“ werden die Standarddienstkonten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste angezeigt. Welche Dienste tatsächlich auf dieser Seite konfiguriert werden, ist von den zu aktualisierenden Funktionen abhängig.  
  
     Authentifizierung und Anmeldeinformationen werden aus der vorherigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz übernommen. Sie können allen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Diensten dasselbe Anmeldekonto zuweisen, oder Sie können jedes Dienstkonto einzeln konfigurieren. Außerdem können Sie angeben, ob Dienste automatisch starten sollen, manuell gestartet werden oder deaktiviert sind. [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, die Dienstkonten einzeln zu konfigurieren, um den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Diensten die Berechtigungen zu gewähren, die mindestens erforderlich sind, um ihre Tasks auszuführen. Weitere Informationen finden Sie unter [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)betreffen.  
  
     Um für alle Dienstkonten in dieser Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]dasselbe Anmeldekonto anzugeben, geben Sie im Feld unten auf dieser Seite die entsprechenden Anmeldeinformationen ein.  
  
     **Sicherheitshinweis:** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Wenn Sie die Angabe der Anmeldeinformationen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste abgeschlossen haben, klicken Sie auf **Weiter**.  
  
15. Geben Sie auf der Seite Upgradeoptionen für die Volltextsuche die Upgradeoptionen für die zu aktualisierenden Datenbanken an. Weitere Informationen finden Sie unter [Upgradeoptionen für die Volltextsuche](./install-sql-server.md).  
  
16. Im Fenster Funktionsregeln wird automatisch fortgefahren, wenn alle Regeln gültig sind.  
  
17. Auf der Seite Das Upgrade kann jetzt ausgeführt werden wird eine Strukturansicht der Installationsoptionen angezeigt, die während des Setups angegeben wurden. Klicken Sie zum Fortsetzen des Vorgangs auf **Installieren**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert zuerst die erforderlichen Komponenten für die ausgewählten Funktionen und dann die Funktionen.  
  
18. Während der Installation wird auf der Seite Installationsstatus der Status angezeigt, sodass Sie während der Installation den Installationsstatus überwachen können.  
  
19. Nach der Installation bietet die Seite Abgeschlossen einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise. Klicken Sie auf **Schließen**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um die Installation von abzuschließen.  
  
20. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen zu Setupprotokolldateien finden Sie unter [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="next-steps"></a>Nächste Schritte  
 Führen Sie nach dem Aktualisieren auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]die folgenden Tasks aus:  
  
-   **Registrieren Sie Ihre Server**: Beim Upgrade werden die Registrierungseinträge für die frühere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz entfernt. Nach dem Aktualisieren müssen Sie die Server neu registrieren.  
  
-   **Aktualisieren Sie die Statistiken**: Zum Optimieren der Abfrageleistung wird empfohlen, nach dem Upgrade die Statistiken für alle Datenbanken zu aktualisieren. Statistiken in benutzerdefinierten Tabellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken aktualisieren Sie mithilfe der gespeicherten Prozedur **sp_updatestats**.  
  
-   **Konfigurieren Sie Ihre neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installation**: Zur Verringerung der Angriffsfläche eines Systems werden zentrale Dienste und Funktionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] selektiv installiert und aktiviert. Weitere Informationen zur Oberflächenkonfiguration finden Sie in der Infodatei für diese Version.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Upgrade von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Abwärtskompatibilität](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
