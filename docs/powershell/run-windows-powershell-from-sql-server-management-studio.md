---
title: Ausführen von Windows PowerShell über SQL Server Management Studio | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Windows PowerShell-Sitzung im Objekt-Explorer im SQL Server Management Studio starten, wobei der Pfad zum Objektspeicherort Ihrer Wahl voreingestellt ist.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 3e74949152adb8ecadc720ced14ff3b29b196fd6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338164"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Ausführen von Windows PowerShell über SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sie können Windows PowerShell-Sitzungen aus dem **Objekt-Explorer** in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]starten. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] startet Windows PowerShell, lädt das Modul **SqlServer** und legt den Pfadkontext auf den zugeordneten Knoten in der Struktur **Objekt-Explorer** fest.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Wenn Sie die Ausführung von PowerShell für ein Objekt im **Objekt-Explorer** angeben, startet [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] eine Windows PowerShell-Sitzung, in denen die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Snap-Ins geladen und registriert wurden. Der Pfad für die Sitzung ist auf den Speicherort des Objekts voreingestellt, auf das Sie in Objekt-Explorer mit der rechten Maustaste geklickt haben. Wenn Sie beispielsweise im Objekt-Explorer mit der rechten Maustaste auf das Datenbankobjekt [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] klicken und dann **PowerShell starten** auswählen, wird der Windows PowerShell-Pfad folgendermaßen festgelegt:  

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Ausführen von PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>So führen Sie PowerShell in SQL Server Management Studio aus

1. Öffnen Sie den **Objekt-Explorer**.

2. Navigieren Sie zum Knoten für das zu verarbeitende Objekt.

3. Klicken Sie mit der rechten Maustaste auf das Objekt, und wählen Sie **PowerShell starten** aus.

## <a name="permissions"></a>Berechtigungen

Wenn PowerShell über [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] geöffnet wurde, wird die Sitzung nicht mit Administratorrechten ausgeführt, wodurch einige Aktivitäten wie Aufrufe der WMI verhindert werden können.  
  
## <a name="see-also"></a>Weitere Informationen

- [SQL Server-PowerShell](sql-server-powershell.md)