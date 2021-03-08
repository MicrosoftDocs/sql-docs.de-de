---
title: Ausführen von Windows PowerShell über SQL Server Management Studio
description: Hier erfahren Sie, wie Sie eine Windows PowerShell-Sitzung im Objekt-Explorer im SQL Server Management Studio starten, wobei der Pfad zum Objektspeicherort Ihrer Wahl voreingestellt ist.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 9fd9b7039680b05515d1f70102408394b5443c9c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839457"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Ausführen von Windows PowerShell über SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sie können Windows PowerShell-Sitzungen in SQL Server Management Studio (SSMS) über den **Objekt-Explorer** starten. SSMS startet Windows PowerShell, lädt das Modul **SqlServer** und legt den Pfadkontext auf den zugeordneten Knoten in der Struktur von **Objekt-Explorer** fest.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Wenn Sie die Ausführung von PowerShell für ein Objekt im **Objekt-Explorer** angeben, startet SSMS eine Windows PowerShell-Sitzung, in die die SQL Server PowerShell-Snap-Ins geladen und registriert wurden. Der Pfad für die Sitzung ist auf den Speicherort des Objekts voreingestellt, auf das Sie in Objekt-Explorer mit der rechten Maustaste geklickt haben.

Wenn Sie beispielsweise im Objekt-Explorer mit der rechten Maustaste auf das Datenbankobjekt AdventureWorks klicken und dann **PowerShell starten** auswählen, wird der Windows PowerShell-Pfad folgendermaßen festgelegt:

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Ausführen von PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>So führen Sie PowerShell in SQL Server Management Studio aus

1. Öffnen Sie den **Objekt-Explorer**.

2. Navigieren Sie zum Knoten für das zu verarbeitende Objekt.

3. Klicken Sie mit der rechten Maustaste auf das Objekt, und wählen Sie **PowerShell starten** aus.

## <a name="permissions"></a>Berechtigungen

Wenn PowerShell über SQL Server Management Studio geöffnet wurde, wird die Sitzung nicht mit Administratorrechten ausgeführt, wodurch einige Aktivitäten wie Aufrufe der WMI verhindert werden können.

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-PowerShell](sql-server-powershell.md)