---
title: Importieren mit SqlPackage
description: Hier erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe der Import-Aktion von „SqlPackage.exe“ automatisieren. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: cecc6de89a9e8f82a64942acb08af7ddee48c5be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081461"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>Parameter und Eigenschaften für die Import-Aktion von SqlPackage
Durch die Import-Aktion von „SqlPackage.exe“ werden das Schema und die Tabellendaten aus einem BACPAC-Paket (BACPAC-Datei) in eine neue oder leere Datenbank in SQL Server oder Azure SQL-Datenbank importiert. Zum Zeitpunkt des Imports in eine bestehende Datenbank darf die Zieldatenbank keine benutzerdefinierten Schemaobjekte enthalten.  

## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>Parameter für die Import-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|Importieren|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:Import /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“.|
|**/SourceFile:**|**/sf**|{string}|Gibt eine Quelldatei an, die als Quelle für eine Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Quellparameter zulässig. |
|**/TargetConnectionString:**|**/tcs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Zieldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Zielparameter aus. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Gibt eine Außerkraftsetzung für den Namen der Datenbank an, die das Ziel der sqlpackage.exe-Aktion darstellt. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Zieldatenbank SQL-Verschlüsselung verwendet werden soll. |
|**/TargetPassword:**|**/tp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Zieldatenbank. |
|**/TargetServerName:**|**/tsn**|{string}|Definiert den Namen des Servers, auf dem die Zieldatenbank gehostet wird. |
|**/TargetTimeout:**|**/tt**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Zieldatenbank an. Für Azure AD wird empfohlen, dass dieser Wert größer oder gleich 30 Sekunden ist.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Zieldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/TargetUser:**|**/tu**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Zieldatenbank. |
|**/TenantId:**|**/tid**|{string}|Repräsentiert die Azure AD-Mandanten-ID oder den Domänennamen. Diese Option ist für die Unterstützung von Gastbenutzern oder importierten Azure AD-Benutzern sowie für Microsoft-Konten wie beispielsweise outlook.com, hotmail.com oder live.com erforderlich. Bei Auslassen dieses Parameters wird die Standardmandanten-ID für Azure AD verwendet. Hierbei wird davon ausgegangen, dass der authentifizierte Benutzer ein nativer Benutzer dieser AD-Instanz ist. In diesem Fall werden jedoch Gastbenutzer oder importierte Benutzer und/oder Microsoft-Konten, die in dieser Azure AD-Instanz gehostet werden, nicht unterstützt, und der Vorgang ist nicht erfolgreich. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Gibt an, ob die universelle Authentifizierung verwendet werden soll. Bei Festlegung auf TRUE wird das interaktive Authentifizierungsprotokoll aktiviert, das MFA unterstützt. Diese Option kann auch für die Azure AD-Authentifizierung ohne MFA verwendet werden. Hierbei wird ein interaktives Protokoll verwendet, bei dem der Benutzer seinen Benutzernamen und sein Kennwort oder eine integrierte Authentifizierung (Windows-Anmeldeinformationen) eingeben muss. Bei Festlegung von „/UniversalAuthentication“ auf TRUE kann in „SourceConnectionString“ (/scs) keine Azure AD-Authentifizierung angegeben werden. Bei Festlegung von „/UniversalAuthentication“ auf FALSE muss in „SourceConnectionString“ (/scs) die Azure AD-Authentifizierung angegeben werden. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="properties-specific-to-the-import-action"></a>Spezifische Eigenschaften für die Import-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definiert die Edition einer Azure SQL-Datenbank.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definiert die maximale Größe einer Azure SQL-Datenbank in GB.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definiert die Leistungsstufe einer Azure SQL-Datenbank, z.B. „P0“ oder „S1“.|
|**/p:**|ImportContributorArguments=(STRING)|Gibt Bereitstellungs-Contributor-Argumente für die Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln.|
|**/p:**|ImportContributors=(STRING)|Gibt die Bereitstellungs-Contributors an, die beim Importieren der BACPAC ausgeführt werden sollen. Dabei sollte es sich um eine Liste der Namen oder IDs der vollqualifizierten Erstellungs-Contributors mit Semikolatrennung handeln.|
|**/p:**|ImportContributorPaths=(STRING)|Gibt Pfade zum Laden zusätzlicher Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln. |
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|Storage=({File&#124;Memory})|Gibt an, wie Elemente gespeichert werden, wenn das Datenbankmodell erstellt wird. Die Standardeinstellung lautet aus Leistungsgründen InMemory. Für große Datenbanken ist die dateigestützte Speicherung erforderlich.|
  

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)