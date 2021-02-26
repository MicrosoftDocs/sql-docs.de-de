---
description: Konfigurieren der Windows-Authentifizierung auf dem Berichtsserver
title: Konfigurieren der Windows-Authentifizierung auf dem Berichtsserver | Microsoft-Dokumentation
ms.date: 02/10/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- Basic authentication
ms.assetid: 8faf2938-b71b-4e61-a172-46da2209ff55
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d3b875eafb10e4234df2cde35bb61bbecfebad94
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489324"
---
# <a name="configure-basic-authentication-on-the-report-server"></a>Konfigurieren der Windows-Authentifizierung auf dem Berichtsserver
  Standardmäßig akzeptiert Reporting Services Anforderungen, die Negotiate- und NTLM-Authentifizierung angeben. Wenn Ihre Bereitstellung Client-Anwendungen oder Browser umfasst, die die Standardauthentifizierung verwenden, müssen Sie die Standardauthentifizierung in die Liste der unterstützten Typen aufnehmen. Zusätzlich müssen Sie den anonymen Zugriff auf die Dateien des Berichts-Generators aktivieren, wenn Sie mit dem Berichts-Generator arbeiten möchten.  
  
 Um die Standardauthentifizierung auf dem Berichtsserver zu konfigurieren, bearbeiten Sie die XML-Elemente und -werte in der Datei RSReportServer.config. Sie können die Beispiele in diesem Thema kopieren und einfügen, um die Standardwerte zu ersetzen.  
  
 Bevor Sie die Standardauthentifizierung aktivieren, stellen Sie sicher, dass sie von der Sicherheitsinfrastruktur unterstützt wird. Unter der Standardauthentifizierung übergibt der Berichtsserver-Webdienst Anmeldeinformationen an die lokale Sicherheitsinstanz. Wenn die Anmeldeinformationen auf ein lokales Benutzerkonto verweisen, wird der Benutzer auf dem Computer, der den Berichtsserver ausführt, von der lokalen Sicherheitsinstanz authentifiziert und erhält ein Sicherheitstoken, das für lokale Ressourcen gültig ist. Anmeldeinformationen für Domänenbenutzerkonten werden an einen Domänencontroller weitergeleitet und von diesem authentifiziert. Das resultierende Ticket gilt für Netzwerkressourcen.  
  
 Wenn Sie das Risiko einschränken möchten, dass Anmeldeinformationen während der Übertragung auf einen Domänencontroller in Ihrem Netzwerk abgefangen werden, benötigen Sie eine Verschlüsselung auf Kanalebene wie Transport Layer Security (TLS), früher als Secure Sockets Layer (SSL) bezeichnet. Die Standardauthentifizierung selbst überträgt den Benutzernamen in Klartext und das Kennwort in base64-Codierung. Die zusätzliche Verschlüsselung auf Kanalebene macht das Paket unlesbar. Weitere Informationen finden Sie unter [Konfigurieren von TLS-Verbindungen auf einem Berichtsserver im einheitlichen Modus](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
 Bedenken Sie, dass Benutzer nach einer Aktivierung der Standardauthentifizierung die Option **Integrierte Sicherheit von Windows** nicht auswählen können, wenn Sie die Verbindungseigenschaften auf eine externe Datenquelle einstellen, die Daten an einen Bericht übergibt. Die Option ist auf den Seiten zu den Datenquelleneigenschaften grau abgeblendet.  
  
> [!NOTE]  
>  Die folgenden Anweisungen beziehen sich auf Berichtsserver, die im einheitlichen Modus ausgeführt werden. Wenn der Berichtserver im integrierten SharePoint-Modus bereitgestellt wird, müssen die Standardauthentifizierungseinstellungen verwendet werden, welche die integrierte Sicherheit von Windows angeben. Der Berichtsserver verwendet interne Erweiterungsfunktionen für die Standard-Windows-Authentifizierung, um Berichtsserver zu unterstützen, die im integrierten SharePoint-Modus ausgeführt werden.  
  
### <a name="to-configure-a-report-server-to-use-basic-authentication"></a>So konfigurieren Sie einen Berichtsserver für die Verwendung der Standardauthentifizierung  
  
1. Öffnen Sie RSReportServer.config in einem Text-Editor.  
  
     Informationen zur Konfigurationsdatei finden Sie im Abschnitt [Dateispeicherort](../report-server/rsreportserver-config-configuration-file.md#bkmk_file_location) im Artikel „RsReportServer.config Configuration File“.
  
2. Suchen Sie \<**Authentication**>.  
  
3. Kopieren Sie die XML-Struktur, die Ihren Anforderungen am besten entspricht. Die erste XML-Struktur stellt Platzhalter bereit, über die Sie alle im nächsten Abschnitt beschriebenen Elemente angegeben können:  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

    ```xml
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsBasic>  
                       <LogonMethod>3</LogonMethod>  
                       <Realm></Realm>  
                       <DefaultDomain></DefaultDomain>  
                 </RSWindowsBasic>  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
    Wenn Sie Standardwerte verwenden, können Sie die minimale Elementstruktur kopieren:  
  
    ```xml
          <AuthenticationTypes>  
                 <RSWindowsBasic/>  
          </AuthenticationTypes>  
    ```  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

    ```xml
      <Authentication>
          <AuthenticationTypes>
                      <RSWindowsBasic/>
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
      <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
      <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>
      </Authentication>
    ```

4. Ersetzen Sie damit die vorhandenen Einträge für \<**Authentication**>.  
  
     Wenn Sie mehrere Authentifizierungstypen verwenden, fügen Sie lediglich das **RSWindowsBasic** -Element ein, löschen Sie jedoch die Einträge für **RSWindowsNegotiate**, **RSWindowsNTLM** oder **RSWindowsKerberos** nicht.  
  
     Beachten Sie, dass Sie **Custom** nicht mit anderen Authentifizierungstypen verwenden können.  
  
5. Ersetzen Sie leere Werte für \<**Realm**> oder \<**DefaultDomain**> durch Werte, die für Ihre Umgebung gültig sind.  
  
6. Speichern Sie die Datei .  
  
7. Wenn Sie eine horizontal skalierte Bereitstellung konfiguriert haben, wiederholen Sie diese Schritte für andere in der Bereitstellung vorhandene Berichtsserver.  
  
8. Starten Sie den Berichtsserver neu, um alle momentan geöffneten Sitzungen zu beenden.  
  
## <a name="rswindowsbasic-reference"></a>Referenz auf RSWindowsBasic  
 Beim Konfigurieren der Standardauthentifizierung können die folgenden Elemente angegeben werden.  
  
|Element|Erforderlich|Gültige Werte|  
|-------------|--------------|------------------|  
|LogonMethod|Ja<br /><br /> Wenn Sie keinen Wert angeben, wird 3 verwendet.|**2** = Netzwerkanmeldung für Server mit hoher Leistungsfähigkeit zur Authentifizierung von Nur-Text-Kennwörtern<br /><br /> **3** = Klartextanmeldung, wobei die Anmeldeinformationen aus dem mit jeder HTTP-Anforderung gesendeten Authentifizierungspaket beibehalten werden. Dadurch kann der Server beim Herstellen von Verbindungen zu anderen Servern im Netzwerk die Identität des Benutzers annehmen. (Standardwert)<br /><br /> Hinweis: Die Werte 0 (für interaktive Anmeldung) und 1 (für Batchanmeldung) werden **NICHT** in [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] unterstützt.|  
|Realm|Optional|Gibt eine Ressourcenpartition mit Autorisierungs- und Authentifizierungsfunktionen an, mit denen Sie den Zugriff auf geschützte Ressourcen in Ihrem Unternehmen steuern können.|  
|DefaultDomain|Optional|Gibt die Domäne an, die vom Server für die Benutzerauthentifizierung verwendet wird. Dieser Wert ist optional. Wenn Sie ihn weglassen, verwendet der Berichtsserver den Computernamen als Domäne. Wenn der Computer zu einer Domäne gehört, ist diese Domäne die Standarddomäne. Wenn Sie den Berichtsserver auf einem Domänencontroller installiert haben, ist die verwendete Domäne die vom Computer gesteuerte.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anwendungsdomänen für Berichtsserveranwendungen](../../reporting-services/report-server/application-domains-for-report-server-applications.md)   
 [Sicherheit und Schutz für Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
