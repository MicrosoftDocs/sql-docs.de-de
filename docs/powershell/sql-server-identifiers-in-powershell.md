---
title: SQL Server-Bezeichner in PowerShell-Pfaden | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Pfade, die Windows PowerShell-Anbieter verwenden, um Datenhierarchien verfügbar zu machen. Außerdem wird die Notwendigkeit des Codierens bestimmter Zeichen erläutert, die in diesen Pfaden nicht von PowerShell unterstützt werden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- PowerShell [SQL Server], identifiers
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- identifiers [SQL Server], PowerShell
- Cmdlets [SQL Server], Decode-Sqlname
ms.assetid: 651099b0-33b4-453a-a864-b067f21eb8b9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: b448e5455b985baffbeb8eb611c3f357e083cff2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354927"
---
# <a name="sql-server-identifiers-in-powershell"></a>SQL Server-Bezeichnern in PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter für Windows PowerShell verwendet [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichner in Windows PowerShell-Pfaden. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichner können Zeichen enthalten, die Windows PowerShell in Pfaden nicht unterstützt. Sie müssen diese Zeichen mit Escapezeichen versehen oder besondere Codierungen für sie verwenden, wenn Sie die Bezeichner in Windows PowerShell-Pfaden verwenden.  
  
[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="sql-server-identifiers-in-windows-powershell-paths"></a>SQL Server-Bezeichner in Windows PowerShell-Pfaden  

Windows PowerShell-Anbieter machen Datenhierarchien mithilfe einer Pfadstruktur verfügbar, die dem Windows-Dateisystem ähnelt. Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter implementiert Pfade zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objekten. Für [!INCLUDE[ssDE](../includes/ssde-md.md)]sind das Laufwerk auf SQLSERVER und der erste Ordner auf \SQL festgelegt, und auf die Datenbankobjekte wird als Container und Elemente verwiesen. Dies ist der Pfad zur Vendor-Tabelle im Purchasing-Schema der [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] -Datenbank in einer Standardinstanz von [!INCLUDE[ssDE](../includes/ssde-md.md)]:  
  
```  
SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichner sind die Namen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objekten, z. B. Tabellen- oder Spaltennamen. Es gibt zwei Arten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichnern:  
  
-   Reguläre Bezeichner sind auf einen Satz von Zeichen beschränkt, die auch in Windows PowerShell-Pfaden unterstützt werden. Diese Namen können in Windows PowerShell-Pfaden verwendet werden, ohne geändert zu werden.  
  
-   Begrenzungsbezeichner können Zeichen verwenden, die in Windows PowerShell-Pfadnamen nicht unterstützt werden. Begrenzungsbezeichner werden Bezeichner in Klammern genannt, wenn sie in Klammern eingeschlossen sind ([IdentifierName]), und Bezeichner in Anführungszeichen, wenn sie in doppelte Anführungszeichen eingeschlossen sind ("IdentifierName"). Wenn ein Begrenzungsbezeichner Zeichen verwendet, die in Windows PowerShell-Pfaden nicht unterstützt werden, müssen diese Zeichen entweder codiert oder mit Escapezeichen versehen werden, bevor der Bezeichner als Container- oder Elementname verwendet werden kann. Codierung funktioniert bei allen Zeichen. Einige Zeichen, z. B. das Doppelpunktzeichen (:), können nicht mit Escapezeichen versehen werden.  
  
## <a name="sql-server-identifiers-in-cmdlets"></a>SQL Server-Bezeichner in Cmdlets  
 Einige [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Cmdlets verfügen über einen Parameter, der einen Bezeichner als Eingabe akzeptiert. Die Parameterwerte werden i. d. R. als von Anführungszeichen umschlossene Zeichenfolgenkonstanten oder in Zeichenfolgenvariablen angegeben. Wenn Bezeichner als Zeichenfolgenkonstanten oder in Variablen angegeben werden, gibt es keinen Konflikt mit dem von Windows PowerShell unterstützten Zeichensatz.  
  
## <a name="sql-server-identifier-tasks"></a>Tasks der SQL Server-Bezeichner  
  
|Taskbeschreibung|Artikel|  
|----------------------|-----------|  
|Beschreibt, wie ein Instanzname angegeben wird, einschließlich der Name des Computers, auf dem die Instanz ausgeführt wird.|[Angeben von Instanzen im SQL Server PowerShell-Anbieter](specify-instances-in-the-sql-server-powershell-provider.md)|  
|Beschreibt, wie die hexadezimale Codierung für Zeichen in Begrenzungsbezeichnern angegeben wird, die in Windows PowerShell-Pfaden nicht unterstützt werden. Beschreibt außerdem, wie die Hexadezimalzeichen decodiert werden.|[Codierung und Decodierung von SQL Server-Bezeichnern](encode-and-decode-sql-server-identifiers.md)|  
|Beschreibt, wie das Windows PowerShell-Escapezeichen für in PowerShell-Pfaden nicht unterstützte Zeichen verwendet wird.|[Escape-Bezeichner für SQL Server](escape-sql-server-identifiers.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server PowerShell-Anbieter](sql-server-powershell-provider.md)   
 [SQL Server-PowerShell](sql-server-powershell.md)   
 [Datenbankbezeichner](../relational-databases/databases/database-identifiers.md)  
  
  
