---
title: SqlPackage.exe
description: In diesem Artikel erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe von SqlPackage.exe automatisieren können. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 4c24670e4bdc5603b824d4e5c5c00f7a101933b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081441"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** ist ein Befehlszeilen-Hilfsprogramm, das die folgenden Aufgaben bei der Datenbankentwicklung automatisiert:  
  
- [Version](#version): Gibt die Buildnummer der SqlPackage-Anwendung zurück.  In Version 18.6 hinzugefügt.

- [Extract:](sqlpackage-extract.md) Erstellt eine Datenschichtanwendungsdatei (.dacpac), die das Schema oder das Schema und die Benutzerdaten aus einer verbundenen SQL-Datenbank enthält  
  
- [Veröffentlichen](sqlpackage-publish.md): Aktualisiert ein Datenbankschema inkrementell, sodass dieses dem Schema einer DACPAC-Quelldatei entspricht Wenn die Datenbank auf dem Server nicht vorhanden ist, wird sie durch den Veröffentlichungsvorgang erstellt. Andernfalls wird eine vorhandene Datenbank aktualisiert.  
  
- [Export](sqlpackage-export.md): Exportiert eine verbundene SQL-Datenbank, einschließlich des Datenbankschemas und der Benutzerdaten, in eine BACPAC-Datei (.bacpac)  
  
- [Import](sqlpackage-import.md): Importiert das Schema und die Tabellendaten aus einer BACPAC-Datei in eine neue Benutzerdatenbank  
  
- [DeployReport:](sqlpackage-deploy-drift-report.md) Erstellt einen XML-Bericht der Änderungen, die durch eine Veröffentlichungsaktion vorgenommen würden  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): Erstellt einen XML-Bericht der Änderungen, die seit der letzten Registrierung an einer registrierten Datenbank vorgenommen wurden.  
  
- [Script:](sqlpackage-script.md) Erstellt ein inkrementelles Transact-SQL-Updateskript, durch das das Schema eines Ziels aktualisiert wird, sodass es dem Schema einer Quelle entspricht.  
  
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

 ## <a name="version"></a>Version

Zeigt die Version von SqlPackage als Buildnummer.  Kann in interaktiven Eingabeaufforderungen und [automatisierten Pipelines](sqlpackage-pipelines.md)verwendet werden.

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Exitcodes

Befehle, die die folgenden Exitcodes zurückgeben:

- 0 = Erfolg
- Ungleich = Fehler


## <a name="parameters"></a>Parameter
Einige Parameter werden von den SqlPackage-Aktionen gemeinsam verwendet. Die folgende Tabelle bietet eine Übersicht über die Parameter. Um weitere Informationen zu erhalten, klicken Sie auf die entsprechende Überschrift für die Aktion.

| Parameter | Kurzform | [Extrahieren](sqlpackage-extract.md#parameters-for-the-extract-action) | [Veröffentlichen](sqlpackage-publish.md#parameters-for-the-publish-action) | [Export](sqlpackage-export.md#parameters-for-the-export-action) | [Importieren](sqlpackage-import.md#parameters-for-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-parameters) | [DriftReport](sqlpackage-deploy-drift-report.md#driftreport-action-parameters) | [Skript](sqlpackage-script.md#parameters-for-the-script-action) |
|---|---|---|---|---|---|---|---|---|
|**/AccessToken:**|**/at**| x | x | x | x | x | x | x |
|**/ClientId:**|**/cid**| | w | | | | | |
|**/DeployScriptPath:**|**/dsp**| | x | | | | | x |
|**/DeployReportPath:**|**/drp**| | x | | | | | x |
|**/Diagnostics:**|**/d**| x | x | x | x | x | x | x |
|**/DiagnosticsFile:**|**/df**| x | x | x | x | x | x | x |
|**/MaxParallelism:**|**/mp**| x | x | x | x | x | x | x |
|**/OutputPath:**|**/op**|  |  |  | | x | x | x |
|**/OverwriteFiles:**|**/of**| x | x | x | | x | x | x |
|**/Profile:**|**/pr**| | x | | | x | | x |
|**/Properties:**|**/p**| x | x | x | x | x | | x |
|**/Quiet:**|**/q**| x | x | x | x | x | x | x |
|**/Secret:**|**/secr**| | w | | | | | |
|**/SourceConnectionString:**|**/scs**| x | x | x | | x | | x | x |
|**/SourceDatabaseName:**|**/sdn**| x | x | x | | x | | x |
|**/SourceEncryptConnection:**|**/sec**| x | x | x | | x | | x |
|**/SourceFile:**|**/sf**| | x | | x | x | | x |
|**/SourcePassword:**|**/sp**| x | x | x | | x | | x |
|**/SourceServerName:**|**/ssn**| x | x | x | | x | | x |
|**/SourceTimeout:**|**/st**| x | x | x | | x | | x |
|**/SourceTrustServerCertificate:**|**/stsc**| x | x | x | | x | | x |
|**/SourceUser:**|**/su**| x | x | x | | x | | x |
|**/TargetConnectionString:**|**/tcs**| | | | x | x | x | x |
|**/TargetDatabaseName:**|**/tdn**| | x | | x | x | x | x |
|**/TargetEncryptConnection:**|**/tec**| | x | | x | x | x | x |
|**/TargetFile:**|**/tf**| x | | x | | x | | x |
|**/TargetPassword:**|**/tp**| | x | | x | x | x | x |
|**/TargetServerName:**|**/tsn**| | x | | x | x | x | x |
|**/TargetTimeout:**|**/tt**| | x | | x | x | x | x |
|**/TargetTrustServerCertificate:**|**/ttsc**| | x | | x | x | x | x |
|**/TargetUser:**|**/tu**| | x | | x | x | x | x |
|**/TenantId:**|**/tid**| x | x | x | x | x | x | x |
|**/UniversalAuthentication:**|**/ua**| x | x | x | x | x | x | x |
|**/Variables:**|**/v**| | | | | x | | x |

## <a name="properties"></a>Eigenschaften
Einige Eigenschaften werden von den SqlPackage-Aktionen gemeinsam verwendet.  Die folgende Tabelle bietet eine Übersicht über die Eigenschaften. Um weitere Informationen zu erhalten, klicken Sie auf die entsprechende Überschrift für die Aktion.

| Eigenschaft | [Extrahieren](sqlpackage-extract.md#properties-specific-to-the-extract-action) | [Veröffentlichen](sqlpackage-publish.md#properties-specific-to-the-publish-action) | [Export](sqlpackage-export.md#properties-specific-to-the-export-action) | [Importieren](sqlpackage-import.md#properties-specific-to-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-properties) | [Skript](sqlpackage-script.md#properties-specific-to-the-script-action) |
|---|---|---|---|---|---|---|
|AdditionalDeploymentContributorArguments=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributors=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributorPaths=(STRING)| | x | | | x | x |
|AllowDropBlockingAssemblies=(BOOLEAN)| | x | | | x | x |
|AllowIncompatiblePlatform=(BOOLEAN)| | x | | | x | x |
|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)| | x | | | x | x |
|BackupDatabaseBeforeChanges=(BOOLEAN)| | x | | | x | x |
|BlockOnPossibleDataLoss=(BOOLEAN 'True')| | x | | | x | x |
|BlockWhenDriftDetected=(BOOLEAN 'True')| | x | | | x | x |
|CommandTimeout=(INT32 '60')| x | x | x | x | x | x |
|CommentOutSetVarDeclarations=(BOOLEAN)| | x | | | x | x |
|CompareUsingTargetCollation=(BOOLEAN)| | x | | | x | x |
|CreateNewDatabase=(BOOLEAN)| | x | | | x | x |
|DacApplicationDescription=(STRING)| w | | | | | |
|DacApplicationName=(STRING)| w | | | | | |
|DacMajorVersion=(INT32 '1')| w | | | | | |
|DacMinorVersion=(INT32 '0')| w | | | | | |
|DatabaseEdition=(ENUM 'Default')| | x | | x | x | x |
|DatabaseLockTimeout=(INT32 '60')| x | x | x | | x | x |
|DatabaseMaximumSize=(INT32)| | x | | x | x | x |
|DatabaseServiceObjective=(STRING)| | x | | x | x | x |
|DeployDatabaseInSingleUserMode=(BOOLEAN)| | x | | | x | x |
|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterReplicatedObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotDropObjectType=(STRING)| | x | | | x | x |
|DoNotDropObjectTypes=(STRING)| | x | | | x | x |
|DropConstraintsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropDmlTriggersNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropIndexesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropObjectsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropPermissionsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropRoleMembersNotInSource=(BOOLEAN)| | x | | | x | x |
|DropStatisticsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|ExcludeObjectType=(STRING)| | x | | | x | x |
|ExcludeObjectTypes=(STRING)| | x | | | x | x |
|ExtractAllTableData=(BOOLEAN)| w | | | | | |
|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')| w | | | | | |
|ExtractReferencedServerScopedElements=(BOOLEAN 'True')| w | | | | | |
|ExtractUsageProperties=(BOOLEAN)| w | | | | | |
|GenerateSmartDefaults=(BOOLEAN)| | x | | | x | x |
|IgnoreAnsiNulls=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreAuthorizer=(BOOLEAN)| | x | | | x | x |
|IgnoreColumnCollation=(BOOLEAN)| | | | | x | x |
|IgnoreColumnOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreComments=(BOOLEAN)| | x | | | x | x |
|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreDdlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDdlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreDefaultSchema=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreExtendedProperties=(BOOLEAN)| x | x | | | x | x |
|IgnoreFileAndLogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFilegroupPlacement=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFileSize=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFillFactor=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreIdentitySeed=(BOOLEAN)| | x | | | x | x |
|IgnoreIncrement=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexPadding=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreKeywordCasing=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreLockHintsOnIndexes=(BOOLEAN)| | x | | | x | x |
|IgnoreLoginSids=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreNotForReplication=(BOOLEAN)| | x | | | x | x |
|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')| | x | | | x | x |
|IgnorePartitionSchemes=(BOOLEAN)| | x | | | x | x |
|IgnorePermissions=(BOOLEAN 'True')| x | x | | | x | x |
|IgnoreQuotedIdentifiers=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreRoleMembership=(BOOLEAN)| | x | | | x | x |
|IgnoreRouteLifetime=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreTableOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreTablePartitionOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreUserLoginMappings=(BOOLEAN)| w | | | | | |
|IgnoreUserSettingsObjects=(BOOLEAN)| | x | | | x | x |
|IgnoreWhitespace=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)| | x | | | x | |
|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)| | x | | | x | |
|ImportContributorArguments=(STRING)| | | | w | | |
|ImportContributors=(STRING)| | | | w | | |
|ImportContributorPaths=(STRING)| | | | w | | |
|IncludeCompositeObjects=(BOOLEAN)| | x | | | x | x |
|IncludeTransactionalScripts=(BOOLEAN)| | x | | | x | x |
|LongRunningCommandTimeout=(INT32)| x | x | x | x | x | x |
|NoAlterStatementsToChangeClrTypes=(BOOLEAN)| | x | | | x | x |
|PopulateFilesOnFileGroups=(BOOLEAN 'True')| | x | | | x | x |
|RegisterDataTierApplication=(BOOLEAN)| | x | | | x | x |
|RunDeploymentPlanExecutors=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCollation=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCompatibility=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseOptions=(BOOLEAN 'True')| | x | | | x | x |
|ScriptDeployStateChecks=(BOOLEAN)| | x | | | x | x |
|ScriptFileSize=(BOOLEAN)| | x | | | x | x |
|ScriptNewConstraintValidation=(BOOLEAN 'True')| | x | | | x | x |
|ScriptRefreshModule=(BOOLEAN 'True')| | x | | | x | x |
|Storage=({File&#124;Memory} 'File')| x | x | x | x | x | x |
|TableData=(STRING)| x | | x | | | |
|TargetEngineVersion=(ENUM 'Latest')| | | w | | | |
|TempDirectoryForTableData=(STRING)| x | | x | | | |
|Treatverificationerrorsaswarnings = (Boolean)| | x | | | x | x |
|UnmodifiableObjectWarnings=(BOOLEAN 'True')| | x | | | x | x |
|VerifyCollationCompatibility=(BOOLEAN 'True')| | x | | | x | x |
|VerifyDeployment=(BOOLEAN 'True')| | x | | | x | x |
|VerifyExtraction=(BOOLEAN)| w | | | | | |
|VerifyFullTextDocumentTypesSupported=(BOOLEAN)| | | w | | | |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Extract-Aktion von SqlPackage](sqlpackage-extract.md).
- Erfahren Sie mehr über die [Publish-Aktion von SqlPackage](sqlpackage-publish.md).
- Erfahren Sie mehr über die [Export-Aktion von SqlPackage](sqlpackage-export.md).
- Erfahren Sie mehr über die [Import-Aktion von SqlPackage](sqlpackage-import.md).
