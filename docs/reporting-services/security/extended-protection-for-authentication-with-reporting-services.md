---
description: Erweiterter Schutz für die Authentifizierung mit Reporting Services
title: Erweiterter Schutz für die Authentifizierung mit Reporting Services | Microsoft-Dokumentation
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: eb5c6f4a-3ed5-430b-a712-d5ed4b6b9b2b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b13ca53aad69647aee5158d86de2cfb16ac04710
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076102"
---
# <a name="extended-protection-for-authentication-with-reporting-services"></a>Erweiterter Schutz für die Authentifizierung mit Reporting Services

  Erweiterter Schutz ist eine Gruppe von Erweiterungen zu den letzten Versionen des [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Windows-Betriebssystems. Erweiterter Schutz verbessert den Schutz der Anmeldeinformationen und der Authentifizierung durch Anwendungen. Das Feature selbst bietet keinen Schutz gegen bestimmte Angriffe, z.B. die Anmeldeinformationen-Weiterleitung, sie stellt jedoch eine Infrastruktur für Anwendungen wie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] bereit, um erweiterten Schutz für die Authentifizierung zu erzwingen.  
  
 Die Hauptauthentifizierungserweiterungen, die Teil des erweiterten Schutzes sind, sind Dienstbindung und Kanalbindung. Die Kanalbindung verwendet ein Kanalbindungstoken (Channel Binding Token oder CBT), um zu überprüfen, ob der zwischen zwei Endpunkten festgelegte Kanal nicht beeinträchtigt wurde. Dienstbindung überprüft das beabsichtigte Ziel von Authentifizierungstokens mithilfe von Dienstprinzipalnamen (Service Principal Names oder SPN). Weitere Hintergrundinformationen zu erweitertem Schutz finden Sie unter [Integrierte Windows-Authentifizierung unter Verwendung von „Erweiterter Schutz“](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90)).  
  
SQL Server Reporting Services (SSRS) unterstützt und erzwingt erweiterten Schutz, der im Betriebssystem aktiviert und in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] konfiguriert wurde. Standardmäßig akzeptiert [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Anforderungen, die Negotiate- oder NTLM-Authentifizierung angeben und daher im Betriebssystem von der Unterstützung des erweiterten Schutzes und der erweiterten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Schutzfunktionen profitieren könnten.  
  
> [!IMPORTANT]  
>  Windows aktiviert den erweiterten Schutz nicht standardmäßig. Informationen zum Aktivieren des erweiterten Schutzes in Windows finden Sie unter [Erweiterter Schutz für die Authentifizierung](https://support.microsoft.com/topic/microsoft-security-advisory-extended-protection-for-authentication-7dd2ee6d-c2e9-3484-2d8e-466261d3f0c7). Sowohl das Betriebssystem als auch der Clientauthentifizierungsstapel müssen den erweiterten Schutz unterstützen, damit die Authentifizierung erfolgreich ist. Bei älteren Betriebssystemen müssen Sie möglicherweise mehr als ein Update für einen Computer mit vollständigem erweiterten Schutz installieren. Informationen zu aktuellen Entwicklungen mit erweitertem Schutz finden Sie in den [aktualisierten Informationen mit erweitertem Schutz](/previous-versions/sql/sql-server-2008/dd146365(v=sql.100)).  

## <a name="reporting-services-extended-protection-overview"></a>Übersicht über Reporting Services mit erweitertem Schutz

SSRS unterstützt und erzwingt erweiterten Schutz, der im Betriebssystem aktiviert wurde. Wenn das Betriebssystem keinen erweiterten Schutz unterstützt oder das Feature im Betriebssystem nicht aktiviert wurde, tritt bei der Authentifizierung der Funktion von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] für erweiterten Schutz ein Fehler auf. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Erweiterter Schutz erfordert auch ein TLS/SSL-Zertifikat. Weitere Informationen finden Sie unter [Konfigurieren von TLS-Verbindungen auf einem Berichtsserver im einheitlichen Modus](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
> [!IMPORTANT]  
>  Der erweiterte Schutz ist in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] standardmäßig nicht aktiviert. Das Feature kann aktiviert werden, indem Sie die Konfigurationsdatei **rsreportserver.config** bearbeiten oder über die WMI-APIs aktualisieren. SSRS stellt keine Benutzeroberfläche bereit, um erweiterte Schutzeinstellungen zu ändern oder anzuzeigen. Weitere Informationen finden Sie im Abschnitt [Konfigurationseinstellungen](#ConfigurationSettings) in diesem Thema.  
  
 Häufige Probleme, die wegen Änderungen in erweiterten Schutzeinstellungen oder falsch konfigurierten Einstellungen auftreten, werden nicht mit offensichtlichen Fehlermeldungen oder Dialogfeldern angezeigt. Probleme mit Bezug auf die erweiterte Schutzkonfiguration und Kompatibilität führen zu Authentifizierungsfehlern und Fehlern in den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Ablaufverfolgungsprotokollen.  
  
> [!IMPORTANT]
>  Einige Technologien für den Datenzugriff unterstützen möglicherweise nicht den erweiterten Schutz. Für die Verbindung mit SQL Server-Datenquellen und der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Katalogdatenbank wird eine Datenzugriffstechnologie verwendet. Falls die Datenzugriffstechnologie den erweiterten Schutz nicht unterstützt, hat dies die folgenden Auswirkungen auf [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
> 
>  -   Auf dem SQL Server, auf dem die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Katalogdatenbank ausgeführt wird, kann der erweiterte Schutz nicht aktiviert sein, da der Berichtsserver ansonsten keine Verbindung zur Katalogdatenbank herstellt und Authentifizierungsfehler zurückgibt.  
> -   Auf SQL Servern, die als [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsdatenquellen verwendet werden, kann der erweiterte Schutz nicht aktiviert werden, da andernfalls Verbindungsversuche durch den Berichtsserver zur Berichtsdatenquelle fehlschlagen und Authentifizierungsfehler zurückgegeben werden.  
> 
>  Die Dokumentation einer Datenzugriffstechnologie sollte Informationen zu Unterstützung des erweiterten Schutzes enthalten.  
  
### <a name="upgrade"></a>Aktualisieren  
  
-   Beim Aktualisieren eines [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Servers auf SQL Server 2016 werden der Datei **rsreportserver.config** Konfigurationseinstellungen mit Standardwerten hinzugefügt. Wenn die Einstellungen bereits vorhanden waren, behält die SQL Server 2016-Installation sie in der Datei **rsreportserver.config** bei.  
  
-   Wenn der Konfigurationsdatei **rsreportserver.config** die Konfigurationseinstellungen hinzugefügt werden, ist das Standardverhalten für die erweiterte Schutzfunktion von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] der deaktivierte Zustand. Sie müssen die Funktion wie in diesem Thema beschrieben aktivieren. Weitere Informationen finden Sie im Abschnitt [Konfigurationseinstellungen](#ConfigurationSettings) in diesem Thema.  
  
-   Der Standardwert für die Einstellung **RSWindowsExtendedProtectionLevel** ist **Aus**.  
  
-   Der Standardwert für die Einstellung **RSWindowsExtendedProtectionScenario** ist **Proxy**.  
  
-   Upgrade Advisor überprüft nicht, ob das Betriebssystem oder die aktuelle Installation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] die Unterstützung des erweiterten Schutzes aktiviert hat.  
  
### <a name="what-reporting-services-extended-protection-does-not-cover"></a>Was der erweiterte Schutz der Reporting Services nicht abdeckt  
 Die folgenden Funktionsbereiche und Szenarios werden von der erweiterten Schutzfunktion von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nicht unterstützt:  
  
-   Autoren von benutzerdefinierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Sicherheitserweiterungen müssen ihrer benutzerdefinierten Sicherheitserweiterung Unterstützung für erweiterten Schutz hinzufügen.  
  
-   Komponenten von Drittanbietern, die einer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation hinzugefügt oder von ihr verwendet werden, müssen vom Drittanbieter aktualisiert werden, um erweiterten Schutz zu unterstützen. Weitere Informationen erhalten Sie vom Drittanbieter.  
  
## <a name="deployment-scenarios-and-recommendations"></a>Bereitstellungsszenarios und -empfehlungen  
 Die folgenden Szenarien veranschaulichen verschiedene Bereitstellungen und Topologien und die empfohlene Konfiguration, um sie mit erweitertem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Schutz zu sichern.  
  
### <a name="direct"></a>Direkt  
 Dieses Szenario beschreibt das direkte Herstellen einer Verbindung mit einem Berichtsserver, z. B. eine Intranetumgebung.  
  
|Szenario|Szenario (Diagramm)|So sichern Sie|  
|--------------|----------------------|-------------------|  
|Direkte TLS-Kommunikation.<br /><br /> Der Berichtsserver erzwingt die Kanalbindung zwischen Client und Berichtsserver.|![RS_ExtendedProtection_DirectSSL](../../reporting-services/security/media/rs-extendedprotection-directssl.gif "RS_ExtendedProtection_DirectSSL")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Direkt** fest.<br /><br /> <br /><br /> – Dienstbindung ist nicht notwendig, da der TLS-Kanal für Kanalbindung verwendet wird.|  
|direkte HTTP-Kommunikation. Der Berichtsserver erzwingt die Dienstbindung zwischen Client und Berichtsserver.|![RS_ExtendedProtection_Direct](../../reporting-services/security/media/rs-extendedprotection-direct.gif "RS_ExtendedProtection_Direct")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Beliebig** fest.<br /><br /> <br /><br /> – Es gibt keinen TLS-Kanal. Daher ist keine Erzwingung der Kanalbindung möglich.<br /><br /> – Dienstbindung kann überprüft werden, es ist jedoch keine vollständige Verteidigung ohne Kanalbindung. Dienstbindung allein schützt nur vor grundlegenden Bedrohungen.|  
  
### <a name="proxy-and-network-load-balancing"></a>Proxy und Netzwerklastenausgleich  
 Clientanwendungen stellen eine Verbindung mit einem Gerät oder einer Software her, die TLS ausführt, und gibt die Anmeldeinformationen zum Server zur Authentifizierung weiter, z. B. ein Extranet, Internet oder Sicheres Intranet. Der Client stellt eine Verbindung mit einem Proxy her, oder alle Clients verwenden einen Proxy.  
  
 Beim Verwenden eines Netzwerklastenausgleichsgeräts (NLB-Geräts) ist die Situation identisch.  
  
|Szenario|Szenario (Diagramm)|So sichern Sie|  
|--------------|----------------------|-------------------|  
|HTTP-Kommunikation. Der Berichtsserver erzwingt die Dienstbindung zwischen Client und Berichtsserver.|![RS_ExtendedProtection_Indirect](../../reporting-services/security/media/rs-extendedprotection-indirect.gif "RS_ExtendedProtection_Indirect")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Proxy|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Beliebig** fest.<br /><br /> <br /><br /> – Es gibt keinen TLS-Kanal. Daher ist keine Erzwingung der Kanalbindung möglich.<br /><br /> – Der Berichtsserver muss konfiguriert werden, um den Namen des Proxyservers zu kennen und um sicherzustellen, dass die Dienstbindung ordnungsgemäß erzwungen wird.|  
|HTTP-Kommunikation.<br /><br /> Der Berichtsserver erzwingt die Kanalbindung zwischen Client und Proxy sowie die Dienstbindung zwischen Client und Berichtsserver.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Proxy|Set <br />                    **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Proxy** fest.<br /><br /> <br /><br /> – TLS-Kanal zum Proxy ist verfügbar, daher kann die Kanalbindung zum Proxy erzwungen werden.<br /><br /> – Auch Dienstbindung kann erzwungen werden.<br /><br /> – Der Proxyname muss dem Berichtsserver bekannt sein, und der Berichtsserveradministrator sollte entweder eine URL-Reservierung mit einem Hostheader für ihn erstellen oder den Proxynamen im Windows-Registrierungseintrag **BackConnectionHostNames** konfigurieren.|  
|Indirekte HTTPS-Kommunikation mit einem sicheren Proxy. Der Berichtsserver erzwingt die Kanalbindung zwischen Client und Proxy sowie die Dienstbindung zwischen Client und Berichtsserver.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Proxy|Set <br />                    **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Proxy** fest.<br /><br /> <br /><br /> – TLS-Kanal zum Proxy ist verfügbar, daher kann die Kanalbindung zum Proxy erzwungen werden.<br /><br /> – Auch Dienstbindung kann erzwungen werden.<br /><br /> – Der Proxyname muss dem Berichtsserver bekannt sein, und der Berichtsserveradministrator sollte entweder eine URL-Reservierung mit einem Hostheader für ihn erstellen oder den Proxynamen im Windows-Registrierungseintrag **BackConnectionHostNames** konfigurieren.|  
  
### <a name="gateway"></a>Gateway  
 Dieses Szenario beschreibt Clientanwendungen, die eine Verbindung mit einem Gerät oder einer Software herstellen, die TLS ausführt und den Benutzer authentifiziert. Dann führt das Gerät oder die Software einen Identitätswechsel für den Benutzerkontext oder einen anderen Benutzerkontext durch, bevor es eine Anforderung an den Berichtsserver stellt.  
  
|Szenario|Szenario (Diagramm)|So sichern Sie|  
|--------------|----------------------|-------------------|  
|indirekte HTTP-Kommunikation.<br /><br /> Gateway erzwingt die Kanalbindung vom Client zum Gateway. Es gibt eine Dienstbindung vom Gateway zum Berichtsserver.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Gatewaygerät|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Beliebig** fest.<br /><br /> <br /><br /> – Kanalbindung vom Client zum Berichtsserver ist nicht möglich, da das Gateway für einen Kontext einen Identitätswechsel durchführt und daher ein neues NTLM-Token erstellt.<br /><br /> – Es gibt kein TLS vom Gateway zum Berichtsserver, daher kann die Kanalbindung nicht erzwungen werden.<br /><br /> – Dienstbindung kann erzwungen werden.<br /><br /> – Das Gatewaygerät sollte vom Administrator so konfiguriert werden, dass Kanalbindung erzwungen wird.|  
|Indirekte HTTPS-Kommunikation mit einem sicheren Gateway. Das Gateway erzwingt die Kanalbindung zwischen Client und Gateway, und der Berichtsserver erzwingt die Kanalbindung zwischen Gateway und Berichtsserver.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Gatewaygerät|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Direkt** fest.<br /><br /> <br /><br /> – Kanalbindung vom Client zum Berichtsserver ist nicht möglich, da das Gateway für einen Kontext einen Identitätswechsel durchführt und daher ein neues NTLM-Token erstellt.<br /><br /> – TLS vom Gateway zum Berichtsserver bedeutet, dass Kanalbindung erzwungen werden kann.<br /><br /> – Dienstbindung ist nicht erforderlich.<br /><br /> – Das Gatewaygerät sollte vom Administrator so konfiguriert werden, dass Kanalbindung erzwungen wird.|  
  
### <a name="combination"></a>Kombination  
 Dieses Szenario beschreibt Extranet- oder Internetumgebungen, in denen der Client einen Proxy verbindet. Dies geschieht in Verbindung mit einer Intranetumgebung, in der ein Client eine Verbindung zum Berichtsserver herstellt.  
  
|Szenario|Szenario (Diagramm)|So sichern Sie|  
|--------------|----------------------|-------------------|  
|Indirekter und direkter Zugriff vom Client auf den Berichtsserverdienst ohne TLS entweder über die Verbindung des Clients mit dem Proxy oder des Clients mit dem Berichtsserver.|1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Proxy<br /><br /> 4) Clientanwendung|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Beliebig** fest.<br /><br /> <br /><br /> – Dienstbindung von Client zum Berichtsserver kann erzwungen werden.<br /><br /> – Der Proxyname muss dem Berichtsserver bekannt sein, und der Berichtsserveradministrator sollte entweder eine URL-Reservierung mit einem Hostheader für ihn erstellen oder den Proxynamen im Windows-Registrierungseintrag **BackConnectionHostNames** konfigurieren.|  
|Indirekter und direkter Zugriff vom Client auf den Berichtsserver, wo der Client eine TLS-Verbindung mit dem Proxy oder dem Berichtsserver herstellt.|![RS_ExtendedProtection_CombinationSSL](../../reporting-services/security/media/rs-extendedprotection-combinationssl.gif "RS_ExtendedProtection_CombinationSSL")<br /><br /> 1) Clientanwendung<br /><br /> 2) Berichtsserver<br /><br /> 3) Proxy<br /><br /> 4) Clientanwendung|Legen Sie **RSWindowsExtendedProtectionLevel** auf **Zulassen** oder **Erfordern** fest.<br /><br /> Legen Sie **RSWindowsExtendedProtectionScenario** auf **Proxy** fest.<br /><br /> <br /><br /> – Kanalbindung kann verwendet werden.<br /><br /> – Der Proxyname muss dem Berichtsserver bekannt sein, und der Berichtsserveradministrator sollte entweder eine URL-Reservierung für den Proxy mit einem Hostheader erstellen oder den Proxynamen im Windows-Registrierungseintrag **BackConnectionHostNames** konfigurieren.|  
  
## <a name="configuring-reporting-services-extended-protection"></a>Konfigurieren des erweiterten Schutzes für Reporting Services  
 Die Datei **rsreportserver.config** enthält die Konfigurationswerte, die das Verhalten des erweiterten Schutzes von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] steuern.  
  
 Weitere Informationen zur Datei **rsreportserver.config** finden Sie unter [RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). Die erweiterten Schutzeinstellungen können auch geändert und mithilfe von WMI-APIs überprüft werden. Weitere Informationen finden Sie unter [SetExtendedProtectionSettings-Methode &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md).  
  
 Wenn die Überprüfung der Konfigurationseinstellungen fehlschlägt, werden die Authentifizierungstypen **RSWindowsNTLM**, **RSWindowsKerberos** und **RSWindowsNegotiate** auf dem Berichtsserver deaktiviert.  
  
###  <a name="configuration-settings-for-reporting-services-extended-protection"></a><a name="ConfigurationSettings"></a> Konfigurationseinstellungen für den erweiterten Schutz der Reporting Services  
 In der folgenden Tabelle sind Informationen zu den Konfigurationseinstellungen in der Datei **rsreportserver.config** für erweiterten Schutz bereitgestellt.  
  
|Einstellung|BESCHREIBUNG|  
|-------------|-----------------|  
|**RSWindowsExtendedProtectionLevel**|Gibt den Grad der Erzwingung des erweiterten Schutzes an. Gültige Werte sind:<br /><br /> **Off**: Standard. Gibt keine Kanal- oder Dienstbindungsüberprüfung an.<br /><br /> **Zulassen** unterstützt erweiterten Schutz, erfordert ihn aber nicht.  Gibt Folgendes an:<br /><br /> – Erweiterter Schutz wird für Clientanwendungen, die unter Betriebssystemen ausgeführt werden, die erweiterten Schutz unterstützen, erzwungen. Die Art, wie Schutz erzwungen wird, wird durch die Einstellung **RsWindowsExtendedProtectionScenario** festgelegt.<br /><br /> – Die Authentifizierung ist für Anwendungen zulässig, die unter Betriebssystemen ausgeführt werden, die keinen erweiterten Schutz unterstützen.<br /><br /> **Erfordern** gibt Folgendes an:<br /><br /> – Erweiterter Schutz wird für Clientanwendungen, die unter Betriebssystemen ausgeführt werden, die erweiterten Schutz unterstützen, erzwungen.<br /><br /> – Die Authentifizierung ist **nicht** für Anwendungen zulässig, die unter Betriebssystemen ohne Unterstützung von erweitertem Schutz ausgeführt werden.|  
|**RsWindowsExtendedProtectionScenario**|Gibt an, welche Arten des erweiterten Schutzes überprüft werden: Kanalbindung, Dienstbindung oder beides. Gültige Werte sind:<br /><br /> **Proxy**: Standard. Gibt Folgendes an:<br /><br /> – Windows-NTLM-, Kerberos- und Negotiate-Authentifizierung, wenn ein Kanalbindungstoken vorhanden ist.<br /><br /> – Dienstbindung wird erzwungen.<br /><br /> **Beliebig** gibt Folgendes an:<br /><br /> – Windows-NTLM-, Kerberos- und Negotiate-Authentifizierung sowie eine Kanalbindung sind nicht erforderlich.<br /><br /> – Dienstbindung wird erzwungen.<br /><br /> **Direkt** gibt Folgendes an:<br /><br /> – Windows-NTLM-, Kerberos- und Negotiate-Authentifizierung, wenn ein CBT vorhanden ist, eine TLS-Verbindung zum aktuellen Dienst vorhanden ist und das CBT für die TLS-Verbindung dem CBT des NTLM-, Kerberos- oder Negotiate-Tokens entspricht.<br /><br /> – Dienstbindung wird nicht erzwungen.<br /><br /> <br /><br /> Hinweis: Die Einstellung **RsWindowsExtendedProtectionScenario** wird ignoriert, wenn **RsWindowsExtendedProtectionLevel** den Wert **OFF** aufweist.|  
  
 Beispieleinträge in der Konfigurationsdatei **rsreportserver.config** :  
  
```  
<Authentication>  
         <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
         <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionLevel>  
</Authentication>  
```  
  
## <a name="service-binding-and-included-spns"></a>Dienstbindung und eingeschlossene SPNs  
 Die Dienstbindung nutzt Dienstprinzipalnamen (Service Principal Names oder SPN) zur Überprüfung des beabsichtigten Ziels von Authentifizierungstokens. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] erstellt mithilfe der vorhandenen URL-Reservierungsinformationen eine Liste von SPNs, die als gültig angesehen werden. Durch das Verwenden der URL-Reservierungsinformationen zur Überprüfung von SPN- und URL-Reservierungen können Systemadministratoren beide von einem einzelnen Standort aus verwalten.  
  
 Die Liste der gültigen SPNs wird aktualisiert, wenn der Berichtsserver startet, die Konfigurationseinstellungen für erweiterten Schutz geändert werden oder wenn die Anwendungsdomäne wiederverwendet wird.  
  
 Für jede Anwendung gibt es eine spezielle gültige Liste der SPNs. Für Berichts-Manager und Berichtsserver werden z. B. verschiedene Listen mit gültigen SPNs berechnet.  
  
 Die Liste der gültigen SPNs, die für eine Anwendung berechnet wurde, wird von den folgenden Faktoren bestimmt:  
  
-   Jede URL-Reservierung.  
  
-   Jeder SPN, der vom Domänencontroller für das Reporting Services-Dienstkonto abgerufen wird.  
  
-   Wenn eine URL-Reservierung Platzhalterzeichen ('*' oder '+') enthält, fügt der Berichtsserver jeden einzelnen Eintrag aus der Hosts-Auflistung hinzu.  
  
### <a name="hosts-collection-sources"></a>Hosts-Auflistungsquellen.  
 In der folgenden Tabelle sind die potenziellen Quellen für die Hosts-Auflistung aufgeführt.  
  
|Typ der Quelle|BESCHREIBUNG|  
|--------------------|-----------------|  
|ComputernameDnsDomäne|Der Name der dem lokalen Computer zugewiesenen DNS-Domäne. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der DNS-Domänenname des virtuellen Clusterservers verwendet.|  
|ComputernameDnsVollqualifiziert|Der vollqualifizierte DNS-Name, der den lokalen Computer eindeutig identifiziert. Dieser Name ist eine Kombination des DNS-Hostnamens und des DNS-Domänennamens und verwendet die Form *Hostname*.*Domänenname*. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der vollqualifizierte DNS-Domänenname des virtuellen Clusterservers verwendet.|  
|ComputernameDnsHostname|Der DNS-Hostname des lokalen Computers. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der DNS-Hostname des virtuellen Clusterservers verwendet.|  
|ComputernameNetBIOS|Der NetBIOS-Name des lokalen Computers. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der NetBIOS-Name des virtuellen Clusterservers verwendet.|  
|ComputernamePhysischeDnsDomäne|Der Name der dem lokalen Computer zugewiesenen DNS-Domäne. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der DNS-Domänenname des lokalen Computers verwendet, nicht der Name des virtuellen Clusterservers.|  
|ComputernamePhysischerDnsVollqualifiziert|Der vollqualifizierte DNS-Name, der den Computer eindeutig identifiziert. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der vollqualifizierte DNS-Name des lokalen Computers verwendet, nicht der Name des virtuellen Clusterservers.<br /><br /> Der vollqualifizierte DNS-Name ist eine Kombination des DNS-Hostnamens und des DNS-Domänennamens und verwendet die Form *Hostname*.*Domänenname*.|  
|ComputernamePhysischerDnsHostname|Der DNS-Hostname des lokalen Computers. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der DNS-Hostname des lokalen Computers verwendet, nicht der Name des virtuellen Clusterservers.|  
|ComputernamePhysischerNetBIOS|Der NetBIOS-Name des lokalen Computers. Wenn der lokale Computer ein Knoten in einem Cluster ist, wird der NetBIOS-Name des lokalen Computers verwendet, nicht der Name des virtuellen Clusterservers.|  
  
Weitere Informationen finden Sie unter [Registrieren eines Dienstprinzipalnamens &#40;SPN&#41; für einen Berichtsserver](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md) und [Informationen zu URL-Reservierungen und -Registrierungen &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).  
  
## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit der Datenbank-Engine unter Verwendung von Erweiterter Schutz](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)   
[Übersicht über den erweiterten Schutz für die Authentifizierung (möglicherweise auf Englisch)](/previous-versions/dotnet/netframework-3.5/dd767318(v=vs.90))   
[Integrierte Windows-Authentifizierung unter Verwendung von „Erweiterter Schutz“](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))   
[Microsoft-Sicherheitsempfehlung: Erweiterter Schutz für die Authentifizierung](https://go.microsoft.com/fwlink/?LinkId=179923)   
[Berichtsserverdienst-Ablaufverfolgungsprotokoll](../../reporting-services/report-server/report-server-service-trace-log.md)   
[RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[SetExtendedProtectionSettings-Methode &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
