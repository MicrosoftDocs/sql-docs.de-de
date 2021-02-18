---
title: Verwalten der Befehlszeilenergänzung (SQL Server PowerShell)
description: Hier erfahren Sie, wie Sie die PowerShell-Registerkartenvervollständigung unter Windows steuern, indem Sie drei Variablen in den SQL Server PowerShell-Modulen ordnungsgemäß verwenden.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 6296848a-890f-4ad3-8d9f-92ed6a79aa00
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: cca82925c95b360bff522bc813798485628d4382
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339038"
---
# <a name="manage-tab-completion-with-sql-server-powershell"></a>Verwalten der Befehlszeilenergänzung in SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell-Snap-Ins führen drei Variablen (**$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** und **$SqlServerIncludeSystemObjects**) zum Steuern der Windows PowerShell-Befehlszeilenergänzung ein. Die Befehlszeilenergänzung reduziert den Tippaufwand durch Zurückgeben von Tabellen mit Elementen, deren Namen mit der eingegebenen Zeichenfolge beginnen.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Wenn Sie die Windows PowerShell-Befehlszeilenergänzung verwenden und einen Teil eines Pfad- oder Cmdlet-Namens eingegeben haben, können Sie die TAB-TASTE drücken, um eine Liste der Elemente anzuzeigen, deren Namen mit dem bereits eingegebenen Teil übereinstimmen. Sie können dann das gewünschte Element aus der Liste auswählen, ohne den Rest des Namens eingeben zu müssen.  

Wenn Sie in einer Datenbank arbeiten, die mehrere Objekte enthält, können die Listen der Befehlszeilenergänzung umfangreich werden. Einige [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objekttypen, z. B. Sichten, verfügen außerdem über zahlreiche Systemobjekte.  

Die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Snap-Ins führen drei Systemvariablen ein, mit denen Sie die von der Befehlszeilenergänzung und **Get-ChildItem** bereitgestellte Informationsmenge steuern können.

## <a name="sqlservermaximumtabcompletion--n"></a>$SqlServerMaximumTabCompletion =** *n*

Gibt die maximale Anzahl der Objekte an, die in der Befehlszeilenergänzungsliste enthalten sein sollen. Wenn Sie die TAB-TASTE an einem Pfadknoten betätigen, der mehr als *n* Objekte aufweist, wird die Befehlszeilenergänzungs-Liste bei *n* abgeschnitten. *n* ist eine ganze Zahl. 0 ist die Standardeinstellung und bedeutet, dass die Anzahl der aufgeführten Objekte nicht begrenzt ist.  

## <a name="sqlservermaximumchilditems--n"></a>$SqlServerMaximumChildItems =** *n*

Gibt die maximale Anzahl der von **Get-ChildItem** angezeigten Objekte an. Wenn Sie **Get-ChildItem** an einem Pfadknoten ausführen, der mehr als *n* Objekte aufweist, wird die Befehlszeilenergänzungs-Liste bei *n* abgeschnitten. *n* ist eine ganze Zahl. 0 ist die Standardeinstellung und bedeutet, dass die Anzahl der aufgeführten Objekte nicht begrenzt ist.  

## <a name="sqlserverincludesystemobjects---true--false-"></a>$SqlServerIncludeSystemObjects =** { **$True** | **$False** }

Wenn **$True**, werden Systemobjekte durch Befehlszeilenergänzung und **Get-ChildItem** angezeigt. Wenn **$False**, werden keine Systemobjekte angezeigt. Die Standardeinstellung ist **$False**.  

## <a name="set-the-sql-server-tab-completion-variables"></a>Festlegen der Variablen für die SQL Server-Befehlszeilenergänzung

Legen Sie für die vom Standardwert zu ändernden Variablen einen neuen Wert fest.  

### <a name="example-powershell"></a>Beispiel (PowerShell)

Im folgenden Beispiel werden alle drei Variablen festgelegt und ihre Einstellungen aufgeführt:  

```powershell
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```

## <a name="see-also"></a>Weitere Informationen

- [Installieren von SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server-PowerShell](sql-server-powershell.md)