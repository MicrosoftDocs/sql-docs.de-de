---
title: Microsoft ODBC Driver for SQL Server
description: Der Microsoft ODBC Driver for SQL Server ermöglicht Konnektivität mit SQL Server und Azure SQL-Datenbank über standardmäßige ODBC-APIs.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464181"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC ist die primäre native Datenzugriffs-API für in C und C++ geschriebene Anwendungen für SQL Server. Für die meisten Datenquellen steht ein ODBC-Treiber zur Verfügung. Andere Programmiersprachen, die ODBC verwenden können, sind u.a. COBOL, Perl, PHP und Python. ODBC ist in Datenintegrationsszenarios weit verbreitet.

Der ODBC-Treiber stellt Tools wie [**sqlcmd**](../../tools/sqlcmd-utility.md) und [**bcp**](../../tools/bcp-utility.md) bereit. Mit dem Hilfsprogramm **sqlcmd** können Sie Transact-SQL-Anweisungen, Systemprozeduren und SQL-Skripts ausführen. Mit dem Hilfsprogramm **bcp** werden Daten zwischen einer Microsoft SQL Server-Instanz und einer Datendatei in einem von Ihnen definierten Format massenkopiert. Mit **bcp** können Sie viele neue Zeilen in SQL Server-Tabellen importieren oder Daten aus Tabellen in Datendateien exportieren.  

## <a name="code-example-in-c"></a>Codebeispiel in C++

Das folgende C++-Beispiel zeigt, wie die ODBC-APIs verwendet werden, um eine Verbindung mit einer Datenbank herzustellen und darauf zuzugreifen:

- [C++-Codebeispiel unter Verwendung von ODBC](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>Download

- ![Download-Abwärtspfeil-eingekreist](../../ssms/media/download-icon.png)[So wird der ODBC-Treiber heruntergeladen](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>Dokumentation

### <a name="features"></a>Features

- [Benutzerdefinierte Keystore-Anbieter](../../connect/odbc/custom-keystore-providers.md)
- [Datenklassifizierung](../../connect/odbc/data-classification.md)
- [Schlüsselwörter und Attribute von DNS- und Verbindungszeichenfolgen](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md) (Die verfügbaren Features gelten mit Ausnahme von OLEDB auch für den ODBC-Treiber für SQL Server)
- [Verwendung von Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [Verwendung von Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [Verwenden der transparenten Netzwerk-IP-Adressauflösung](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [Verwenden von XA-Transaktionen](../../connect/odbc/use-xa-with-dtc.md)

### <a name="linux-and-macos"></a>Linux und macOS

- [Installieren des Treibers unter Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Installieren des Treibers unter macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [Connecting to SQL Server (Herstellen einer Verbindung mit SQL Server)](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [Herstellen einer Verbindung mit **bcp**](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [Herstellen einer Verbindung mit **sqlcmd**](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [Datenzugriffsablaufverfolgung](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [Häufig gestellte Fragen](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [Installieren des Treiber-Managers](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [Bekannte Probleme](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [Programmierrichtlinien](../../connect/odbc/linux-mac/programming-guidelines.md)
- [Versionsanmerkungen](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [Versionshinweise (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [Unterstützung für Hochverfügbarkeit und Notfallwiederherstellung](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [Nutzung der Integrierten Authentifizierung (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [Beispiel für asynchrone Ausführung (Benachrichtigungsmethode)](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Verbindungsresilienz im Windows ODBC-Treiber](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [Treiberfähiges Verbindungspooling](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [Features und Behavior Changes](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [Release Notes for ODBC to SQL Server on Windows (Versionshinweise zu ODBC für SQL Server unter Windows)](windows/release-notes-odbc-sql-server-windows.md)
- [Systemanforderungen, Installation und Treiberdateien](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>Community

- [Blog zu SQL Server-Treibern](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [SQL Server-Datenzugriffsforum](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
