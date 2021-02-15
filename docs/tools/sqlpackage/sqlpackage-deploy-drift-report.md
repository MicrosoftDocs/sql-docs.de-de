---
title: DeployReport- und DriftReport-Aktionen von SqlPackage
description: In diesem Artikel erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe der DeployReport- und DriftReport-Aktionen von „SqlPackage.exe“ automatisieren. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 8f58ec917fa6c26e4095649c33fca8b4048b96b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081501"
---
# <a name="sqlpackage-deploy-report-and-drift-report"></a>DeployReport- und DriftReport-Aktionen von SqlPackage
Die Aktion [DeployReport](#deployreport-action-parameters) von „SqlPackage.exe“ erstellt einen XML-Bericht für die Änderungen, die von einer Publish-Aktion durchgeführt werden würden.
Die Aktion [DriftReport](#driftreport-action-parameters) von „SqlPackage.exe“ erstellt einen XML-Bericht für die Änderungen, die seit der letzten Registrierung an der registrierten Datenbank vorgenommen wurden.  

## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="deployreport-action-parameters"></a>Parameter für die DeployReport-Aktion

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

## <a name="deployreport-action-properties"></a>Eigenschaften der DeployReport-Aktion

|Eigenschaft|Wert|BESCHREIBUNG|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Gibt zusätzliche Bereitstellungs-Contributorargumente für die Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Gibt zusätzliche Bereitstellungs-Contributors an, die beim Bereitstellen des Dacpacs ausgeführt werden sollen. Dabei sollte es sich um eine Liste der Namen oder IDs der vollqualifizierten Erstellungs-Contributors mit Semikolatrennung handeln.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Gibt Pfade zum Laden zusätzlicher Bereitstellungs-Contributors an. Dabei sollte es sich um eine Liste von Werten mit Semikolatrennung handeln. | 
|**/p:**|AllowDropBlocking Assemblies=(BOOLEAN)|Diese Eigenschaft wird von der SqlClr-Bereitstellung verwendet, damit alle blockierenden Assemblys im Rahmen des Bereitstellungsplans gelöscht werden. Wenn die verweisende Assembly gelöscht werden muss, werden Assemblyupdates durch blockierende oder verweisende Assemblys standardmäßig blockiert.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Gibt an, ob versucht werden soll, die Aktion trotz inkompatibler SQL Server-Plattformen auszuführen.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Wenn diese Eigenschaft auf TRUE festgelegt ist, wird das Verschieben von Daten in einer Tabelle mit Sicherheit auf Zeilenebene nicht blockiert. Der Standardwert ist "false".|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Sichert die Datenbank, bevor Änderungen bereitgestellt werden.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')| Mit dieser Eigenschaft wird angegeben, dass der Vorgang während des Schritts zur Schemaüberprüfung beendet wird, wenn die resultierenden Schemaänderungen einen Datenverlust verursachen könnten, auch aufgrund von Datengenauigkeit oder einer Datentypänderung, die einen Umwandlungsvorgang erfordert. Der Standardwert (`True`) bewirkt, dass der Vorgang unabhängig davon beendet wird, ob die Zieldatenbank Daten enthält.  Eine Ausführung mit einem `False`-Wert für BlockOnPossibleDataLoss kann während der Ausführung des Bereitstellungsplans trotzdem fehlschlagen, wenn Daten auf dem Ziel vorhanden sind, die nicht in den neuen Spaltentyp konvertiert werden können. |
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
  
## <a name="driftreport-action-parameters"></a>Parameter für die DriftReport-Aktion

|Parameter|Kurzform|Wert|Beschreibung|
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

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)
