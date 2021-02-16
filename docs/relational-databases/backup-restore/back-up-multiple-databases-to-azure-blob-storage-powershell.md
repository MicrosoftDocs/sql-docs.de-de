---
title: 'Sichern von mehreren Datenbanken: Azure Blob Storage'
description: Dieser Artikel enthält Beispielskripts, die verwendet werden können, um SQL Server-Sicherungen im Azure Blob Storage-Dienst mit PowerShell-Cmdlets zu automatisieren.
titleSuffix: PowerShell
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: f7008339-e69d-4e20-9265-d649da670460
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3fb357b16572f34c906942e79be2d8488b49a15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347718"
---
# <a name="back-up-multiple-databases-to-azure-blob-storage---powershell"></a>Sichern mehrerer Datenbanken in Azure Blob Storage – PowerShell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dieses Thema enthält Beispielskripts, die verwendet werden können, um Sicherungen im Azure Blob Storage-Dienst mit PowerShell-Cmdlets zu automatisieren.  
  
## <a name="overview-of-powershell-cmdlets-for-backup-and-restore"></a>Übersicht über PowerShell-Cmdlets für Sicherungen und Wiederherstellungen

Die **Backup_SqlDatabase** und die **Restore-SqlDatabase** sind die beiden wichtigsten Cmdlets, die für Sicherungs- und Wiederherstellungsvorgänge verfügbar sind.

Darüber hinaus gibt es weitere Cmdlets, die erforderlich sein können, um Sicherungen in Microsoft Azure Blob Storage zu automatisieren, wie z.B. die **SqlCredential**-Cmdlets.

Eine Liste aller verfügbaren Cmdlets finden Sie unter [SqlServer-Cmdlets](/powershell/module/sqlserver).
  
> [!TIP]  
> Die **SqlCredential**-Cmdlets werden in Szenarios für den Azure Blob Storage bei der Sicherung und Wiederherstellung verwendet. Weitere Details zu Anmeldeinformationen und deren Verwendung finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).
  
### <a name="powershell-for-multi-database-multi-instance-backup-operations"></a>PowerShell für mehrere Datenbanken, Sicherungsvorgänge mit mehreren Instanzen

Die folgenden Abschnitte enthalten die Skripts für verschiedene Vorgänge wie das Erstellen von SQL-Anmeldeinformationen in mehreren SQL Server-Instanzen, das Sichern aller Benutzerdatenbanken in einer Instanz von SQL Server usw. Sie können diese Skripts verwenden, um Sicherungsvorgänge gemäß den Anforderungen Ihrer Umgebung zu automatisieren oder zu planen. Die Skripts, die hier bereitgestellt werden, sind Beispiele und können für Ihre Umgebung geändert oder erweitert werden.  
  
Folgende Überlegungen sind bei Beispielskripts zu bedenken:  
  
- SQL Server PowerShell implementiert Cmdlets, um in der Pfadstruktur zu navigieren, die die Hierarchie von Objekten darstellt, die von einem PowerShell-Anbieter unterstützt werden. Wenn Sie zu einem Knoten im Pfad navigiert haben, können Sie andere Cmdlets verwenden, um grundlegende Vorgänge für das aktuelle Objekt auszuführen.

  Weitere Informationen finden Sie unter [Navigate SQL Server PowerShell Paths](../../powershell/navigate-sql-server-powershell-paths.md).

- **Get-ChildItem**-Cmdlet: Welche Informationen von **Get-ChildItem** zurückgegeben werden, hängt vom Speicherort in einem SQL Server PowerShell-Pfad ab. Wenn der Speicherort auf der Computerebene liegt, gibt dieses Cmdlets alle SQL Server-Datenbank-Engine-Instanzen zurück, die auf dem Computer installiert sind. Oder, wenn der Speicherort aber auf Objektebene, wie z. B. Datenbanken, liegt, dann gibt dieses Cmdlet eine Liste von Datenbankobjekten zurück. Standardmäßig gibt das **Get-ChildItem** -Cmdlet keine Systemobjekte zurück. Verwenden Sie den Parameter `–Force`, um die Systemobjekte anzuzeigen.

- Für alle Sicherungs- und Wiederherstellungsvorgänge des Azure Blob Storage-Diensts werden ein Azure-Speicherkonto und SQL-Anmeldeinformationen benötigt.
  
### <a name="create-a-sql-credential-on-all-instances-of-sql-server"></a>Erstellen von SQL-Anmeldeinformationen für alle Instanzen von SQL Server

Das folgende Skript kann verwendet werden, um generische SQL-Anmeldeinformationen für alle Instanzen von SQL Server auf dem Computer zu erstellen. Wenn bereits vorhandene Anmeldeinformationen mit dem gleichen Namen für eine der Instanzen des Computers vorliegen, zeigt das Skript den Fehler an und wird dann fortgesetzt.  
  
```powershell
# load the sqlps module
import-module sqlps  
  
# set parameters
$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$storageKey = "<myStorageAccessKey>"  
$secureString = ConvertTo-SecureString $storageKey -AsPlainText -Force  
$credentialName = "myCredential-$(Get-Random)"

Write-Host "Generate credential: " $credentialName
  
#cd to sql server and get instances  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and create a SQL credential, output any errors
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials"
        New-SqlCredential -Name $credentialName -Identity $storageAccount -Secret $secureString -Path $path -ea Stop | Out-Null
        Write-Host "...generated credential $($path)\$($credentialName)."  }
    catch { Write-Host $_.Exception.Message } }
```

> [!NOTE]
> Die Anweisung `-ea Stop | Out-Null` wird für die benutzerdefinierte Ausnahmeausgabe verwendet. Wenn Sie PowerShell-Standardfehlermeldungen bevorzugen, kann diese Anweisung entfernt werden. 

### <a name="remove-a-sql-credential-from-all-instances-of-sql-server"></a>Entfernen von SQL-Anmeldeinformationen aus allen Instanzen von SQL Server

Das folgende Skript kann verwendet werden, um bestimmte Anmeldeinformationen aus allen SQL Server-Instanzen auf dem Computer zu entfernen. Wenn die Anmeldeinformationen in einer bestimmten Instanz nicht vorhanden sind, wird eine Fehlermeldung angezeigt, und das Skript wird fortgesetzt, bis alle Instanzen überprüft sind.  
  
```powershell
import-module sqlps

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$credentialName = "<myCredential>"

Write-Host "Delete credential: " $credentialName

cd $sqlPath
$instances = Get-ChildItem

#loop through instances and delete a SQL credential
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials\$($credentialName)"
        Remove-SqlCredential -Path $path -ea Stop | Out-Null
        Write-Host "...deleted credential $($path)."  }
    catch { Write-Host $_.Exception.Message } }
```  
  
### <a name="full-backup-for-all-databases"></a>Vollständige Sicherung aller Datenbanken

Mit dem folgenden Skript erstellen Sie Sicherungen aller Datenbanken auf dem Computer. Dies gilt sowohl für Benutzerdatenbanken als auch für die **msdb** -Systemdatenbank. Das Skript filtert **tempdb** - und **model** -Systemdatenbanken heraus.  
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $backupUrlContainer
  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and backup all databases (excluding tempdb and model)
foreach ($instance in $instances)  {
    $path = "$($sqlPath)\$($instance.DisplayName)\databases"
    $databases = Get-ChildItem -Force -Path $path | Where-object {$_.name -ne "tempdb" -and $_.name -ne "model"}

    foreach ($database in $databases) {
        try {
            $databasePath = "$($path)\$($database.Name)"
            Write-Host "...starting backup: " $databasePath
            Backup-SqlDatabase -Database $database.Name -Path $path -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
            Write-Host "...backup complete."  }
        catch { Write-Host $_.Exception.Message } } }
```  
  
### <a name="full-backup-for-system-databases-only-on-a-specific-instance-of-sql-server"></a>Vollständige Sicherung für Systemdatenbanken auf nur einer bestimmten SQL Server-Instanz

Das vollständige Skript kann zum Sichern der **master** - und der **msdb** -Datenbanken in einer benannten Instanz von SQL Server verwendet werden. Das gleiche Skript kann für jede Instanz verwendet werden, indem der Wert des Instanzparameters geändert wird. Die Standardinstanz von SQL Server hat den Namen `DEFAULT`.
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$instanceName = "DEFAULT"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $instanceName " to " $backupUrlContainer
  
cd "$($sqlPath)\$($instanceName)"

#loop through instance and backup specific databases
$databases = "master", "msdb"  
foreach ($database in $databases) {
    try {
        Write-Host "...starting backup: " $database
        Backup-SqlDatabase -Database $database -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
        Write-Host "...backup complete." }
    catch { Write-Host $_.Exception.Message } }
```  
  
## <a name="see-also"></a>Weitere Informationen

[SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

[SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)