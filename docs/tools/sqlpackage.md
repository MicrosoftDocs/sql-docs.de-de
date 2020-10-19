---
title: SqlPackage.exe
description: In diesem Artikel erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe von SqlPackage.exe automatisieren können. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: pensivebrian
ms.author: broneill
ms.reviewer: drswkier; sstein
ms.date: 09/29/2020
ms.openlocfilehash: c4a7fb02521a20dffa95c45cc8a345c243c4ae0e
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92005540"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** ist ein Befehlszeilen-Hilfsprogramm, das die folgenden Aufgaben bei der Datenbankentwicklung automatisiert:  
  
- [Extract:](#extract-parameters-and-properties) Erstellt eine Datenbankmomentaufnahme (DACPAC-Datei) von einer SQL Server- oder Azure SQL-Livedatenbank.  
  
- [Veröffentlichen](#publish-parameters-properties-and-sqlcmd-variables): Aktualisiert ein Datenbankschema inkrementell, sodass dieses dem Schema einer DACPAC-Quelldatei entspricht Wenn die Datenbank auf dem Server nicht vorhanden ist, wird sie durch den Veröffentlichungsvorgang erstellt. Andernfalls wird eine vorhandene Datenbank aktualisiert.  
  
- [Export](#export-parameters-and-properties): Exportiert eine Livedatenbank – einschließlich des Datenbankschemas und der Benutzerdaten – aus SQL Server oder Azure SQL-Datenbank in ein BACPAC-Paket (BACPAC-Datei).  
  
- [Import](#import-parameters-and-properties): Importiert das Schema und die Tabellendaten aus einem BACPAC-Paket in eine neue Benutzerdatenbank einer Instanz von SQL Server oder Azure SQL-Datenbank.  
  
- [DeployReport:](#deployreport-parameters-and-properties) Erstellt einen XML-Bericht der Änderungen, die durch eine Veröffentlichungsaktion vorgenommen würden  
  
- [DriftReport](#driftreport-parameters): Erstellt einen XML-Bericht der Änderungen, die seit der letzten Registrierung an einer registrierten Datenbank vorgenommen wurden.  
  
- [Script:](#script-parameters-and-properties) Erstellt ein inkrementelles Transact-SQL-Updateskript, durch das das Schema eines Ziels aktualisiert wird, sodass es dem Schema einer Quelle entspricht.  
  
In der Befehlszeile von **SqlPackage.exe** können diese Aktionen zusammen mit aktionsspezifischen Parametern und Eigenschaften angegeben werden.  

**[Aktuelle Version herunterladen](sqlpackage-download.md)** Weitere Informationen über die neueste Version finden Sie in den [Versionshinweisen](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Anwendungsbeispiele

**Generieren eines Vergleichs zwischen Datenbanken mithilfe von DACPAC-Dateien mit SQL-Skriptpausgabe**

Erstellen Sie zunächst eine DACPAC-Datei aus Ihren letzten Datenbankänderungen:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Erstellen Sie eine DACPAC-Datei Ihres Datenbankziels (das keine Änderungen aufweist):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Erstellen Sie ein SQL-Skript, das die Unterschiede der zwei DACPAC-Dateien generiert:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```

Mit dem folgenden Befehl wird die sqlpackage-Version angezeigt:

```
sqlpackage.exe /Version
 ```


## <a name="extract-parameters-and-properties"></a>Extrahieren von Parametern und Eigenschaften
Durch eine SqlPackage.exe-Extract-Aktion wird ein Schema einer Livedatenbank aus SQL Server bzw. Azure SQL-Datenbank in einem DACPAC-Paket (DACPAC-Datei) erstellt. Standardmäßig sind in der DACPAC-Datei keine Daten enthalten. Verwenden Sie die [Exportaktion](#export-parameters-and-properties), wenn die Daten mit einbezogen werden sollen. 

### <a name="help-for-the-extract-action"></a>Hilfe zur Extract-Aktion

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

### <a name="properties-specific-to-the-extract-action"></a>Spezifische Eigenschaften für die Extract-Aktion

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

## <a name="publish-parameters-properties-and-sqlcmd-variables"></a>Parameter, Eigenschaften und SQLCMD-Variablen für "Publish"

Eine Veröffentlichungsaktion von "SqlPackage.exe" aktualisiert inkrementell das Schema einer Zieldatenbank, sodass dieses der Struktur einer Quelldatenbank entspricht. Durch die Veröffentlichung eines Bereitstellungspakets, das Benutzerdaten für alle oder eine Teilmenge der Tabellen enthält, werden zusätzlich zum Schema auch die Tabellendaten aktualisiert. Das Schema und die Daten in bestehenden Tabellen der Zieldatenbank werden durch die Datenbereitstellung überschrieben. Für Tabellen, die im Bereitstellungspaket nicht enthalten sind, werden das bestehende Schema bzw. die Daten in der Zieldatenbank durch die Datenbereitstellung nicht geändert.  

### <a name="help-for-publish-action"></a>Hilfe zur Publish-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|Veröffentlichen|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/AzureKeyVaultAuthMethod:**|**/akv**|{Interactive&#124;ClientIdSecret}|Gibt an, welche Authentifizierungsmethode für den Zugriff auf Azure Key Vault verwendet werden soll, wenn ein Veröffentlichungsvorgang Änderungen an einer verschlüsselten Tabelle/Spalte umfasst |
|**/ClientId:**|**/cid**|{string}|Gibt die Client-ID an, die zur Authentifizierung bei Azure Key Vault verwendet wird (sofern erforderlich). |
|**/DeployScriptPath:**|**/dsp**|{string}|Hiermit wird ein optionaler Dateipfad zur Ausgabe des Bereitstellungsskripts angegeben. Wenn bei Azure-Bereitstellungen TSQL-Befehle zum Erstellen oder Ändern der Masterdatenbank verwendet werden, wird ein Skript in den gleichen Pfad geschrieben, wobei der Name der Ausgabedatei „Dateiname_Master.sql“ lautet. |
|**/DeployReportPath:**|**/drp**|{string}|Hiermit wird ein optionaler Dateipfad zur Ausgabe der XML-Datei mit dem Bereitstellungsbericht angegeben. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Profile:**|**/pr**|{string}|Gibt den Dateipfad zu einem DAC-Veröffentlichungsprofil an. Im Profil ist eine Auflistung von Eigenschaften und Variablen definiert, die beim Generieren von Ausgaben verwendet werden.|
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:Publish /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“.|
|**/Secret:**|**/secr**|{string}|Gibt das Clientgeheimnis an, das zur Authentifizierung bei Azure Key Vault verwendet wird (sofern erforderlich). |
|**/SourceConnectionString:**|**/scs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Quelldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Quellparameter aus. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definiert den Namen der Quelldatenbank. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Quelldatenbank die SQL-Verschlüsselung verwendet werden soll. |
|**/SourceFile:**|**/sf**|{string}|Gibt eine Quelldatei an, die anstelle einer Datenbank als Quelle für eine Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Quellparameter zulässig. |
|**/SourcePassword:**|**/sp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Quelldatenbank. |
|**/SourceServerName:**|**/ssn**|{string}|Definiert den Namen des Servers, auf dem die Quelldatenbank gehostet wird. |
|**/SourceTimeout:**|**/st**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Quelldatenbank an. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Quelldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/SourceUser:**|**/su**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Quelldatenbank. |
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
|**/Variables:**|**/v**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Variable an: {VariableName}={Value}. Die DACPAC-Datei enthält die Liste gültiger SQLCMD-Variablen. Wenn nicht für jede Variable ein Wert angegeben wird, wird ein Fehler ausgegeben. |

### <a name="properties-specific-to-the-publish-action"></a>Spezifische Eigenschaften für die Publish-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Gibt zusätzliche Bereitstellungs-Contributorargumente für die Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Gibt zusätzliche Bereitstellungs-Contributors an, die beim Bereitstellen des Dacpacs ausgeführt werden sollen. Dabei sollte es sich um eine Liste der Namen oder IDs der vollqualifizierten Erstellungs-Contributors mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Gibt Pfade zum Laden zusätzlicher Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Diese Eigenschaft wird von der SqlClr-Bereitstellung verwendet, damit alle blockierenden Assemblys im Rahmen des Bereitstellungsplans gelöscht werden. Wenn die verweisende Assembly gelöscht werden muss, werden Assemblyupdates durch blockierende oder verweisende Assemblys standardmäßig blockiert.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Gibt an, ob versucht werden soll, die Aktion trotz inkompatibler SQL Server-Plattformen auszuführen.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Wenn diese Eigenschaft auf TRUE festgelegt ist, wird das Verschieben von Daten in einer Tabelle mit Sicherheit auf Zeilenebene nicht blockiert. Der Standardwert ist "false".|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Sichert die Datenbank, bevor Änderungen bereitgestellt werden.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Gibt an, dass der Veröffentlichungszeitraum beendet werden soll, wenn durch den Veröffentlichungsvorgang ein Datenverlust verursacht werden könnte.|
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Gibt an, ob die Aktualisierung einer Datenbank, deren Schema nicht mehr mit der Registrierung übereinstimmt oder aus der Registrierung entfernt wurde, blockiert wird.|
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.|
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Gibt an, ob die SETVAR-Variablendeklaration im generierten Veröffentlichungsskript auskommentiert werden soll. Dies empfiehlt sich beispielsweise, wenn Sie die Werte über die Befehlszeile eingeben möchten und mithilfe eines Tool wie SQLCMD.EXE veröffentlichen.|
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Diese Einstellung bestimmt, wie die Datenbanksortierung während der Bereitstellung behandelt wird. Standardmäßig wird die Sortierung der Zieldatenbank aktualisiert, wenn sie nicht mit der durch die Quelle angegebenen Sortierung übereinstimmt. Wenn diese Option festgelegt ist, sollte die Sortierung der Zieldatenbank (oder des Servers) verwendet werden.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Gibt an, ob die Zieldatenbank beim Veröffentlichen in einer Datenbank aktualisiert bzw. gelöscht und neu erstellt werden soll.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definiert die Edition einer Azure SQL-Datenbank.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')|Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definiert die maximale Größe einer Azure SQL-Datenbank in GB.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definiert die Leistungsstufe einer Azure SQL-Datenbank, z.B. „P0“ oder „S1“.|
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Beim Wert TRUE wird die Datenbank vor der Bereitstellung in den Einzelbenutzermodus geschaltet.|
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')|Gibt an, ob DDL-Trigger (Data Definition Language) am Anfang des Veröffentlichungsprozesses deaktiviert und am Ende der Veröffentlichungsaktion erneut aktiviert werden.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Beim Wert "true" werden Change Data Capture-Objekte nicht geändert.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Gibt an, ob replizierte Objekte während der Überprüfung identifiziert werden.|
|**/p:**|DoNotDropObjectType=(STRING)|Ein Objekttyp, der nicht gelöscht werden soll, wenn „DropObjectsNotInSource“ auf TRUE festgelegt ist. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|DoNotDropObjectTypes=(STRING)|Eine durch Semikolons getrennte Liste von Objekttypen, die nicht gelöscht werden sollen, wenn „DropObjectsNotInSource“ den Wert TRUE besitzt. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Gibt an, ob Einschränkungen, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Gibt an, ob DML-Trigger, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Gibt an, ob erweiterte Eigenschaften, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, aus der Zieldatenbank gelöscht werden, wenn Sie eine Veröffentlichung in einer Datenbank vornehmen.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Gibt an, ob Indizes, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Gibt an, ob Objekte, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden. Dieser Wert hat Vorrang vor „DropExtendedProperties“.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Gibt an, ob Berechtigungen, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Gibt an, ob Rollenmitglieder, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht definiert sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Gibt an, ob Statistiken, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|ExcludeObjectType=(STRING)|Ein Objekttyp, der während der Bereitstellung ignoriert werden soll. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Eine durch Semikolon getrennte Liste der Objekttypen, die während der Bereitstellung ignoriert werden sollen. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Stellt automatisch einen Standardwert bereit, wenn eine Tabelle mit Daten anhand einer Spalte aktualisiert wird, die keine NULL-Werte zulässt.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der ANSI NULLS-Einstellung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Gibt an, ob Unterschiede im Autorisierer beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Gibt an, ob Unterschiede in den Spaltensortierungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Tabellenspaltenreihenfolge beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreComments=(BOOLEAN)|Gibt an, ob Unterschiede in den Kommentaren beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede im Dateipfad für den Kryptografieanbieter beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank oder auf einem Server ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Gibt an, ob Unterschiede im Standardschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Gibt an, ob Unterschiede in den erweiterten Eigenschaften beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Pfaden für Dateien und Protokolldateien beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei der Platzierung von Objekten in FILEGROUPs beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Dateigröße beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Füllfaktor für den Indexspeicher beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Dateipfad für den Volltextkatalog beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Gibt an, ob Unterschiede im Seed für eine Identitätsspalte beim Veröffentlichen von Updates für eine Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Gibt an, ob Unterschiede im Inkrement für eine Identitätsspalte beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Indexoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Indexauffüllung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Groß-/Kleinschreibung von Schlüsselwörtern beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Gibt an, ob Unterschiede in den Sperrhinweisen für Indizes beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Sicherheits-ID (SID) beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Gibt an, ob die Einstellungen für „Nicht für Replikation“ beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Gibt an, ob die Platzierung eines Objekts in einem Partitionsschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden soll.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Gibt an, ob Unterschiede in den Partitionsschemas und -funktionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Gibt an, ob Unterschiede in den Berechtigungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Einstellung für Bezeichner in Anführungszeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Gibt an, ob Unterschiede in den Rollenmitgliedschaften von Anmeldenamen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei dem Zeitraum, über den SQL Server die Route in der Routingtabelle beibehält, beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Semikolons zwischen T-SQL-Anweisungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenpartitionsoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.  Diese Option gilt nur für SQL-Pool-Datenbanken (Data Warehouse) von Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Gibt an, ob Unterschiede in den Benutzereinstellungsobjekten beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Leerzeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für CHECK-Einschränkungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für Fremdschlüssel beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Alle zusammengesetzten Elemente als Teil einer einzigen Veröffentlichungsvorgangs einschließen.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Gibt an, ob beim Veröffentlichen in einer Datenbank nach Möglichkeit Transaktionsanweisungen verwendet werden sollen.|
|**/p:**|LongRunningCommandTimeout=(INT32)|Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Gibt an, dass eine Assembly bei einer Abweichung von der Veröffentlichungsaktion immer gelöscht und neu erstellt werden soll, anstatt eine ALTER ASSEMBLY-Anweisung auszugeben.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Gibt an, ob beim Erstellen einer neuen FileGroup in der Zieldatenbank ebenfalls eine neue Datei erstellt wird.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Gibt an, ob das Schema beim Datenbankserver registriert wird.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Gibt an, ob DeploymentPlanExecutor-Contributors ausgeführt werden sollen, wenn andere Vorgänge ausgeführt werden.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbanksortierung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbankkompatibilität beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Gibt an, ob die Eigenschaften der Zieldatenbank als Teil des Veröffentlichungsvorgangs festgelegt oder aktualisiert werden sollen.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Gibt an, ob Anweisungen im Veröffentlichungsskript generiert werden, um zu überprüfen, ob der Datenbankname und der Servername mit den im Datenbankprojekt angegebenen Namen übereinstimmen.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Steuert, ob beim Hinzufügen einer Datei zu einer Dateigruppe die Größe angegeben wird.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Nach Ende der Veröffentlichung werden alle Bedingungen als ein Satz überprüft, wodurch Datenfehler vermieden werden, die durch eine CHECK- oder FOREIGN KEY-Einschränkung während der Veröffentlichung verursacht werden. Wenn FALSE festgelegt wird, werden Einschränkungen ohne Überprüfung der entsprechenden Daten veröffentlicht.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Schließt Aktualisierungsanweisungen am Ende des Veröffentlichungsskripts ein.|
|**/p:**|Storage=({File&#124;Memory})|Gibt an, wie Elemente gespeichert werden, wenn das Datenbankmodell erstellt wird. Die Standardeinstellung lautet aus Leistungsgründen InMemory. Für große Datenbanken ist die dateigestützte Speicherung erforderlich.|
|**/p:**|Treatverificationerrorsaswarnings = (Boolean)|Gibt an, ob während der Veröffentlichungsüberprüfung aufgetretene Fehler als Warnungen behandelt werden sollen. Die Überprüfung wird für den generierten Bereitstellungsplan ausgeführt, bevor der Plan für Ihre Zieldatenbank ausgeführt wird. Bei der Planüberprüfung werden Probleme erkannt, z. B. der Verlust von reinen Zielobjekten (z. B. Indizes), die gelöscht werden müssen, um eine Änderung vorzunehmen. Bei der Überprüfung werden auch Situationen erkannt, in denen Abhängigkeiten (z. B. eine Tabelle oder Sicht) aufgrund eines Verweises auf ein zusammengesetztes Projekt vorhanden sind, jedoch nicht in der Zieldatenbank vorkommen. Dies ist möglicherweise der Fall, um eine vollständige Liste aller Probleme zu erhalten, statt zu verhindern, dass die Veröffentlichungsaktion beim ersten Fehler beendet wird.
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Gibt an, ob Warnungen generiert werden sollen, wenn Unterschiede in Objekten gefunden werden, die nicht änderbar sind (z. B. wenn die Dateigröße oder Dateipfade für eine Datei unterschiedlich sind).|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Gibt an, ob die Kompatibilität von Sortierungen überprüft wird.|
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Gibt an, ob vor der Veröffentlichung Überprüfungen ausgeführt werden sollen, durch die die Veröffentlichungsaktion beendet wird, wenn Probleme vorliegen, die eine erfolgreiche Veröffentlichung blockieren könnten. Die Veröffentlichungsaktion könnte beispielsweise beendet werden, wenn in der Zieldatenbank Fremdschlüssel enthalten sind, die im Datenbankprojekt nicht vorhanden sind. Dies verursacht Fehler bei der Veröffentlichung.|
|

### <a name="sqlcmd-variables"></a>SQLCMD-Variablen

In der folgenden Tabelle wird das Format der Option beschrieben, mit der Sie den Wert einer SQL-Befehlsvariablen (**sqlcmd**) außer Kraft setzen können, die während einer Veröffentlichungsaktion verwendet wird. Die Werte der in der Befehlszeile angegebenen Variablen überschreiben andere Werte, die der Variablen zugewiesen sind (z. B. Werte in einem Veröffentlichungsprofil).  
  
|Parameter|Standard|BESCHREIBUNG|  
|-------------|-----------|---------------|  
|**/Variables:{PropertyName}={Value}**||Gibt ein Name-Wert-Paar für eine aktionsspezifische Variable an: {VariableName}={Value}. Die DACPAC-Datei enthält die Liste gültiger SQLCMD-Variablen. Wenn nicht für jede Variable ein Wert angegeben wird, wird ein Fehler ausgegeben.|  
  
## <a name="export-parameters-and-properties"></a>Parameter und Eigenschaften für den Export

Durch eine SqlPackage.exe-Exportaktion wird eine Livedatenbank aus SQL Server bzw. Azure SQL-Datenbank in ein BACPAC-Paket (BACPAC-Datei) exportiert. Standardmäßig sind die Daten für alle Tabellen in der BACPAC-Datei enthalten. Optional können Sie auch nur eine Teilmenge der Tabellen angeben, für die Daten exportiert werden sollen. Die Überprüfung der Exportaktion stellt die Kompatibilität von Azure-SQL-Datenbank mit der vollständigen Zieldatenbank sicher, auch wenn nur eine Teilmenge der Tabellen für den Export angegeben wird.  
  
### <a name="help-for-export-action"></a>Hilfe zur Export-Aktion

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

### <a name="properties-specific-to-the-export-action"></a>Spezifische Eigenschaften für die Export-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Gibt den Typ des Hintergrundspeichers an, der während der Extraktion für das Schemamodell verwendet wird.|
|**/p:**|TableData=(STRING)|Gibt die Tabelle an, aus der Daten extrahiert werden. Geben Sie den Tabellennamen mit oder ohne Klammern um die Namensteile im folgenden Format an: Schemaname.Tabellen-ID. Diese Option kann mehrfach angegeben werden.|
|**/p:**|TempDirectoryForTableData=(STRING)|Gibt das temporäre Verzeichnis an, das zum Puffern von Tabellendaten verwendet wird, bevor diese in die Paketdatei geschrieben werden.|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|Gibt an, wie die Ziel-Engine-Version erwartet wird. Dies hat Auswirkungen darauf, ob Objekte zugelassen werden, die von Microsoft Azure SQL-Datenbank-Servern mit V12-Funktionen unterstützt werden, z. B. speicheroptimierte Tabellen in der generierten BACPAC.|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Gibt an, ob die unterstützten Volltextdokumenttypen für Microsoft Azure SQL-Datenbank Version 12 überprüft werden sollen|
  
## <a name="import-parameters-and-properties"></a>Parameter und Eigenschaften für den Import

Durch eine SqlPackage.exe-Importaktion werden das Schema und die Tabellendaten aus einem BACPAC-Paket (BACPAC-Datei) in eine neue oder leere Datenbank in SQL Server oder Azure SQL-Datenbank importiert. Zum Zeitpunkt des Imports in eine bestehende Datenbank darf die Zieldatenbank keine benutzerdefinierten Schemaobjekte enthalten.  
  
### <a name="help-for-command-actions"></a>Hilfe zu Befehlsaktionen

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

Spezifische Eigenschaften für die Import-Aktion

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
  
## <a name="deployreport-parameters-and-properties"></a>Parameter und Eigenschaften für "DeployReport"

Durch eine **SqlPackage.exe**-Berichtsaktion wird ein XML-Bericht der Änderungen erstellt, die durch eine Veröffentlichungsaktion vorgenommen würden.  
  
### <a name="help-for-deployreport-action"></a>Hilfe zur DeployReport-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|DeployReport|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OutputPath:**|**/op**|{string}|Gibt den Dateipfad an, in dem Ausgabedateien generiert werden. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Profile:**|**/pr**|{string}|Gibt den Dateipfad zu einem DAC-Veröffentlichungsprofil an. Im Profil ist eine Auflistung von Eigenschaften und Variablen definiert, die beim Generieren von Ausgaben verwendet werden. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:DeployReport /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“. |
|**/SourceConnectionString:**|**/scs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Quelldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Quellparameter aus. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definiert den Namen der Quelldatenbank. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Quelldatenbank die SQL-Verschlüsselung verwendet werden soll. |
|**/SourceFile:**|**/sf**|{string}|Gibt eine Quelldatei an, die anstelle einer Datenbank als Quelle für eine Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Quellparameter zulässig. |
|**/SourcePassword:**|**/sp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Quelldatenbank. |
|**/SourceServerName:**|**/ssn**|{string}|Definiert den Namen des Servers, auf dem die Quelldatenbank gehostet wird. |
|**/SourceTimeout:**|**/st**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Quelldatenbank an. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Quelldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/SourceUser:**|**/su**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Quelldatenbank. |
|**/TargetConnectionString:**|**/tcs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Zieldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Zielparameter aus. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Gibt eine Außerkraftsetzung für den Namen der Datenbank an, die das Ziel der sqlpackage.exe-Aktion darstellt. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Zieldatenbank SQL-Verschlüsselung verwendet werden soll. |
|**/TargetFile:**|**/tf**|{string}|Gibt eine Zieldatei (d. h. eine DACPAC-Datei) an, die anstelle einer Datenbank als Ziel der Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Zielparameter zulässig. Dieser Parameter ist für Aktionen ungültig, die ausschließlich Datenbankziele unterstützen.|
|**/TargetPassword:**|**/tp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Zieldatenbank. |
|**/TargetServerName:**|**/tsn**|{string}|Definiert den Namen des Servers, auf dem die Zieldatenbank gehostet wird. |
|**/TargetTimeout:**|**/tt**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Zieldatenbank an. Für Azure AD wird empfohlen, dass dieser Wert größer oder gleich 30 Sekunden ist.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Zieldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/TargetUser:**|**/tu**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Zieldatenbank. |
|**/TenantId:**|**/tid**|{string}|Repräsentiert die Azure AD-Mandanten-ID oder den Domänennamen. Diese Option ist für die Unterstützung von Gastbenutzern oder importierten Azure AD-Benutzern sowie für Microsoft-Konten wie beispielsweise outlook.com, hotmail.com oder live.com erforderlich. Bei Auslassen dieses Parameters wird die Standardmandanten-ID für Azure AD verwendet. Hierbei wird davon ausgegangen, dass der authentifizierte Benutzer ein nativer Benutzer dieser AD-Instanz ist. In diesem Fall werden jedoch Gastbenutzer oder importierte Benutzer und/oder Microsoft-Konten, die in dieser Azure AD-Instanz gehostet werden, nicht unterstützt, und der Vorgang ist nicht erfolgreich. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Gibt an, ob die universelle Authentifizierung verwendet werden soll. Bei Festlegung auf TRUE wird das interaktive Authentifizierungsprotokoll aktiviert, das MFA unterstützt. Diese Option kann auch für die Azure AD-Authentifizierung ohne MFA verwendet werden. Hierbei wird ein interaktives Protokoll verwendet, bei dem der Benutzer seinen Benutzernamen und sein Kennwort oder eine integrierte Authentifizierung (Windows-Anmeldeinformationen) eingeben muss. Bei Festlegung von „/UniversalAuthentication“ auf TRUE kann in „SourceConnectionString“ (/scs) keine Azure AD-Authentifizierung angegeben werden. Bei Festlegung von „/UniversalAuthentication“ auf FALSE muss in „SourceConnectionString“ (/scs) die Azure AD-Authentifizierung angegeben werden. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Variable an: {VariableName}={Value}. Die DACPAC-Datei enthält die Liste gültiger SQLCMD-Variablen. Wenn nicht für jede Variable ein Wert angegeben wird, wird ein Fehler ausgegeben. |

## <a name="properties-specific-to-the-deployreport-action"></a>Spezifische Eigenschaften für die DeployReport-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Gibt zusätzliche Bereitstellungs-Contributorargumente für die Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Gibt zusätzliche Bereitstellungs-Contributors an, die beim Bereitstellen des Dacpacs ausgeführt werden sollen. Dabei sollte es sich um eine Liste der Namen oder IDs der vollqualifizierten Erstellungs-Contributors mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Gibt Pfade zum Laden zusätzlicher Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln. | 
|**/p:**|AllowDropBlocking Assemblies=(BOOLEAN)|Diese Eigenschaft wird von der SqlClr-Bereitstellung verwendet, damit alle blockierenden Assemblys im Rahmen des Bereitstellungsplans gelöscht werden. Wenn die verweisende Assembly gelöscht werden muss, werden Assemblyupdates durch blockierende oder verweisende Assemblys standardmäßig blockiert.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Gibt an, ob versucht werden soll, die Aktion trotz inkompatibler SQL Server-Plattformen auszuführen.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Wenn diese Eigenschaft auf TRUE festgelegt ist, wird das Verschieben von Daten in einer Tabelle mit Sicherheit auf Zeilenebene nicht blockiert. Der Standardwert ist "false".|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Sichert die Datenbank, bevor Änderungen bereitgestellt werden.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Gibt an, dass der Veröffentlichungszeitraum beendet werden soll, wenn durch den Veröffentlichungsvorgang ein Datenverlust verursacht werden könnte.|
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Gibt an, ob die Aktualisierung einer Datenbank, deren Schema nicht mehr mit der Registrierung übereinstimmt oder aus der Registrierung entfernt wurde, blockiert wird. |
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück. |
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Gibt an, ob die SETVAR-Variablendeklaration im generierten Veröffentlichungsskript auskommentiert werden soll. Dies empfiehlt sich beispielsweise, wenn Sie die Werte über die Befehlszeile eingeben möchten und mithilfe eines Tool wie SQLCMD.EXE veröffentlichen. |
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Diese Einstellung bestimmt, wie die Datenbanksortierung während der Bereitstellung behandelt wird. Standardmäßig wird die Sortierung der Zieldatenbank aktualisiert, wenn sie nicht mit der durch die Quelle angegebenen Sortierung übereinstimmt. Wenn diese Option festgelegt ist, sollte die Sortierung der Zieldatenbank (oder des Servers) verwendet werden. |
|**/p:**|CreateNewDatabase=(BOOLEAN)|Gibt an, ob die Zieldatenbank beim Veröffentlichen in einer Datenbank aktualisiert bzw. gelöscht und neu erstellt werden soll. |
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definiert die Edition einer Azure SQL-Datenbank.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definiert die maximale Größe einer Azure SQL-Datenbank in GB.|
|**/p:**|DatabaseServiceObjective=(STRING)|Definiert die Leistungsstufe einer Azure SQL-Datenbank, z.B. „P0“ oder „S1“. |
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Beim Wert TRUE wird die Datenbank vor der Bereitstellung in den Einzelbenutzermodus geschaltet. |
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| Gibt an, ob DDL-Trigger (Data Definition Language) am Anfang des Veröffentlichungsprozesses deaktiviert und am Ende der Veröffentlichungsaktion erneut aktiviert werden.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Beim Wert "true" werden Change Data Capture-Objekte nicht geändert.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Gibt an, ob replizierte Objekte während der Überprüfung identifiziert werden.|
|**/p:**|DoNotDropObjectType=(STRING)|Ein Objekttyp, der nicht gelöscht werden soll, wenn „DropObjectsNotInSource“ auf TRUE festgelegt ist. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers. |
|**/p:**|DoNotDropObjectTypes=(STRING)|Eine durch Semikolons getrennte Liste von Objekttypen, die nicht gelöscht werden sollen, wenn „DropObjectsNotInSource“ den Wert TRUE besitzt. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Gibt an, ob Einschränkungen, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Gibt an, ob DML-Trigger, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Gibt an, ob erweiterte Eigenschaften, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, aus der Zieldatenbank gelöscht werden, wenn Sie eine Veröffentlichung in einer Datenbank vornehmen.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Gibt an, ob Indizes, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Gibt an, ob Objekte, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden. Dieser Wert hat Vorrang vor „DropExtendedProperties“.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Gibt an, ob Berechtigungen, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Gibt an, ob Rollenmitglieder, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht definiert sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Gibt an, ob Statistiken, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|ExcludeObjectType=(STRING)|Ein Objekttyp, der während der Bereitstellung ignoriert werden soll. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Eine durch Semikolon getrennte Liste der Objekttypen, die während der Bereitstellung ignoriert werden sollen. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Stellt automatisch einen Standardwert bereit, wenn eine Tabelle mit Daten anhand einer Spalte aktualisiert wird, die keine NULL-Werte zulässt.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der ANSI NULLS-Einstellung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Gibt an, ob Unterschiede im Autorisierer beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Gibt an, ob Unterschiede in den Spaltensortierungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Tabellenspaltenreihenfolge beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreComments=(BOOLEAN)|Gibt an, ob Unterschiede in den Kommentaren beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede im Dateipfad für den Kryptografieanbieter beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank oder auf einem Server ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Gibt an, ob Unterschiede im Standardschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Gibt an, ob Unterschiede in den erweiterten Eigenschaften beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Pfaden für Dateien und Protokolldateien beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei der Platzierung von Objekten in FILEGROUPs beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Dateigröße beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll. |
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Füllfaktor für den Indexspeicher beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Dateipfad für den Volltextkatalog beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.| 
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Gibt an, ob Unterschiede im Seed für eine Identitätsspalte beim Veröffentlichen von Updates für eine Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreIncrement=(BOOLEAN)|Gibt an, ob Unterschiede im Inkrement für eine Identitätsspalte beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Indexoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Indexauffüllung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Groß-/Kleinschreibung von Schlüsselwörtern beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Gibt an, ob Unterschiede in den Sperrhinweisen für Indizes beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')| Gibt an, ob Unterschiede in der Sicherheits-ID (SID) beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Gibt an, ob die Einstellungen für „Nicht für Replikation“ beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Gibt an, ob die Platzierung eines Objekts in einem Partitionsschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden soll.|
 |**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Gibt an, ob Unterschiede in den Partitionsschemas und -funktionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Gibt an, ob Unterschiede in den Berechtigungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Einstellung für Bezeichner in Anführungszeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Gibt an, ob Unterschiede in den Rollenmitgliedschaften von Anmeldenamen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei dem Zeitraum, über den SQL Server die Route in der Routingtabelle beibehält, beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Semikolons zwischen T-SQL-Anweisungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenpartitionsoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.  Diese Option gilt nur für Data Warehouse-Datenbanken von Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Gibt an, ob Unterschiede in den Benutzereinstellungsobjekten beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Leerzeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für CHECK-Einschränkungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für Fremdschlüssel beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.| 
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Alle zusammengesetzten Elemente als Teil einer einzigen Veröffentlichungsvorgangs einschließen.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Gibt an, ob beim Veröffentlichen in einer Datenbank nach Möglichkeit Transaktionsanweisungen verwendet werden sollen.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Gibt an, dass eine Assembly bei einer Abweichung von der Veröffentlichungsaktion immer gelöscht und neu erstellt werden soll, anstatt eine ALTER ASSEMBLY-Anweisung auszugeben. |
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Gibt an, ob beim Erstellen einer neuen FileGroup in der Zieldatenbank ebenfalls eine neue Datei erstellt wird. |
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Gibt an, ob das Schema beim Datenbankserver registriert wird. 
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Gibt an, ob DeploymentPlanExecutor-Contributors ausgeführt werden sollen, wenn andere Vorgänge ausgeführt werden.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbanksortierung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbankkompatibilität beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen. |
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Gibt an, ob die Eigenschaften der Zieldatenbank als Teil des Veröffentlichungsvorgangs festgelegt oder aktualisiert werden sollen. |
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Gibt an, ob Anweisungen im Veröffentlichungsskript generiert werden, um zu überprüfen, ob der Datenbankname und der Servername mit den im Datenbankprojekt angegebenen Namen übereinstimmen.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Steuert, ob beim Hinzufügen einer Datei zu einer Dateigruppe die Größe angegeben wird. |
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Nach Ende der Veröffentlichung werden alle Bedingungen als ein Satz überprüft, wodurch Datenfehler vermieden werden, die durch eine CHECK- oder FOREIGN KEY-Einschränkung während der Veröffentlichung verursacht werden. Wenn FALSE festgelegt wird, werden Einschränkungen ohne Überprüfung der entsprechenden Daten veröffentlicht.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Schließt Aktualisierungsanweisungen am Ende des Veröffentlichungsskripts ein.|
|**/p:**|Storage=({File&#124;Memory})|Gibt an, wie Elemente gespeichert werden, wenn das Datenbankmodell erstellt wird. Die Standardeinstellung lautet aus Leistungsgründen InMemory. Für große Datenbanken ist die dateigestützte Speicherung erforderlich.|
|**/p:**|Treatverificationerrorsaswarnings = (Boolean)|Gibt an, ob während der Veröffentlichungsüberprüfung aufgetretene Fehler als Warnungen behandelt werden sollen. Die Überprüfung wird für den generierten Bereitstellungsplan ausgeführt, bevor der Plan für Ihre Zieldatenbank ausgeführt wird. Bei der Planüberprüfung werden Probleme erkannt, z. B. der Verlust von reinen Zielobjekten (z. B. Indizes), die gelöscht werden müssen, um eine Änderung vorzunehmen. Bei der Überprüfung werden auch Situationen erkannt, in denen Abhängigkeiten (z. B. eine Tabelle oder Sicht) aufgrund eines Verweises auf ein zusammengesetztes Projekt vorhanden sind, jedoch nicht in der Zieldatenbank vorkommen. Dies ist möglicherweise der Fall, um eine vollständige Liste aller Probleme zu erhalten, statt zu verhindern, dass die Veröffentlichungsaktion beim ersten Fehler beendet wird. |
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Gibt an, ob Warnungen generiert werden sollen, wenn Unterschiede in Objekten gefunden werden, die nicht änderbar sind (z. B. wenn die Dateigröße oder Dateipfade für eine Datei unterschiedlich sind).| 
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Gibt an, ob die Kompatibilität von Sortierungen überprüft wird.| 
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Gibt an, ob vor der Veröffentlichung Überprüfungen ausgeführt werden sollen, durch die die Veröffentlichungsaktion beendet wird, wenn Probleme vorliegen, die eine erfolgreiche Veröffentlichung blockieren könnten. Die Veröffentlichungsaktion könnte beispielsweise beendet werden, wenn in der Zieldatenbank Fremdschlüssel enthalten sind, die im Datenbankprojekt nicht vorhanden sind. Dies verursacht Fehler bei der Veröffentlichung. |
  
## <a name="driftreport-parameters"></a>Parameter für "DriftReport"

Durch eine **SqlPackage.exe**-Berichtsaktion wird ein XML-Bericht der Änderungen erstellt, die seit der letzten Registrierung an der registrierten Datenbank vorgenommen wurden.  
  
### <a name="help-for-driftreport-action"></a>Hilfe zur DriftReport-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|DriftReport|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OutputPath:**|**/op**|{string}|Gibt den Dateipfad an, in dem Ausgabedateien generiert werden. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“.|
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

## <a name="script-parameters-and-properties"></a>Skriptparameter und -eigenschaften

Durch eine **SqlPackage.exe**-Skriptaktion wird ein inkrementelles Transact-SQL-Updateskript erstellt, durch das das Schema einer Zieldatenbank aktualisiert wird, sodass es dem Schema einer Quelldatenbank entspricht.  
  
### <a name="help-for-the-script-action"></a>Hilfe zur Script-Aktion

|Parameter|Kurzform|Wert|BESCHREIBUNG|
|---|---|---|---|
|**/Action:**|**/a**|Skript|Gibt die auszuführende Aktion an. |
|**/AccessToken:**|**/at**|{string}| Gibt das Zugriffstoken für die tokenbasierte Authentifizierung an, das beim Herstellen einer Verbindung mit der Zieldatenbank verwendet werden soll. |
|**/DeployScriptPath:**|**/dsp**|{string}|Hiermit wird ein optionaler Dateipfad zur Ausgabe des Bereitstellungsskripts angegeben. Wenn bei Azure-Bereitstellungen TSQL-Befehle zum Erstellen oder Ändern der Masterdatenbank verwendet werden, wird ein Skript in den gleichen Pfad geschrieben, wobei der Name der Ausgabedatei „Dateiname_Master.sql“ lautet. |
|**/DeployReportPath:**|**/drp**|{string}|Hiermit wird ein optionaler Dateipfad zur Ausgabe der XML-Datei mit dem Bereitstellungsbericht angegeben. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Gibt an, ob die Diagnoseprotokollierung in der Konsole ausgegeben wird. Der Standardwert lautet „False“. |
|**/DiagnosticsFile:**|**/df**|{string}|Gibt eine Datei an, in der Diagnoseprotokolle gespeichert werden. |
|**/MaxParallelism:**|**/mp**|{int}| Gibt den Parallelitätsgrad für gleichzeitige Vorgänge in einer Datenbank an. Der Standardwert ist 8. |
|**/OutputPath:**|**/op**|{string}|Gibt den Dateipfad an, in dem Ausgabedateien generiert werden. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Gibt an, ob vorhandene Dateien von sqlpackage.exe überschrieben werden sollen. Bei Angabe von FALSE wird die Aktion von „sqlpackage.exe“ abgebrochen, wenn eine vorhandene Datei gefunden wird. Der Standardwert ist TRUE. |
|**/Profile:**|**/pr**|{string}|Gibt den Dateipfad zu einem DAC-Veröffentlichungsprofil an. Im Profil ist eine Auflistung von Eigenschaften und Variablen definiert, die beim Generieren von Ausgaben verwendet werden.|
|**/Properties:**|**/p**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Eigenschaft an: {PropertyName}={Value}. Die Eigenschaftennamen zu einer spezifischen Aktion finden Sie in der Hilfe. Beispiel: sqlpackage.exe /Action:Script /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Gibt an, ob ausführliches Feedback unterdrückt wird. Der Standardwert lautet „False“.|
|**/SourceConnectionString:**|**/scs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Quelldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Quellparameter aus. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Definiert den Namen der Quelldatenbank. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Quelldatenbank die SQL-Verschlüsselung verwendet werden soll. |
|**/SourceFile:**|**/sf**|{string}|Gibt eine Quelldatei an, die als Quelle für eine Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Quellparameter zulässig. |
|**/SourcePassword:**|**/sp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Quelldatenbank. |
|**/SourceServerName:**|**/ssn**|{string}|Definiert den Namen des Servers, auf dem die Quelldatenbank gehostet wird. |
|**/SourceTimeout:**|**/st**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Quelldatenbank an. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Quelldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/SourceUser:**|**/su**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Quelldatenbank. |
|**/TargetConnectionString:**|**/tcs**|{string}|Gibt eine gültige SQL Server/Azure-Verbindungszeichenfolge für die Zieldatenbank an. Die Angabe dieses Parameters schließt die Verwendung aller anderen Zielparameter aus. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Gibt eine Außerkraftsetzung für den Namen der Datenbank an, die das Ziel der sqlpackage.exe-Aktion darstellt. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Gibt an, ob für die Verbindung mit der Zieldatenbank SQL-Verschlüsselung verwendet werden soll. |
|**/TargetFile:**|**/tf**|{string}| Gibt eine Zieldatei (d. h. eine DACPAC-Datei) an, die anstelle einer Datenbank als Ziel der Aktion verwendet werden soll. Bei Verwendung dieses Parameters sind keine anderen Zielparameter zulässig. Dieser Parameter ist für Aktionen ungültig, die ausschließlich Datenbankziele unterstützen.|
|**/TargetPassword:**|**/tp**|{string}|Definiert in SQL Server-Authentifizierungsszenarios das Kennwort für den Zugriff auf die Zieldatenbank. |
|**/TargetServerName:**|**/tsn**|{string}|Definiert den Namen des Servers, auf dem die Zieldatenbank gehostet wird. |
|**/TargetTimeout:**|**/tt**|{int}|Gibt das Timeout in Sekunden für das Herstellen einer Verbindung mit der Zieldatenbank an. Für Azure AD wird empfohlen, dass dieser Wert größer oder gleich 30 Sekunden ist.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Gibt an, ob zum Verschlüsseln der Verbindung mit der Zieldatenbank TLS verwendet und das Durchlaufen der Zertifikatkette zum Überprüfen der Vertrauenswürdigkeit umgangen werden soll. |
|**/TargetUser:**|**/tu**|{string}|Definiert in SQL Server-Authentifizierungsszenarios den SQL Server-Benutzer für den Zugriff auf die Zieldatenbank. |
|**/TenantId:**|**/tid**|{string}|Repräsentiert die Azure AD-Mandanten-ID oder den Domänennamen. Diese Option ist für die Unterstützung von Gastbenutzern oder importierten Azure AD-Benutzern sowie für Microsoft-Konten wie beispielsweise outlook.com, hotmail.com oder live.com erforderlich. Bei Auslassen dieses Parameters wird die Standardmandanten-ID für Azure AD verwendet. Hierbei wird davon ausgegangen, dass der authentifizierte Benutzer ein nativer Benutzer dieser AD-Instanz ist. In diesem Fall werden jedoch Gastbenutzer oder importierte Benutzer und/oder Microsoft-Konten, die in dieser Azure AD-Instanz gehostet werden, nicht unterstützt, und der Vorgang ist nicht erfolgreich. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Gibt an, ob die universelle Authentifizierung verwendet werden soll. Bei Festlegung auf TRUE wird das interaktive Authentifizierungsprotokoll aktiviert, das MFA unterstützt. Diese Option kann auch für die Azure AD-Authentifizierung ohne MFA verwendet werden. Hierbei wird ein interaktives Protokoll verwendet, bei dem der Benutzer seinen Benutzernamen und sein Kennwort oder eine integrierte Authentifizierung (Windows-Anmeldeinformationen) eingeben muss. Bei Festlegung von „/UniversalAuthentication“ auf TRUE kann in „SourceConnectionString“ (/scs) keine Azure AD-Authentifizierung angegeben werden. Bei Festlegung von „/UniversalAuthentication“ auf FALSE muss in „SourceConnectionString“ (/scs) die Azure AD-Authentifizierung angegeben werden. <br/> Weitere Informationen zur universellen Active Directory-Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{PropertyName}={Value}|Gibt ein Name-Wert-Paar für eine aktionsspezifische Variable an: {VariableName}={Value}. Die DACPAC-Datei enthält die Liste gültiger SQLCMD-Variablen. Wenn nicht für jede Variable ein Wert angegeben wird, wird ein Fehler ausgegeben. |

### <a name="properties-specific-to-the-script-action"></a>Spezifische Eigenschaften für die Script-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Gibt zusätzliche Bereitstellungs-Contributorargumente für die Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln.
|**/p:**|AdditionalDeploymentContributors=(STRING)|Gibt zusätzliche Bereitstellungs-Contributors an, die beim Bereitstellen des Dacpacs ausgeführt werden sollen. Dabei sollte es sich um eine Liste der Namen oder IDs der vollqualifizierten Erstellungs-Contributors mit Semikolatrennung handeln.
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Gibt Pfade zum Laden zusätzlicher Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Diese Eigenschaft wird von der SqlClr-Bereitstellung verwendet, damit alle blockierenden Assemblys im Rahmen des Bereitstellungsplans gelöscht werden. Wenn die verweisende Assembly gelöscht werden muss, werden Assemblyupdates durch blockierende oder verweisende Assemblys standardmäßig blockiert.
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Gibt an, ob versucht werden soll, die Aktion trotz inkompatibler SQL Server-Plattformen auszuführen.
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Wenn diese Eigenschaft auf TRUE festgelegt ist, wird das Verschieben von Daten in einer Tabelle mit Sicherheit auf Zeilenebene nicht blockiert. Der Standardwert ist "false".
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Sichert die Datenbank, bevor Änderungen bereitgestellt werden.
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')|Gibt an, dass der Veröffentlichungszeitraum beendet werden soll, wenn durch den Veröffentlichungsvorgang ein Datenverlust verursacht werden könnte.
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Gibt an, ob die Aktualisierung einer Datenbank, deren Schema nicht mehr mit der Registrierung übereinstimmt oder aus der Registrierung entfernt wurde, blockiert wird.
|**/p:**|CommandTimeout=(INT32 '60')|Gibt das Befehlstimeout in Sekunden zum Ausführen von Abfragen in SQL Server zurück.
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Gibt an, ob die SETVAR-Variablendeklaration im generierten Veröffentlichungsskript auskommentiert werden soll. Dies empfiehlt sich beispielsweise, wenn Sie die Werte über die Befehlszeile eingeben möchten und mithilfe eines Tool wie SQLCMD.EXE veröffentlichen.
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Diese Einstellung bestimmt, wie die Datenbanksortierung während der Bereitstellung behandelt wird. Standardmäßig wird die Sortierung der Zieldatenbank aktualisiert, wenn sie nicht mit der durch die Quelle angegebenen Sortierung übereinstimmt. Wenn diese Option festgelegt ist, sollte die Sortierung der Zieldatenbank (oder des Servers) verwendet werden.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Gibt an, ob die Zieldatenbank beim Veröffentlichen in einer Datenbank aktualisiert bzw. gelöscht und neu erstellt werden soll.
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Definiert die Edition einer Azure SQL-Datenbank.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Gibt das Datenbank-Sperrtimeout für Abfragen an SQL Server in Sekunden an. Verwenden Sie „-1“, um unbegrenzt zu warten.|
|**/p:**|DatabaseMaximumSize=(INT32)|Definiert die maximale Größe einer Azure SQL-Datenbank in GB.
|**/p:**|DatabaseServiceObjective=(STRING)|Definiert die Leistungsstufe einer Azure SQL-Datenbank, z.B. „P0“ oder „S1“.
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Beim Wert TRUE wird die Datenbank vor der Bereitstellung in den Einzelbenutzermodus geschaltet.
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| Gibt an, ob DDL-Trigger (Data Definition Language) am Anfang des Veröffentlichungsprozesses deaktiviert und am Ende der Veröffentlichungsaktion erneut aktiviert werden.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Beim Wert "true" werden Change Data Capture-Objekte nicht geändert.
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Gibt an, ob replizierte Objekte während der Überprüfung identifiziert werden.
|**/p:**|DoNotDropObjectType=(STRING)|Ein Objekttyp, der nicht gelöscht werden soll, wenn „DropObjectsNotInSource“ auf TRUE festgelegt ist. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.
|**/p:**|DoNotDropObjectTypes=(STRING)|Eine durch Semikolons getrennte Liste von Objekttypen, die nicht gelöscht werden sollen, wenn „DropObjectsNotInSource“ den Wert TRUE hat. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Gibt an, ob Einschränkungen, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) vorhanden sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Gibt an, ob DML-Trigger, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Gibt an, ob erweiterte Eigenschaften, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, aus der Zieldatenbank gelöscht werden, wenn Sie eine Veröffentlichung in einer Datenbank vornehmen.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Gibt an, ob Indizes, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Gibt an, ob Objekte, die nicht in der Datenbankmomentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden. Dieser Wert hat Vorrang vor „DropExtendedProperties“.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Gibt an, ob Berechtigungen, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht vorhanden sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Gibt an, ob Rollenmitglieder, die in der Datenbankmomentaufnahme (DACPAC-Datei) nicht definiert sind, bei der Veröffentlichung von Updates in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Gibt an, ob Statistiken, die nicht in der Datenbank-Momentaufnahme (DACPAC-Datei) enthalten sind, bei der Veröffentlichung in einer Datenbank aus der Zieldatenbank gelöscht werden.|
|**/p:**|ExcludeObjectType=(STRING)|Ein Objekttyp, der während der Bereitstellung ignoriert werden soll. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.
|**/p:**|ExcludeObjectTypes=(STRING)|Eine durch Semikolon getrennte Liste der Objekttypen, die während der Bereitstellung ignoriert werden sollen. Gültige Objekttypnamen sind Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles und ServerTriggers.
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Stellt automatisch einen Standardwert bereit, wenn eine Tabelle mit Daten anhand einer Spalte aktualisiert wird, die keine NULL-Werte zulässt.
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der ANSI NULLS-Einstellung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Gibt an, ob Unterschiede im Autorisierer beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Gibt an, ob Unterschiede in den Spaltensortierungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Tabellenspaltenreihenfolge beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreComments=(BOOLEAN)|Gibt an, ob Unterschiede in den Kommentaren beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede im Dateipfad für den Kryptografieanbieter beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank oder auf einem Server ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der Datendefinitionssprachentrigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Gibt an, ob Unterschiede im Standardschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Gibt an, ob Unterschiede in der Reihenfolge der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Gibt an, ob Unterschiede im aktivierten bzw. deaktivierten Status der DML-Trigger beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Gibt an, ob Unterschiede in den erweiterten Eigenschaften beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Pfaden für Dateien und Protokolldateien beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei der Platzierung von Objekten in FILEGROUPs beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Dateigröße beim Veröffentlichen in einer Datenbank ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Füllfaktor für den Indexspeicher beim Veröffentlichen ignoriert werden sollen oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Gibt an, ob Unterschiede beim Dateipfad für den Volltextkatalog beim Veröffentlichen in einer Datenbank ignoriert werden sollen, oder ob eine Warnung ausgegeben werden soll.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Gibt an, ob Unterschiede im Seed für eine Identitätsspalte beim Veröffentlichen von Updates für eine Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Gibt an, ob Unterschiede im Inkrement für eine Identitätsspalte beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Indexoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Indexauffüllung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Groß-/Kleinschreibung von Schlüsselwörtern beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Gibt an, ob Unterschiede in den Sperrhinweisen für Indizes beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')| Gibt an, ob Unterschiede in der Sicherheits-ID (SID) beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Gibt an, ob die Einstellungen für „Nicht für Replikation“ beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Gibt an, ob die Platzierung eines Objekts in einem Partitionsschema beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden soll.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Gibt an, ob Unterschiede in den Partitionsschemas und -funktionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Gibt an, ob Unterschiede in den Berechtigungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Gibt an, ob Unterschiede in der Einstellung für Bezeichner in Anführungszeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Gibt an, ob Unterschiede in den Rollenmitgliedschaften von Anmeldenamen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Gibt an, ob Unterschiede bei dem Zeitraum, über den SQL Server die Route in der Routingtabelle beibehält, beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Semikolons zwischen T-SQL-Anweisungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Gibt an, ob Unterschiede in den Tabellenpartitionsoptionen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.  Diese Option gilt nur für Data Warehouse-Datenbanken von Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Gibt an, ob Unterschiede in den Benutzereinstellungsobjekten beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Gibt an, ob Unterschiede in den Leerzeichen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für CHECK-Einschränkungen beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Gibt an, ob Unterschiede im Wert der WITH NOCHECK-Klausel für Fremdschlüssel beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Alle zusammengesetzten Elemente als Teil einer einzigen Veröffentlichungsvorgangs einschließen.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Gibt an, ob beim Veröffentlichen in einer Datenbank nach Möglichkeit Transaktionsanweisungen verwendet werden sollen.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Gibt das Timeout für zeitintensive Befehle beim Ausführen von Abfragen an SQL Server in Sekunden zurück. Verwenden Sie „0“, um unbegrenzt zu warten.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Gibt an, dass eine Assembly bei einer Abweichung von der Veröffentlichungsaktion immer gelöscht und neu erstellt werden soll, anstatt eine ALTER ASSEMBLY-Anweisung auszugeben.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Gibt an, ob beim Erstellen einer neuen FileGroup in der Zieldatenbank ebenfalls eine neue Datei erstellt wird.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Gibt an, ob das Schema beim Datenbankserver registriert wird.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Gibt an, ob DeploymentPlanExecutor-Contributors ausgeführt werden sollen, wenn andere Vorgänge ausgeführt werden.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbanksortierung beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Gibt an, ob Unterschiede in der Datenbankkompatibilität beim Veröffentlichen in einer Datenbank ignoriert oder aktualisiert werden sollen.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Gibt an, ob die Eigenschaften der Zieldatenbank als Teil des Veröffentlichungsvorgangs festgelegt oder aktualisiert werden sollen.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Gibt an, ob Anweisungen im Veröffentlichungsskript generiert werden, um zu überprüfen, ob der Datenbankname und der Servername mit den im Datenbankprojekt angegebenen Namen übereinstimmen.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Steuert, ob beim Hinzufügen einer Datei zu einer Dateigruppe die Größe angegeben wird.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Nach Ende der Veröffentlichung werden alle Bedingungen als ein Satz überprüft, wodurch Datenfehler vermieden werden, die durch eine CHECK- oder FOREIGN KEY-Einschränkung während der Veröffentlichung verursacht werden. Wenn FALSE festgelegt wird, werden Einschränkungen ohne Überprüfung der entsprechenden Daten veröffentlicht.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Schließt Aktualisierungsanweisungen am Ende des Veröffentlichungsskripts ein.|
|**/p:**|Storage=({File&#124;Memory})|Gibt an, wie Elemente gespeichert werden, wenn das Datenbankmodell erstellt wird. Die Standardeinstellung lautet aus Leistungsgründen InMemory. Für große Datenbanken ist die dateigestützte Speicherung erforderlich.|
|**/p:**|Treatverificationerrorsaswarnings = (Boolean)|Gibt an, ob während der Veröffentlichungsüberprüfung aufgetretene Fehler als Warnungen behandelt werden sollen. Die Überprüfung wird für den generierten Bereitstellungsplan ausgeführt, bevor der Plan für Ihre Zieldatenbank ausgeführt wird. Bei der Planüberprüfung werden Probleme erkannt, z. B. der Verlust von reinen Zielobjekten (z. B. Indizes), die gelöscht werden müssen, um eine Änderung vorzunehmen. Bei der Überprüfung werden auch Situationen erkannt, in denen Abhängigkeiten (z. B. eine Tabelle oder Sicht) aufgrund eines Verweises auf ein zusammengesetztes Projekt vorhanden sind, jedoch nicht in der Zieldatenbank vorkommen. Dies ist möglicherweise der Fall, um eine vollständige Liste aller Probleme zu erhalten, statt zu verhindern, dass die Veröffentlichungsaktion beim ersten Fehler beendet wird.|
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Gibt an, ob Warnungen generiert werden sollen, wenn Unterschiede in Objekten gefunden werden, die nicht änderbar sind (z. B. wenn die Dateigröße oder Dateipfade für eine Datei unterschiedlich sind).|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Gibt an, ob die Kompatibilität von Sortierungen überprüft wird.
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Gibt an, ob vor der Veröffentlichung Überprüfungen ausgeführt werden sollen, durch die die Veröffentlichungsaktion beendet wird, wenn Probleme vorliegen, die eine erfolgreiche Veröffentlichung blockieren könnten. Die Veröffentlichungsaktion könnte beispielsweise beendet werden, wenn in der Zieldatenbank Fremdschlüssel enthalten sind, die im Datenbankprojekt nicht vorhanden sind. Dies verursacht Fehler bei der Veröffentlichung.|

## <a name="exit-codes"></a>Exitcodes

Befehle, die die folgenden Exitcodes zurückgeben:

- 0 = Erfolg
- Ungleich = Fehler
