---
title: Exportieren mit SqlPackage
description: Hier erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe der Export-Aktion von „SqlPackage.exe“ automatisieren. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 942d0c84e697654e67d48ce708792eb8bcf770aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081481"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>Parameter und Eigenschaften für die Export-Aktion von SqlPackage
Mit der Aktion „SqlPackage.exe Export“ wird eine verbundene Datenbank in eine BACPAC-Datei (.bacpac) exportiert. Standardmäßig sind die Daten für alle Tabellen in der BACPAC-Datei enthalten. Optional können Sie auch nur eine Teilmenge der Tabellen angeben, für die Daten exportiert werden sollen. Die Überprüfung der Exportaktion stellt die Kompatibilität von Azure-SQL-Datenbank mit der vollständigen Zieldatenbank sicher, auch wenn nur eine Teilmenge der Tabellen für den Export angegeben wird. 

## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>Parameter für die Export-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|Exportieren|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:Export /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“.|
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

## <a name="properties-specific-to-the-export-action"></a>Spezifische Eigenschaften für die Export-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Gibt den Typ des Hintergrundspeichers an, der während der Extraktion für das Schemamodell verwendet wird.|
|**/p:**|TableData=(STRING)|Gibt die Tabelle an, aus der Daten extrahiert werden. Geben Sie den Tabellennamen mit oder ohne Klammern um die Namensteile im folgenden Format an: Schemaname.Tabellen-ID. Diese Option kann mehrfach angegeben werden.|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|Gibt an, wie die Ziel-Engine-Version erwartet wird. Dies hat Auswirkungen darauf, ob Objekte zugelassen werden, die von Microsoft Azure SQL-Datenbank-Servern mit V12-Funktionen unterstützt werden, z. B. speicheroptimierte Tabellen in der generierten BACPAC.|
|**/p:**|TempDirectoryForTableData=(STRING)|Gibt das temporäre Verzeichnis an, das zum Puffern von Tabellendaten verwendet wird, bevor diese in die Paketdatei geschrieben werden.|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Gibt an, ob die unterstützten Volltextdokumenttypen für Microsoft Azure SQL-Datenbank Version 12 überprüft werden sollen|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)