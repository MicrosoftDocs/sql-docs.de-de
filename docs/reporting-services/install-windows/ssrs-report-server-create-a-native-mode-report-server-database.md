---
title: Erstellen einer Berichtsserver-Datenbank im einheitlichen Modus (Konfigurations-Manager) | Microsoft-Dokumentation
description: Bei Reporting Services im einheitlichen Modus wird eine SQL Server-Datenbank als interner Speicher verwendet. Die Datenbank ist erforderlich und wird zum Speichern von veröffentlichten Berichten, Modellen, freigegebenen Datenquellen, Sitzungsdaten, Ressourcen und Servermetadaten verwendet.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], databases
- databases [Reporting Services], creating
ms.assetid: 81b9f4ad-800b-4688-8b47-a5a83dc8ff10
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dab7b82e27ba57974a4bdb8e679164f6316ecd4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067125"
---
# <a name="create-a-native-mode-report-server-database-report-server-configuration-manager"></a>Erstellen einer Berichtsserver-Datenbank im einheitlichen Modus (Berichtsserver-Konfigurations-Manager)

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)]

Der einheitliche Modus für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] verwendet eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank als internen Speicher. Die Datenbank ist erforderlich und wird zum Speichern von veröffentlichten Berichten, Modellen, freigegebenen Datenquellen, Sitzungsdaten, Ressourcen und Servermetadaten verwendet.  

Um eine Berichtsserver-Datenbank zu erstellen oder die Verbindungszeichenfolge oder die Anmeldeinformationen zu ändern, verwenden Sie die Optionen auf der Datenbankseite im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager.  
  
## <a name="when-to-create-or-configure-the-report-server-databases"></a>Der richtige Zeitpunkt zum Erstellen bzw. Konfigurieren von Berichtsserver-Datenbanken  
 Sie müssen die Berichtsserver-Datenbank erstellen und konfigurieren, wenn Sie den Berichtsserver im Modus für die ausschließliche Installation von Dateien installiert haben.  
  
 Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in der Standardkonfiguration für den einheitlichen Modus installiert haben, wurde die Berichtsserver-Datenbank automatisch während der Installation der Berichtsserver-Instanz erstellt und konfiguriert. Mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager können Sie die von Setup automatisch konfigurierten Einstellungen anzeigen bzw. ändern.  
  
##  <a name="before-you-start"></a><a name="rsdbrequirements"></a> Vorbereitungen  
 Das Erstellen bzw. die Konfiguration einer Berichtsserver-Datenbank erfolgt in mehreren Schritten. Bevor Sie die Berichtsserver-Datenbank erstellen, sollten Sie sich überlegen, wie Sie die folgenden Elemente angeben möchten:  
  
 **Datenbankserver auswählen**  
 Überprüfen Sie die unterstützten Versionen der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], und überprüfen Sie die unterstützten Editionen im Thema [Erstellen einer Berichtsserver-Datenbank &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md).  
  
 **TCP/IP-Verbindungen aktivieren**  
 Aktivieren Sie TCP/IP-Verbindungen für das [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Bei einigen [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Editionen wird TCP/IP nicht standardmäßig aktiviert. In diesem Thema finden Sie Anweisungen.  
  
 **Port öffnen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
 Wenn Sie Firewallsoftware verwenden, müssen Sie bei einem Remoteserver den Port öffnen, auf dem der [!INCLUDE[ssDE](../../includes/ssde-md.md)] lauscht.  
  
 **Berichtsserver-Anmeldeinformationen festlegen**  
 Legen Sie fest, wie der Berichtsserver eine Verbindung mit den Berichtsserver-Datenbanken herstellen soll. Zu den Anmeldeinformations-Typen gehören das Domänenbenutzerkonto, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank-Benutzerkonto und das Berichtsserver-Dienstkonto.  
  
 Diese Anmeldeinformationen werden verschlüsselt und in der Datei RSReportServer.config gespeichert. Der Berichtsserver verwendet diese Anmeldeinformationen für ausgehende Verbindungen zur Berichtsserver-Datenbank. Wenn Sie ein Windows-Benutzerkonto oder ein Datenbank-Benutzerkonto verwenden möchten, müssen Sie ein bereits bestehendes Konto angeben. Der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager erstellt zwar einen Benutzernamen für die Anmeldung und legt die erforderlichen Berechtigungen fest, erstellt jedoch kein Konto. Weitere Informationen finden Sie unter [Konfigurieren einer Berichtsserver-Datenbankverbindung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
 **Sprache für den Berichtsserver festlegen**  
 Wählen Sie eine Sprache für den Berichtsserver aus. Vordefinierte Rollennamen, Beschreibungen und die Ordner vom Typ Meine Berichte werden nicht in verschiedenen Sprachen angezeigt, wenn sich die Benutzer mit verschiedenen Sprachversionen eines Browsers beim Server anmelden.  
  
 **Anmeldeinformationen zum Erstellen und Bereitstellen der Datenbank überprüfen**  
 Vergewissern Sie sich, dass Sie über Kontoanmeldeinformationen verfügen, die Berechtigungen zum Erstellen von Datenbanken auf der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz aufweisen. Diese Anmeldeinformationen werden für eine einmalige Verbindung verwendet, die zum Erstellen der Berichtsserver-Datenbank und von **RSExecRole** dient. Wenn noch kein Benutzername für die Anmeldung vorhanden ist, wird ein Datenbank-Benutzername für das Konto erstellt, das vom Berichtsserver für die Verbindung mit der Datenbank verwendet wird. Sie können eine Verbindung unter dem [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto herstellen, mit dem Sie angemeldet sind, oder einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank-Benutzernamen verwenden.  
  
### <a name="to-enable-access-to-a-remote-report-server-database"></a>So aktivieren Sie den Zugriff auf eine Remoteberichtsserver-Datenbank  
  
1.  Wenn Sie eine [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Remote-Instanz verwenden, melden Sie sich beim Datenbankserver an, um TCP/IP-Verbindungen zu überprüfen bzw. zu aktivieren.  
  
2.  Zeigen Sie auf **Start**, auf **Alle Programme**, auf **Microsoft SQL Server**, dann auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.  
  
3.  Öffnen Sie **SQL Server-Netzwerkkonfiguration**.  
  
4.  Wählen Sie die Datenbankinstanz aus.  
  
5.  Klicken Sie mit der rechten Maustaste auf **TCP/IP** , und wählen Sie dann **Aktiviert** aus.  
  
6.  Starten Sie den Dienst neu.  
  
7.  Öffnen Sie die Firewallsoftware, und öffnen Sie den Port, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lauscht. Für die Standardinstanz ist dies normalerweise Port 1433 für TCP/IP-Verbindungen. Weitere Informationen zur Windows-Firewall finden Sie unter [Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
### <a name="to-create-a-local-report-server-database"></a>So erstellen Sie eine lokale Berichtsserver-Datenbank  
  
1.  Starten Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager, und stellen Sie eine Verbindung mit der Berichtsserverinstanz her, für die Sie die Datenbank erstellen. Weitere Informationen finden Sie unter [Report Server Configuration Manager &#40;Native Mode&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md) (Konfigurations-Manager des Berichtsservers – &#40;einheitlicher Modus&#41;).  
  
2.  Wählen Sie auf der Seite „Datenbank“ die Option **Datenbank ändern** aus.  
  
3.  Wählen Sie **Neue Berichtsserver-Datenbank erstellen** und dann **Weiter** aus.  
  
4.  Stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, die Sie zum Erstellen und Hosten der Berichtsserver-Datenbank verwenden möchten.  
  
    1.  Geben Sie die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] -Instanz ein, die Sie verwenden möchten. Der Assistent zeigt eine lokale [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz an, die als Standardinstanz ausgeführt wird, sofern sie verfügbar ist. Andernfalls müssen Sie den Server und die Instanz eingeben, die verwendet werden sollen. Benannte Instanzen werden in folgendem Format angegeben: \<servername>\\<Instanzname\>.  
  
    2.  Geben Sie die Anmeldeinformationen ein, die für eine einmalige Verbindung mit [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet werden, um die Berichtsserver-Datenbanken zu erstellen. Weitere Informationen zur Verwendung dieser Anmeldeinformationen finden Sie in diesem Thema unter [Vorbereitungen](#rsdbrequirements) .  
  
    3.  Wählen Sie **Verbindung testen** aus, um die Verbindung zum Server zu überprüfen.  
  
    4.  Wählen Sie **Weiter** aus.  
  
5.  Geben Sie Eigenschaften an, die zum Erstellen der Datenbank verwendet werden. Weitere Informationen zur Verwendung dieser Eigenschaften finden Sie in diesem Thema unter [Vorbereitungen](#rsdbrequirements) .  
  
    1.  Geben Sie den Namen der Berichtsserver-Datenbank ein. Daraufhin werden eine temporäre Datenbank und die primäre Datenbank erstellt. Verwenden Sie ggf. einen beschreibenden Namen, um sich besser daran erinnern zu können, wie die Datenbank verwendet wird. Bedenken Sie, dass der angegebene Name während der gesamten Lebensdauer der Datenbank verwendet wird. Eine erstellte Berichtsserver-Datenbank kann nicht mehr umbenannt werden.  
  
    2.  Wählen Sie die Sprache aus, in der die Rollendefinitionen und Meine Berichte angezeigt werden sollen.  
  
    3.  Der Berichtsservermodus wird immer auf **Einheitlich** festgelegt.  
  
    4.  Wählen Sie **Weiter** aus.  
  
6.  Geben Sie die Anmeldeinformationen an, die vom Berichtsserver zum Herstellen der Verbindung zur Berichtsserver-Datenbank verwendet werden.  
  
    1.  Geben Sie den Authentifizierungstyp an:  
  
         Wählen Sie **Datenbank-Anmeldeinformationen** aus, um die Verbindung mithilfe eines bereits definierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank-Benutzernamens herzustellen. Die Verwendung von Datenbank-Anmeldinformationen wird empfohlen, wenn der Berichtsserver sich auf einem Computer in einer anderen Domäne, einer nicht vertrauenswürdigen Domäne oder hinter einer Firewall befindet.  
  
         Wählen Sie **Windows-Anmeldeinformationen** , wenn Sie ein Domänenbenutzerkonto mit geringen Privilegien verwenden, das über die Berechtigung zur Anmeldung bei dem Computer und dem Datenbankserver verfügt.  
  
         Wählen Sie **Anmeldeinformationen des Diensts** , wenn der Berichtsserver die Verbindung mithilfe seines Dienstkontos herstellen soll. Mit dieser Option stellt der Server die Verbindung mit integrierter Sicherheit her; Anmeldeinformationen werden nicht verschlüsselt oder gespeichert.  
  
    2.  Wählen Sie **Weiter** aus.  
  
7.  Überprüfen Sie die Informationen über die Zusammenfassungsseite, um zu überprüfen, ob die Einstellungen korrekt sind, und wählen Sie dann **Weiter** aus.  
  
8.  Überprüfen Sie die Verbindung durch Auswählen einer URL auf der Seite „Berichtsserver-URL“. Die URLs müssen definiert werden, damit dieser Test funktioniert. Wenn die Verbindung zur Berichtsserverdatenbank gültig ist, wird die Berichtsserver-Ordnerhierarchie angezeigt. Weitere Informationen finden Sie unter [Überprüfen einer Installation von Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  

## <a name="change-database-credentials"></a>Ändern von Datenbank-Anmeldeinformationen

Der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager stellt einen Assistenten zum Ändern der Anmeldeinformationen bereit, der Sie durch alle Schritte der Neukonfiguration des Kontos führt, anhand dessen der Berichtsserver die Verbindung mit der Berichtsserverdatenbank herstellt. Wenn Sie die Anmeldeinformationen ändern, aktualisiert der Konfigurations-Manager alle Berechtigungen und Datenbank-Anmeldedaten auf dem Datenbankserver für die Berichtsserver-Datenbank, die vom Berichtsserver verwendet wird. 

1.  Starten Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager, und stellen Sie eine Verbindung mit der Berichtsserverinstanz her, für die Sie die Datenbank erstellen. Weitere Informationen finden Sie unter [Report Server Configuration Manager &#40;Native Mode&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md) (Konfigurations-Manager des Berichtsservers – &#40;einheitlicher Modus&#41;).  
  
2.  Wählen Sie auf der Seite „Datenbank“ die Option **Anmeldeinformationen ändern** aus. 

3.  Stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, die Sie zum Erstellen und Hosten der Berichtsserver-Datenbank verwenden möchten.  
  
    1.  Geben Sie die Anmeldeinformationen ein, die für eine einmalige Verbindung mit [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet werden, um die Berichtsserver-Datenbanken zu erstellen. Weitere Informationen zur Verwendung dieser Anmeldeinformationen finden Sie in diesem Thema unter [Vorbereitungen](#rsdbrequirements) .  
  
    2.  Wählen Sie **Verbindung testen** aus, um die Verbindung zum Server zu überprüfen.  
  
    3.  Wählen Sie **Weiter** aus.  

4.  Geben Sie die Anmeldeinformationen an, die vom Berichtsserver zum Herstellen der Verbindung zur Berichtsserver-Datenbank verwendet werden.  
  
    1.  Geben Sie den Authentifizierungstyp an:  
  
         Wählen Sie **Datenbank-Anmeldeinformationen** aus, um die Verbindung mithilfe eines bereits definierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank-Benutzernamens herzustellen. Die Verwendung von Datenbank-Anmeldinformationen wird empfohlen, wenn der Berichtsserver sich auf einem Computer in einer anderen Domäne, einer nicht vertrauenswürdigen Domäne oder hinter einer Firewall befindet.  
  
         Wählen Sie **Windows-Anmeldeinformationen** , wenn Sie ein Domänenbenutzerkonto mit geringen Privilegien verwenden, das über die Berechtigung zur Anmeldung bei dem Computer und dem Datenbankserver verfügt.  
  
         Wählen Sie **Anmeldeinformationen des Diensts** , wenn der Berichtsserver die Verbindung mithilfe seines Dienstkontos herstellen soll. Mit dieser Option stellt der Server die Verbindung mit integrierter Sicherheit her; Anmeldeinformationen werden nicht verschlüsselt oder gespeichert.  
  
    2.  Wählen Sie **Weiter** aus. 

5. Überprüfen Sie die Einstellungen, und wählen Sie **Weiter** aus.

6. Nachdem die Änderungen vorgenommen wurden, wählen Sie **Fertig stellen** aus.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer Verbindung mit der Berichtsserver-Datenbank](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Verwalten eines Berichtsservers von Reporting Services im einheitlichen Modus](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[Berichtsserver-Konfigurations-Manager](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
