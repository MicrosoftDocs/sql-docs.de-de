---
title: Protokolle für MSSQLSERVER-Eigenschaften (Registerkarte "Erweitert")
description: Hier erfahren Sie mehr über die Vorteile und Anforderungen des Features „Erweiterter Schutz für die Authentifizierung“ für die SQL Server-Datenbank-Engine. Dabei wird gezeigt, wie Sie dieses aktivieren und konfigurieren.
ms.custom: seo-lt-2019
ms.date: 01/22/2021
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 34858dd9fa8e155c93ca486bd62499c36c302e31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349711"
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Protokolle für MSSQLSERVER-Eigenschaften (Registerkarte "Erweitert")

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Mit der Registerkarte **Erweitert** im Dialogfeld **Protokolle für MSSQLSERVER-Eigenschaften** können Sie das Feature **Erweiterter Schutz für die Authentifizierung** für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] konfigurieren. **Erweiterter Schutz** ist eine Funktion der vom Betriebssystem implementierten Netzwerkkomponenten. **Erweiterter Schutz** ist in Windows 7 und Windows Server 2008 R2 verfügbar und in Service Packs für ältere Betriebssysteme enthalten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist sicherer, wenn Verbindungen möglichst mithilfe des **erweiterten Schutzes** hergestellt werden. Einige Funktionen von **Erweiterter Schutz** setzen die Auswahl von **Verschlüsselung erzwingen** auf der Registerkarte **Flags** voraus.

> [!IMPORTANT]  
> **Erweiterter Schutz** ist in Windows standardmäßig nicht aktiviert. Informationen zum Aktivieren des **erweiterten Schutzes** finden Sie in den folgenden Artikeln:
> - [Erweiterter Schutz für Windows \<extendedProtection\>](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)
> - [Erweiterter Schutz für die Authentifizierung](/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview)

Weitere Informationen zum Konfigurieren anderer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste finden Sie unter [Verwalten der Datenbank-Engine-Dienste](../../database-engine/configure-windows/manage-the-database-engine-services.md). Eine umfassende Beschreibung des erweiterten Schutzes finden Sie unter [Herstellen einer Verbindung mit der Datenbank-Engine unter Verwendung von Erweiterter Schutz](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md).

**Erweiterter Schutz** wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ab [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]vollständig unterstützt. Für andere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clientanbieter wird **Erweiterter Schutz** derzeit nicht unterstützt.

## <a name="options"></a>Tastatur

### <a name="extended-protection"></a>wird von

Es gibt drei mögliche Werte:  

- **Off**: bedeutet, dass der **erweiterte Schutz** deaktiviert ist. Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz akzeptiert Verbindungen von jedem beliebigen Client, unabhängig davon, ob er geschützt ist oder nicht. **Aus** ist mit älteren und nicht gepatchten Betriebssystemen kompatibel, bietet aber weniger Sicherheit. Verwenden Sie diese Einstellung nur, wenn Sie wissen, dass die Clientbetriebssysteme keinen erweiterten Schutz unterstützen.

- **Zulässig**: Bedeutet, dass **Erweiterter Schutz** für Verbindungen von Betriebssystemen vorausgesetzt wird, die die Funktion **Erweiterter Schutz** unterstützen. Verbindungen von ungeschützten Clientanwendungen, die auf geschützten Clientbetriebssystemen ausgeführt werden, werden abgelehnt. **Erweiterter Schutz** wird für Verbindungen von ungeschützten Betriebssystemen ignoriert. Diese Einstellung ist sicherer als **Aus**, bietet jedoch nicht die höchste Sicherheit. Verwenden Sie diese Einstellung in gemischten Umgebungen, in denen einige Betriebssysteme oder Anwendungen die Funktion **Erweiterter Schutz** unterstützen, andere jedoch nicht.

- **Erforderlich**: bedeutet, dass für eine Verbindung von einer geschützten Anwendung auf einem geschützten Betriebssystem ausgehen muss, damit sie akzeptiert wird. Diese Einstellung stellt die sicherste der drei Optionen dar. Verbindungen die jedoch von Betriebssystemen stammen, die den **erweiterten Schutz** nicht unterstützen, können keine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen.

### <a name="accepted-ntlm-spns"></a>Akzeptierte NTLM-SPNs

Eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz kann durch mehr als einem NTLM-Dienstprinzipalnamen identifiziert werden. Sie können die Dienstprinzipalnamen in durch Semikolons getrennte Zeichenfolgen aufführen. Der Wert **MSSQLSvc/HostName1.Contoso.com;MSSQLSvc/HostName2.Contoso.com** zeigt beispielsweise an, dass Clients, die versuchen, eine Verbindung mit SPNs mit dem Namen **MSSQLSvc/HOST1.Contoso.com** oder **MSSQLSvc/HOST2.Contoso.com** herzustellen, zulässig sind. Die maximale Länge der Variablen beträgt 2048 Zeichen.

## <a name="see-also"></a>Weitere Informationen

[Extended Protection for Authentication with Reporting Services (Erweiterter Schutz für die Authentifizierung mit Reporting Services)](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)