---
description: MSSQLSERVER_2511
title: MSSQLSERVER_2511 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 2511 (Database Engine error)
ms.assetid: 9a00c0ed-eb4b-4fae-8016-192396006c37
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 04c039dae7cdddecd6b85788042da76b8d09cfc1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487044"
---
# <a name="mssqlserver_2511"></a>MSSQLSERVER_2511
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|2511|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DBCC_KEYS_OUT_OF_ORDER|  
|Meldungstext|Tabellenfehler: Objekt-ID %d, Index-ID %d, Partitions-ID %I64d, Zuordnungseinheits-ID %I64d (%.*ls-Typ). Falsche Schlüsselreihenfolge auf Seite %S_PGID, Slots %d und %d.|  
  
## <a name="explanation"></a>Erklärung  
Im angegebenen Index wurden Schlüssel mit falscher Reihenfolge erkannt. Die Seite, in der die Schlüssel enthalten sind, ist möglicherweise beschädigt.  
  
## <a name="user-action"></a>Benutzeraktion  
Erstellen Sie den angegebenen Index mithilfe der ALTER INDEX REBUILD-Anweisung neu.  
  
