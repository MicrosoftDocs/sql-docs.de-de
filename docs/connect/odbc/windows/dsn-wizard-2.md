---
description: Bildschirm 2 des Datenquellen-Assistenten (ODBC Driver for SQL Server)
title: Datenquellen-Assistent – Bildschirm 2 (ODBC Driver for SQL Server) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 74e061e4ac66da2dfdc165e66338f3ffb70387ea
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837287"
---
# <a name="data-source-wizard-screen-2"></a>Datenquellen-Assistent (Bildschirm 2)

Geben Sie die Authentifizierungsmethode an, und richten Sie Einträge des erweiterten Clients von Microsoft SQL Server sowie den Anmeldenamen und das Kennwort ein, die der ODBC Driver for SQL Server zum Herstellen einer Verbindung mit SQL Server verwendet, während die Datenquelle konfiguriert wird.

## <a name="options"></a>Optionen

### <a name="with-integrated-windows-authentication"></a>Mit integrierter Windows-Authentifizierung

Gibt an, dass der Treiber eine sichere (oder vertrauenswürdige) Verbindung mit einer SQL Server-Instanz anfordert. Wenn diese Option aktiviert ist, verwendet SQL Server unabhängig vom aktuellen Anmeldesicherheitsmodus auf dem Server die integrierte Anmeldesicherheit, um Verbindungen mit dieser Datenquelle herzustellen. Alle angegebenen Anmelde-IDs oder Kennwörter werden ignoriert. Der SQL Server-Systemadministrator muss Ihren Windows-Anmeldenamen mit einer SQL Server-Anmelde-ID verknüpft haben (beispielsweise in SQL Server Management Studio).

Optional können Sie einen Dienstprinzipalnamen (Service Principal Name, SPN) für den Server angeben.

### <a name="with-active-directory-integrated-authentication"></a>Mit integrierter Active Directory-Authentifizierung

Gibt an, dass der Treiber sich unter Verwendung von Azure Active Directory bei SQL Server authentifiziert. Wenn diese Option aktiviert ist, verwendet SQL Server unabhängig vom aktuellen Anmeldesicherheitsmodus auf dem Server die integrierte Anmeldesicherheit von Azure Active Directory, um Verbindungen mit dieser Datenquelle herzustellen.

### <a name="with-sql-server-authentication"></a>Mit SQL Server-Authentifizierung

Gibt an, dass der Treiber sich mit einer Anmelde-ID und dem zugehörigen Kennwort bei SQL Server authentifiziert.

### <a name="with-active-directory-password-authentication"></a>Mit Active Directory-Kennwortauthentifizierung

Gibt an, dass der Treiber sich mit einer Azure Active Directory-Anmelde-ID und dem zugehörigen Kennwort bei SQL Server authentifiziert.

### <a name="with-active-directory-interactive-authentication"></a>Mit interaktiver Active Directory-Authentifizierung

Gibt an, dass der Treiber sich mit dem interaktiven Azure Active Directory-Modus und der Angabe einer Anmelde-ID bei SQL Server authentifiziert. Durch diese Option wird das Dialogfeld zur Eingabe der Azure-Authentifizierungsdaten aktiviert.

### <a name="with-managed-identity-authentication"></a>Mit Authentifizierung mittels verwalteter Identitäten

Gibt an, dass der Treiber sich mit einer verwalteten Identität bei SQL Server authentifiziert.

### <a name="with-active-directory-service-principal-authentication"></a>Mit der Active Directory-Authentifizierung mit einem Dienstprinzipal

Gibt an, dass der Treiber sich mit einem Azure Active Directory-Dienstprinzipal bei SQL Server authentifiziert


### <a name="login-id"></a>Login ID

Gibt die Anmelde-ID an, die der Treiber beim Herstellen einer Verbindung mit SQL Server verwendet, wenn eine der folgenden Optionen ausgewählt ist: **Mit SQL Server-Authentifizierung anhand der vom Benutzer eingegebenen Anmelde-ID und des Kennworts** oder **Mit Active Directory-Kennwortauthentifizierung über eine vom Benutzer eingegebene Anmelde-ID und ein Kennwort** oder **Mit interaktiver Active Directory-Authentifizierung anhand einer vom Benutzer eingegebenen Anmelde-ID**. Wenn die Option für die **Authentifizierung mit verwalteten Identitäten** ausgewählt ist, geben Sie die Objekt-ID der verwalteten Identität an, oder geben Sie keinen Wert an, um die Standardidentität zu verwenden. Dieses Feld gilt nur für die Verbindung, die zum Bestimmen der Standardeinstellungen des Servers hergestellt wird. Es gilt nicht für nachfolgende Verbindungen, die mit der Datenquelle hergestellt werden, nachdem sie erstellt wurde, es sei denn, Sie verwenden die Authentifizierung mit einer verwalteten Identität.

### <a name="password"></a>Kennwort

Gibt das Kennwort an, das der Treiber beim Herstellen einer Verbindung mit SQL Server verwendet, wenn eine der folgenden Optionen ausgewählt ist: **Mit SQL Server-Authentifizierung anhand der vom Benutzer eingegebenen Anmelde-ID und des Kennworts** oder **Mit Active Directory-Kennwortauthentifizierung über eine vom Benutzer eingegebene Anmelde-ID und ein Kennwort**. Dieses Feld gilt nur für die Verbindung, die zum Bestimmen der Standardeinstellungen des Servers hergestellt wird. Es gilt nicht für nachfolgende Verbindungen, die mit der neuen Datenquelle hergestellt wurden.

Sowohl das Feld für **Anmelde-ID** als auch das Feld für **Kennwort** sind deaktiviert, wenn **Mit integrierter Windows-Authentifizierung** oder **Mit integrierter Active Directory-Authentifizierung** ausgewählt ist.

### <a name="next"></a>Nächste

Wechselt zum nächsten Bildschirm des Assistenten

### <a name="back"></a>Zurück

Kehrt zum vorherigen Bildschirm des Assistenten zurück

## <a name="next-steps"></a>Nächste Schritte

[Datenquellen-Assistent (Bildschirm 1)](../../../connect/odbc/windows/dsn-wizard-1.md)

[Datenquellen-Assistent (Bildschirm 3)](../../../connect/odbc/windows/dsn-wizard-3.md)

