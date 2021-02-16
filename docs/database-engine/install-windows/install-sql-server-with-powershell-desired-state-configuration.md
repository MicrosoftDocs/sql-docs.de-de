---
title: 'Installation: PowerShell Desired State Configuration'
description: Installieren Sie SQL Server mithilfe von PowerShell DSC und informieren Sie sich über das anfängliche Setup einer eigenständigen Instanz von SQL Server 2017 unter Windows Server 2016.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.devlang: PowerShell
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
author: randomnote1
ms.author: dareist
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a1105db9984a8ff52485247e6699c578ec350986
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061445"
---
# <a name="install-sql-server-with-powershell-desired-state-configuration"></a>Installieren von SQL Server mit PowerShell Desired State Configuration

Haben Sie sich jemals durch die Installationsoberfläche von SQL Server geklickt, indem Sie immer wieder auf die gleichen Schaltflächen geklickt und dieselben Informationen eingegeben haben, ohne darüber nachzudenken? Und nach der Installation stellen Sie fest, dass Sie vergessen haben, die DBA-Gruppe in der **sysadmin**-Rolle festzulegen. Dann mussten Sie die folgenden Schritte ausführen:
* Den Einzelbenutzermodus starten
* Die geeigneten Benutzer oder Gruppen hinzufügen
* SQL Server wieder in den Mehrbenutzermodus versetzen
* Tests durchführen 

Noch schlimmer ist jedoch, dass Sie nun die gesamte Installation in Frage stellen. „Was habe ich noch vergessen?“ Fragen Sie sich möglicherweise.

Erfahren Sie mehr über [PowerShell Desired State Configuration (DSC)](/powershell/scripting/dsc/overview/overview). Mithilfe von DSC können Sie eine Konfigurationsvorlage erstellen, die Sie für zahlreiche Server wiederverwenden können. Je nach Build müssen Sie möglicherweise einige der Setupparameter anpassen. Dies sollte jedoch kein Problem darstellen, da Sie alle Standardeinstellungen beibehalten können. Damit wird vermieden, dass die Eingabe eines wichtigen Parameters vergessen wird.

In diesem Artikel wird das Setup einer eigenständigen Instanz von SQL Server 2017 unter Windows Server 2016 mithilfe der DSC-Ressource **SqlServerDsc** erläutert. Vorkenntnisse zu DSC sind hilfreich, da die Funktionsweise von DSC nicht genauer erläutert wird.

Folgendes ist für diese exemplarische Vorgehensweise erforderlich:

- Ein Computer, auf dem Windows Server 2016 ausgeführt wird
- Ein Installationsmedium für SQL Server 2017
- Die DSC-Ressource **SqlServerDsc**

## <a name="prerequisites"></a>Voraussetzungen

In den meisten Fällen wird DSC verwendet, um die erforderlichen Voraussetzungen zu erfüllen. Zur Veranschaulichung werden die Voraussetzungen in diesem Artikel jedoch manuell verarbeitet.

## <a name="install-the-sqlserverdsc-dsc-resource"></a>Installieren der DSC-Ressource „SqlServerDsc“

Laden Sie die DSC-Ressource [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc) mithilfe des Cmdlets [Install-Module](/powershell/module/powershellget/Install-Module) aus dem [PowerShell-Katalog](https://www.powershellgallery.com/) herunter. 

> [!NOTE]
> Stellen Sie sicher, dass PowerShell **als Administrator ausgeführt** wird, wenn Sie das Modul installieren.

```PowerShell
Install-Module -Name SqlServerDsc
```

### <a name="get-the-sql-server-2017-installation-media"></a>Abrufen des SQL Server 2017-Installationsmediums
Laden Sie das SQL Server 2017-Installationsmedium auf dem Server herunter. In diesem Fall wurde SQL Server 2017 Enterprise aus einem Visual Studio-Abonnement heruntergeladen, und die ISO-Datei wurde nach `C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso` kopiert.

Nun muss die ISO-Datei in ein Verzeichnis extrahiert werden:

```PowerShell
New-Item -Path C:\SQL2017 -ItemType Directory
$mountResult = Mount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso' -PassThru
$volumeInfo = $mountResult | Get-Volume
$driveInfo = Get-PSDrive -Name $volumeInfo.DriveLetter
Copy-Item -Path ( Join-Path -Path $driveInfo.Root -ChildPath '*' ) -Destination C:\SQL2017\ -Recurse
Dismount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso'
```

## <a name="create-the-configuration"></a>Erstellen der Konfiguration

### <a name="configuration"></a>Konfiguration

Erstellen Sie die Konfigurationsfunktion, die aufgerufen wird, um die [MOF-Dokumente (Managed Object Format)](/windows/desktop/WmiSdk/managed-object-format--mof-) zu generieren:

```PowerShell
Configuration SQLInstall
{...}
```

### <a name="modules"></a>Module

Importieren Sie die Module in die aktuelle Sitzung. Durch diese Module weiß das Konfigurationsdokument, wie die MOF-Dokumente erstellt werden sollen. Die DSC-Engine wird außerdem angewiesen, wie die MOF-Dokumente auf dem Server angewendet werden sollen:

```PowerShell
Import-DscResource -ModuleName SqlServerDsc
```

### <a name="resources"></a>Ressourcen

#### <a name="net-framework"></a>.NET Framework

SQL Server ist vom .NET Framework abhängig. Deshalb müssen Sie sicherstellen, dass .NET Framework installiert ist, bevor Sie SQL Server installieren. Die Ressource **WindowsFeature** wird verwendet, um das Feature „**Net-Framework-45-Core“´´** zu installieren:

```PowerShell
WindowsFeature 'NetFramework45'
{
     Name = 'Net-Framework-45-Core'
     Ensure = 'Present'
}
```

#### <a name="sqlsetup"></a>SqlSetup

Die Ressource **SqlSetup** wird verwendet, um DSC mitzuteilen, wie SQL Server installiert werden soll. Die folgenden Parameter sind für eine Basisinstallation erforderlich:

- **InstanceName:** Der Name der Instanz. Verwenden Sie **MSSQLSERVER** als Standardinstanz.
- **Features:** Die Features, die installiert werden sollen. In diesem Beispiel wird nur das Feature **SQLEngine** installiert.
- **SourcePath:** Der Pfad zum Installationsmedium für SQL Server. In diesem Beispiel wurde das Installationsmedium für SQL Server unter `C:\SQL2017` gespeichert. Mit einer Netzwerkfreigabe können Sie den auf dem Server genutzten Speicherplatz verringern.
- **SQLSysAdminAccounts:** Die Benutzer oder Gruppen, die Mitglieder in der Rolle **sysadmin** sein sollen. In diesem Beispiel wird der lokalen Gruppe „Administrators“ der **sysadmin**-Zugriff gewährt. 

> [!NOTE]
> Von dieser Konfiguration wird in Umgebungen mit hoher Sicherheit abgeraten.

Eine vollständige Liste mit Beschreibungen der Parameter, die für **SqlSetup** verfügbar sind, finden Sie im [GitHub-Repository „SqlServerDsc“](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlsetup).

Mit der Ressource **SqlSetup** wird lediglich SQL Server installiert. Die angewendeten Einstellung werden **nicht** beibehalten. Ein Beispiel hierfür ist der zur Installation angegebene Parameter **SQLSysAdminAccounts**. Administratoren können Anmeldedaten zwar zur Rolle **sysadmin** hinzufügen oder aus ihr entfernen, aber die Ressource **SqlSetup** ist davon nicht betroffen. Verwenden Sie die Ressource [SqlServerRole](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlserverrole), wenn DSC die Mitgliedschaft bei der **sysadmin**-Rolle erzwingen soll.

#### <a name="finish-configuration"></a>Fertigstellen der Konfiguration

```PowerShell
Configuration SQLInstall
{
     Import-DscResource -ModuleName SqlServerDsc

     node localhost
     {
          WindowsFeature 'NetFramework45'
          {
               Name   = 'NET-Framework-45-Core'
               Ensure = 'Present'
          }

          SqlSetup 'InstallDefaultInstance'
          {
               InstanceName        = 'MSSQLSERVER'
               Features            = 'SQLENGINE'
               SourcePath          = 'C:\SQL2017'
               SQLSysAdminAccounts = @('Administrators')
               DependsOn           = '[WindowsFeature]NetFramework45'
          }
     }
}
```

## <a name="build-and-deploy"></a>Erstellen und Bereitstellen

### <a name="compile-the-configuration"></a>Kompilieren der Konfiguration

Referenzieren Sie das Konfigurationsskript per DOT-Quellenentnahme:

```PowerShell
. .\SQLInstallConfiguration.ps1
```

Führen Sie die Konfigurationsfunktion aus:

```PowerShell
SQLInstall
```

Ein Verzeichnis namens **SQLInstall** wird im Arbeitsverzeichnis erstellt. Dieses enthält eine Datei namens **localhost.mof**. Überprüfen Sie den Inhalt dieser MOF-Datei, die die kompilierte DSC-Konfiguration anzeigt.

### <a name="deploy-the-configuration"></a>Bereitstellen der Konfiguration

Rufen Sie das Cmdlet **Start-DscConfiguration** auf, um die DSC-Bereitstellung von SQL Server zu beginnen. Die folgenden Parameter werden dem Cmdlet bereitgestellt:

- **-Path:** Legt den Pfad zum Ordner fest, der die bereitzustellenden MOF-Dokumente enthält, z. B. `C:\SQLInstall`.
- **-Wait:** Legt fest, dass auf den Abschluss des Konfigurationsauftrags gewartet werden soll.
- **-Force:** Legt fest, dass vorhandene DSC-Konfigurationen überschrieben werden sollen.
- **-Verbose:** Legt fest, dass die ausführliche Ausgabe angezeigt werden soll. Dies ist hilfreich, wenn Sie zum ersten Mal eine Konfiguration zur Problembehandlung per Push übertragen.

```PowerShell
Start-DscConfiguration -Path C:\SQLInstall -Wait -Force -Verbose
```

Während die Konfiguration angewendet wird, zeigt die ausführliche Ausgabe an, was passiert. Solange keine Fehler (roter Text) ausgelöst werden, wenn **Operation 'Invoke CimMethod' complete** (‚Invoke CimMethod‘-Vorgang wurde erfolgreich abgeschlossen) angezeigt wird, sollte SQL Server nun installiert sein.

## <a name="validate-installation"></a>Überprüfen der Installation

### <a name="dsc"></a>DSC

Mit dem Cmdlet [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/test-dscconfiguration) können Sie ermitteln, ob der aktuelle Zustand des Servers dem gewünschten Zustand entspricht. In diesem Fall fragen Sie den Zustand der SQL Server-Installation ab. Das Cmdlet **Test-DscConfiguration** sollte **TRUE** zurückgeben:

```PowerShell
PS C:\> Test-DscConfiguration
True
```

### <a name="services"></a>Dienste

Beim Abrufen der Dienstliste werden nun SQL Server-Dienste zurückgegeben:

```PowerShell
PS C:\> Get-Service -Name *SQL*
Status  Name           DisplayName
------  ----           -----------
Running MSSQLSERVER    SQL Server (MSSQLSERVER)
Stopped SQLBrowser     SQL Server Browser
Running SQLSERVERAGENT SQL Server Agent (MSSQLSERVER)
Running SQLTELEMETRY   SQL Server CEIP service (MSSQLSERVER)
Running SQLWriter      SQL Server VSS Writer
```

### <a name="sql-server"></a>SQL Server

```PowerShell
PS C:\> & sqlcmd -S $env:COMPUTERNAME
1> SELECT @@SERVERNAME
2> GO
1> quit
```

## <a name="see-also"></a>Weitere Informationen

[Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview)

[Install SQL Server from the command prompt (Installieren von SQL Server über die Eingabeaufforderung)](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

[Install SQL Server by using a configuration file (Installieren von SQL Server mithilfe einer Konfigurationsdatei)](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)
