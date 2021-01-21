---
description: Konfiguration der benutzerdefinierten oder Formularauthentifizierung auf dem Berichtsserver
title: Konfigurieren der benutzerdefinierten oder Formularauthentifizierung auf dem Berichtsserver | Microsoft-Dokumentation
ms.date: 04/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Forms authentication, configuring
- custom authentication [Reporting Services]
ms.assetid: e8601a8f-e66d-4649-8e4d-a46ca20ec7d0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bca083e29d0d03a5a5470f58b4e004d81cd27a36
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596628"
---
# <a name="configure-custom-or-forms-authentication-on-the-report-server"></a>Konfiguration der benutzerdefinierten oder Formularauthentifizierung auf dem Berichtsserver

Reporting Services stellt eine erweiterbare Architektur zur Verfügung, mit der Sie benutzerdefinierte oder formularbasierte Authentifizierungsmodule integrieren können. Möglicherweise ist es ratsam, eine benutzerdefinierte Authentifizierungserweiterung zu implementieren, wenn die Bereitstellungsanforderungen keine Windows-integrierte Sicherheit oder grundlegende Authentifizierung umfassen. Das häufigste Szenario für die Verwendung der benutzerdefinierten Authentifizierung ist eine Unterstützung des Internet- oder Extranetzugriffs auf eine Webanwendung. Durch das Ersetzen der standardmäßigen Windows-Authentifizierungserweiterung durch eine benutzerdefinierte Erweiterung können Sie besser steuern, wie externen Benutzern Zugriff auf den Berichtsserver gewährt wird.  

In der Praxis erfordert das Bereitstellen einer benutzerdefinierten Authentifizierungserweiterung mehrere Schritte, darunter das Kopieren von Assemblys und Anwendungsdateien, das Bearbeiten von Konfigurationsdateien und das Testen. Dieses Thema konzentriert sich ganz auf die Authentifizierungseinstellungen, die Sie in den Konfigurationsdateien angeben.  

> [!NOTE]
>  Zum Erstellen einer benutzerdefinierten Authentifizierungserweiterung sind benutzerdefinierter Code und Kenntnisse der [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] -Sicherheit erforderlich. Wenn Sie keine benutzerdefinierte Authentifizierungserweiterung erstellen möchten, können Sie [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory-Gruppen und -Konten verwenden, aber Sie sollten den Rahmen einer Berichtsserverbereitstellung deutlich verringern. Weitere Informationen zur benutzerdefinierten Authentifizierung finden Sie unter [Implementing a Security Extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).

Wenn Sie eine Formular- oder benutzerdefinierte Authentifizierungserweiterung in einer SQL Server Reporting Services-Umgebung verwenden möchten, die mit einem SharePoint-Produkt integriert ist, müssen Sie die SharePoint-Website für die Verwendung der von Ihnen gewählten Authentifizierungsmethode konfigurieren. Weitere Informationen über das Konfigurieren der Authentifizierung in SharePoint finden Sie unter [Authentication Samples](/previous-versions/office/sharepoint-2007-products-and-technologies/cc262069(v=office.12)) im [!INCLUDE[msCoName](../../includes/msconame-md.md)] Developer Network (MSDN).



### <a name="to-configure-a-report-server-to-use-custom-authentication"></a>So konfigurieren Sie einen Berichtsserver für die Verwendung der benutzerdefinierten Authentifizierung

1.  Öffnen Sie RSReportServer.config in einem Text-Editor.

2.  Suchen Sie \<**Authentication**>.

3.  Kopieren Sie die folgende XML-Struktur:

    ```
    <Authentication>
          <AuthenticationTypes>
                 <Custom />
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    ```

4.  Ersetzen Sie damit die vorhandenen Einträge für \<**Authentication**>.

     Beachten Sie, dass Sie **Custom** nicht mit anderen Authentifizierungstypen verwenden können.

5.  Speichern Sie die Datei.

6.  Öffnen Sie die Datei Web.config für den Berichtsserver. Standardmäßig befindet sich diese Datei unter "Programme\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\ReportServer".

7.  Suchen Sie **authentication mode** , und legen Sie dafür **Forms** fest.

    ```
    <authentication mode = "Forms" />
    ```

8.  Suchen Sie **identity impersonate** , und legen Sie dafür **False** fest.

    ```
    <identity impersonate = "false" />  
    ```
9. Fügen Sie der Konfigurationsdatei die Elementstruktur **PassThroughCookies** hinzu. Weitere Informationen finden Sie unter [Konfigurieren des Webportals für die Übergabe von benutzerdefinierten Authentifizierungscookies](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md).
  
10. Speichern Sie die Datei.  
  
11. Wenn Sie eine horizontal skalierte Bereitstellung konfiguriert haben, wiederholen Sie alle vorherigen Schritte für andere in der Bereitstellung vorhandene Berichtsserver.  
  
12. Starten Sie den Berichtsserver neu, um alle momentan geöffneten Sitzungen zu beenden.  

## <a name="see-also"></a>Weitere Informationen

[Implementieren von Sicherheitserweiterungen](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
[Benutzerdefiniertes Reporting Services-Sicherheitsbeispiel (GitHub)](https://github.com/Microsoft/Reporting-Services/tree/master/CustomSecuritySample)  
[Authentifizierung mit dem Berichtsserver](../../reporting-services/security/authentication-with-the-report-server.md)   
[RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Konfigurieren der Standardauthentifizierung auf dem Berichtsserver](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
[Configure Windows Authentication on the Report Server (Konfigurieren der Windows-Authentifizierung auf dem Berichtsserver)](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
Haben Sie dazu Fragen? [Besuchen Sie das Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)