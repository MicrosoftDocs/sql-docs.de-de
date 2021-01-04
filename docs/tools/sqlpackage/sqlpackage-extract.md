---
title: Extrahieren mit SqlPackage
description: Hier erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe der Extract-Aktion von „SqlPackage.exe“ automatisieren. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: abefb39814213426d863fa3839c4095eadc82249
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577850"
---
# <a name="sqlpackage-extract-parameters-and-properties"></a>Parameter und Eigenschaften für die Extract-Aktion von SqlPackage
Die Extract-Aktion von „SqlPackage.exe“ erstellt ein Schema einer Livedatenbank von SQL Server oder Azure SQL-Datenbank in einem DACPAC-Paket (DACPAC-Datei). Standardmäßig sind in der DACPAC-Datei keine Daten enthalten. Verwenden Sie die [Exportaktion](sqlpackage-export.md), wenn die Daten mit einbezogen werden sollen. 

## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Wenn eine Datenbank mit Kennwortanmeldeinformationen (z. B. ein SQL-Authentifizierungsbenutzer) extrahiert wird, wird das Kennwort durch ein anderes Kennwort mit geeigneter Komplexität ersetzt. SqlPackage- oder DacFx-Benutzer sollten das Kennwort ändern, nachdem die DACPAC-Datei veröffentlicht wurde.

## <a name="parameters-for-the-extract-action"></a>Parameter für die Extract-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|Extract|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:Extract /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“. |
|**/SourceConnectionString:**|**/scs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Quelldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Quellparameter aus. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definiert den Namen der Quelldatenbank. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Quelldatenbank die SQL-Verschlüsselung verwendet werden soll. |
|**/SourcePassword:**|**/sp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Quelldatenbank. |
|**/SourceServerName:**|**/ssn**|{string}|Definiert den Namen des Servers, auf dem die Quelldatenbank gehostet wird. |
|**/SourceTimeout:**|**/st**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Quelldatenbank an. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Quelldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/SourceUser:**|**/su**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Quelldatenbank. |
|**/TargetFile:**|**/tf**|{string}| Gibt eine Zieldatei (d. h. eine DACPAC-Datei) an, die anstelle einer Datenbank als Ziel der Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Zielparameter zulässig. Dieser Parameter ist für Aktionen ungültig, die ausschließlich Datenbankziele unterstützen.| 
|**/TenantId:**|**/tid**|{string}|Repräsentiert die Azure AD-Mandanten-ID oder den Domänennamen. Diese Option ist für die Unterstützung von Gastbenutzern oder importierten Azure AD-Benutzern sowie für Microsoft-Konten wie beispielsweise outlook.com, hotmail.com oder live.com erforderlich. Bei Auslassen dieses Parameters wird die Standardmandanten-ID für Azure AD verwendet. Hierbei wird davon ausgegangen, dass der authentifizierte Benutzer ein nativer Benutzer dieser AD-Instanz ist. In diesem Fall werden jedoch Gastbenutzer oder importierte Benutzer und/oder Microsoft-Konten, die in dieser Azure AD-Instanz gehostet werden, nicht unterstützt, und der Vorgang ist nicht erfolgreich. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Gibt an, ob die universelle Authentifizierung verwendet werden soll. Bei Festlegung auf TRUE wird das interaktive Authentifizierungsprotokoll aktiviert, das MFA unterstützt. Diese Option kann auch für die Azure AD-Authentifizierung ohne MFA verwendet werden. Hierbei wird ein interaktives Protokoll verwendet, bei dem der Benutzer seinen Benutzernamen und sein Kennwort oder eine integrierte Authentifizierung (Windows-Anmeldeinformationen) eingeben muss. Bei Festlegung von „/UniversalAuthentication“ auf TRUE kann in „SourceConnectionString“ (/scs) keine Azure AD-Authentifizierung angegeben werden. Bei Festlegung von „/UniversalAuthentication“ auf FALSE muss in „SourceConnectionString“ (/scs) die Azure AD-Authentifizierung angegeben werden. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="properties-specific-to-the-extract-action"></a>Spezifische Eigenschaften für die Extract-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.|
|**/p:**|DacApplicationDescription=(STRING)|Definiert die in den DACPAC-Metadaten zu speichernde Anwendungsbeschreibung.|
|**/p:**|DacApplicationName=(STRING)|Definiert den in den DACPAC-Metadaten zu speichernden Anwendungsnamen. Der Standardwert entspricht dem Datenbanknamen.|
|**/p:**|DacMajorVersion=(INT32 '1')|Definiert die in den DACPAC-Metadaten zu speichernde Hauptversion.|
|**/p:**|DacMinorVersion=(INT32 '0')|Definiert die in den DACPAC-Metadaten zu speichernde Nebenversion.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|ExtractAllTableData=(BOOLEAN)|Gibt an, ob Daten aus allen Benutzertabellen extrahiert werden. Bei Festlegung auf TRUE werden Daten aus allen Benutzertabellen extrahiert, und es ist nicht möglich, einzelne Benutzertabellen für das Extrahieren von Daten anzugeben. Bei Festlegung auf FALSE können Sie eine oder mehrere Benutzertabellen zum Extrahieren von Daten angeben.|
|**/p:**|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')|Beim Wert "true" werden nur Objekte im Anwendungsbereich für die angegebene Quelle extrahiert. Beim Wert "false" werden alle Objekte für die angegebene Quelle extrahiert.|
|**/p:**|ExtractReferencedServerScopedElements=(BOOLEAN 'True')|Beim Wert TRUE werden Anmelde-, Serverüberwachungs- und Anmeldeinformationsobjekte extrahiert, auf die von Quelldatenbankobjekten verwiesen wird.|
|**/p:**|ExtractUsageProperties=(BOOLEAN)|Gibt an, ob Einsatzeigenschaften wie die Anzahl von Tabellenzeilen und Indexgröße aus der Datenbank extrahiert werden.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Gibt an, ob erweiterte Eigenschaften ignoriert werden sollen.|
|**/p:**|IgnorePermissions=(BOOLEAN 'True')|Gibt an, ob Berechtigungen ignoriert werden sollen.|
|**/p:**|IgnoreUserLoginMappings=(BOOLEAN)|Gibt an, ob Beziehungen zwischen Benutzern und Anmeldenamen ignoriert werden.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Gibt den Typ des Hintergrundspeichers an, der während der Extraktion für das Schemamodell verwendet wird.|
|**/p:**|TableData=(STRING)|Gibt die Tabelle an, aus der Daten extrahiert werden. Geben Sie den Tabellennamen mit oder ohne Klammern um die Namensteile im folgenden Format an: Schemaname.Tabellen-ID. Diese Option kann mehrfach angegeben werden.|
|**/p:**| TempDirectoryForTableData=(STRING)|Gibt das temporäre Verzeichnis an, das zum Puffern von Tabellendaten verwendet wird, bevor diese in die Paketdatei geschrieben werden.|
|**/p:**|VerifyExtraction=(BOOLEAN)|Gibt an, ob die extrahierte DACPAC-Datei überprüft werden soll.|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)