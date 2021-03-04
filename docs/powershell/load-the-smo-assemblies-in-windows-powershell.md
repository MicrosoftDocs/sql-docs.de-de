---
title: Laden der SMO-Assemblys in Windows PowerShell
description: Hier erfahren Sie, wie die SQL Server Management Objects-Assemblys (SMO) in PowerShell-Skripts unter Windows geladen werden, die nicht den SQL Server PowerShell-Anbieter verwenden.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 81fb0cc3452cc04228e1f0e34f5be94f6cefd7af
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838064"
---
# <a name="load-the-smo-assemblies-in-windows-powershell"></a>Laden der SMO-Assemblys in Windows PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In diesem Artikel wird beschrieben, wie die Assemblys von SQL Server Management Objects (SMO) in Windows PowerShell-Skripts geladen werden, die nicht den SQL Server PowerShell-Anbieter verwenden.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Der bevorzugte Mechanismus zum Laden der SMO-Assemblys besteht im Laden des **SqlServer**-Moduls. Der im Modul enthaltene [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter lädt die SMO-Assemblys automatisch und implementiert außerdem Funktionen, die die Nützlichkeit der SMO-Objekte in PowerShell-Skripts erweitern.

In zwei Szenarien müssen die SMO-Assemblys direkt geladen werden:  

- Das Skript verweist vor dem ersten Befehl, der auf den Anbieter oder die Cmdlets der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Snap-Ins verweist, auf ein SMO-Objekt.  

- Sie möchten SMO-Code portieren, der in einer anderen Sprache geschrieben wurde, die weder den Anbieter noch Cmdlets verwendet (z. B. C# oder Visual Basic).  

## <a name="example-loading-the-sql-server-management-objects"></a>Beispiel: Laden von SQL Server Management Objects

Mit folgendem Code werden die SMO-Assemblys geladen:  

```powershell
# Loads the SQL Server Management Objects (SMO)  

$ErrorActionPreference = "Stop"
  
$sqlpsreg="HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.SqlServer.Management.PowerShell.sqlps"  
  
if (Get-ChildItem $sqlpsreg -ErrorAction "SilentlyContinue")  
{  
    throw "SQL Server Provider for Windows PowerShell is not installed."  
}  
else  
{  
    $item = Get-ItemProperty $sqlpsreg  
    $sqlpsPath = [System.IO.Path]::GetDirectoryName($item.Path)  
}  
  
$assemblylist =
"Microsoft.SqlServer.Management.Common",  
"Microsoft.SqlServer.Smo",  
"Microsoft.SqlServer.Dmf ",  
"Microsoft.SqlServer.Instapi ",  
"Microsoft.SqlServer.SqlWmiManagement ",  
"Microsoft.SqlServer.ConnectionInfo ",  
"Microsoft.SqlServer.SmoExtended ",  
"Microsoft.SqlServer.SqlTDiagM ",  
"Microsoft.SqlServer.SString ",  
"Microsoft.SqlServer.Management.RegisteredServers ",  
"Microsoft.SqlServer.Management.Sdk.Sfc ",  
"Microsoft.SqlServer.SqlEnum ",  
"Microsoft.SqlServer.RegSvrEnum ",  
"Microsoft.SqlServer.WmiEnum ",  
"Microsoft.SqlServer.ServiceBrokerEnum ",  
"Microsoft.SqlServer.ConnectionInfoExtended ",  
"Microsoft.SqlServer.Management.Collector ",  
"Microsoft.SqlServer.Management.CollectorEnum",  
"Microsoft.SqlServer.Management.Dac",  
"Microsoft.SqlServer.Management.DacEnum",  
"Microsoft.SqlServer.Management.Utility"  
  
foreach ($asm in $assemblylist)  
{  
    $asm = [Reflection.Assembly]::LoadWithPartialName($asm)  
}  
  
Push-Location  
cd $sqlpsPath  
update-FormatData -prependpath SQLProvider.Format.ps1xml
Pop-Location  
```

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-PowerShell](sql-server-powershell.md)