---
title: Verwenden von SQL Server PowerShell-Pfaden
description: Hier erfahren Sie, wie Sie Informationen mithilfe von Cmdlets oder den Methoden und Eigenschaften des Objekts bearbeiten und abrufen, das vom Anbieterpfad identifiziert wird.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 0b6878c7e0e6435bbb763629547437e9c22b062e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838727"
---
# <a name="work-with-sql-server-powershell-paths"></a>Verwenden von SQL Server PowerShell-Pfaden

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Nach der Navigation zu einem Knoten in einem [!INCLUDE[ssDE](../includes/ssde-md.md)] -Anbieterpfad können Sie mit den Methoden und Eigenschaften des [!INCLUDE[ssDE](../includes/ssde-md.md)] -Verwaltungsobjekts, das dem Knoten zugeordnet ist, Arbeiten ausführen oder Informationen abrufen.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Nach der Navigation zu einem Knoten in einem [!INCLUDE[ssDE](../includes/ssde-md.md)]-Anbieterpfad können Sie zwei Arten von Aktionen ausführen:  

- Sie können Windows PowerShell-Cmdlets ausführen, die Vorgänge an Knoten durchführen, z.B. **Rename-Item**.  

- Sie können die Methoden vom zugeordneten [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Modell des Verwaltungsobjekts aufrufen, z. B. SMO. Wenn Sie beispielsweise zum Knoten Databases in einem Pfad navigieren, können Sie die Methoden und Eigenschaften der <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse verwenden.  

Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter wird zum Verwalten der Objekte in einer Instanz von [!INCLUDE[ssDE](../includes/ssde-md.md)]verwendet. Er wird nicht verwendet, um mit den Daten in Datenbanken zu arbeiten. Wenn Sie zu einer Tabelle oder einer Sicht navigiert sind, können Sie den Anbieter nicht dazu verwenden, Daten auszuwählen, einzufügen, zu aktualisieren oder zu löschen. Verwenden Sie das Cmdlet **Invoke-Sqlcmd** , um Daten in Tabellen und Sichten aus der Windows PowerShell-Umgebung abzufragen oder zu ändern. Weitere Informationen finden Sie unter [Cmdlet Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd).  

##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> Auflisten von Methoden und Eigenschaften  

**Auflisten von Methoden und Eigenschaften**  

Sie können das Cmdlet **Get-Member** verwenden, um die für bestimmte Objekte oder Objektklassen verfügbaren Methoden und Eigenschaften anzuzeigen.  

### <a name="examples-listing-methods-and-properties"></a>Beispiele: Auflisten von Methoden und Eigenschaften

In diesem Beispiel wird eine Windows PowerShell-Variable auf die <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse von SMO festgelegt und werden die Methoden und Eigenschaften aufgeführt:  

```powershell
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 Sie können **Get-Member** ebenfalls verwenden, um die dem Endknoten eines Windows PowerShell-Pfads zugeordneten Methoden und Eigenschaften aufzulisten.  
  
 In diesem Beispiel wird zum Knoten Databases in einem SQLSERVER:-Pfad navigiert, und die Auflistungseigenschaften werden aufgeführt:  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 In diesem Beispiel wird zum Knoten AdventureWorks2012 in einem SQLSERVER:-Pfad navigiert, und die Objekteigenschaften werden aufgeführt:  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  

##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> Verwenden von Methoden und Eigenschaften  

**Verwenden von SMO-Methoden und Eigenschaften**  

Sie können SMO-Methoden und Eigenschaften dazu verwenden, Arbeiten an Objekten eines [!INCLUDE[ssDE](../includes/ssde-md.md)] -Anbieterpfads auszuführen.  

### <a name="examples-using-methods-and-properties"></a>Beispiele: Verwenden von Methoden und Eigenschaften

In diesem Beispiel wird die **Schema** -Eigenschaft von SMO verwendet, um eine Liste der Tabellen aus dem Sales-Schema in AdventureWorks2012 abzurufen:  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```

In diesem Beispiel wird die SMO-Methode **Script** verwendet, um ein Skript zu erstellen, das die **CREATE VIEW** -Anweisungen enthält, die Sie zur erneuten Erstellung der Sichten in AdventureWorks2012 benötigen:  

```powershell
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```

In diesem Beispiel wird mit der Methode SMO **Create** eine Datenbank erstellt und dann mit der Eigenschaft **State** angezeigt, ob die Datenbank vorhanden ist:  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```

## <a name="see-also"></a>Weitere Informationen

- [SQL Server PowerShell-Anbieter](sql-server-powershell-provider.md)
- [Navigieren in SQL Server PowerShell-Pfaden](navigate-sql-server-powershell-paths.md)
- [Konvertieren von URNs in SQL Server-Anbieterpfade](/powershell/module/sqlserver/Convert-UrnToPath)
- [SQL Server-PowerShell](sql-server-powershell.md)