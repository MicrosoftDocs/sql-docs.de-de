---
title: OLE-Automatisierungsresultsets | Microsoft-Dokumentation
description: Hier erfahren Sie, dass das Array als Resultset an den Client zurückgegeben wird, wenn eine OLE-Automatisierungseigenschaft oder -methode Daten in einem Array mit einer oder zwei Dimensionen zurückgibt.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [SQL Server], OLE Automation
- two-dimensional arrays
- one-dimensional arrays
- result sets [SQL Server], OLE Automation
- OLE Automation [SQL Server], result sets
- arrays [SQL Server]
ms.assetid: b2f99e33-2303-427c-94b9-9d55f8e2a6ab
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cae5f0fb362b0e3e303a17b1fdfbf5beca3abe2c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479121"
---
# <a name="ole-automation-result-sets"></a>OLE-Automatisierungsresultsets
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Wenn eine OLE-Automatisierungseigenschaft oder -methode Daten in einem ein- oder zweidimensionalen Array zurückgibt, wird das Array als Resultset an den Client zurückgegeben:  
  
-   Ein eindimensionales Array wird dem Client als einzeiliges Resultset zurückgegeben, das so viele Spalten wie Elemente im Array enthält. Ein Array(10) wird z. B. als einzelne Zeile mit 10 Spalten zurückgegeben.  
  
-   Ein zweidimensionales Array wird dem Client als Resultset zurückgegeben, dessen Anzahl an Spalten der Anzahl der Elemente in der ersten Dimension des Arrays entspricht und dessen Anzahl an Zeilen der Anzahl der Elemente in der zweiten Dimension des Arrays entspricht. Ein Array(2,3) wird z. B. als 2 Spalten in 3 Zeilen zurückgegeben.  
  
 Wenn der Rückgabewert für eine Eigenschaft oder eine Methode ein Array ist, geben sp_OAGetProperty oder sp_OAMethod ein Resultset an den Client zurück. (Ausgabeparameter für Methoden können nicht einem Array entsprechen.) Diese Prozeduren durchsuchen alle Datenwerte des Arrays, um die geeigneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datentypen und -Datenlängen für jede Spalte des Resultsets zu ermitteln. Für eine bestimmte Spalte verwenden diese Prozeduren den Datentyp und die -länge, die erforderlich sind, um alle Datenwerte in dieser Spalte darzustellen.  
  
 Wenn alle Datenwerte einer Spalte denselben Datentyp aufweisen, wird dieser Datentyp für die gesamte Spalte verwendet. Wenn Datenwerte in einer Spalte aus unterschiedlichen Datentypen bestehen, wird der Datentyp für die gesamte Spalte entsprechend der folgenden Tabelle ausgewählt. Zum Verwenden der folgenden Tabelle wählen Sie einen Datentyp auf der Achse der linken Zeile und einen zweiten Datentyp auf der Achse der obersten Spalte aus. Der Schnittpunkt von Zeile und Spalte beschreibt den Datentyp der Resultset-Spalte.  
  
|   | **int** | **float** | **money** | **datetime** | **varchar** | **nvarchar** |
| - | ------- | --------- | --------- | ------------ | ----------- | ------------ |
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Gespeicherte OLE-Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
 [OLE-Automatisierungsprozeduren (Serverkonfigurationsoption)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
  
