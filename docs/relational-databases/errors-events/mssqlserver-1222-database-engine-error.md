---
description: MSSQLSERVER_1222
title: MSSQLSERVER_1222 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1222 (Database Engine error)
ms.assetid: c5b1c2f4-f591-4cc1-aa17-204636a27f29
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e77d9799a8d78c34ee47b13fc728f20361c3cb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197157"
---
# <a name="mssqlserver_1222"></a>MSSQLSERVER_1222
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|1222|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|LK_TIMEOUT|  
|Meldungstext|Das Timeout für Sperranforderung wurde überschritten.|  
  
## <a name="explanation"></a>Erklärung  
Eine erforderliche Ressource wurde durch eine weitere Transaktion länger gesperrt, als diese Abfrage warten konnte.  
  
## <a name="user-action"></a>Benutzeraktion  
Führen Sie die folgenden Aufgaben aus, um das Problem zu beheben:  
  
1.  Lokalisieren Sie nach Möglichkeit die Transaktion, durch die die erforderliche Ressource gesperrt wird. Verwenden Sie die dynamischen Verwaltungssichten **sys.dm_os_waiting_tasks** und **sys.dm_tran_locks**.  
  
2.  Beenden Sie gegebenenfalls die Transaktion, wenn die Sperre durch die Transaktion weiterhin besteht.  
  
3.  Führen Sie die Abfrage erneut aus.  

Ändern Sie das Sperrtimeout oder die problematischen Transaktionen, sodass die Sperre für eine kürzere Zeit besteht, wenn der Fehler häufiger auftritt.  
  
