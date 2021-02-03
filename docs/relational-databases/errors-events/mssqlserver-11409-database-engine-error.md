---
description: MSSQLSERVER_11409
title: MSSQLSERVER_11409 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 11409 (Database Engine error)
ms.assetid: 99b71a1c-a72d-4ca9-9d00-4230c9042ba5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ad5ea6caf9aec21ffcad0c7b2c0ebc9ec48b76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197212"
---
# <a name="mssqlserver_11409"></a>MSSQLSERVER_11409
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|11409|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|ALTERCOL_COLSET_DROP|  
|Meldungstext|Der '%.*ls'-Spaltensatz in der '%.\*ls'-Tabelle kann nicht entfernt werden, da die Tabelle mehr als 1025 Spalten enthält.|  
  
## <a name="explanation"></a>Erklärung  
Tabellen können maximal 1024 Spalten enthalten, die nicht als Spalte mit geringer Dichte oder berechnete Spalte festgelegt sind. Wenn die Tabelle aufgrund von Sparsespalten mehr als 1024 Spalten aufweist, muss für die Tabelle ein Spaltensatz definiert werden. Die Tabelle, auf die verwiesen wird, weist mehr als 1024 Spalten auf, und Sie haben versucht, den Spaltensatz zu entfernen.  
  
## <a name="user-action"></a>Benutzeraktion  
Mit den aktuellen Spalten in der Tabelle müssen Sie den Spaltensatz beibehalten.  
  
Zum Entfernen des Spaltensatzes entfernen Sie zunächst Spalten aus der Tabelle, bis nur noch 1024 Spalten enthalten sind. Dann können Sie den Spaltensatz entfernen.  
  
