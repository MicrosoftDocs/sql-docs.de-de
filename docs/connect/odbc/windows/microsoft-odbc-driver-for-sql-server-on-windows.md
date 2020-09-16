---
description: Microsoft ODBC Driver for SQL Server on Windows
title: Microsoft ODBC Driver for SQL Server unter Windows | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b10cfc22-6a2c-4707-a456-0dcec317982b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64a2c7ff06534865c4105ab28c2f3bc42b783de3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462236"
---
# <a name="microsoft-odbc-driver-for-sql-server-on-windows"></a>Microsoft ODBC Driver for SQL Server on Windows
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Microsoft ODBC-Treiber für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sind eigenständige ODBC-Treiber, die eine Anwendungsprogrammierschnittstelle (Application Programming Interface, API) bieten, die die ODBC-Standardschnittstellen für Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] implementiert.

Der Microsoft ODBC Driver for SQL Server kann verwendet werden, um neue Anwendungen zu erstellen. Außerdem können Sie Ihre älteren Anwendungen, die zurzeit einen älteren ODBC-Treiber verwenden, aktualisieren. Der ODBC-Treiber für SQL Server unterstützt Verbindungen zu Azure SQL-Datenbank, Azure SQL Data Warehouse und [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

## <a name="summary"></a>Zusammenfassung

| Version       | Unterstützte Features      |
| ------------- |---------------| 
| Microsoft ODBC Driver 17 for SQL Server | <ul><li>Always Encrypted-Unterstützung für die BCP-API</li><li>Das neue Verbindungszeichenfolgenattribut „UseFMTONLY“ bewirkt, dass der Treiber in besonderen Fällen ältere Metadaten verwendet, die temporäre Tabellen erfordern.</li>
| Microsoft ODBC Driver 13.1 for SQL Server     | <ul><li>Always Encrypted</li><li>Azure AD-Authentifizierung</li><li>Always On-Verfügbarkeitsgruppen (Availability Groups, AG)</li></ul>   | 
| Microsoft ODBC Driver 13 for SQL Server      | <ul><li>Internationaler Domänenname (IDN)</li></ul> |
| Microsoft ODBC Driver 11 für SQL Server | <ul><li>Treiberfähiges Verbindungspooling</li><li>Verbindungsstabilität</li><li>Asynchrone Ausführung (Abrufmethode)</li></ul> |    

## <a name="documentation"></a>Dokumentation  
Diese Dokumentation für den Microsoft ODBC Driver für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] enthält:  
  
-   [Release Notes for ODBC to SQL Server on Windows (Versionshinweise zu ODBC für SQL Server unter Windows)](../../../connect/odbc/windows/release-notes-odbc-sql-server-windows.md)  
-   [Funktionen von Microsoft ODBC Driver for SQL Server unter Windows](../../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)  
-   [Systemanforderungen, Installation und Treiberdateien](../../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)  
-   [Treiberfähiges Verbindungspooling im ODBC Driver for SQL Server](../../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)  
-   [Beispiel für asynchrone Ausführung &#40;Benachrichtigungsmethode&#41;](../../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)  
-   [Verbindungsresilienz im Windows ODBC-Treiber](../../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)  
-   [Verwenden von Always Encrypted mit dem ODBC-Treiber](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
-   [Verwenden von Azure Active Directory mit dem ODBC-Treiber](../../../connect/odbc/using-azure-active-directory.md) 
-   [Verwenden der transparenten Netzwerk-IP-Adressauflösung](../../../connect/odbc/using-transparent-network-ip-resolution.md)   

## <a name="community"></a>Community  
- [Blog zu SQL Server-Treibern](https://techcommunity.microsoft.com/t5/sql-server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [SQL Server-Datenzugriffsforum](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
  
## <a name="see-also"></a>Weitere Informationen  
- [Erstellen von Anwendungen mit SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)   
- [FAQ zu SQL Server Native Client](https://msdn.microsoft.com/sqlserver/aa937707.aspx)   
- [ODBC-Programmierreferenz](../../../odbc/reference/odbc-programmer-s-reference.md)   
- [SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
