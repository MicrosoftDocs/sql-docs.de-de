---
title: Active Directory-Authentifizierung für SQL Server für Linux
titleSuffix: SQL Server
description: Dieser Artikel enthält eine Übersicht über Active Directory-Authentifizierung für SQL Server für Linux.
ms.date: 04/01/2019
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: da2de7b66923ce82813fd8863556ac08b422bded
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350957"
---
# <a name="active-directory-authentication-for-sql-server-on-linux"></a>Active Directory-Authentifizierung für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dieser Artikel enthält eine Übersicht über Active Directory-Authentifizierung (AD-Authentifizierung) für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux. AD-Authentifizierung wird in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auch als integrierte Authentifizierung bezeichnet.

## <a name="ad-authentication-overview"></a>AD-Authentifizierung: Übersicht

AD-Authentifizierung ermöglicht es domäneneingebundenen Clients unter Windows oder Linux, sich mit ihren Domänenanmeldeinformationen und dem Kerberos-Protokoll bei [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu authentifizieren.

AD-Authentifizierung hat die folgenden Vorteile gegenüber [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Authentifizierung:

- Benutzer werden über einmaliges Anmelden authentifiziert, ohne dass sie zur Eingabe eines Kennworts aufgefordert werden.
- Durch Erstellen von Anmeldungen für AD-Gruppen können Sie Zugriff und Berechtigungen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] über AD-Gruppenmitgliedschaften verwalten.  
- Jeder Benutzer hat eine einzige Identität in Ihrer Organisation, sodass Sie nicht nachverfolgen müssen, welche [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Anmeldungen welchen Personen entsprechen.   
- AD ermöglicht es Ihnen, eine zentralisierte Kennwortrichtlinie in Ihrer gesamten Organisation zu erzwingen.

## <a name="configuration-steps"></a>Konfigurationsschritte

Um Active Directory-Authentifizierung verwenden zu können, benötigen Sie einen AD-Domänencontroller (Windows) in Ihrem Netzwerk.

Ausführliche Informationen dazu, wie AD-Authentifizierung konfiguriert wird, finden Sie im [Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux](sql-server-linux-active-directory-authentication.md). Die folgende Liste entspricht eine Zusammenfassung mit einem Link zu jedem Abschnitt des Tutorials:

1. [Verknüpfen eines Hosts für SQL Server für Linux mit einer Active Directory-Domäne](sql-server-linux-active-directory-join-domain.md)
1. [Erstellen eines AD-Benutzers für SQL Server und Festlegen des Dienstprinzipalnamens](sql-server-linux-active-directory-authentication.md#createuser).
1. [Konfigurieren der KEYTAB-Datei für den SQL Server-Dienst](sql-server-linux-active-directory-authentication.md#configurekeytab)
1. [Schützen der KEYTAB-Datei](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Konfigurieren von SQL Server für die Verwendung der KEYTAB-Datei für die Kerberos-Authentifizierung](sql-server-linux-active-directory-authentication.md#configurekeytab)
1. [Erstellen von AD-basierten SQL Server-Anmeldeinformationen in Transact-SQL](sql-server-linux-active-directory-authentication.md#createsqllogins)
1. [Herstellen einer Verbindung mit SQL Server über die AD-Authentifizierung](sql-server-linux-active-directory-authentication.md#connect)

## <a name="known-issues"></a>Bekannte Probleme

- Derzeit ist CERTIFICATE die einzige unterstützte Authentifizierungsmethode für einen Datenbankspiegelungs-Endpunkt. Die WINDOWS-Authentifizierungsmethode wird in einer zukünftigen Version aktiviert.
- SQL Server für Linux unterstützt das NTLM-Protokoll nicht für Remoteverbindungen. Für lokale Verbindungen kann NTLM möglicherweise verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Implementieren der Active Directory-Authentifizierung für SQL Server für Linux finden Sie im [Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux](sql-server-linux-active-directory-authentication.md).
