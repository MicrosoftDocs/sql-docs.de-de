---
title: Verwalten von SQL Server für Linux mit PowerShell Core
description: Erfahren Sie mehr über SQL Server PowerShell, indem Sie sich einige Beispiele zur Verwendung von SQL Server PowerShell mit PowerShell Core (PS Core) unter macOS und Linux ansehen.
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: 8018a90b3483f9baf4b58a4b057fa93685c6d828
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016325"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>Verwalten von SQL Server für Linux mit PowerShell Core

In diesem Artikel werden [SQL Server PowerShell](../powershell/sql-server-powershell.md) und einige Beispiele zur Verwendung mit PowerShell Core (PS Core) unter macOS und Linux vorgestellt. PowerShell Core ist jetzt ein Open Source-Projekt auf [GitHub](https://github.com/powershell/powershell).

## <a name="cross-platform-editor-options"></a>Plattformübergreifende Editor-Optionen

Alle unten genannten Schritte mit PowerShell Core funktionieren in einem regulären Terminal. Alternativ dazu können Sie die Schritte auch in einem Terminal in VS Code oder Azure Data Studio ausführen.  Sowohl VS Code als auch Azure Data Studio sind unter macOS und Linux verfügbar.  Weitere Informationen zu Azure Data Studio finden Sie in dieser [Schnellstartanleitung](../azure-data-studio/quickstart-sql-server.md).  Sie können auch die Verwendung der [PowerShell-Erweiterung](../azure-data-studio/extensions/powershell-extension.md) in Betracht ziehen.

## <a name="installing-powershell-core"></a>Installieren von PowerShell Core

Weitere Informationen zur Installation von PowerShell Core auf verschiedenen unterstützten und experimentellen Plattformen finden Sie in den folgenden Artikeln:

- [Installieren von PowerShell Core unter Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [Installieren von PowerShell Core unter Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Installieren von PowerShell Core unter macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Installieren von PowerShell Core unter ARM](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>Installieren des SqlServer-Moduls

Das `SqlServer`-Modul wird im [PowerShell-Katalog](https://www.powershellgallery.com/packages/SqlServer/) verwaltet. Wenn Sie mit SQL Server arbeiten, sollten Sie immer die neueste Version der PowerShell-Moduls „SqlServer“ verwenden.

Um das SqlServer-Modul zu installieren, öffnen Sie eine PowerShell Core-Sitzung, und führen Sie den folgenden Code aus:

```powerhsell
Install-Module -Name SqlServer
```

Weitere Informationen zum Installieren des SqlServer-Moduls aus dem PowerShell-Katalog finden Sie auf dieser [Seite](../powershell/download-sql-server-ps-module.md).

## <a name="using-the-sqlserver-module"></a>Verwenden des SqlServer-Moduls

Starten Sie zunächst PowerShell Core.  Wenn Sie auf einem macOS- oder Linux-Computer arbeiten, öffnen Sie eine *Terminalsitzung*, und geben Sie **pwsh** ein, um eine neue PowerShell Core-Sitzung zu starten.  Unter Windows verwenden Sie <kbd>Win</kbd>+<kbd>R</kbd> und geben `pwsh` ein, um eine neue PowerShell Core-Sitzung zu starten.

```
pwsh
```

SQL Server stellt ein PowerShell-Modul namens **SqlServer** bereit. Sie können das Modul **SqlServer** verwenden, um die SQL Server-Komponenten (SQL Server-Anbieter und -Cmdlets) in eine PowerShell-Umgebung oder ein PowerShell-Skript zu importieren.

Kopieren Sie den folgenden Befehl in die PowerShell-Eingabeaufforderung, um das Modul **SqlServer** in Ihre aktuelle PowerShell-Sitzung zu importieren:

```powershell
Import-Module SqlServer
```

Geben Sie den folgenden Befehl in die PowerShell-Eingabeaufforderung ein, um zu überprüfen, ob das **SqlServer**-Modul ordnungsgemäß importiert wurde:

```powershell
Get-Module -Name SqlServer
```

PowerShell sollte Informationen ähnlich der folgenden Ausgabe anzeigen:

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Herstellen einer Verbindung mit SQL Server und Abrufen von Serverinformationen

In den folgenden Schritten wird PowerShell Core verwendet, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen und einige Servereigenschaften anzuzeigen.

Kopieren Sie die folgenden Befehle in die PowerShell-Eingabeaufforderung. Wenn Sie diese Befehle ausführen, führt PowerShell Folgendes aus:
- Ein Dialogfeld wird angezeigt, das Sie auffordert, den Hostnamen oder die IP-Adresse Ihrer Instanz einzugeben.
- Das Dialogfeld *Bei PowerShell anmelden* wird angezeigt, das Sie auffordert, die Anmeldeinformationen einzugeben. Sie können Ihren *SQL-Benutzernamen* und Ihr *SQL-Kennwort* verwenden, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen.
- Das Cmdlet **Get-SqlInstance** wird verwendet, um eine Verbindung mit dem **Server** herzustellen und einige Eigenschaften anzuzeigen.

Optional können Sie einfach die `$serverInstance`-Variable durch die IP-Adresse oder den Hostnamen Ihrer SQL Server-Instanz ersetzen.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell sollte Informationen ähnlich der folgenden Ausgabe anzeigen:

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------                   -------    ------------ -----------  ------------ ----------------
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu
```

> [!NOTE]
> Wenn für diese Werte nichts angezeigt wird, ist beim Herstellen einer Verbindung mit der SQL Server-Zielinstanz höchstwahrscheinlich ein Fehler aufgetreten. Stellen Sie sicher, dass Sie dieselben Verbindungsinformationen verwenden können, um eine Verbindung über SQL Server Management Studio herzustellen. Überprüfen Sie anschließend die [Empfehlungen zur Verbindungsproblembehandlung](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Verwenden des SQL Server PowerShell-Anbieters

Die Verwendung des [SQL Server PowerShell-Anbieters](../powershell/sql-server-powershell-provider.md) ist eine weitere Option zum Herstellen einer Verbindung mit Ihrer SQL Server-Instanz.  Dieser Anbieter ermöglicht Ihnen die Navigation Ihrer SQL Server-Instanz über die Befehlszeile, so als würden Sie in der Struktur im Objekt-Explorer navigieren.  Dieser Anbieter wird standardmäßig als PSDrive (PowerShell-Laufwerk) namens `SQLSERVER:\` dargestellt, das Sie verwenden können, um Verbindungen mit SQL Server-Instanzen herzustellen, auf die Ihr Domänenkonto zugreifen kann, und in diesen zu navigieren.  Informationen zum Einrichten der Active Directory-Authentifizierung für SQL Server für Linux finden Sie in den [Konfigurationsschritten](./sql-server-linux-active-directory-auth-overview.md#configuration-steps).

Für den SQL Server PowerShell-Anbieter können Sie auch die SQL-Authentifizierung verwenden. Verwenden Sie hierzu das Cmdlet `New-PSDrive`, um ein neues PSDrive zu erstellen, und geben Sie die entsprechenden Anmeldeinformationen zum Herstellen einer Verbindung an.

Das folgende Beispiel zeigt, wie Sie unter Verwendung der SQL-Authentifizierung ein neues PSDrive erstellen.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Sie können überprüfen, ob das Laufwerk erstellt wurde, indem Sie das Cmdlet `Get-PSDrive` ausführen.

```powershell
Get-PSDrive
```

Sobald Sie das neue PSDrive erstellt haben, können Sie damit beginnen, darin zu navigieren.

```powershell
dir SQLonDocker:\Databases
```

Die Ausgabe kann wie folgt aussehen.  Möglicherweise stellen Sie fest, dass die Ausgabe derjenigen ähnelt, die SSMS auf dem Datenbankknoten anzeigt.  Es werden die Benutzerdatenbanken angezeigt, aber nicht die Systemdatenbanken.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Wenn Sie alle Datenbanken in Ihrer Instanz anzeigen müssen, bietet sich beispielsweise das Cmdlet `Get-SqlDatabase` an.

## <a name="get-databases"></a>Abrufen von Datenbanken

Ein wichtiges Cmdlet, das Sie kennen müssen, ist `Get-SqlDatabase`.  Bei vielen Vorgängen, die eine Datenbank oder Objekte in einer Datenbank einbeziehen, kann das Cmdlet `Get-SqlDatabase` verwendet werden.  Wenn Sie Werte für den `-ServerInstance`-Parameter und den `-Database`-Parameter angeben, wird nur dieses Datenbankobjekt abgerufen.  Wenn Sie jedoch nur den `-ServerInstance`-Parameter angeben, wird eine vollständige Liste aller Datenbanken in dieser Instanz zurückgegeben.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

Hier sehen Sie ein Beispiel einer möglichen Rückgabe des oben genannten Get-SqlDatabase-Befehls:

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>Untersuchen von SQL Server-Fehlerprotokollen

In den folgenden Schritten wird PowerShell Core zum Untersuchen von Fehlerprotokollen zu Ihrer SQL Server-Instanz unter Linux verwendet.

Kopieren Sie die folgenden Befehle in die PowerShell-Eingabeaufforderung. Die Ausführung dieser Befehle kann einige Minuten dauern. Diese Befehle bewirken Folgendes:
- Ein Dialogfeld wird angezeigt, das Sie auffordert, den Hostnamen oder die IP-Adresse Ihrer Instanz einzugeben.
- Das Dialogfeld *Bei PowerShell anmelden* wird angezeigt, das Sie auffordert, die Anmeldeinformationen einzugeben. Sie können Ihren *SQL-Benutzernamen* und Ihr *SQL-Kennwort* verwenden, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen.
- Das Cmdlet **Get-SqlErrorLog** wird verwendet, um eine Verbindung mit der SQL Server-Instanz unter Linux herzustellen und die Fehlerprotokolle abzurufen, die seit gestern (**Yesterday**) aufgetreten sind.

Alternativ dazu können Sie die `$serverInstance`-Variable durch die IP-Adresse oder den Hostnamen Ihrer SQL Server-Instanz ersetzen.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>Erkunden der zurzeit in PS Core verfügbaren Cmdlets
Für das SqlServer-Modul stehen derzeit 109 Cmdlets in Windows PowerShell zur Verfügung, davon sind aber nur 62 in PSCore verfügbar. Unten finden Sie eine vollständige Liste der derzeit verfügbaren 62 Cmdlets.  Eine ausführliche Dokumentation aller Cmdlets im SqlServer-Modul finden Sie in der [Cmdlet-Referenz](/powershell/module/sqlserver/) zu SqlServer.

Der folgende Befehl zeigt alle Cmdlets an, die in der von Ihnen verwendeten PowerShell-Version verfügbar sind.

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup_SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>Weitere Informationen
- [SQL Server-PowerShell](../powershell/sql-server-powershell.md)