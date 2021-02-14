---
title: Hosten einer Datenbank auf einer verwalteten Instanz
description: Erfahren Sie, wie Sie eine MDS-Datenbank (Master Data Services) erstellen und konfigurieren und in einem Azure SQL-verwaltete Instanz hosten.
ms.custom: ''
ms.date: 07/01/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19519697-c219-44a8-9339-ee1b02545445
author: v-redu
ms.author: lle
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 0ae7d5ad7bdb7874e4f6a99a1a99ea08c228fced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339484"
---
# <a name="host-an-mds-database-on-a-managed-instance"></a>Hosten einer MDS-Datenbank auf einer verwalteten Instanz

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In diesem Artikel wird beschrieben, wie Sie eine MDS-Datenbank (Master Data Services) auf einer verwalteten Instanz konfigurieren.
  
## <a name="preparation"></a>Vorbereitung

Zum Vorbereiten von müssen Sie eine Azure SQL-verwaltete Instanz erstellen und konfigurieren und den Webanwendungs Computer konfigurieren.

### <a name="create-and-configure-the-database"></a>Erstellen und Konfigurieren der Datenbank

1. Erstellen Sie eine verwaltete Instanz mit einem virtuellen Netzwerk. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines SQL-verwaltete Instanz](/azure/sql-database/sql-database-managed-instance-get-started) .

1. Konfigurieren Sie eine Point-to-Site-Verbindung. Weitere Informationen finden Sie [unter Konfigurieren einer Point-to-Site-Verbindung mit einem vnet unter Verwendung der nativen Azure-Zertifikat Authentifizierung: Azure-Portal](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) .

1. Konfigurieren Sie Azure Active Directory Authentifizierung mit SQL verwaltete Instanz. Ausführliche Informationen finden Sie [unter Konfigurieren und Verwalten der Azure Active Directory Authentifizierung mit SQL](/azure/sql-database/sql-database-aad-authentication-configure) .

### <a name="configure-web-application-machine"></a>Webanwendungs Computer konfigurieren

1. Installieren Sie ein Point-to-Site-Verbindungs Zertifikat und ein VPN, um sicherzustellen, dass der Computer auf die verwaltete Instanz zugreifen kann. Anweisungen hierzu finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem vnet unter Verwendung der nativen Azure-Zertifikat Authentifizierung: Azure-Portal](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) .

1. Installieren Sie die folgenden Rollen und Features:
   - Rollen:
     - Internetinformationsdienste
     - Webverwaltungstools
     - IIS-Verwaltungskonsole
     - WWW (World Wide Web)-Dienste
     - Anwendungsentwicklung
     - .NET-Erweiterbarkeit 3.5
     - .NET-Erweiterbarkeit 4.5
     - ASP.NET 3.5
     - ASP.NET 4.5
     - ISAPI-Erweiterungen
     - ISAPI-Filter
     - Allgemeine HTTP-Funktionen
     - Standarddokument
     - Verzeichnissuche
     - HTTP-Fehler
     - Statischer Inhalt
     - Integrität und Diagnose
     - HTTP-Protokollierung
     - Anforderungsüberwachung
     - Leistung
     - Komprimierung statischer Inhalte
     - Sicherheit
     - Anforderungsfilterung
     - Windows-Authentifizierung
       > [!NOTE]
       > WebDAV-Veröffentlichung nicht installieren

   - Features:
     - .NET Framework 3.5 (einschließlich .NET 2.0 und 3.0)
     - .NET Framework 4.5 Advanced Services
     - ASP.NET 4.5
     - WCF Services
     - HTTP-Aktivierung (erforderlich)
     - TCP-Portfreigabe
     - Windows-Prozessaktivierungsdienst
     - Prozessmodell
     - .NET-Umgebung
     - Konfiguration-APIs
     - Komprimierung dynamischer Inhalte

## <a name="install-and-configure-an-mds-web-application"></a>Installieren und Konfigurieren einer MDS-Webanwendung

Als nächstes installieren und konfigurieren Sie [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

### <a name="install-sql-server-2019"></a>Installieren von SQL Server 2019

Verwenden Sie den Installations-Assistenten für SQL Server Setup oder eine Eingabeaufforderung, um zu installieren [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

1. Öffnen `Setup.exe` Sie, und führen Sie die Schritte im Installations-Assistenten aus.

2. Wählen Sie auf der Seite [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]**Funktionsauswahl** unter **Freigegebene Funktionen** aus.
Mit dieser Aktion wird Folgendes installiert:
   - [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]
   - Assemblys
   - Ein Windows PowerShell-Snap-in
   - Ordner und Dateien für Webanwendungen und-Dienste.

   ![Screenshot der Seite "Funktionsauswahl"](../master-data-services/media/mds-sqlserver2019-config-mi-sqlfeatureselection.png "MDS-SQLServer2019-config-MI_SQLFeatureSelection")  

### <a name="set-up-the-database-and-website"></a>Einrichten der Datenbank und der Website

1. Verbinden Sie den Azure-Virtual Network, um sicherzustellen, dass Sie eine Verbindung mit der verwalteten Instanz herstellen können.

   ![Screenshot des Test-Mi-VPN, das eine Verbindung mit dem Azure-Virtual Network herstellt.](../master-data-services/media/mds-sqlserver2019-config-mi-p2svpnconnect.png "MDS-SQLServer2019-config-MI_P2SVPNConnect")

1. Öffnen Sie das, [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] und wählen Sie dann im linken Bereich **Daten Bank Konfiguration** aus.

1. Wählen Sie **Datenbank erstellen** , um den **Assistenten zum Erstellen einer Datenbank** zu öffnen. Wählen Sie **Weiter** aus.

1. Füllen Sie auf der Seite **Daten Bank Server** das Feld **SQL Server Instanz** aus, und wählen Sie dann den **Authentifizierungstyp** aus. Wählen Sie **Verbindung testen** aus, um zu bestätigen, dass Sie Ihre Anmelde Informationen zum Herstellen einer Verbindung mit der Datenbank über den ausgewählten Authentifizierungstyp verwenden Wählen Sie **Weiter** aus.

   > [!NOTE]
   > - Eine SQL Server-Instanz sieht wie folgt aus `xxxxxxx.xxxxxxx.database.windows.net` :.
   > - Wählen Sie für eine verwaltete Instanz aus den Authentifizierungs Typen **"SQL Server Konto"** und **"aktueller Benutzer – Active Directory integriert** " aus.
   > - Wenn Sie **Aktueller Benutzer – Active Directory** als Authentifizierungstyp integriert auswählen, ist das Feld **Benutzername** schreibgeschützt und zeigt das aktuell angemeldete Windows-Benutzerkonto an. Wenn Sie SQL Server 2019 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem virtuellen Azure-Computer (VM) ausführen, zeigt das Feld **Benutzername** den VM-Namen und den Benutzernamen für das lokale Administrator Konto auf dem virtuellen Computer an.

   Ihre Authentifizierung muss die Regel **"sysadmin"** für verwaltete Instanzen enthalten.

   ![Screenshot der Seite "Daten Bank Server" des Assistenten zum Erstellen einer Datenbank.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbconnect.png "MDS-SQLServer2019-config-MI_CreateDBConnect")  

1. Geben Sie im Feld **Datenbankname** einen Namen ein. Um eine Windows-Sortierung auszuwählen, deaktivieren Sie optional das Kontrollkästchen **SQL Server Standardsortierung** , und wählen Sie mindestens eine der verfügbaren Optionen aus. Beispielsweise wird die **Groß-/Kleinschreibung** beachtet. Wählen Sie **Weiter** aus.

   ![Screenshot der Seite "Datenbank" des Assistenten zum Erstellen einer Datenbank.](../master-data-services/media/mds-sqlserver2019-config-mi-createddbname.png "MDS-SQLServer2019-config-MI_CreatedDBName")

1. Geben Sie im Feld **Benutzername** das Windows-Konto des Standard-Super Benutzers für an [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] . Ein Administrator hat Zugriff auf alle Funktionsbereiche und kann alle Modelle hinzufügen, löschen und aktualisieren.

   ![Screenshot der Seite "Administrator Konto" des Assistenten zum Erstellen einer Datenbank.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbusername.png "MDS-SQLServer2019-config-MI_createDBUserName")

1. Wählen Sie **weiter** aus, um eine Zusammenfassung der Einstellungen für die [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Datenbank anzuzeigen. Klicken Sie erneut auf  **weiter** , um die Datenbank zu erstellen. Die Seite Status **und fertig** Stellung wird angezeigt.

1. Nachdem die Datenbank erstellt und konfiguriert wurde, klicken Sie auf **Fertig** stellen.

   Weitere Informationen zu den Einstellungen im Assistenten zum **Erstellen einer Datenbank** finden Sie unter [Assistent zum Erstellen einer Datenbank &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).

1. Wählen Sie auf der Seite **Daten Bank Konfiguration** in der die Option [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] **Datenbank auswählen** aus.

1. Wählen Sie **verbinden**, wählen Sie die Datenbank aus, [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] und klicken Sie dann auf **OK**

   ![Screenshot des Dialog Felds "Verbindung mit Datenbank herstellen".](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_connectDBName")

1. [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]Wählen Sie in im linken Bereich **Webkonfiguration** aus.

1. Wählen Sie im Listenfeld **Website** die Option **Standard Website** aus, und wählen Sie dann **Erstellen** aus, um eine Webanwendung zu erstellen.

   ![Screenshot des Dialog Felds "Konfigurations-Manager für Master Data Services".](../master-data-services/media/mds-sqlserver2019-config-mi-webconfiguration.png "MDS-SQLServer2019-config-MI_WebConfiguration")

   > [!NOTE]
   > Wenn Sie **Standard Website** auswählen, müssen Sie eine Webanwendung separat erstellen. Wenn Sie im Listenfeld **neue Website erstellen** auswählen, wird die Anwendung automatisch erstellt.

1. Geben Sie im Abschnitt **Anwendungs Pool** einen anderen Benutzernamen ein, geben Sie das Kennwort ein, und klicken Sie dann auf **OK**.

   ![Screenshot des Dialog Felds "Anwendungs Verwaltung".](../master-data-services/media/mds-sqlserver2019-config-mi-createwebapplication.png "MDS-SQLServer2019-config-MI_CreateWebApplication")

   > [!NOTE]
   > Stellen Sie sicher, dass der Benutzer mit der Active Directory integrierten Authentifizierung, die Sie kürzlich erstellt haben, auf die Datenbank zugreifen kann. Alternativ dazu können Sie die Verbindung später ändern `web.config` .

   Weitere Informationen zum Dialogfeld **Webanwendung erstellen** finden Sie unter [Dialogfeld "Webanwendung erstellen" &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).

1. Wählen Sie  im Fenster Webanwendung im Fenster **Webanwendung** die Anwendung aus, die Sie erstellt haben, und wählen Sie dann im Abschnitt **Anwendung einer Datenbank zuordnen** die Option **auswählen** aus.

1. Wählen Sie **verbinden** aus, und wählen Sie die [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Datenbank aus, die Sie der Webanwendung zuordnen möchten. Klicken Sie auf **OK**.

   Die Einrichtung der Website ist abgeschlossen. Auf der Seite **Webkonfiguration** werden nun die von Ihnen ausgewählte Website, die von Ihnen erstellte Webanwendung und die der Anwendung zugeordnete [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Datenbank angezeigt.

   ![Screenshot des Webkonfigurations Abschnitts.](../master-data-services/media/mds-sqlserver2019-config-mi-webconfigselectdb.png "MDS-SQLServer2019-config-MI_WebConfigSelectDB")

1. Wählen Sie **Übernehmen**. Die Meldung zum **vervollständigen der Konfiguration** wird angezeigt. Wählen Sie im Meldungs Feld **OK** aus, um die-Webanwendung zu starten. Die Adresse der Website lautet `http://server name/web application/`.

## <a name="configure-authentication"></a>Konfigurieren der Authentifizierung

Um die verwaltete Instanzdatenbank mit der-Webanwendung zu verbinden, müssen Sie den anderen Authentifizierungstyp ändern.

Suchen Sie die `web.config` Datei unter `C:\Program Files\Microsoft SQL Server\150\Master Data Services\WebApplication` . Ändern Sie ConnectionString, um den anderen Authentifizierungstyp zum Herstellen einer Verbindung mit der verwalteten Instanzdatenbank zu ändern.

Der Standard Authentifizierungstyp ist `Active Directory Integrated` wie in der folgenden Beispiel Verbindungs Zeichenfolge dargestellt:

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Integrated&quot;" />
   ```

MDS unterstützt auch Active Directory Kenn Wort Authentifizierung und SQL Server Authentifizierung, wie in den folgenden Beispiel Verbindungs Zeichenfolgen gezeigt:

- Active Directory-Kennwortauthentifizierung

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Password&quot; ; UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!" />
   ```

- SQL Server-Authentifizierung

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;User ID=UserName;Password=MyPassword!;" />
   ```

## <a name="upgrade-ssmdsshort_md-and-sql-database-version"></a>Upgrade [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] und SQL-Datenbankversion

### <a name="upgrade-ssmdsshort_md"></a>Zuführen [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]

Installieren Sie das **kumulative Update SQL Server 2019**. [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] wird automatisch aktualisiert.

### <a name="upgrade-sql-server"></a>Aktualisieren von SQL Server

Möglicherweise wird der folgende Fehler angezeigt: `The client version is incompatible with the database version` nach der Installation des **kumulativen Updates SQL Server 2019**.
![Screenshot des Master Data Services Fehlers.](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbpage.png "MDS-SQLServer2019-config-MI_UpgradeDBPage")

Um dieses Problem zu beheben, müssen Sie die Datenbankversion aktualisieren:

1. Öffnen [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] Sie das, und wählen Sie dann im linken Bereich  **Daten Bank Konfiguration** aus.

1. Wählen Sie auf der Seite **Daten Bank Konfiguration** in der die Option [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] **Datenbank auswählen** aus.

1. Wählen [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Sie die Datenbank aus, die Sie der Webanwendung zugeordnet haben. Wählen Sie **verbinden** aus, und klicken Sie dann auf **OK**.

   ![Screenshot des Dialog Felds "Verbindung mit einer Master Data Service-Datenbank herstellen".](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_ConnectDBName")

1. Wählen Sie **Datenbank aktualisieren...** .

   ![Screenshot der Option "Datenbank aktualisieren".](../master-data-services/media/mds-sqlserver2019-config-mi-selectupgradedb.png "MDS-SQLServer2019-config-MI_SelectUpgradeDB")

1. Klicken Sie im Assistenten zum Aktualisieren von Datenbanken auf der **Willkommens** Seite auf weiter und auf der Seite **Upgradeüberprüfung** auf **weiter** .

   ![Screenshot der Seite "Upgradeüberprüfung" des Assistenten zum Aktualisieren von Datenbanken](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbwizard.png "MDS-SQLServer2019-config-MI_UpgradeDBWizard")

1. Wählen Sie **Fertig** stellen aus, nachdem alle Aufgaben abgeschlossen sind.

## <a name="see-also"></a>Weitere Informationen

- [Master Data Services-Datenbank](../master-data-services/master-data-services-database.md)
- [Master Data Manager-Webanwendung [Master Data Services]](../master-data-services/master-data-manager-web-application.md)
- [Datenbankkonfiguration &#40;Seite im Konfigurations-Manager für Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)
- [Neues in Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)