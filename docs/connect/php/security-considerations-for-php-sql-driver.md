---
description: Sicherheitsüberlegungen für die Microsoft-Treiber für PHP für SQL Server
title: Sicherheitsüberlegungen für die Microsoft-Treiber für PHP für SQL Server | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- security considerations
ms.assetid: a8c1a570-9204-454f-b94c-ba34f54d487c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 36c5826b81c68229c5c5bffba7f19d17187447a5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88414386"
---
# <a name="security-considerations-for-the-microsoft-drivers-for-php-for-sql-server"></a>Sicherheitsüberlegungen für die Microsoft-Treiber für PHP für SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Dieses Thema beschreibt Sicherheitsüberlegungen speziell für die Entwicklung, die Bereitstellung und den Betrieb von Anwendungen, die [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]verwenden. Ausführlichere Informationen zum Thema „SQL Server-Sicherheit“ finden Sie unter [Übersicht über die SQL Server-Sicherheit](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/overview-of-sql-server-security).  
  
## <a name="connect-using-windows-authentication"></a>Herstellen einer Verbindung mithilfe der Windows-Authentifizierung  
Aus folgenden Gründen sollte, wann immer möglich, die Windows-Authentifizierung für die Verbindung zu SQL Server verwendet werden:  
  
-   **Während der Authentifizierung werden keine Anmeldeinformationen über das Netzwerk übergeben.** Benutzernamen und Kennwörter werden nicht in die Verbindungszeichenfolge der Datenbank eingebettet. Deshalb können böswillige Benutzer oder Angreifer die Anmeldeinformationen nicht durch Überwachen des Netzwerks oder durch Einsehen der Verbindungszeichenfolgen in Konfigurationsdateien abrufen.  
  
-   **Die Benutzer werden über eine zentralisierte Kontoverwaltung verwaltet** Sicherheitsrichtlinien, wie z. B. Gültigkeitszeiträume für Passwörter, Vorgaben zur Mindestlänge von Passwörtern und die Sperrung von Konten nach mehreren ungültigen Anmeldeanfragen werden umgesetzt.  
  
Informationen zum Herstellen einer Verbindung mit einem Server mithilfe der Windows-Authentifizierung finden Sie unter [Vorgehensweise: Herstellen einer Verbindung mithilfe der Windows-Authentifizierung](../../connect/php/how-to-connect-using-windows-authentication.md).  
  
Wenn Sie eine Verbindung mithilfe der Windows-Authentifizierung herstellen, sollten Sie Ihre Umgebung so konfigurieren, dass SQL Server das Kerberos-Authentifizierungsprotokoll verwenden kann. Weitere Informationen finden Sie unter [How to make sure that you are using Kerberos authentication when you create a remote connection to an instance of SQL Server 2005 (So stellen Sie sicher, dass Sie Kerberos-Authentifizierung verwenden, wenn Sie eine Remoteverbindung mit einer Instanz von SQL Server 2005 erstellen)](https://support.microsoft.com/en-ca/help/909801/how-to-make-sure-that-you-are-using-kerberos-authentication-when-you-c) oder unter [Kerberos-Authentifizierung und SQL Server](https://msdn.microsoft.com/library/cc280744.aspx).  
  
## <a name="use-encrypted-connections-when-transferring-sensitive-data"></a>Verwenden Sie verschlüsselte Verbindungen, wenn Sie sensible Daten übertragen.  
Sie sollten immer verschlüsselte Verbindungen verwenden, wenn der SQL Server sensible Daten empfangen/senden soll. Informationen zum Aktivieren verschlüsselter Verbindungen finden Sie unter [How to Enable Encrypted Connections to the Database Engine (SQL Server Configuration Manager) (Aktivieren von verschlüsselten Verbindungen für die Datenbank-Engine (SQL Server-Konfigurations-Manager))](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md). Zum Herstellen einer sicheren Verbindung mit dem [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], verwenden Sie das Attribut „Encrypt connection“ beim Verbinden mit dem Server. Weitere Informationen zu Verbindungsattributen finden Sie unter [Connection Options](../../connect/php/connection-options.md).  
  
## <a name="use-parameterized-queries"></a>Verwenden parametrisierter Abfragen  
Verwenden Sie parametrisierte Abfragen, um das Risiko von Angriffen durch Einschleusung von SQL-Befehlen zu senken. Beispiele für die Ausführung parametrisierter Abfragen finden Sie unter [How to: Perform Parameterized Queries](../../connect/php/how-to-perform-parameterized-queries.md).  
  
Weitere Informationen zu Angriffen durch Einschleusung von SQL-Befehlen und Sicherheitsüberlegungen dazu finden Sie unter [Einschleusen von SQL-Befehlen](https://msdn.microsoft.com/library/ms161953.aspx).  
  
## <a name="do-not-accept-server-or-connection-string-information-from-end-users"></a>Akzeptieren Sie KEINE Server- oder Verbindungszeichenfolgeinformationen von Endbenutzern.  
Schreiben Sie Anwendungen so, dass Endbenutzer keine Server- oder Verbindungszeichenfolgeinformationen an die Anwendungen übermitteln können. Sie können die Angriffsfläche für böswillige Aktivitäten reduzieren, indem Sie strikte Kontrolle über die Server- und Verbindungszeichenfolgeinformationen behalten.  
  
## <a name="turn-warningsaserrors-on-during-application-development"></a>Aktivieren von „WarningsAsErrors“ während der Anwendungsentwicklung  
Legen Sie während der Anwendungsentwicklung für den Schalter „ **WarningsAsErrors** “ den Wert **true** fest, damit vom Treiber ausgegebenen Warnungen wie Fehler behandelt werden. Dadurch können Sie Warnungen angehen, bevor Sie Ihre Anwendung bereitstellen. Weitere Informationen finden Sie unter [Handling Errors and Warnings](../../connect/php/handling-errors-and-warnings.md).  
  
## <a name="secure-logs-for-deployed-application"></a>Sichere Protokolle für bereitgestellte Anwendungen  
Stellen Sie sicher, dass die Protokolle bereitgestellter Anwendungen an einem sicheren Ort gespeichert werden, oder dass die Protokollierung deaktiviert ist. Dies schützt gegen mögliche Zugriffe von Endbenutzern auf Informationen, die in die Protokolldateien geschrieben wurden. Weitere Informationen finden Sie unter [Logging Activity](../../connect/php/logging-activity.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Programmierhandbuch für die Microsoft-Treiber für PHP für SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
  
