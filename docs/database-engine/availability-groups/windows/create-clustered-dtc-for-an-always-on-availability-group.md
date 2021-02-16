---
title: Erstellen einer gruppierten DTC-Ressource für eine Verfügbarkeitsgruppe
description: In diesem Thema werden Sie durch eine vollständige Konfiguration einer gruppierten DTC-Ressource für eine SQL Server AlwaysOn-Verfügbarkeitsgruppe geführt.
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4974d6e0dcacfeae0c37092ea43bb529177c174b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350816"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>Erstellen einer gruppierten DTC-Ressource für eine Always On-Verfügbarkeitsgruppe

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

In diesem Thema werden Sie durch eine vollständige Konfiguration einer gruppierten DTC-Ressource für eine SQL Server AlwaysOn-Verfügbarkeitsgruppe geführt. Für die vollständige Konfiguration kann bis zu einer Stunde erforderlich sein. 

In der exemplarischen Vorgehensweise werden eine gruppierte DTC-Clusterressource und die SQL Server-Verfügbarkeitsgruppen erstellt, um die Anforderungen zu erfüllen, die unter [Cluster-DTC für Verfügbarkeitsgruppen in SQL Server 2016](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md) formuliert sind.

In der exemplarischen Vorgehensweise werden PowerShell- und T-SQL-Skripts (Transact-SQL) verwendet.  Für viele T-SQL-Skripts muss der **SQLCMD-Modus** aktiviert sein.  Weitere Informationen zum **SQLCMD-Modus** finden Sie unter [Aktivieren von SQLCMD-Skripts im Abfrage-Editor](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md).  Das PowerShell-Modul **FailoverClusters** muss importiert werden.  Informationen zum Importieren eines PowerShell-Moduls finden Sie unter [Importieren eines PowerShell-Moduls](/powershell/scripting/developer/module/importing-a-powershell-module).  Für diese exemplarische Vorgehensweise wird Folgendes angenommen:
- Alle Anforderungen aus [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md) sind erfüllt.  
- Die Domäne ist `contoso.lab`.
- Der Benutzer hat die Berechtigung zum Erstellen von Computerobjekten in der Organisationseinheit, in der die DTC-Netzwerknamenressource erstellt wird.
- Der Benutzer ist ein Domänenbenutzer, der Administratorrechte für alle Knoten im Cluster hat.
- Eine Dateifreigabe namens `sqlbackups` wurde für Sicherungen erstellt.
- Die Standardinstanzen von SQL Server haben die Namen `SQLNODE1` und `SQLNODE2`.
- Für alle Instanzen von SQL Server wird dasselbe Dienstkonto verwendet.
- Der Benutzer ist in allen Instanzen von SQL Server Mitglied der festen SQL Server-Rolle „sysadmin“.
- Das Standardergebnis von Transaktionen, die DTC nicht auflösen konnte, wird auf `presume commit`festgelegt.
- Für den Spiegelungsendpunkt wird der Port `5022`verwendet.
- Es gibt keine weiteren Verfügbarkeitsgruppen oder gruppierten DTC-Ressourcen.
- Clusterdetails (vorhanden):
  - Name: `Cluster`
  - Netzwerkname: `Cluster Network 1`
  - Nodes: `SQLNODE1, SQLNODE2`
  - Freigegebener Speicher: `Cluster Disk 3` (im Besitz von `SQLNODE1`)
- Clusterdetails (zu erstellen):
  - Netzwerknamenressource: `DTCnet1`
  - DTC-Netzwerknamenressource: `DTC1`
  - Physische DTC-Datenträgerressource:`DTCDisk1`
  - DTC-IP- und Subnetzressource: `192.168.2.54`, `255.255.255.0`
  - DTC-IP-Ressource: `DTCIP1`

## <a name="1-check-operating-system"></a>1. Überprüfen des Betriebssystems
Für unterstützte verteilte Transaktionen muss [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unter Windows Server 2016 oder Windows Server 2012 R2 ausgeführt werden.  Für Windows Server 2012 R2 müssen Sie das Update in KB3090973 installieren, das unter [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973) verfügbar ist.  In diesem Skript wird die Version des Betriebssystems und wird überprüft, ob der Hotfix 3090973 installiert werden muss.  Führen Sie das folgende PowerShell-Skript auf `SQLNODE1` aus.

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2.   Konfigurieren von Firewallregeln
In diesem Skript wird die Firewall so konfiguriert, dass DTC-Datenverkehr sowohl auf jedem SQL-Server, der ein Replikat der Verfügbarkeitsgruppe hostet, als auch auf allen anderen Servern zugelassen wird, die an der verteilten Transaktion beteiligt sind.  Außerdem wird die Firewall im Skript so konfiguriert, dass sie Verbindungen für den Datenbankspiegelungs-Endpunkt zulässt.  Führen Sie das folgende PowerShell-Skript auf `SQLNODE1` aus.

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.  Konfigurieren von **Lösung für unklare Transaktion** 
In diesem Skript wird die Serverkonfigurationsoption **Lösung für unklare Transaktion** für unsichere Transaktionen auf „presume commit“ festgelegt.  Führen Sie das folgende T-SQL-Skript in SQL Server Management Studio (SSMS) für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4. Erstellen von Testdatenbanken
Im Skript wird eine Datenbank namens `AG1` auf `SQLNODE1` und eine Datenbank namens `dtcDemoAG1` auf `SQLNODE2` erstellt.  Führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5.   Erstellen von Endpunkten
In diesem Skript wird ein Endpunkt namens `AG1_endpoint` erstellt, der am TCP-Port `5022` lauscht.  Führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6.   Vorbereiten der Datenbanken für die Verfügbarkeitsgruppe
Im Skript wird `AG1` in `SQLNODE1` gesichert und in `SQLNODE2` wiederhergestellt.  Führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7.   Erstellen der Verfügbarkeitsgruppe
[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] müssen mit dem Befehl **REATE AVAILABILITY GROUP** und der Klausel **WITH DTC_SUPPORT = PER_DB** erstellt werden.  Zurzeit können Sie eine vorhandene Verfügbarkeitsgruppe nicht ändern.  Der Assistent für neue Verfügbarkeitsgruppen lässt es nicht zu, dass Sie die DTC-Unterstützung für eine neue Verfügbarkeitsgruppe aktivieren.  Im folgenden Skript wird die neue Verfügbarkeitsgruppe erstellt und mit der sekundären zusammengeführt.  Führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> Sie können DTC nicht auf einem vorhandenen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktivieren.  In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wird für eine bestehende Verfügbarkeitsgruppe folgende Syntax akzeptiert:  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> Es wird aber keine tatsächliche Konfigurationsänderung vorgenommen.  Sie können die **dtc_support** -Konfiguration mit der folgenden T-SQL-Abfrage bestätigen:  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> Die einzige Möglichkeit zum Aktivieren von DTC-Unterstützung für eine Verfügbarkeitsgruppe besteht darin, eine Verfügbarkeitsgruppe über Transact-SQL zu erstellen.
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.  Vorbereiten von Clusterressourcen

In diesem Skript werden die DTC-abhängigen Ressourcen vorbereitet: Datenträger und IP.  Der freigegebene Speicher wird dem Windows-Cluster hinzugefügt.  Es werden Netzwerkressourcen erstellt, und dann wird der DTC erstellt und als Ressource für die Verfügbarkeitsgruppe deklariert.  Führen Sie das folgende PowerShell-Skript auf `SQLNODE1` aus. Vielen Dank an [Allan Hirt](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) für das Skript.

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9.   Aktivieren des Netzwerk-DTCs 

Im folgenden Skript wird DTC-Netzwerkzugriff für den gruppierten DTC-Dienst aktiviert, damit sich Remotecomputer über das Netzwerk bei MSDTC anmelden können, um sich an verteilten Transaktionen zu beteiligen.  Führen Sie das folgende PowerShell-Skript auf `SQLNODE1` aus.

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.  Deaktivieren und Beenden des lokalen DTC-Diensts auf jedem Knoten

Deaktivieren Sie den lokalen DTC auf beiden Knoten, um sicherzustellen, dass verteilte Transaktionen die gruppierte DTC-Ressource verwenden.  Im folgenden Skript wird der lokale DTC-Diensts auf jedem Knoten deaktiviert und beendet.  Führen Sie das folgende PowerShell-Skript auf `SQLNODE1` aus.
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.  Beenden und Neustarten des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Diensts für jede Instanz

Wenn der gruppierte DTC-Dienst vollständig konfiguriert ist, müssen Sie jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]in der Verfügbarkeitsgruppe beenden und neu starten, um sicherzustellen, dass [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] für die Nutzung dieses DTC-Diensts registriert ist.

Wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienst erstmalig eine verteilte Transaktion erfordert, registriert er sich beim DTC-Dienst. SQL Server-Dienst wird den DTC-Dienst weiterhin verwenden, bis er neu gestartet ist. Wenn ein gruppierter DTC-Dienst verfügbar ist, registriert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sich beim gruppierten DTC-Dienst. Wenn kein gruppierter DTC-Dienst verfügbar ist, registriert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sich beim lokalen DTC-Dienst. Beenden Sie jede [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz, und starten Sie diese neu, um zu überprüfen, ob [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] beim gruppierten DTC-Dienst registriert. 

Führen Sie die Schritte aus, die im folgenden T-SQL-Skript enthalten sind:
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.  Testkonfiguration

Dieser Test verwendet einen Verbindungsserver von `SQLNODE1`zu `SQLNODE2`, um eine verteilte Transaktion zu erstellen.  Vergewissern Sie sich, dass sich das primäre Replikat der Verfügbarkeitsgruppe in `SQLNODE1`. Um die Konfiguration zu testen, führen Sie folgende Schritte aus:

- Erstellen von Verbindungsservern
- Ausführen einer verteilten Transaktion

### <a name="create-linked-servers"></a>Erstellen von Verbindungsservern  
Im folgenden Skript werden zwei Verbindungsserver in `SQLNODE1`erstellt.  Führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1`aus.

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>Ausführen einer verteilten Transaktion
In diesem Skript wird zunächst die aktuelle DTC-Transaktionsstatistik zurückgegeben.  Anschließend wird im Skript eine verteilte Transaktion ausgeführt, in der Datenbanken aus `SQLNODE1` und `SQLNODE2`verwendet werden.  Danach wird im Skript erneut die DTC-Transaktionsstatistik zurückgegeben, die jetzt eine erhöhte Anzahl zeigen sollte.  Stellen Sie eine physische Verbindung mit `SQLNODE1` und führen Sie das folgende T-SQL-Skript in SSMS für `SQLNODE1` im **SQLCMD-Modus** aus.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> Die `USE AG1` -Anweisung muss ausgeführt werden, um sicherzustellen, dass der Datenbankkontext auf `AG1`festgelegt ist.  Andernfalls wird Ihnen die folgende Fehlermeldung angezeigt: „Der Transaktionskontext wird von einer anderen Sitzung verwendet.“