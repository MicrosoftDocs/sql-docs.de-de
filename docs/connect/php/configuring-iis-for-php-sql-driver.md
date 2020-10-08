---
title: Konfigurieren von IIS für die PHP-Treiber
description: Erfahren Sie, wie IIS zum Hosten von PHP-Anwendungen konfiguriert wird, die die Treiber für PHP für SQL Server verwenden. Die hier aufgelisteten Ressourcen sind spezifisch für die Verwendung von FastCGI mit IIS.
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- configuring, Internet Information Services
ms.assetid: d2dc75d3-9bf7-481c-85f2-8b6310b21461
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7ab31be628d4c93f0e4331e3d63d47c4924a6fa7
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726878"
---
# <a name="configuring-iis-for-the-microsoft-drivers-for-php-for-sql-server"></a>Konfigurieren der IIS für Microsoft-Treiber für PHP für SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Dieses Thema enthält Links zu Ressourcen auf der [Website der Internet-Informationsdienste (IIS)](https://www.iis.net/), die für das Konfigurieren von IIS zum Hosten von PHP-Anwendungen relevant sind. Die hier aufgelisteten Ressourcen sind spezifisch für die Verwendung von FastCGI mit IIS. FastCGI ist ein Standardprotokoll, das den ausführbaren Dateien der gemeinsamen Gatewayschnittstelle (CGI) eines Anwendungsframeworks die Kommunikation mit dem Webserver ermöglicht. FastCGI unterscheidet sich vom CGI-Standardprotokoll in dem Punkt, dass FastCGI die CGI-Prozesse für mehrere Anforderungen verwendet.  
  
## <a name="tutorials"></a>Tutorials  
Die folgenden Links führen zu Tutorials zum Einrichten von FastCGI für PHP und zum Hosten von PHP-Anwendungen in IIS 6.0 und IIS 7.0:  
  
-   [FastCGI mit PHP](/iis/web-hosting/web-server-for-shared-hosting/fastcgi-with-php)  
-   [Die Verwendung von FastCGI zum Hosten von PHP-Anwendungen unter IIS 7.0](/iis/application-frameworks/install-and-configure-php-applications-on-iis/using-fastcgi-to-host-php-applications-on-iis)  
-   [Die Verwendung von FastCGI zum Hosten von PHP-Anwendungen unter IIS 6.0](/iis/application-frameworks/install-and-configure-php-applications-on-iis/using-fastcgi-to-host-php-applications-on-iis-60)  
-   [Konfigurieren einer FastCGI-Erweiterung für IIS 6.0](/iis/application-frameworks/install-and-configure-php-on-iis/configuring-the-fastcgi-extension-for-iis-60)  
  
## <a name="video-presentations"></a>Videopräsentationen  
Die folgenden Links führen zu Videopräsentationen zum Einrichten von FastCGI für PHP und zur Verwendung von IIS 7.0-Features zum Hosten von PHP-Anwendungen:  
  
-   [Einrichten von FastCGI für PHP](/iis/application-frameworks/running-php-applications-on-iis/set-up-fastcgi-for-php)  
-   [Partying with PHP on Microsoft Internet Information Services 7](/iis/application-frameworks/running-php-applications-on-iis/mix08-partying-with-php-on-microsoft-internet-information-services-7-and-above)  
  
## <a name="support-resources"></a>Supportressourcen  
Die folgenden Foren bieten Communityunterstützung für FastCGI unter IIS:  
  
-   [FastCGI-Handler](https://forums.iis.net/1103.aspx)  
-   [IIS 7 – FastCGI-Modul](https://forums.iis.net/1104.aspx)  
  
## <a name="see-also"></a>Weitere Informationen  
[Getting Started with the Microsoft Drivers for PHP for SQL Server (Erste Schritte mit dem Microsoft-Treiber für PHP für SQL Server)](../../connect/php/getting-started-with-the-php-sql-driver.md)

[Programmierhandbuch für die Microsoft-Treiber für PHP für SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)

[Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  
